---
title: "Extracting Restaurant Metadata"
date: 2025-09-12
---

With [a simple scraper for restaurant pages](https://jaredadler.github.io/local-good-spots-blog/2025/09/09/restaurant-page-scraper.html) written, I manually chose a few sample restaurant pages, saved their raw HTML locally, and started work on this step: extracting restaurant metadata from a restaurant page.

In your browser, take a look at the page source for this restaurant page as an example:

https://tabelog.com/tokyo/A1317/A131701/13312586/

Until I'm further along in the project, I don't know exactly which metadata from a restaurant would be useful for a model's understanding of the restaurant's similarity to someone's preferences and criteria. So I started with a few obvious ones:

- the name of the restaurant
- the review score of the restaurant
- the number of reviews that have been submitted
- the "genre" of restaurant (which looks more like tags than an exclusive category)
- the closest train station to the restaurant



I gave claude the task to read the local HTML and write a python script to read in a file and use BeautifulSoup to extract that metadata, returning it as a JSON object. My thinking was that Claude may struggle with being given too many metadata to identify and extract at once, so I aimed to start with five metadata and write new prompts to refactor the python script and add more metadata.

I provided the following prompt:

```
I will have a directory of files which each contain the raw HTML of one restaurant on tabelog. Each raw HTML contains a restaurant name, review score, number of reviews, list of genre, and closest train station. An example file is /home/jared/projects/data/good-local-spots/scraped_content.txt in highlevelparser.py, create a function which takes a filepath as input, uses an html parser like BeautifulSoup to extract those restaurant metadata, and returns it as a JSON object following this format {"restaurantName": "", "reviewScore": "", "reviewCount": "", "restaurantGenre": ["", ""], "closestTrainStation": ""}
```

Claude couldn't find the section of HTML that specified train station, so I had to follow up with more specific instructions specifying the text nearby the train station provided:

```
Adjust the function parse_tabelog_html in highlevelparser.py to identify the train station listed next to the following title: "最寄り駅:"
```

Claude seems to be able to identify the HTML tags for each metadata and writes something like the following for each metadata:

```python
script_tags = soup.find_all('script', type='application/ld+json')
for script in script_tags:
    try:
        json_data = json.loads(script.string)
        if json_data.get('@type') == 'Restaurant':
            result["restaurantName"] = json_data.get('name', '')
            
            # Extract review score and count
            aggregate_rating = json_data.get('aggregateRating', {})
            result["reviewScore"] = str(aggregate_rating.get('ratingValue', ''))
            result["reviewCount"] = str(aggregate_rating.get('ratingCount', ''))
            
            # Extract genres from servesCuisine
            serves_cuisine = json_data.get('servesCuisine', '')
            if serves_cuisine:
                result["restaurantGenre"] = [genre.strip() for genre in serves_cuisine.split('、')]
            
            break
    except (json.JSONDecodeError, AttributeError):
        continue
```

This implementation would only work consistently if the page structure for each restaurant page is the same and if that page structure stays the same. Relying on those assumptions, I yield a simple script using BeautifulSoup to extract this metadata.

I then kept building on this to add more metadata. In some of these cases I manually examined the browser-rendered restaurant page and/or the raw HTML in order to give Claude more specific instructions.

Extract restaurant address:

```text
In the raw html of a Tabelog restaurant page, there's a table titled "店舗情報（詳細）". Change the function parse_tabelog_html in highlevelparser.py to also extract the restaurant address and include it in the result JSON as a field "address". Find the row in the table called 住所 and parse it. Only take the first line of text in the field.
```

Extract restaurant telephone number:

```text
Update the function parse_tabelog_html in highlevelparser.py to extract the restaurant phone number, which should be available in a field called "telephone"
```

Extract restaurant price range:

```text
In the same table called 店舗基本情報, there is a field called 予算 which lists the expected price range for lunch and dinner. Adjust the function parse_tabelog_html in highlevelparser.py to extract lunch and dinner price range bounds, returning them in the json structure as the following fields: priceDinnerMin, priceDinnerMax, priceLunchMin, priceLunchMax
```

Extract restaurant price range (attempt 2):

```text
In the same table called 店舗基本情報, there is a field called 予算 which lists the expected price range for lunch and dinner. Adjust the function parse_tabelog_html in highlevelparser.py to extract lunch and dinner price range bounds, returning them in the json structure as the following fields: priceDinnerMin, priceDinnerMax, priceLunchMin, priceLunchMax

Here is an example of the expected raw HTML in the file which has lunch and dinner min and max values. Use the div labels and classes to extract the specific lunch and dinner ranges
<div class="rdheader-budget">
<p class="c-rating-v3 c-rating-v3--s rdheader-budget__icon">
<i class="c-rating-v3__time c-rating-v3__time--dinner" aria-label="Dinner" role="img"></i>
<span class="c-rating-v3__val">
  <a href="https://tabelog.com/tokyo/A1317/A131706/13120700/dtlratings/#price-range" class="rdheader-budget__price-target">￥5,000～￥5,999</a>
</span>
<p class="c-rating-v3 c-rating-v3--s rdheader-budget__icon">
<i class="c-rating-v3__time c-rating-v3__time--lunch" aria-label="Lunch" role="img"></i>
<span class="c-rating-v3__val">
  <a href="https://tabelog.com/tokyo/A1317/A131706/13120700/dtlratings/#price-range" class="rdheader-budget__price-target">～￥999</a>
</span>
</p>
</div>
```

Altogether, the script returns the following fields in a JSON object:

- restaurantName: the name of the restaurant
- reviewScore: the review score for the restaurant
- reviewCount: the number of reviews posted for the restaurant
- restaurantGenre: the genre of food served by the restaurant
- closestTrainStation: the closest train station to the restaurant
- address: the address of the restaurant
- telephone: the restaurant's telephone number
- priceDinnerMin: the lower bound price range for dinner
- priceDinnerMax: the upper bound price range for dinner
- priceLunchMin: the lower bound price range for lunch
- priceLunchMax: the upper bound price range for lunch

Claude's implementation for extraction are a mix of getting specific fields in the HTML and regex-searching the page text. It would be hard to read and maintain. I might spend more time manually refactoring it or prompting Claude to refactor for a more organized and consistent implementation.

Thinking ahead, when this is run for tens of thousands of restaurant pages, it may need to be adjusted to write the result to a table in a database, but for now it is a good proof of concept and I can proceed with the next part of the project.

The script developed is here:

https://github.com/jaredadler/local-good-spots/blob/main/parserestaurantpage.py

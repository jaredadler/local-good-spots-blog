---
title: "Wider data extraction: get areas in a city"
date: 2025-09-26
---

The next piece I worked on (I had to step away from this project for a few weeks was a high level scraper.

I found a useful "site map" for restaurant pages that follows a hierarchy as follows:

```text

all
- city (e.g. tokyo, osaka)
  - area (e.g. jimbocho, hatagaya)
   - restaurant name furigana's first character (e.g. ル, ボ)
     - restaurant

```

Knowing this hierarchy could make a crawling task easier assuming it is complete.

First, I asked Claude to build a simple scraper to obtain a list of the areas (and URLs for those areas) of a city and write to a file. I was able to get a script working with a few follow-up prompts as follows:

```text
This restaurant review site has a sitemap organized by city, with this URL structure: https://tabelog.com/sitemap/tokyo/

On this page, there are a list of areas each with a link to a subpage.

Write a script that uses requests library to get the page contents, then parses the contents with BeautifulSoup and returns an array of urls for each area listed on the page.

```

This prompt didn't return anything, so I added this follow-up:

```text
The function get_area_urls in file getareasincity.py does not actually retrieve any URLs for areas in a city. An example area URL is like the following: https://tabelog.com/sitemap/tokyo/A1301-A130101/, please refactor the way beautifulsoup is used to parse the page contents to select the urls properly.
```

Once that was working, I added a write to local file:

```text
refactor the function in getareasincity.py to write the resulting array to a text file. One line per URL, with one column for the area name and one column for the URL. Write to the following location f"{os.getenv('LOCAL_GOOD_SPOTS_OUTPUT_DIR', '/restaurants/')}cities/{city}.txt"
```

The resulting script is [here](https://github.com/jaredadler/local-good-spots/blob/ff736fc2bd027dcbfdff5f326d9e57b1a3c57073/getareasincity.py)! On to the next one.






---
title: "Wider data extraction: get restaurants in an area"
date: 2025-09-29
---

With the work mentioned in the previous post [here](https://jaredadler.github.io/local-good-spots-blog/2025/09/26/areas-in-city.html), the next step is to pick an area and retrieve a list of all the restaurants in an area. In each area site map page there is a list of all japanese katakana characters (sort of like A-Z letters in an alphabet), with a link to a page listing all the restaurants in that area that start with that sound (regardless of whether the restaurant name is in latin, katakana, hiragana, kanji, or some combination). These pages appear to follow a consistent URL structure. An example area page:

https://tabelog.com/sitemap/tokyo/A1316-A131602/

And a list of restaurants in that area which start with "ko"

https://tabelog.com/sitemap/tokyo/A1316-A131602/ko/

So my objective is to have Claude write a script that takes an area page, loads each katakana sub-page, and collects the names and URLs of restaurants listed.

I started with that as a prompt:

```text

Tabelog's site map has area pages. Area pages break up the restaurants in an area by the first sound in their name (katakana), and have a sub-page for each sound. Write a function getrestaurantsinarea.py that takes a tabelog area page (example page https://tabelog.com/sitemap/tokyo/A1316-A131602/), loads the restaurant sub-page for each sound, and return a list of all of the restaurants (name and URL) in the area.

```

The task is simple enough that Claude got it in one shot. The implementation is a little more fragile than it could be, by simply looking for links on an area page that have a one or two-character `/{}/` structure suffixed to the area page URL. I could have instead asked it to refactor to construct all *possible* katakana character sub-pages from an array of katakana characters, then read each one, but both would work fine.

Then I further asked for refactoring to structure the result and write it to a local file:

``` text

refactor the function in getrestaurantsinarea.py to write the resulting array to a text file. One line per URL, with one column for the area name and one column for the URL. Write to the following location f"{os.getenv('LOCAL_GOOD_SPOTS_OUTPUT_DIR', '/restaurants/')}/{city}/{area}.txt", where {area} is the name of the area.

```

And one correction to write the restaurant name:

```text

getrestaurantsinarea.py does not write the restaurant name, it looks like it writes the area name. The restaurant name is the text of the link to the restaurant. Adjust the function to write the restaurant name and URL.

```

Running this script would execute `requests.get()` 40+ times. It is possible that some delay in these calls will be beneficial to avoid overloading the host or being flagged as non-human. I will come back to that later.

For now, with only a few prompts, I have a working script to write a list of all the restaurants in an area. The script can be found here:

https://github.com/jaredadler/local-good-spots/blob/64fdbf79ed197c73a39b8fbcfb07e50fab4b594b/getrestaurantsinarea.py
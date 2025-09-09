---
title: "A Simple scraper for restaurant pages"
date: 2025-09-09
---

This is a simple piece to have a python script read a URL and write to a specific directory.

The script I wrote is here:

https://github.com/jaredadler/local-good-spots/pull/1/files

First I asked Claude to create a function that takes a specified URL and specified output directory, read the URL using standard Python libraries, and write to the output directory. This worked straight away and the script defined a function `scrape_url` which uses `requests` and writes to a specified directory.

I adjusted this slightly as I expect to call this function for each restaurant page. The restaurant pages appear to follow a predictable URL structure with three IDs, first for top-level area (like Tokyo, Osaka), second for lower-level area (like Akasaka, Nakano), and third for restaurant ID. Not knowing if the restaurant ID is globally unique, I asked Claude to write a function that can parse a URL and construct a filename assuming this consistent URL structure. It's not included as `tabelog_restaurant_url_to_filename`.

I also recognized that since there are more scraping tasks I will need to do, having the `scrape_url` function remain generic might be useful later, so I manually refactored the function to take a filename parameter, passing `tabelog_restaurant_url_to_filename(url)` to `scrape_url as intended usage.

Tested it locally and feel this is sufficient for now. It might need further updates if this is designed to run on AWS.
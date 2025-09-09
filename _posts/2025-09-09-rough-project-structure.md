---
title: "Project approach"
date: 2025-09-08
---

I haven't thought through this whole project yet, but after some brainstorming I have an idea of the first milestone I want to reach.

Some context first: there are some sites used by Japanese citizens and residents to search and explore various restaurants, restaurant reviews, and photos, similar to Yelp in the US. One restaurant page can be quite data rich in terms of both metadata about the restaurant (where they're located, what type of cuisine they serve, whether they take credit cards, etc) and opinions about the restaurant by customers written with reviews and review scores.

My first milestone goal is to build a dataset with which to provide to LLMs to achieve my goal of a service that can show you the local good spots that meet your criteria.

This dataset should have both structured restaurant information and unstructured data (such as photos and user reviews) that an LLM could use to judge whether a restaurant meets some user's criteria and preferences.

To build this dataset, I list out the following to-dos:

- get a list all of the restaurant pages in an area of Japan
- get a restaurant's page data and parse/save the contents
- parse restaurant page HTML and transform its contents to structured metadata about the restaurant
- get a restaurant's review data (it looks like each review has an individual URL) and parse/save the contents
- design a data model for storing structured restaurant metadata and restaurant review data
- build a database/lake for restaurant metadata and review data
- insert structured restaurant metadata and review data into the database

I aim to get this done first on a small area of restaurants so that I can also start working on the application of the data for answering questions and suggesting good local spots using LLMs.

In order to scale this out to Tokyo as a whole (or further than that), I expect I'll need to deploy some of the above functionality to AWS ECS so that scraping can be run regularly and at scale.

So for now I'll focus on the above to-dos and will share updates for each piece.
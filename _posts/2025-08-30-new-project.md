---
title: "A New Project with Tokyo Restaurants and LLMs"
date: 2025-08-30
---

I'm Jared, a data scientist from the US. I spent 9 years living in Tokyo, Japan and recently moved to Taipei, Taiwan.

Tokyo always has a steady stream of tourists coming to experience Japan, with recent years setting new records for the number of foreign visitors. Over the years, I've had old friends, colleagues, and others in my extended social network get in touch to ask me for tips on what to do and where to eat. I love helping them and trying to be their tour guide based on my personal experience and ability to search for more local information in Japanese. Surely they have already done some of their own research but I feel like they are looking for some on-the-ground information and something that other tourists like them wouldn't have heard about. However, it's quite common for friends incoming to Tokyo to throw out quite broad questions and requests (sometimes paired with really specific requirements):

- Friend: "I like miso ramen, where's a good place to eat miso ramen (in Tokyo)"
  - Me: "uhh, there are hundreds of good miso ramen in Tokyo, no need to travel cross town to try a specific shop. Where is your hotel? I'll find a good one that's convenient for you."
- Friend: "I saw this kakigori (shaved ice) on instagram, have you tried it? Is it any good? Or do you know a better one I should try?"
  - Me: "yes I know that one. I always see people lining up for it. Let me find some other options for you that aren't as hyped in case you don't want to waste part of your trip waiting in a long line for that one."
- Friend: "I'm attending a conference in Tokyo near Shinagawa station staying in Oimachi. What are some good local spots for dinner? By the way, I don't like cigarette smoke and my partner doesn't eat pork."
  - Let me check what places I have marked on Google maps around that area and get back to you. What is your price range? And are you ok with a place that serves pork but at least has other options on the menu?
 
Typically I enjoy helping friends with these kind of questions, but it undeniably takes a bit of time to really answer them thoroughly, jogging my memory related to their question, searching the web myself in Japanese reading reviews and guides, and sometimes asking other local friends. It struck me that for travelers while Google Maps, social media, and ChatGPT can give you lots of stimulus - suggestions, lists of restaurants, review scores, and so on - discerning travelers will still be suspicious of their own judgement of what is worth trying, and with limited time on their trip, they seek some way to double-check their tentative plan or go beyond their research and find some vetted/low-risk "hidden gems".

Other than my own personal experience, I think what I do for those friends can actually be somewhat automated with LLMs: understand their travel plans and specific question, narrow down parts of the internet that have information about that, and synthesize that information to summarize it and make some tailored suggestions or recommendations.

If all travelers to a tourism-heavy city like Tokyo had access to this kind of service, my hope would be that it leads travelers to discover more local places to enjoy, helping many to avoid the areas and shops/restauratns that are already swarming with tourists and diffusing the increasing "over-tourism" feeling and perception between locals and travelers (in reality there are still plenty of neighborhoods in Tokyo without much tourism traffic full of local shops and restaurants not hostile to foreigners that would benefit from inbound business).

In this blog I will document my attempt to build this kind of tool for anyone to use. Besides my desire to solve this problem for tourists based on my personal experience as a long-term local foreigner, my other desire is to work on a project where I can use the latest advancements in AI in every aspect possible: (1) coding with AI agents, (2) extracting structured information from unstructured data/text available on the web, and (3) implementing a question-and-answer service with access to this specific information, enabling a more tailored service than what generic ChatGPT/Gemini services are able to deliver.

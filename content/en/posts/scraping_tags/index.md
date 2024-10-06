---
date: "2024-10-06"
draft: false
topics: 
  - Deckhelper_Project
title: Scraping Card Tags
---

I built a first proof of concept for my project idea. Here is how it went:
<!--more-->
In my [previous article](https://doubrixel.github.io/flemnitzer/en/posts/deckhelper_project_idea/) I described a tool idea that could help beginners build decks using Scryfall Tagger tags. To realize this, I need to scrape data from [EDHREC](https://edhrec.com/) and from [Scryfall tagger](https://tagger.scryfall.com/) since they both don't offer an API.

My game plan is the following:
1. Take a commander name
2. Find the EDHREC page
3. Scrape all the cards EDHREC lists for the commander including their synergy score
4. Scrape Scryfall Tagger to find the tags for each of the cards
5. Calculate scores for each tag to rank them
6. Do some recommendations (even though I don't know how I want to present those yet)

Taking a commander name is simple. I chose Etali, Primal Storm. ![Etali, Primal Storm](https://cards.scryfall.io/normal/front/a/1/a18fdaf9-964d-45e9-bd40-a8fc745ddd1e.jpg) She is the commander of my mono red *'I cast your spells for free and hope you die'* deck.

Finding the EDHREC page is no problem either. Just smash together https://edhrec.com/commanders/ and the commander name in [kebab case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case): etali-primal-storm.

Now comes the part that is most interesting to me: scraping websites. Since I've never done this before, I started off asking chatGPT. For a Node.js environment, it recommended using cheerio for static websites or puppeteer for dynamic websites. I thought that Scryfall tagger seems rather static, so I asked chatGPT for a cheerio code snippet to scrape the site. The snippet worked, but as a response, I found the loading screen that is displayed when opening Scryfall tagger. I never recognized it.

Knowing that the website to scrape is dynamic, I used puppeteer. It's quite fascinating to see a browser being handled by your program. Also, I do love how good generating code with ChatGPT works. I literally just copy-pasted the HTML container with the tags from Scryfall tagger and told chatGPT to extract those. It worked on the first try. Impressive!

Scraping EDHREC was a bit more challenging since I needed to instruct puppeteer to navigate to the ungrouped table view of the cards to make the card names and synergy scores scrapable easily. During the process, I learned that you can enter CSS selectors in the DOM search using the dev tools of a browser. That saved me a lot of time when trying to figure out a CSS selector that only selects the button I want.

To calculate the synergy scores for each tag, I take all recommended cards having the tag and add up their synergy scores.

Linking all these elements together, I now have a program that takes a commander name in kebab case and returns the top ten highest synergy tags. The program still has a few issues.

First of all, the process of scraping the tags for a card takes almost one second due to the loading time of Scryfall tagger. Since EDHREC recommends a little over one hundred cards, my program takes about two minutes for one commander. I see multiple possible solutions here. I could use a threshold and just ignore each card that has an absolute synergy value of less than 10. I could also implement caching, so I won't need to scrape the card again once it's known by my program. A third option would be to only scrape the names of all tags from Scryfall tagger, and for each tag query, Scryfall for all the cards having the tag. Saving those relations in a database would then allow me to access the tags of a card way faster.

My second critique is that some of the resulting tags are meta tags about the name. Etali, Primal Storm apparently has high synergy with [alliterative card names](https://tagger.scryfall.com/tags/card/alliteration). While one could call this a fun fact, it isn't useful for deck building.

Lastly, some of the gameplay tags don't seem relevant. Many of the cards recommended for Atla Palani, Nest Tender have a [unique type line](https://tagger.scryfall.com/tags/card/unique-type-line)[^1]. ![Atla Palani, Nest Tender](https://cards.scryfall.io/normal/front/2/b/2b8414f7-22c3-4e1c-934b-4a0e7acf951d.jpg) Having that doesn't seem to make the card any more useful than one that has a common type line in this deck. I could imagine this tag being relevant in a zoo deck where one wants to play as many different creature types as possible. So how could I find out programmatically whether a tag is truly relevant for a commander? Another example for this is the tag [noncreature-tribal](https://tagger.scryfall.com/tags/card/noncreature-tribal) which says that a card cares about a certain tribe entering but doesn't care if it's a creature or not. Many tribal cards carry this tag, and it is irrelevant for most tribal decks.

I should probably create a list of tags that is filtered out by default and leave the user the choice to remove them from the filter for the edge cases where the tags are truly relevant.

I want to finish this article with an exemplary output of the current program state for the Reaper King:
![Reaper King](https://cards.scryfall.io/normal/front/5/0/502740bf-0bff-4358-8996-1a27e5f0343f.jpg)

|tag|synergy score|
|-|-|
|alliteration|550|
|changeling|546|
|protects-creature|289|
|flicker-creature|287|
|repeatable token generator|279|
|mana filter|278|
|tribal-choose|278|
|card types in graveyard matter|243|
|synergy-artifact|221|
|french vanilla|219|

[^1]: https://mtg.fandom.com/wiki/Type_line

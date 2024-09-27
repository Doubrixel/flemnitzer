---
date: "2024-09-27"
draft: false
title: Project Idea - EDH Deckbuilding Helper
---

I am a passionate [EDH](https://magic.wizards.com/en/formats/commander) player. EDH is a quite casual format of Magic: The Gathering.\
To play EDH, one needs to have an EDH-Deck that consists of exactly 100 cards.<!--more-->
There may only be one copy of each card in the deck. Basic lands are the only exception. You may have multiple copies of those in your deck.\
You choose a legendary creature to serve as your commander and build the rest of your deck around their color identity and unique abilities. [^1]\
For me, the process of finding a commander I like and building a deck around it is a lot of fun. For beginners, this process can be quite overwhelming. There are literally thousands of cards to choose from, and you need to select exactly 100 singleton cards to be your deck.

There are several tools available that help you find the cards you want in your deck. For me, the most important ones are [EDHREC](https://edhrec.com/), [Scryfall](https://scryfall.com/), and [Sryfall tagger](https://tagger.scryfall.com/).\
EDHREC computes decklist statistics from popular deck-building websites and provides an overview of the most played cards for each commander.[^2] This website can give you a gist of what strategies other people use with a commander.\
Scryfall is a really powerful card search engine. Once you understand the [advanced search syntax](https://scryfall.com/docs/syntax), you can precisely find the cards that suit your commander and strategy.\
Sometimes it can be challenging to find cards with certain effects. That is what scryfall tagger is for. Tagger has gameplay tags for almost every effect. You can find cards that [draw cards](https://tagger.scryfall.com/tags/card/draw), cards that [give indestructible](https://tagger.scryfall.com/tags/card/gives-indestructible), or even cards with a typical [red effect](https://tagger.scryfall.com/tags/card/red-effect) or a [punny name](https://tagger.scryfall.com/tags/card/punny-name).

When I want to build a deck for a new commander, I use the following workflow:
1. Look up what cards other people typically use with my commander on EDHREC.
2. Come up with a deck strategy.
3. Look for the card effects I need using Scryfall with the tags that fit my desired play style.
4. Choose cards that fit in my budget.

While this process is creative work and a lot of fun, it also needs much concentration, time, and card knowledge. Looking at many commanders trying to find one with strategies I like can be a painstaking process.

That's why I want to build a tool that automates the steps outlined above. One shall be able to enter the name of a commander and receive the most important gameplay tags for the commander and some card recommendations that suit their commander.\
This would reduce the time and effort needed to understand a commander. Speeding up the process of finding commanders I like will give me more time to tinker with commanders that suit my style.

[^1]: https://magic.wizards.com/en/formats/commander
[^2]: https://edhrec.com/faq

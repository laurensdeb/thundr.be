---
title: 'What’s up with our Federal Parliament? A look at the Chamber of Representatives through its data.'
summary: "The last few weeks we’ve been working on a project for the Datavisualisation course of Bart Mesuere along with four fellow students. Over the course of 12 weeks we’ve developed a Scraper/API and built a number of very interesting visualizations on the Chamber of Representatives of our Federal Parliament. In this post we'll go into detail about the data and story behind this project."
date: 2021-05-07
permalink: /posts/2020/05/parliament
showAuthor: true
header:
  image: parliament-header.png
sharingLinks: 
- twitter
- facebook
- linkedin
- email
tags:
  - open data
  - projects
  - politics
  - federal parliament
aliases:
    - /posts/2020/05/parliament
    - /parlement
---


{{< lead >}}
The last few weeks I’ve been working on a project for the Datavisualisation course of <a href="https://twitter.com/BartMesuere">Bart Mesuere</a> along with four of my fellow students. Over the course of 12 weeks we’ve developed <a href="https://parlement.thundr.be">a Scraper/API</a> and built <a href="https://parlement-in-data.thundr.be">a number of very interesting visualizations</a> on the Chamber of Representatives of our Federal Parliament. In this post I wanted to go into detail about the data and story behind this project.
{{</ lead >}}

![](/img/posts/parliament-header.png)


## This will be easy... 
*…or so we thought.*

The Chamber of Representatives has had an Open Data strategy since 2016 according to its website. **However**, our hopefulness quickly turned into despair upon seeing the actual [“Open Data Portal”](https://data.dekamer.be), the data in the API turned out to be **woefully out-of-date** and **incomplete**. Details on the legislation debated in the parliament were only available for the past legislature (2014-2019). Also, no specifics are given on the name votes in the plenary meetings (i.e., we couldn't answer what each member voted on a specific piece of legislation).

## This will be hard...
*...back to square one.*

Being Computer Science students, we took on the challenge of building our own API. For this, our goal was to provide all the up-to-date data the current API lacks as well as offer it for a number of previous, historic legislatures too.  Fortunately, the Chamber does offer **HTML** versions of the meeting notes for its **plenary meetings**. (*Don't start imagining anything fancy, these are simply [exports](https://www.dekamer.be/doc/PCRI/html/55/ip102x.html) made from the Microsoft Word application that do mostly follow the same template.*)

![Sample Meeting Minutes from the Parliament](/img/posts/image-meetingnotes.png)

On a more **technical** note: We implemented the parsing logic in Python, aided by the [BeautifulSoup 4](https://pypi.org/project/beautifulsoup4/) library. (Available under the MIT license on [Github](https://github.com/laurensdeb/Federal-Parliament-Scraper).) While the data from the meeting notes contains an enormous wealth of data, we also have to enrich this with information from a few other public databases provided by The Chamber (i.e., for details on the legislation such as authorship and thematic labeling and for information on questions such as the responding minister or department). With regards to party affiliation and demographics of the members, we’re using [WikiData](https://www.wikidata.org/wiki/Wikidata:Main_Page) and [Wikipedia](https://wikipedia.org).

The parser itself provides a JSON export, which we use to build an API (which you can find in a hosted version at [parlement.thundr.be](https://parlement.thundr.be)). This build step is run on Github Actions (which, to our great joy, provides free build minutes to open-source projects), and publishes an updated version of the API using the latest meeting notes every weekday. Hosting is offered to us for free by [Netlify](https://netlify.com) and [CloudFlare](https://cloudflare.com). For now, this seems to be holding up quite well against the growing popularity of our work.

## Okay, this is really hard.
*But, we fixed it, right?*

Turns out building an API out of meeting notes that are still being written manually by the staff of the Chamber has a few extra **complexities**. We discovered quite some **inconsistencies** in the reports, mostly vote counts that were off with the names listed as having voted. 

However occasionally the mistakes in the report were more significant. Let’s have a look at the meeting notes of [December 15th, 2011](https://parlement-in-data.thundr.be/meeting?session=53&meeting=61) and [December 22nd, 2011](https://parlement-in-data.thundr.be/meeting?session=53&meeting=62)... When parsing the name votes in this meeting we noticed one very unusual name: “Jef Ramaekers”. While parsing the data from these meetings our parser errored out with a big warning: **“Member not found”**. After consulting the [electoral lists](https://nl.wikipedia.org/wiki/Categorie:Kandidatenlijsten_verkiezingen_in_België) for the legislature 2011-2014 we couldn’t find this member with any of the parties. However, a cursory Google search did learn us there is a former minister named [Jef Ramaekers](https://nl.wikipedia.org/wiki/Jef_Ramaekers), who unfortunately passed away in 2004.

![Image of Jef Ramaekers (source: Wikimedia Commons)](https://upload.wikimedia.org/wikipedia/commons/6/68/Eerste_steenlegging_Atheneum_Hof_van_Riemen.png)

Being **outstanding citizens** and all, we decided to fulfill our civic duties and e-mail the Chamber to get more details on this strange inclusion in the reported votes. Sadly, the archives of the Parliament were unable to provide us with an erratum. It seems to be, as we expected, a material error in the meeting notes. *Alas, no details on what actually happened here... But our scraper is able to accurately identify issues with the meeting notes, something which might even come in handy to help the parliament itself in the future.*

## How can we improve this ?
While it might be easy to blame the manual note-taking on all these issues we’ve experienced over the past few weeks, the absence of a proper open data strategy seems to be the real culprit here. While [efforts](https://data.dekamer.be) have been made to start such an initiative, they unfortunately seem to have hit a dead end up to now.

Perhaps our [Federal Parliament](https://dekamer.be) could find inspiration in regional and local initiatives, such as the [LBLOD project](https://lokaalbestuur.vlaanderen.be/lokale-besluiten-als-gelinkte-open-data) in Flanders. Here, manual notetakers are aided using Linked Data to create more structured versions of their traditional meeting notes during municipal council meetings.

Especially in times when many parties are advocating for an overhaul of our voting system, it is paramount to give people the right data to make an informed choice in the voting booth. With big ideas like making the switch from compulsory to voluntary voting and getting rid of the so-called “list votes” (where a voter could give their vote to an entire party instead of an individual member), properly informing voters is becoming more important than ever before.

If you’re interested in helping out this initiative, be sure to check out our [Github](https://github.com/laurensdeb/Federal-Parliament-Scraper) repository. Or, for the less tech-savy among us, get in touch with your representative in the parliament and help us advocate for better information on our federal politics. More information on the API that was built on top of all this can be found [here](https://parlement.thundr.be)

## Bonus: Explore our visualisations
*Now that you've read this huge explainer, it would be sad if we didn't reward you for your time, don't you agree?*

On top of the scraper and API outlined above, we've constructed several visualizations on our Chamber of Representatives [as a whole](https://parlement-in-data.thundr.be), [at a party level](https://parlement-in-data.thundr.be/party) and [on the individual MP's](https://parlement-in-data.thundr.be/member). For this, we were guided by the lecturer of this course, [Bart Mesuere](https://twitter.com/BartMesuere), with a lot of valuable insights and feedback.

## Special thanks
The project detailed in this blog post was built together with four of my fellow [Computer Science](https://informatica.ugent.be) students at [Ghent University](https://ugent.be), to whom I want to express my special thanks for their contributions to the succes of this massive undertaking:
- Niels Dossche
- Glenn Feys
- Tibo Vande Moortele
- Chato De Veirman

Also a big thank you to Bart Mesuere, and of course to everyone who has engaged with our visualizations and data over the last few days.
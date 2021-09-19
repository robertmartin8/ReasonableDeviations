---
layout: post
title: How I use Notion
category: productivity
---

This post is a fairly comprehensive discussion of how I use Notion (a free personal knowledge management app) to organise various aspects of my life: project management, reading, academics, plans/goals, investing, and more. The post is not designed to be read linearly -- pick and choose the bits that are relevant to you.

<!--more-->

Inevitably, this is going to sound like one massive ad for Notion. I have no affiliation with Notion, I'm just a very happy user. It's certainly not a perfect piece of software, but it has radically improved my productivity and efficiency. 

## The philosophy of Notion

I firmly believe that a good piece of personal knowledge management (PKM) software is an integral aspect of productivity and general efficiency in the modern day. The reality is that we consume and generate vast quantities of knowledge, whether that's personal plans, projects, or work stuff. PKM apps aim to help you organise this, freeing up RAM in your brain. 

We are fortunate to be living in the golden age of PKM. For several years, Evernote and OneNote were the only real players (and these were essentially just linear extensions of the classic "folders and files" paradigm). Recently, however, many apps have sprung up which have dramatically expanded the horizons of PKM -- apps like Notion, Roam, Obsidian, RemNote, and many others. 

Although each of these apps serves a slightly different purpose, one commonality is that they have evolved far beyond simple note-taking. Notion provides extremely flexible "databases", which can be viewed as tables, lists, or Kanban boards. Roam and Obsidian emphasise the linking of ideas, allowing you to build (and visualise!) your second brain. RemNote automatically generates flashcards based on your notes, allowing for optimal learning via spaced repetition.

I use Notion, Obsidian, and RemNote -- each for slightly different purposes. This post focuses on Notion. I don't expect that my entire workflow will be interesting to everybody, so please use the table of contents below to navigate to the specific areas that are relevant to you. The list is in approximate descending order of how much I use each piece of functionality.

A caveat: I am constantly refining my systems. There is no guarantee that my workflow will look the same a year from now. Similarly, if you are new to PKM, don't be daunted by the apparent complexity -- Rome wasn't built in a day. I started with a blank slate, then incrementally added things that I thought would be useful; I watched youtube videos in which various productivity-tubers explained their workflows, and picked different parts that I like; I went down rabbit holes setting up complex Notion tables, then scrapped them in favour of simpler setups.

Ultimately, the core philosophy of PKM is that you need to find a system that helps you achieve what you want to achieve. It is unlikely that what works best for me will also be optimal for you, though there may be certain motifs that come in handy. 

With all this out of the way, let's begin! 

## Contents

<!-- TOC -->
- [Prelude: intro to my Notion workspace](#prelude-intro-to-my-notion-workspace)
- [How I use Notion for projects](#how-i-use-notion-for-projects)
    - [PyPortfolioOpt](#pyportfolioopt)
    - [This blog](#this-blog)
    - [Collaborative projects](#collaborative-projects)
- [How I use Notion to track books](#how-i-use-notion-to-track-books)
    - [Other content consumption](#other-content-consumption)
- [How I use Notion for short term plans](#how-i-use-notion-for-short-term-plans)
- [How I use Notion for long term goals](#how-i-use-notion-for-long-term-goals)
- [How I used Notion for academics](#how-i-used-notion-for-academics)
    - [Tracking lecture progress and problem sheets](#tracking-lecture-progress-and-problem-sheets)
    - [Exam revision](#exam-revision)
- [How I use Notion as a second brain](#how-i-use-notion-as-a-second-brain)
- [How I use Notion for investing](#how-i-use-notion-for-investing)
    - [Tracking themes](#tracking-themes)
    - [Investment research](#investment-research)
    - [Investment pitches](#investment-pitches)
- [How I use Notion to track my learning](#how-i-use-notion-to-track-my-learning)
- [Conclusion](#conclusion)

<!-- /TOC -->

## Prelude: intro to my Notion workspace

Before diving into specific use cases, I will briefly explain how Notion works and give a high-level overview of my setup.

The fundamental unit in Notion is a **content block**. Notion has a huge list of possible content blocks. Here is a small subset of them:

- text and text derivatives, e.g headings, bulleted lists, toggle-able bulleted lists
- all kinds of media (embedded images, videos, audio etc)
- code and equations (typed in LaTeX)
- tables and Kanban boards (popularised by Trello)
- calendars

A **page** contains as many content blocks as you want, arranged however you want. What makes Notion so powerful is that pages can be arbitrarily grouped and nested. For example, each of the items in a Kanban board is itself a page, which you can click and open. If you wanted, you could therefore have a Kanban board where each item contains its own Kanban board etc (though I can't think why you'd want that!). Below is an image of my Projects and Learning page (more details later), which illustrates nested pages and mixed block types.

<center>
<img src="{{ site.imageurl }}notion/intro_mixed_blocks.png" style="width:100%;"/>
</center>

The page I visit the most is the Dashboard -- a home page of sorts. It links to many of my other pages (which house subpages and/or link to other pages).

<center>
<img src="{{ site.imageurl }}notion/dashboard.png" style="width:100%;"/>
</center>

Each page can be given an emoji and cover photo. I initially thought this was gimmicky, but it's a little thing that truly sparks joy and encourages me to stay organised.

I also have an Inbox page, which is essentially where I dump stuff that I may want to revisit later. This includes:

- Media to process e.g URLs, papers, podcasts episodes etc.
- Useful web tools
- Cool projects/startups I've come across
- Random thoughts or ideas  
- A list of blogs that I like 

<center>
<img src="{{ site.imageurl }}notion/inbox.png" style="width:80%;"/>
</center>

## How I use Notion for projects 

One of Notion's key strengths is project management. This is because administrating a project requires you to keep track of a tonne of information in different formats: important tasks, long-term goals, strategy brainstorming, admin details, historical progress, etc. 

Notion's flexibility means that you can view each of these data types in the most natural way, for example: visualising tasks using Kanban (Trello) boards, creating tables to store a list of documents, or having simple text files to store deployment details.

In this section, I give three examples of projects that I manage using Notion.

### PyPortfolioOpt

PyPortfolioOpt is an open-source python library that I developed. I use a Kanban board in a Notion page to keep track of the features I am working on. 

<center>
<img src="{{ site.imageurl }}notion/pf_kanban.png" style="width:100%;"/>
</center>

Each item in the Kanban board is itself a page, which can contain any other content blocks.

<center>
<img src="{{ site.imageurl }}notion/pf_kanban_item.png" style="width:80%;"/>
</center>

In addition to the Kanban board, I use toggle lists to keep track of PyPortfolioOpt's professional users and citations, and have various subpages to draft publicity or explore derivative projects.

<center>
<img src="{{ site.imageurl }}notion/pf_misc.png" style="width:100%;"/>
</center>

The "Admin details" subpage contains instructions for deployment, builds etc. After a few months away from the project, this is the kind of stuff I forget!

### This blog

Notion is very useful for managing the writing process of blog posts. At any given time I have a bunch of ideas, half-written posts, etc. A Kanban board helps me prioritise which to work on. 

<center>
<img src="{{ site.imageurl }}notion/rd.png" style="width:100%;"/>
</center>

Within each Kanban subpage, I write out a rough plan for the post and jot down any relevant notes or ideas. 

<center>
<img src="{{ site.imageurl }}notion/rd_item.png" style="width:100%;"/>
</center>

### Collaborative projects

Notion is also excellent for collaborative projects. In the free plan, a page can be shared with up to 5 guests. 

Earlier this year I was exploring a business opportunity with some colleagues (related to crypto market making). We used Notion to assign tasks (with due dates) to different members, and we could comment on each other's pages and blocks. 

<center>
<img src="{{ site.imageurl }}notion/collab.png" style="width:100%;"/>
</center>

I also used Notion to build a resource library for the project. Each row represents a piece of content (e.g a book or journal article). Once read, you add your name to the "read by" column. We can then perform complicated filtering on the database. In the example below, I filtered to find the resources that nobody had yet read and saved this as an "unread" view -- allowing us to track our collective blind spots.

<center>
<img src="{{ site.imageurl }}notion/collab2.png" style="width:100%;"/>
</center>


For the same resource database, I created another **view** (same data but different way of looking at it), filtered to find the resources that *I* hadn't read (sorted in descending order of priority).

<center>
<img src="{{ site.imageurl }}notion/collab3.png" style="width:80%;"/>
</center>

Although I've only used Notion for one collaborative project, it was a fantastic experience.

## How I use Notion to track books 

I am obsessed with reading books. But I don't just want to read, I want to *retain* the information I consume and incorporate the ideas into my thinking. My overall workflow for reading books is as follows:

1. Read the book on Kindle, highlighting as much as possible. (When reading physical books, use sticky notes instead).
2. After reading the book, collate all the highlights. For kindle highlights, I've got a [python script](https://github.com/robertmartin8/kindleclippings) that pulls the notes into text files, though if most of your books are from Amazon it may be easier to use something like [Readwise](https://readwise.io/).
3. Copy-paste these highlights into a Notion page for the book.
4. Reorganise the highlights, grouping them by topic so it's easier to revisit them in future. 
4. Write a book review within the Notion page and post this to Goodreads.
5. Summarise the book in my own words (ideally in ~5 bullet points) and put this summary into Obsidian.

This workflow is not as efficient as it could be – for instance, I could probably automate the transfer of highlights from my Kindle to Notion. But actually, I appreciate the "slowness" because it forces me to pay more attention to the highlights, improving my retention.

This is my Books database: it contains all of the book pages, tagged by genre (along with some other metadata).

<center>
<img src="{{ site.imageurl }}notion/books_db.png" style="width:80%;"/>
</center>

Below is a page for *The Success Equation* by Michael Mauboussin. Some things to note:

- I try to write a one-sentence headline summarising my takeaway from the book. This can be difficult, but it forces me to internalise the key message from the book.
- Notice that within the book review there are clickable links to other books. Making links between different things I read is *incredibly* important to me -- it makes me more creative, and invites a deeper contextual understanding of ideas.
- The act of categorising quotes by topic can also be quite draining but it's very worthwhile retrospectively when I'm trying to resurface a particular idea/theme.
- Using author tags and genre tags allows the book to show up in filtered searches.

<center>
<img src="{{ site.imageurl }}notion/book_eg.png" style="width:80%;"/>
</center>

The final step of my workflow is to write a summary (different to the book review) in an Obsidian page. Obsidian is my "second-brain", containing all my linked ideas: the graph view shows how the particular piece of content fits into my network of content/ideas.

<center>
<img src="{{ site.imageurl }}notion/obsidian_ex.png" style="width:100%;"/>
</center>


### Other content consumption

My "Books" database is actually just a view into a more generalised "Information Media" database, containing web articles, blog posts, youtube videos etc that I've found interesting in addition to books. 

<center>
<img src="{{ site.imageurl }}notion/info_media.png" style="width:100%;"/>
</center>

I use the Notion web clipper to store articles I've read (or started reading) into this table. After consuming a piece of content, if I feel that it contains ideas that are relevant to me, I will make a short summary of it (ideally ~3 bullet points) then add it to Obsidian.

## How I use Notion for short term plans

There are two primary ways I use Notion for short term plans: daily plans and daily intentions (the two are linked)

A **daily intention** is one well-defined thing that I hope to achieve on a given day. This can be work/school-related (tends to be the case for me), but occasionally I put in fun ones like "meet a new person" or "check in on a friend". 

Caveat: I have likely bastardised the "daily intentions" concept. If you google "daily intentions" you may see things like "I intend to be present / to have fun today / to be valued today". Rather than having such ambiguous and saccharine intentions, I prefer to have specific practical goals and I aim for a hit rate of about 50% -- that is, I choose goals that are sufficiently stretching that I only achieve about half of them. This can be discouraging, but my view is that if you're achieving all your goals, the goals aren't ambitious enough.

<center>
<img src="{{ site.imageurl }}notion/intentions.png" style="width:100%;"/>
</center>

These daily intentions link with my **daily plans**. Every evening, I reflect on the previous day, write a comment, and give it a quality rating. I then create tomorrow's daily plan and daily intention. The example below shows one of my daily plans shortly before final exams ("tripos Qs" means past exam questions).

<center>
<img src="{{ site.imageurl }}notion/daily_plans.png" style="width:80%;"/>
</center>


## How I use Notion for long term goals

I mentally break out long term goals into three categories: 

1. Specific things I want to do (e.g preparing for an internship, planning a holiday). I view this as a project management exercise, so use the structure described [above](#how-i-use-notion-for-projects).
2. General long term goals e.g New Years resolutions. I use simple text pages for this (could also be done in any other software).
2. Blocks of time that I want to fill productively (e.g term holidays, lockdowns etc). This merits some further discussion.

The below table is a list of things I wanted to get done during the "Rona holiday" -- the period in May-July 2020 when Easter term at Cambridge was "cancelled" due to Covid. 

- Tasks were tagged by "purpose" – what long term area of my life do they contribute to? This encouraged me to stay balanced and approach personal development from a more holistic perspective (rather than solely focusing on e.g careers).
- I gave each task a deliverable. Instead of passively watching an online course or leafing through a textbook, I challenged myself to transmute that content into a blog post, set of public notes, or code.

<center>
<img src="{{ site.imageurl }}notion/rona_holiday.png" style="width:80%;"/>
</center>

## How I used Notion for academics

Notion played a *critical* role in helping me achieve my academic goals. For context, this section pertains to the Cambridge Part II Astrophysics tripos (i.e 3rd-year Astrophysics). 

My overall study plan was as follows:

1. Watch lectures and make handwritten notes based on them (these notes need not be comprehensive).
2. Apply this knowledge to the weekly problem sheets, paying special attention to weaknesses.
3. Make comprehensive flashcards with everything that needs to be memorised.
4. Do a tonne of past papers and flashcards.
5. Profit

Some caveats:

- I don't use Notion for taking class notes -- for that, I use Notability on my iPad to make handwritten notes, and RemNote to make flashcards. I only used Notion to help administrate the execution of the above plan.
- The overall philosophy of a physics course may be quite different to that of other subjects, so the above plan may not be relevant to you (in which case the Notion setup will be even less relevant).
- Even if you're studying the same course, my usage of Notion was heavily optimised for the particular format of the Cambridge course.

Here is a basic overview of the structure of the Astro course:

- Astrophysics is split into eight compulsory lecture courses. 
  - At the end of the year, each exam paper contains one question from every course. You may only submit up to six questions, but three complete questions can get you a 1st class.
  - You can thus choose not to study certain courses -- the only downside is that you'll have less choice in the exam. 
  - Exam marks are nonlinear, rewarding complete answers. Getting 16/20 is *much* more valuable than getting 14/20. This is a key factor in exam strategy, encouraging judicious specialisation before the exams.
- In terms of teaching, there were 12 lectures a week, supplemented by two supervisions. A supervision (supo) is a small group session wherein 2-3 students discuss their problem sheet attempts with a supervisor (normally a PhD student or professor). Supo work is handed in several days before the supo and represents the bulk of the workload during term.

Given these rules, my goal was to "game the system" to maximise my academic performance for a fixed amount of effort, strategising around the constraints to see where I could find a competitive advantage. 

### Tracking lecture progress and problem sheets

For each lecture course, I used Notion to track how many lectures I had watched, as well as how many lectures were required for me to complete the next problem sheet. This meant that even as I slipped behind in lectures (a common occurrence) I always knew how far behind I was. 

<center>
<img src="{{ site.imageurl }}notion/supo_tracker.png" style="width:80%;"/>
</center>

Each lecture course had its own page, which I used to track:

- Areas that I wanted to revisit, e.g concepts/techniques from lectures that I didn't understand the first time around.
- Important derivations that I'd need to be able to reproduce in an exam.
- Which subtopics within the course I would "bin" (i.e not answer an exam question on), and which subtopics I was weak at (would only choose that exam question if there was nothing better).

<center>
<img src="{{ site.imageurl }}notion/sdsg.png" style="width:80%;"/>
</center>

To organise my progress in making flashcards, I created a new view on the same table to track how many pages of my notes I had converted into flashcards. This screenshot was taken after the exams, hence the 100% progress. 

<center>
<img src="{{ site.imageurl }}notion/flashcards.png" style="width:80%;"/>
</center>

### Exam revision

When it came to exam preparation, I needed a system that allowed me to:

- Understand what type of questions tend to come up. Is there any pattern or periodicity that can be exploited? For example, perhaps every year there is always a question on perturbation theory, or every three years there's a question about phase transitions. Are there certain derivations that frequently show up?
- Figure out which questions I'm good at -- in particular, which questions I can do quickly and accurately (time tends to be the limiting factor in the Astro exams). 
- Identify what I need to remember and what I should be able to derive.
- Highlight weaknesses -- either so that I can work on them, or so that I can avoid these questions in the exams. 

To that end, I built a big Notion database that tracked my attempts at past exam questions.

<center>
<img src="{{ site.imageurl }}notion/tripos_q.png" style="width:80%;"/>
</center>

Each exam question is a subpage in which I tracked:

- Metadata, like what course and what year/paper the question belongs to
- The course chapter to which the questions belong
- How long it took me to do the question and how hard I found it
- Whether I did the question "properly" (exam conditions)
- Whether I had subsequently revisited the question. There is no point in doing exam questions if you don't revisit your attempts and learn from them.

Furthermore, I summarised what each part of the question is asking for, and noted down anything specific I needed to revisit.

<center>
<img src="{{ site.imageurl }}notion/pqm_page.png" style="width:80%;"/>
</center>

Having a rigorous tracking methodology helped me get more out of the past paper questions by ensuring that I actively reflected on the question content and my attempts, building up a mental picture of my strengths and weaknesses. 

However, the game-changer was that after a certain point, I had a large Notion database containing both the content of the questions as well as my performance on these questions. I could then perform complex queries on this data to refine my exam strategy.

For example, I could filter by course and by paper to identify patterns. The screenshot below shows a filter of the paper 1 quantum mechanics questions (across all years). This filter helped me identify that paper 1 QM questions typically involve harmonic oscillators, which were normally easy or medium difficulty.

<center>
<img src="{{ site.imageurl }}notion/pqm_filter.png" style="width:80%;"/>
</center>

Another example: after completing the first couple of exams, I wanted to find all questions within the galaxies course related to topics that hadn't yet come up, since these topics had a very high probability of coming up on the last papers. This is a simple filter:

<center>
<img src="{{ site.imageurl }}notion/sdsg_filter.png" style="width:80%;"/>
</center>

The unfortunate truth is that anytime an exam format gives you discretion (e.g about which questions you can answer), exam strategy becomes just as important as knowing the content. Notion is a fantastic tool for refining this strategy and making data-driven decisions.

## How I use Notion as a second brain

After reading *How to Take Smart Notes* by Sonke Ahrens, I became aware of the "zettelkasten" note-taking method and built it in Notion.

There is a tonne of information about zettelkastens out there, so I will just link to a [youtube video](https://www.youtube.com/watch?v=lOY-drtTJX0&ab_channel=MukulKhanna) by Mukul Khanna that implements zettelkasten in Notion (I basically duplicated his setup).

Full disclosure: I have since migrated my zettelkasten to Obsidian. Perhaps I'll do a separate write-up on this in future!

## How I use Notion for investing

Notion helps me manage several aspects of investing/trading:

1. Tracking ideas related to specific themes/styles (e.g covid, spinoffs), as well as reflect on my trading performance. 
2. Conducting investment research
3. Storing investment pitches for posterity, so I can better understand my strengths/weaknesses as a trader.


### Tracking themes

<center>
<img src="{{ site.imageurl }}notion/investing.png" style="width:80%;"/>
</center>

I use subpages to track specific themes. For example, covid obviously created major dislocations and I made a Notion subpage to track my thoughts on it. Separately, I became interested in spinoff stocks so created a table to track active situations:

<center>
<img src="{{ site.imageurl }}notion/spinoffs.png" style="width:80%;"/>
</center>

### Investment research

To manage my investment research, I have a table containing subpages for each asset (tagged by asset class).

<center>
<img src="{{ site.imageurl }}notion/investment_research.png" style="width:80%;"/>
</center>

Each subpage contains links to resources, as well as my thoughts on the investment.

<center>
<img src="{{ site.imageurl }}notion/uranium.png" style="width:80%;"/>
</center>

For equities, I use the structure discussed in my [Equity Investing Checklist]({{ site.url }}/notes/investing_checklist).


### Investment pitches

After doing my research, if I want to add an idea to my portfolio, I write a pitch. This contains:

- One sentence introduction to the company (industry, current share price, current multiples)
- Investment thesis and key levers.
- How my view of the levers differs from consensus
- Risk factors (identify risks using a pre-mortem analysis)
- How the company fits into my portfolio (in terms of diversification or sector risk)
- Price target and exit conditions

Putting pitches into writing is a fantastic habit to get into. It keeps you accountable and helps you distinguish between skill and luck.  

I keep all of these pitches in a table partitioned into open and closed positions. For the closed positions, I tag it based on whether it was a success or failure (note: if a stock I bought went up but for reasons other than what I pitched, I flag it as "neutral").

<center>
<img src="{{ site.imageurl }}notion/investments.png" style="width:80%;"/>
</center>

## How I use Notion to track my learning

I use Notion to track things I want to self-study (e.g using online courses or textbooks). There is nothing special here: I use a Kanban board, and each page contains links to various learning resources (not notes though -- those would go in Notability, RemNote, or Obsidian).

<center>
<img src="{{ site.imageurl }}notion/learning.png" style="width:80%;"/>
</center>

Sadly, free time to self-study things is hard to come by -- hence the low position of this section within the post.

## Conclusion

I hope this post has given you some ideas as to the kind of things Notion can be useful for! Writing this post has made me realise just how important the app is for my productivity. 

At a philosophical level, I think Notion is a wonderful example of how "software eats the world". In the past, note-taking apps like OneNote or Evernote were just digital equivalents of existing systems: folders, notebooks, pieces of paper. But at some point there was a phase change: apps like Notion let you do things that are *impossible* in the physical world.

I consider myself to be a Notion power user, but at times I feel like I'm only scratching the surface of what the app can do. For example, you can build and host websites in Notion (with no code), and with the recently-released API, the spectrum of possibilities has widened even further.

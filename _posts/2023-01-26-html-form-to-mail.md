---
layout: post
title:  "mailto links FTW"
date:   2023-01-23 20:55:00 +0100
categories: html form mailto 
postLink: mailto
---
{% assign relatedPosts=site.documents | where:"postLink", page.postLink | sort: 'language' %}

<div class="language">
|
    {% for p in relatedPosts %}
      <a class="{{ p.language }}" href="{{ site.base-url }}{{ p.url }}">{{ p.language }}</a> |
    {% endfor %}
</div><br/>
<hr>
<br/>

**tl;dr**

Sometimes generating email content from a form is preferable to saving all the data in a DB on your server.

## Shiny new Blazor

Last year I was asked to build a new request form for a local non profit association. They own BBQ site, with a hut that they rent out to the public and the requests for it did not follow any template. Therefore most of the time important information to handle the request in a timely manner was missing, resulting in a back and forth of email and sms messages, to gather the missing info.

As a C# developer I had been looking for a small project to check out Blazor and here I saw a chance to not only try the "new" framework, but also develop something useful with it.

Once I started I needed about four or five evenings for getting a working solution, that allowed for

- selecting dates from datepicker
- disable dates based on the data in a text file on the server
- sending emails to the requester and the people handling the renting process

It worked on my machine and I even got it deployed to Azure without too much of a hassle.
To my surprise even updating the text file worked on the deployd version with me having to created obscure Azure data volumes or some such.

After some styling withou bootstrap it even looked like something you would like to use when trying to rent a local BBQ place.

## Blazor dead end

And then the daily life kicked in and the solution set there waiting to go live. For a month, two months, three... . When I got back to it I finally asked myself how to get this into a production ready state.

And due to [European GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation) considerations simply putting in into the Azure cloud no longer was an option. Also taking into account that having Azure use your domain will cost you each month, kind of put a final nail into the Blazor solution's coffin.

But what else could work?

## "Low" tech solution

Since Blazor was ut of the race I started thinking about the most "low" tech solution that would be up to the challenge.

At this point I looked at the limitations I was faced with:

1. Improve the quality of data that is contained in the request on the first contact
2. The server only provides PHP scripting support (and I really didn't want to go down _that_ particular road)
3. Don't store personal data on an internet facing machine 

The next solution that came to my mind was using JavaScript to modify a `mailto` link and down that road I went.

This solution I hoped would work within the above mentioned boundaries _and_ would have the additional advantage that the user had the final say on which data they would send.

## HTML and JS FTW

Off I went implementing a form with HTML, JS, a bit of JQuery UI and PureCSS.

For the functionality I got away with two evenings of hacking away on the existing page. I then spent another three to do some re-styling of the page with PureCSS, making the whole page more responsive.

The page is now live and I'm waiting to get some feedback on the process. The board members were already happy with the first shot, but some real life testing will show if the solution is good enough and where some tweaking is still to be done.

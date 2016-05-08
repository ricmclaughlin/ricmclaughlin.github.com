---
layout: post
title: "Moscow Prioritization Isn't from Russia"
description: ""
category: posts
tags: [specialsauce]
---

{% include JB/setup %}
First off, MoSCoW prioritization isn’t from Russia. And it’s not communist either. It’s an agile project management technique that is part of the DSDM agile project management methodology.

And one of those part acronym, part word, words. MoSCoW is short for “<u>M</u>ust have, <u>S</u>hould have, <u>C</u>ould have, <u>W</u>on’t have”

Not only is MoSCoW a funny acronym, its best way to figure out which features should be included in a project. Its true value is found when combined with project time boxing. This makes sense because project time boxing and MoSCoW prioritization are designed to work together by our friends at the DSDM Consortium.

The underlying ideas behind MoSCoW prioritization are worth talking about before we launch into an example of it’s use. That way it will make a lot more sense when you go to implement MoSCoW prioritization yourself. Some of the ideas behind MoSCoW prioritization are:
<ul>
<li>Less might be more – Have you heard the saying “80% of the solution comes from 20% of the functionality?” You and I both know this is true; there is a small number of features, often known as a minimum usable subset of features, that make a website work. The rest of the features are “nice to have.” MoSCoW embraces this idea by first figuring out what the minimum usable subset is and then classifying the rest of the features important but not critical.</li>
<li>Workarounds are ok – It’s often painful to defer a feature until a later release. But is it a good idea to put a project at risk of non-delivery to implement a feature that could be worked around outside of the website? Nope. When using MoSCoW prioritization, features that are not part of the minimum usable subset can be worked around if they don’t make it in the final result of the project.</li>
<li>Deliver value early - All project features are important. Everyone would agree they should be prioritized to deliver the greatest and most immediate business benefits early in the project. With this in mind, MoSCoW prioritization establishes the priority order in which to deliver project features instead of delivering what makes sense from a development perspective.</li>
<li>Cutting features isn’t much fun – Getting anyone who wanted a particular feature in a website project to cut it completely out of a project is a tough sell. However, prioritizing a feature against the other features in the project to figure, which must, should, or could get delivered by the project is a lot easier. MoSCoW embraces this idea by having multiple levels of priority for features in a project not just “in the project” or “out of the project”</li>
</ul>
These approaches make so much sense to me. What’s also nice is how easy it is to figure out. The most basic part of MoSCoW prioritization is the categorization system. Each feature can be categorized “Must have”, “Should have”, “Could have”, or “Won’t have”.

Anything labeled as "Must have" has to be delivered in the project time box for the solution to be of use. In fact, lots of folks think the “Must” in “Must have” is an acronym “<u>M</u>inimum <u>U</u>sable <u>S</u>ubse<u>T</u>” of features. “Minimum Usable Subset” of features is the minimum amount of functionality that will solve the business problem at hand. However, if all the “Must have” features are complete at the end of the project the project must be considered a success.

”Should have” feature are items are nearly as important as the “Must have” and should be included if it is at all possible. "Should have” items are often as important as “Must have” features but have workarounds allowing another way of satisfying the requirement.

”Could have” features are less critical than “should haves” and are "nice to have." But, a few “Could have” features in the project can substantially increase customer satisfaction.

"Won’t have” features are either not critical or low return features. “Won’t have” features won’t be delivered in the current project and are deferred to the next project.

DSDM advises keeping “Must have” features hours fewer than 60% of the total project hours. And “Should have” and “Could have” features should be about 20% each of total project features. In higher risk scenarios, like a really tight timeline or a deliverable with numerous risk issues, I would try to keep “Must have” features down to less than 50% of the time box.

One thing that DSDM does not advise is to let the total project feature hours (the number of hours for features prioritized Must, Should and Could) exceed the total hours in the time box.

But experience tells me that having more project feature hours prioritized than time box hours is a good idea. In fact, I find it’s ok to add up to 50% more project feature hours than time box hours.

Why allow a project to have more project feature hours than the time box hours? Well, by their nature your estimates for each feature should be conservative. And what if things go better than expected and the project can deliver MORE than originally expected? If this happens, it’s good to have some features already queued up and ready to implement.

So the next question how do you do it?
<h3>
How to Use MoSCoW Prioritization in an Agile Website Project</h3>
Let’s revisit our supertoybox.com e-commerce project we talked starting working through in the last article about time boxing.

So far, the supertoybox.com project has a large defined set of features we need to prioritize for the first site release on 11/5, just in time for the Christmas season. Here is our feature list:

<div class="separator" style="clear: both; text-align: center;">
<a href="/assets/themes/ricify/images/features.gif" imageanchor="1" style="margin-left:1em; margin-right:1em"><img border="0" height="538" width="193" src="/assets/themes/ricify/images/features.gif" /></a></div>

Using some simple calculations, our timeframe of about 13 weeks and 5 resources cranking about 170 hours a week gives up a project time box of 2210 hours. Given that, our mission is to prioritize the list of features and end up with about 2210 hours of work that will yield a good first release of the supertoybox.com website.

<strong>1. Prioritize “Must have” features</strong> - On the first pass through the list I like identify the “Must have” features. At a minimum our site has to have a basic product catalog, search, and a shopping cart with tax and shipping calculations. We will also need to do credit card transactions, have some way of getting orders out of the system and a page framework to hang the functionality on. That’s pretty much the bare minimum. So our first pass looks like:

<div class="separator" style="clear: both; text-align: center;">
<a href="/assets/themes/ricify/images/MoSCoW_prioritization_must.gif" imageanchor="1" style="margin-left:1em; margin-right:1em"><img alt="List of project features marked Must have, Won't have, should have and could have using MoSCoW Prioritization" border="0" height="538" width="237" src="/assets/themes/ricify/images/MoSCoW_prioritization_must.gif" /></a></div>

Using this initial prioritization, the “Must have” hours they are about 57% of total time box hours, which is about where we want to be.

<strong>2. Prioritize “Won’t have” features </strong>- Next, I like to see what I can easily defer out of the project to make it nearly the size of the time box. I try to look for large features than can be worked around and are not technically required to make the project work. For this project I would look to defer system integration and do manual order entry. We can also safely defer the reporting and statistics feature because we could likely get that information from our ERP system. Features like multiple address shipments, product category admin and the wish list can also safely be deferred. That puts us with a prioritization like:

<div class="separator" style="clear: both; text-align: center;">
<a href="/assets/themes/ricify/images/MoSCoW_prioritization_wont.gif" imageanchor="1" style="margin-left:1em; margin-right:1em"><img alt="List of project features marked Must have, Won't have, should have and could have using MoSCoW Prioritization" border="0" height="538" width="237" src="/assets/themes/ricify/images/MoSCoW_prioritization_wont.gif" /></a></div>

After deferring these features, our project is down to an estimated 2240 hours, which is just about 1% larger than our time box. That’s good.

<strong>3. Prioritize the “Should have” features</strong> - "Should have” features complete functionality that are categorized “Must have.” I’d look at browse product by age, the CVV2 and paypal and shipping confirmation emails as very important but not critical features to include in this first release. Now our prioritization looks like:

<div class="separator" style="clear: both; text-align: center;">
<a href="/assets/themes/ricify/images/MoSCoW_prioritization_should.gif" imageanchor="1" style="margin-left:1em; margin-right:1em"><img border="0" alt="List of project features marked Must have, Won't have, should have and could have using MoSCoW Prioritization" height="538" width="237" src="/assets/themes/ricify/images/MoSCoW_prioritization_should.gif" /></a></div>

<strong>4. Prioritize the “Could have” features</strong> - I like this step; all the rest of the features are “Could have” features by default. That puts us to a completed prioritization that looks like:

<div class="separator" style="clear: both; text-align: center;">
<a href="/assets/themes/ricify/images/MoSCoW.gif" imageanchor="1" style="margin-left:1em; margin-right:1em"><img alt="List of project features marked Must have, Won't have, should have and could have using MoSCoW Prioritization" border="0" height="538" width="237" src="/assets/themes/ricify/images/MoSCoW.gif" /></a></div>

We get pretty lucky on this project because the “Won’t have” features are pretty easy to defer and the “Must”, “Should” and “Could” features are not much bigger than the 2210 time box:

<div class="separator" style="clear: both; text-align: center;">
<a href="/assets/themes/ricify/images/MoSCoW_prioritization_metrics.gif" imageanchor="1" style="margin-left:1em; margin-right:1em"><img alt="MoSCoW Prioritization project metrics including must have, should have, could have and won't have feature hours metrics" border="0" height="118" width="333" src="/assets/themes/ricify/images/MoSCoW_prioritization_metrics.gif" /></a></div>

At first glance, we have enough time to deliver the 1260 hours of “Must have” features in our time box of 2210 which ends up at 57% of the total time box hours. That’s good. The total project feature hours for this release is only 101% of the time box hours.

But, how do we make sure that we can deliver ALL the “Must have” features and pack in as many of the “Should have” and “Could have” features as possible? Use this technique as part of a fantastic overall approach - 
<h1>
MoSCoW Prioritization Resources</h1>
<a href="https://www.dsdm.org/content/moscow-prioritisation"> DSDM</a> has a great outline of MoSCoW prioritization. 

Wikipedia has some good information on <a href="http://en.wikipedia.org/wiki/DSDM">DSDM</a>, <a href="https://en.wikipedia.org/wiki/Timeboxing">time boxing</a>, <a href="http://en.wikipedia.org/wiki/MoSCoW">MoSCoW prioritization</a> and <a href="http://en.wikipedia.org/wiki/Iterative_development">iterative development</a>.
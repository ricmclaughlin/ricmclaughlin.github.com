---
layout: post
title: "Get Great Website Usability Using Joint Application Design and Simple Usability Testing"
description: ""
category: posts
tags: [special-sauce, solution-delivery]
---
{% include JB/setup %}
All websites are supposed to be easy to use. They have to be. If your website isn’t, then your visitors will bail and find a website that is. But how in the world do you sure your website has ease of use?

My answer, which came after a good bit of trial and error, is to use Joint Application Design workshops for website design. Then follow that up with some simple usability testing.

But most websites groups don’t really do too much to improve the designs of their websites. Heck, most folks don’t even use usability testing on websites to see if they are easy to use or not. And I can see why. When I worked on ACT! at Symantec we did a lot of usability testing and I have to admit it didn’t work that well.

Our usability testing went something like this: We would work really hard to get the application features complete and get rid of the bugs that made the usability testing impossible. Then we would hunker down in our high dollar usability lab and pump 15 to 20 people through the testing process over about 2 weeks.  We would then pull some test ‘results’ together.

There were two big of problem with this approach. We didn’t have a great idea of what to test before hand so we ended up with a big glob of anecdotal comments about the app – not actionable “this is wrong” sort of feedback. And the feedback came so late in the project we couldn’t really act on the results - we had to wait and integrate that test results into the NEXT release.

To make usability testing really work for us we needed usability feedback before we even got the application complete so we had time to fix the problem. And the feedback needed to be formatted into a “fix this problem” sort of statements. It would have been good if the testing could be a little less intense and time consuming as well.

These experiences led me to a big dilemma when I saw the really bad user interfaces coming out of the first few website projects I managed.  It was clear that “Big Usability Testing” – big as in big test lab, big group of folks being testing, big glob of results at the end – just wasn’t going to work on websites either. Yet something had to be done to improve the usability of the websites we were making.

Without a lot of great ideas of how to improve usability on websites, I adapted a couple of application development ideas to website development: <a href="http://en.wikipedia.org/wiki/Joint_application_design ">Joint Application Design</a> (JAD) workshops to improve a website usability design and <a href="http://en.wikipedia.org/wiki/Beta_test#Beta">beta testing</a> to test the website design’s effectiveness.

I ran across the JAD technique when I was doing client server development for a bit right after I left Symantec. JAD is an “interactive systems design concept involving discussion groups in a workshop setting” normally used as a requirements gathering exercise using a cross section of technical and business participants.

I saw it really work well for requirements gathering and some light application design. So it seemed to make sense to apply the workshop setting and cross-functional participants to the entire user interface design process as well.

The JAD workshop applied to website design worked something like this:
<ul>
  <li>Create a list of the features that the website needs to do</li>
  <li>Get 5-8 folks with differing backgrounds and viewpoints into a conference room</li>
  <li>Conduct a workshop to create a wire frame website on a white board.</li>
  <li> Turn the white board wire frame into HTML with some text added for things you can’t explain in the wire frame.</li>
</ul>
I tried using the JAD technique for the website design of a pretty simple site with a consulting client. After reviewing the HTML wire frame from the results of our JAD workshop with client management, we did a follow-up JAD workshop to address the issues they brought up.

Once we had the final wire frame assembled, low and behold, we had created a much more usable website design than any one person could have done individually.

For usability testing, we adapted “Beta testing” like I had seen done at Symantec on ACT! to website development. It was really easy to implement beta testing – once the website was stable enough to use without crashing we posted a version of the website out for review by some external partners.

The good news is that we got some feedback from folks outside the project. But the feedback we got wasn’t focused. It was at the “tweak this and tweak that” level and not the in-depth feedback we needed. The results were still too late in the development process to act upon without holding up the website release.

After the first experiment of how to improve website usability, I found that the JAD workshops really, really improved website usability. But beta testing didn’t give us any better feedback than Big Usability Testing even though it was a lot easier to pull off.

Fast-forward several years. I found great results using UI JAD workshops to design websites in a variety of settings – consulting and for internal customers. But still no improvement on testing the UI that was developed during the process.

Then I got to reading this really great book, <a href="https://www.sensible.com/dmmt.html">Don’t Make Me Think</a> by Steve Krug. Krug describes a really stripped down version of Big Usability Testing that was specially tailored for websites. The process it described took almost all of the practices of Big Usability Testing, simplified them and moved testing earlier into the development process.

Krug doesn’t really name this stripped down website usability testing, so for lack of a better term, let’s call it “simple usability testing.” Here is a summary practices of Big Usability Testing, the thorny issues they cause and a set of simplified practice Krug recommends instead.
<table class="table table-bordered" summary="Here is a summary practices of Big Usability Testing, the thorny issues they cause and a set of simplified practice Krug recommends instead.">
<tbody>
<tr>
<th>Big Usability Testing</th>
<th>Big Usability Testing Problem</th>
<th>Simple Usability Testing Solution</th>
</tr>
<tr>
<th>Buy equipment and make a dedicated usability testing lab</th>
<th>$$$$ - a usability lab costs a lot of money</th>
<th>Find a conference room, bring your video camera from home</th>
</tr>
<tr>
<th>Hire a usability expert</th>
<th>$$$ - a usability staff costs buck few website can afford</th>
<th>You test, you analyze the results, you figure out the solution</th>
</tr>
<tr>
<th>Create a usability study</th>
<th>It takes a long time to make and digest</th>
<th>Make a list of problems found and prioritize them</th>
</tr>
<tr>
<th>Test functional software</th>
<th>Once software works it’s hard to change the UI</th>
<th>Test anything (wire frames, software, mockups) that will give the user an understanding of what you are doing</th>
</tr>
<tr>
<th>One round of usability testing</th>
<th>What if you didn’t fix the big problems you found in the first round of testing?</th>
<th>Do more than one round of testing</th>
</tr>
<tr>
<th>Test between 10 and 20 people</th>
<th>Testing a lot of folks takes a lot of time (and money)</th>
<th>Test between 3 and 8 people and be ok with that</th>
</tr>
</tbody></table>

This sort of process made so much sense that I had to try it out. Using a little project that I was working on, I did some simple usability testing after the design was done and guess what? Worked great. I had a couple of those usability testing magic moments where things I had never thought about came to light. It was brilliant.

So to boil that all down, I have had great results iteratively doing Joint application design to improve the initial UI design usability and then applying “simple usability testing” from Krug’s “Don’t make me think” book to assure that the design is good.  It’s not particularly difficult to do and the results are really good. Let’s walk through the steps of how to do these two processes together.
<h3>Steps to improve your website’s usability</h3>
<ol>
  <li>Assemble a set of features – Before you start designing anything you really need a list of features that the website is going to implement. Without a well-defined list of features your design team just isn’t going to know what to design.</li>
  <li>Pick a cross-functional design group – To get a really good design you need the right folks to get in there and make the design workshop happen. Ideally, you would have at least two business non-technical folks, a web producer/HTML person, a senior developer, and the project manager. You want a good cross section of viewpoints and interests in your design workshops. And the folks you select should be flexible and easy to work with.</li>
  <li>Set up the JAD workshop - Get a conference room with a lot of whiteboards, reserve it for the whole day and get cracking. If this is the first time through the process it may make sense to show up with a first draft wire frame to kick things off. Ideally the person that will be “approving” the UI design should be participating. If folks can’t stay for the whole time, don’t include them. Don’t allow cell phone/crackberries/laptops.</li>
  <li>Conduct the JAD workshop - Spend a couple of hours to an entire day creating or revising a wire frame on a white board. Hopefully you can moderate this yourself. All participants should have an equal say in the design. Look for consensus in the design but don’t force it. It’s not critical to get down to one design but it will be great if that happens. But do make sure that your website design guidelines will allow what you are designing… it makes no sense to make a design that can’t be implemented on your website.</li>
  <li> Develop a wire frame – Using the notes and white board drawing get your web producer to create a wire frame UI in HTML. In cases where it is not clear what should happen on a page then add some text descriptions or call outs. For instance, if there should be some AJAX functionality happening on a page don’t get bogged down in developing it now. Describe what the functionality should do in a note on the page someplace.</li>
  <li> Iterate – Do steps 3 to 5 again to improve your design at least once. At a minimum you should have at least 2 JAD workshops with the same set of people.</li>
  <li>Make a usability test script – Before you can start testing you need to make a script that will walk the user through each specific task you want to evaluate. You don’t have to start from scratch on this – Steve Krug’s website has a really good great starter script. http://www.sensible.com/Downloads/script.doc</li>
  <li>Find a testing location and equipment – All you need to test is a conference room or, heaven forbid, a quiet cube someplace. You will want to videotape each testing session so bring your video camera and tripod from home.</li>
  <li>Conduct the testing – Line up 3 – 8 people to test. Bring them in. Walk them through the script. Make sure and pay them $50-100 in cash as they leave. It’s great if they are in your exact website demographic target but it’s not as critical as you might think.</li>
  <li>Review results – While reviewing the testing make two lists: one for general issues that came up during the sessions and one for problems specific to the new things you are testing. Prioritize the issues to fix and possible solutions if they pop up. In an ideal world, the same design group you used during your JAD workshop would review the testing videos.</li>
  <li>Redesign as needed – Do another JAD workshop to address the issues brought up in usability testing. Look to solve the high priority problems specific to the new functionality you are developing and any of the general problems that can be fixed. It’s convenient but often challenging to review the results and redesign based on the results in the same session.</li>
  <li>Redevelop – Now get in there and rework the wire frame based on the JAD workshop results.</li>
  <li>Iterate – Do steps 7 to 12 again so that your team will do at least two rounds of usability test and redesign.</li>
  <li>Start development – Once you start usability testing, I would try to start involving more application folks in the design process or at least giving them access to the wire frames as they come out. The idea is to jump-start the lower level business logic and database design processes using the wire frame to stimulate the discussion.</li>
</ol>
I’d try to use 2 JAD workshops and 2 rounds of usability testing, like I have document in the process now, for large chunks of functionality or content. The entire process could take as little two weeks if folks can be dedicated or mostly dedicated and could take a month and a half if folks are not dedicated. It will probably take at least a month the first time through.

For smaller project cut the number of JAD workshops and usability testing iterations down from there as you see fit.
<h3>Summing up - Get Great Website Usability Using Joint Application Design and Simple Usability Testing</h3>
Using a Joint Application Design workshop for your website’s user interface and the using simply usability testing as outlined in Krug’s excellent book “Don’t make me think” is really going to improve your site’s usability. And without a huge change to the development process you have in place now. If you really want to get it all together then try combining this approach to website usability with time boxing, MoSCoW prioritization and iterative development. Give it a try and let me know how it works.
<h2>Joint Application Design Resources</h2>
Great summary of the problems JAD <a href="http://www.bee.net/bluebird/jaddoc.htm">looks to solve and how</a>.

Great summary of the <a href="https://en.wikipedia.org/wiki/Website_wireframe">wireframe process</a>.
<h2>Usability Testing Resources</h2>
The man himself, Steve Krug, has a great site on <a href="http://www.sensible.com/">website usability</a>.

The best resource on website usability has got to be <a href="https://www.nngroup.com/articles/">Jakob Nielson’s website</a>.
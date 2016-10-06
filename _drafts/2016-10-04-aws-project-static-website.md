---
layout: post
title: "AWS Project Static Website"
description: ""
category: projects
tags: [aws s3 route53]
---
{% include JB/setup %}


Business Problem: 
The client is a small website designer and maintenance company that has more than 75 static websites generated using Jekyll that they run using.

http://docs.aws.amazon.com/gettingstarted/latest/swh/getting-started-create-bucket.html

Deployment:

Overall, the rollout of a static site to s3 is a solved problem. I'd skip the route 53 automatic changing part of the solution right now - this adds little value and costs money. s3 will be quite cheap and from the looks of this is straightforward. Just requires some configuration...

Easymenu.io site deployment plan version 0
Initial Setup - we can do this on monday. - Assuming you have a travis CI account and an aws account. 
Create an S3 Bucket for customer website. http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html (https://qwiklabs.com/focuses/2355?search=93339 for a quick overview)
Configure S3 location a website -> http://docs.aws.amazon.com/AmazonS3/latest/dev/HowDoIWebsiteConfiguration.html
create CNAME record for the naked domain and www subdomain that points to the S3 bucket URL
add travis.yml file to jekyll project as outlined in the cloudcannon article
create s3_website.yml and set environment variables in travis - this allows the s3_website component to operate without private key values IN github. It is standard practice to store configuration info inside the target environment and this is what we would be doing.
After we figure it out the first time this should take like 15 minutes to setup for new customer. There is nothing to develop; no value to add.

Development work flow:

Development Workflow version 0 - This is kinda hacky; WILL work in the short term

1. in a vanilla jekyll site, place compiled JS and CSS the right place in your build tree - this right place is in the \assets folder. (you have to replace your compiled stuff everytime it change) 
2. Complete your changes and be ready to deploy. 
3. commit to github
4. magic happens and site is deployed from Travis to s3.

Dev workflow version 1 - 
assuming you are thinking of using the  https://github.com/huphtur/jekyll-foundation-zurb-template right now the gulp file in this project does jekyll and all the other optimizations then copies the stuff on TOP of the compiled jekyll stuff. The gulp process as it stands now looks like

1. Clean - deletes everything in the /dist subdir
2. runs jekyll and that converts the pages and stuff in the /dist folder
3. runs sass, js and other optimization tasks that copy stuff ON TOP OFF stuff in the /dist folder (in production mode it writes the .map files too)
4. If in dev mode it starts up browser sync

The problem is that when you check this code into CodeCannon the results of the sass, js and other process won't run so the site CSS/JS etc won't get compiled. In fact, there are some pretty significant hacks in the gulp file to make it work like the way it works now.

Ideally, the process would look like:
1. Clean
2. Optimize all the JS/CSS and place the results of these processes it in the /assets folder; ignore the location for the "source" of these files in jekyll build process  (in production mode it writes the .map files too)
3. Run jekyll build
4. If in dev mode it starts up browser sync

In other words, some changes to the gulp file are required to make it work with CloudCannon - essentially change the order from jekyll then run optimization processes to optimization processes then jekyll.

Please confirm you are thinking of using the https://github.com/huphtur/jekyll-foundation-zurb-template!

Talk soon,
rm
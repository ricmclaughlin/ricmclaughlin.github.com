---
layout: post
title: "AWS - Elastic Transcoder"
description: ""
category: posts
tags: [aws, elastic-trancoder, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

[Elastic Transcoder](https://aws.amazon.com/elastictranscoder/) is a media transcoder in the cloud. It converts media files from their original source format to other formats playable on smartphones, tablets and PCs. It has many preset for most media types. You pay for minutes you transcode and the resolution you transcode for.

Components of Elastic Transcoder include:

- Pipelines - a transcoding workflow process including source and destination bucket that runs the jobs; can be pause if things are going wrong; also outputs a thumbnail of the video

- Jobs - the work definition that runs in a workflow process including input keys and outputs key settings along with encryption settings as well.

- Presets - a predefined configuration of settings for jobs
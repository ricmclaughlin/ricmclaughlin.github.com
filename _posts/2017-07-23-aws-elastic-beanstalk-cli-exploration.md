---
layout: post
title: "AWS - Elastic Beanstalk CLI Exploration"
description: ""
category: projects
tags: [aws, elastic-beanstalk, aws-dev-ops-pro, aws-solutions-arch-pro, cli-exploration]
---
{% include JB/setup %}

This is a very simple exploration of [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) from the [EB CLI](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html). To get started first install the [AWS CLI] (http://docs.aws.amazon.com/cli/latest/userguide/installing.html), then use `aws configure` to [configure it](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) for your account and region. Then install the [EB CLI](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html).

There are numerous commands with in the EB CLI, and this walkthrough isn't going to cover all of them. Here are the commands, we won't be covering: 

* `eb abort` - cancel env configuration changes

* `eb appversion` - manage the application versions lifecycle

* `eb clone` - clone the env

* `eb codesource` - enable/disable integration with AWS CodeCommit

* `eb labs` - support for future functionality testing from CLI

* `eb platform` - manages platforms and environments

* `eb restore` - fire up terminated environment

* `eb scale` - adjust scaling for an environment

* `eb setenv` & `eb printenv`  - prints environment vars

* `eb swap` - does a swap URL for a blue/green sort of dealie

* `eb upgrade` - upgrade the environment

Now we have the less-than-super-smart options... seriously, who uses a CLI and needs these commands?

* `eb console` - open browser to eb console..

* `eb ssh` - ssh into a running instance... 


# Preconfigured Container 

During this scenario we are going to create a Docker hosted application using a preconfigured container and deploy it in us-west-2. You will need to [install Docker](https://docs.docker.com/engine/installation/) and have the daemon running.

## Application Setup

Create a super simple app by creating a directory then an app file, a requirements file then a docker file

```bash
$ mkdir flask-app
$ cd flask-app
$ touch application.py
$ touch requirements.txt
$ touch Dockerfile
```

Add this text to the application.py file:

```python
# application.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'yo, docker! yo, docker!'

if __name__ == '__main__':
    app.run()
```
then this single line to the requirements.txt file:

```text
flask
```

then this line to the Dockerfile

```text
FROM amazon/aws-eb-python:3.4.2-onbuild-3.5.1
```

then build the container and run it...

```bash
$ docker build -t my-app-image .
$ docker run -it --rm -p 3000:8080 my-app-image
```

In your favorite browser access the running application at `http://localhost:3000/` and confirm it is running. Assuming it is let's package the application for use with Elastic Beanstalk using the eb CLI tool.

```bash
$ mv Dockerfile Dockerfile.local
$ zip ../myapp.zip -r * .[^.]*
```

## eb Command Line Tool

To setup an Elastic Bean Stalk environment from the command line, initialize the eb CLI tool, picking us-west2 as the region, create a new application, answer yes you are using python and select the "Python 3.4 (Preconfigured - Docker)", no SSH.

```bash
$ cd flask-app
$ eb init
```

Annoyingly, there is no way to confirm application creation worked from the CLI... assume it did. Or look in the Console.

Now to create the environment. By default, the `eb create <my-environment-name>` command makes a load balanced with t2.micro instances. That is close to what we want... but let's customize it with a single instance and confirm we are using a t2.micro to avoid charges as much as possible:

```bash
eb create flask-app-env -r us-west-2 --single --instance_type t2.micro
```

During the `eb create` process, press ctrl-c and then use the `eb events` command to tail the events stream then `eb health` to confirm the good health of the environment. I really like using `eb status` for a high level overview of an application so try that out as well. To complete the fun, do `eb open` to open the live application in browser window.

Now, let's update the application by changing the application.py file text like:

```python
# application.py
---snip---
def hello_world():
    return 'yo, baby, yo baby, yo. Docker, yeah!'
---snip---
```

then testing it by starting the application local:

```bash
eb run local
```
and accessing the locally running application by typing `localhost:3000` into your browser...

Super chill, that works. Let's deploy the application again by doing:

```bash
eb deploy 
```

Done with this part of the scenario, let's terminate the environment.

```bash
eb terminate flask-app-env
```

then delete the application... wait, there is NO way to delete an app using the eb CLI. Instead use the AWS CLI:

```bash
aws elasticbeanstalk delete-application --application-name flask-app
```

---
title: Static Site Post Scheduler
description: Open source static site scheduler tool
date: 2017-03-11
thumbnail: https://cloud.githubusercontent.com/assets/4726921/23232988/fdabd3fa-f955-11e6-84bd-c8a939841360.png
layout: Post
authors:
 - DavidWells
---

<p align="center">
  <img width="415" height="204" src="https://cloud.githubusercontent.com/assets/532272/23386639/779ce26c-fd0c-11e6-9e54-f33281e17719.jpg">
</p>

Like many static site's we use markdown + github for all of our [blog content](https://github.com/serverless/blog/).

Having content under version control comes with some great benefits:

- Anyone can submit content, fix typos & update anything via pull requests
- Version control - Roll back & see the history of any given post
- No CMS lock in - We can easily port to any static site generator
- It's just simple - No user accounts to manage, no CMS software to upgrade, no plugins to install.

All that said, there are some missing features when it comes to running your blog via a static site generator.

The biggest missing feature is the ability to schedule posts to publish at a specific time.

### **Not anymore baby!**

Until now, publishing a post was a manual process of merging a post branch into the `master` branch of our [blog repo](https://github.com/serverless/blog/).

While not the end of the world, it was an inconvenience for our content team needing to be awake super early to manually click a button.

So I thought to myself... There has got to be a better way... a better **serverless** way.

## Introducing the Post Scheduler for Static Websites

The post scheduler is a serverless project enables people running their sites on any static website generator the ability to schedule posts.

For.... **free!** That's right, under the generous free tier of AWS you can deploy this project for your site and run well under the free tier limits.

## How does it work?

1. A github webhook fires when a pull request (aka new posts or site content) is updated.

2. If the pull request comment has a comment matching `schedule(MM/DD/YYYY H:MM pm)` & the person is a collaborator on the project, the post/content gets scheduled for you.

3. A serverless cron job runs every hour to check if a post is ready to be published.

4. When the post is ready to be published, the cron function automatically merges the branch into `master` and your site, if you have CI/CD built in, will redeploy itself.

VIDEO HERE

### Github Webhook Architecture

![cloudcraft - post scheduler webhook](https://cloud.githubusercontent.com/assets/532272/23387076/2e7960b2-fd0f-11e6-88da-49517b27d8ae.png)

### Cron Job Architecture

![cloudcraft - post scheduler cron setup](https://cloud.githubusercontent.com/assets/532272/23388042/e129772e-fd14-11e6-96ca-ff23a019a51e.png)

## Install Instructions

1. Clone down the repository and run `npm install` to instal the dependencies

2. Duplicate `config.prod.example.json` into a new file called `config.prod.json` and insert your Github username, API token, and webhook secret

  ```json
  {
    "serviceName": "blog-scheduler",
    "region": "us-west-2",
     "TIMEZONE": "America/Los_Angeles",
    "GITHUB_REPO": "serverless/blog",
    "GITHUB_WEBHOOK_SECRET": "YOUR_GITHUB_WEBHOOK_SECRET_HERE",
    "GITHUB_API_TOKEN": "YOUR_GITHUB_API_TOKEN_HERE",
    "GITHUB_USERNAME": "YOUR_GITHUB_USERNAME_HERE"
  }
  ```

3. Deploy the service with `serverless deploy`. If you need to setup serverless, please see [these install instructions](https://github.com/serverless/serverless#quick-start).

4. Take the POST endpoint returned from deploy and plug it into your [repositories settings in github](https://youtu.be/b_DVXgiByec?t=1m9s)

  ![image](https://cloud.githubusercontent.com/assets/532272/23144203/e0dada50-f77a-11e6-8da3-7bdbcaf8f2a0.png)

  1. Add your github webhook listener URL into the `Payload URL` and choose type `application/json`

  2. Plugin your `GITHUB_WEBHOOK_SECRET` defined in your config file

  3. Select which github events will trigger your webhook

  4. Select Issue comments, these will be where you insert `schedule(MM/DD/YYYY H:MM pm)` comments in a given PR

5. Submit a PR and give it a go!

## Automate all the things!

**Before:**

We needed someone to manually merge a post into the `master` branch of our site. **Boo 🙈***

**After:**

We are sipping margaritas on the beach while posts are being published automatically. **Yay 🎉***
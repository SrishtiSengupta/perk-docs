---
title: Deploying Perk Apps to Heroku
description: Instructions for deploying perk apps to Heroku.
date: 2016-03-04
layout: topic.html
order: 400
---

[Heroku](https://www.heroku.com) is a cloud platform that lets you build, deliver, monitor and scale apps. Perk integrates easily with Heroku so that you can deploy your apps quickly without worrying about managing your own servers (although you can do that too if you want to).

## Deploy Automatically
[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/alarner/perk)

## Deploy Manually

> Note: this guide assumes that you have git installed.

### Register for a Heroku account

1. Follow this link to [sign up for Heroku](https://signup.heroku.com)
	* Choose **Node.js** as your primary development language.
1. Confirm your email by clicking on the link they send to you
1. Verify your account with a credit card
	* You will not be billed unless you install paid addons. All of the addons in this guide have free plans, but you still need to [Add your credit card info](https://heroku.com/verify) to Heroku

### Install the command line tools

Heroku has a really developer friendly command line tool for deploying your sites. It's basically as simple as `git push heroku master` once you get it all set up. To install the [command line tools](https://toolbelt.heroku.com/), follow the instructions on the page below:

[Heroku Toolbelt](https://toolbelt.heroku.com/)

Next, open up the command line and run `heroku login` to connect with your newly created Heroku account. You will need to type in your Heroku credentials.

### Deploy your Perk app the first time

Now you're all ready to deploy your app. Follow the steps below to go live!

On the command line make sure you are in the root directory of the app you want to deploy. Run the following commands from inside the root directory of your Perk project:

1. If you haven't already set up a git repository for your project...
	* `git init`
	* `git add .`
	* `git commit -m "Initial commit"`
1. `heroku create`
	* This will spin up a new Heroku server.
1. `heroku addons:create heroku-redis:hobby-dev`
	* This will spin up a free Redis server to store your user sessions.
1. `heroku addons:create heroku-postgresql:hobby-dev`
	* This will spin up a free PostgreSQL database to store your data.
1. `heroku config:set SESSION_SECRET=your_secret_goes_here`
	* Specifies a secret session code for encrypting your user sessions.
1. `heroku config:set NODE_ENV=heroku`
	* Instructs Perk to use heroku config file.
1. `git push heroku master`
	* Pushes your code to Heroku and deploy your app.
	* Runs your production build process for compiling scripts and styles.
	* Runs any database migrations that you've added.
1. `heroku open`
	* Opens the deployed app in your browser.
	* If you get an error that says `howhap-middleware requires an express session.` just wait a few minutes. Sometimes Heroku takes a while to boot up the redis server.

### Re-deploying

Any time you make changes to your app, you can simply add and commit those changes. Lastly, follow these steps to deploy:

1. `npm version patch`
	* This will update the version of your app (inside of your package.json). If you've made changes to front-end code (scripts or styles) this will update their version so that any previously cached files on your users devices will be re-downloaded.
1. `git push heroku master`
	* Pushes your code to Heroku and deploy your app.
	* Runs your production build process for compiling scripts and styles.
	* Runs any database migrations that you've added.

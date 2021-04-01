---
layout: page
title: Slack Bot — Short Assignment
published: true
comment_term: sa-slack-bot
---

![](http://i.giphy.com/5xtDarzqYMWFigufLws.gif)

Are you ready to invent the next AI?  Any self-respecting bot needs to be able to communicate via Slack. The bot will be able to do things like respond to messages and message users as they join your Slack team. It will be a simple [Node.js](https://nodejs.org/en/) and [Express.js](http://expressjs.com/) app and run on [Heroku](https://www.heroku.com/).

![](http://i.giphy.com/iUaHtkDPYX3Mc.gif){: .fancy .small}


## Slack Bot Basics
In Slack, bot users are similar to human users in that they have names, profile pictures, exist as part of the team, can post messages, are invited to channels, and more. The main difference is that bots are controlled programmatically via an API that Slack provides. We'll be building a custom bot that listens to certain events, like a message or a new member joining your team, and responds accordingly.


## Some Setup
Begin by cloning the starter code from [express+babel+eslint](https://github.com/dartmouth-cs52-18s/express-babel-starter).

1. **Slack**

    🚀 From the Slack desktop app, click on the team name in the top-left and then go to "Apps & Integrations." Search for "bot" and click the top result, "Bots." Click "Add Configuration". Choose a name for your bot and fill in the details for the bot. Add the Slack API token to the env file just as we did with previous assignments.

    ![](img/slack_bot_add.png){: .fancy}

	**Hint:**
    ```javascript
    import dotenv from 'dotenv';
    dotenv.config({ silent: true });

    // and then the secret is usable this way:
    process.env.SLACK_BOT_TOKEN
    ```

    Note: Don't forget to set this environment variable on Heroku in the settings tab

1. **Express**

    🚀Open `server.js`. Just like the previous assignments this is our main file for running the project.

1. **Run Dev Mode**

	```bash
	$ yarn dev
    ```

## Your First Bot Words

Ok so now you have a little server running, but how does it talk to Slack?

1. 🚀Let's add a library to do just that.

    ```bash
    yarn add botkit@0.7.4
    ```

    We are going to use [botkit](https://github.com/howdyai/botkit), which is a library that helps create conversational bots.  

1. Import the library.

    🚀in `server.js` add:

    ```js
    import botkit from 'botkit';
    ```

1. Setup Bot Controller

    🚀After we create the express app, let's set up botkit.

    ```js
    // botkit controller
    const controller = botkit.slackbot({
      debug: false,
    });

    // initialize slackbot
    const slackbot = controller.spawn({
      token: process.env.SLACK_BOT_TOKEN,
      // this grabs the slack token we exported earlier
    }).startRTM(err => {
      // start the real time message client
      if (err) { throw new Error(err); }
    });

    // prepare webhook
    // for now we won't use this but feel free to look up slack webhooks
    controller.setupWebserver(process.env.PORT || 3001, (err, webserver) => {
      controller.createWebhookEndpoints(webserver, slackbot, () => {
        if (err) { throw new Error(err); }
      });
    });
    ```

    If you notice an error: `Error: not_authed` this is because you forgot to export/set the environment variable for the SLACK_BOT_TOKEN.

    If you have trouble setting up your environment you can use a .env file with the [dotenv node package](https://www.npmjs.com/package/dotenv) as we've done previously.

1. Let's Say Hi!

    ```js
    // example hello response
    controller.hears(['hello', 'hi', 'howdy'], ['direct_message', 'direct_mention', 'mention'], (bot, message) => {
      bot.reply(message, 'Hello there!');
    });
    ```

    Give it a shot, try direct messaging your bot in Slack!

    ![](img/jackjack_speaks.png)

1. What about names?

    Want the bot to respond to the user by name?

    Well we have full access to [Slack's web api](https://api.slack.com/methods/users.info). Let's look up the user's name.

    ```js
    bot.api.users.info({ user: message.user }, (err, res) => {
      if (res) {
        bot.reply(message, `Hello, ${res.user.name}!`);
      } else {
        bot.reply(message, 'Hello there!');
      }
    });
    ```

    You would do this inside of the callback to controller.hears.


## Now for the Real Stuff!

Ok now your Bot knows how to say hi.  But let's make it do some useful stuff!

The rest of this assignment is more hands-off. We'll provide some direction and resources but you'll be looking up API docs and adding more cool stuff to your bot.

<!-- So far we've been using Botkit.  [Botkit](https://github.com/howdyai/botkit) has support for more complex [conversations](https://github.com/howdyai/botkit/blob/master/examples/convo_bot.js). This might come in handy. -->


The Botkit library provides us with a convenient wrapper around Slack's API. Our bot connects to Slack's RTM API and opens a WebSocket connection with Slack. *If you set `debug=true` in the botkit initialization you can see how it polls the Slack servers.*

### Events
The Slack server issues **events** that are then consumed by clients. These are things like [messages](https://api.slack.com/events/message) and [team join](https://api.slack.com/events/team_join) events. Botkit can hook up to any of Slack's events.  `.hears` is a fancier way of listening to message events.   

<!-- 🚀Botkit [slack event integration](https://github.com/howdyai/botkit/blob/master/docs/slack-events-api.md). -->

For instance:

```js
controller.on('user_typing', (bot, message) => {
  bot.reply(message, 'stop typing!');
});
```

will make your bot a jerk! 😡

![](img/jackjack_is_a_jerk.png)

The format of the `message` object is defined on the Slack documentation for [message events](https://api.slack.com/events/message).

## Make Your Bot Do More
At this point you've achieved a general understanding of what goes into making a Slack bot and have implemented some functionalities. Now, go and see what else you can do with your Slack bot. Brainstorm, read documentation, and experiment. Make your bot the best that it can be!

### Wait But Food

Except first, lets make your bot actually helpful.  I am hungry, and I want your bot to suggest places to eat.

🚀Add in [Yelp API for node](https://github.com/tonybadguy/yelp-fusion) with
```bash
$ yarn add yelp-fusion
```

🚀You'll need to [create and app and generate API keys](https://www.yelp.com/developers/v3/manage_app), similar to what you had to do for Slack.

Once You have your keys add them to your .env file
```bash
export YELP_API_KEY="YOUR_API_KEY"
export YELP_CLIENT_ID="YOUR_CLIENT_ID"
```

You can grab the yelp client like this
```javascript
const yelpClient = yelp.client(process.env.YELP_API_KEY);
```

Our client gives us a convenient search api. We can use the following to make a
query

```javascript
  yelpClient.search({
    term:'Sushi',
    location: 'hanover, nh'
  }).then(response => {
    console.log(response.jsonBody.businesses[0].name);
  }).catch(e => {
    console.log(e);
  });
```

Tip: Yelp results come back looking something like:

```js
[{ id: 'GXX6xbCK1EBH9X6zJf3wVA',
    alias: 'sushiya-hanover-2',
    name: 'Sushiya',
    image_url:
     'https://s3-media2.fl.yelpcdn.com/bphoto/IeGQHPRAhXUlU9p_2omi-w/o.jpg',
    is_closed: false,
    url:
     'https://www.yelp.com/biz/sushiya-hanover-2?adjust_creative=AbS0KeC0gXt5NvEk3AEUeg&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=AbS0KeC0gXt5NvEk3AEUeg',
    review_count: 61,
    categories: [ [Object], [Object], [Object] ],
    rating: 3.5,
    coordinates: { latitude: 43.699705710707, longitude: -72.2894281825115 },
    transactions: [],
    price: '$$',
    location:
     { address1: '72 S Main St',
       address2: '',
       address3: '',
       city: 'Hanover',
       zip_code: '03755',
       country: 'US',
       state: 'NH',
       display_address: [Array] },
    phone: '+16036434000',
    display_phone: '(603) 643-4000',
    distance: 7525.2386648073125 },
},]
```

So you'll need to process those to filter out results you find useful.

Try something like:

```js
response.jsonBody.businesses.forEach(business => {
  // do something with business
});
```

instead of a c-style for loop.

Here's some sample output that your bot too can return. Just slack [jackjack](https://cs52-dartmouth.slack.com/messages/@jackjack/) and tell him you are `hungry`.

![sample output](img/sample_yelp.png)


## Data Stores

We're not covering the datastores here. So your bot will forget conversations it has had. Botkit does provide a [storage api](https://github.com/howdyai/botkit#storing-information).

If you want to try setting that up, its **extra credit** to have a convo that isn't only in memory.  Heroku has free Mongo addons so that might be a direction to take.

## Default/help responses

Don't forget to make your bot able to respond to any messaged directed at it with at least a "what are you even talking about" — and it should also tell people what it is capable of doing for them in response to @yourbotname help


## Deploy On Heroku

Ok the last step is to deploy your bot to Heroku!

1. 🚀Head over to [Heroku](https://www.heroku.com/) and login/sign up. Then, make a new app. Head over to "Settings" and add a Config Variable `SLACK_BOT_TOKEN` with value set to the API token of the Slack bot you made in step 1. You probably also need to add all your YELP keys, and any other API's you used.

1. Follow the steps under "Deploy Using Heroku Git".

You may have noticed a file named `Procfile` in the project. This tells Heroku what commands to run in its [dynos](https://devcenter.heroku.com/articles/dynos). Our `Procfile` is just one line, `web: yarn prod`, where `web` defines the dyno type (this one receives HTTP traffic), `yarn prod` is the command defined in `package.js` that we want to run.


## Outgoing Webhook

Heroku dynos will typically sleep after some period of time. Your bot will not wake up automatically based on the slack RTM because that is a queue that it is your bots responsibility to check.

There are ways around this, but a workaround for now would be to add an an [Outgoing Webhook](https://api.slack.com/outgoing-webhooks) that would wake up your bot.  Invite your bot to the **#bots** channel on slack and have the bot wake up on an outgoing webhook that mentions their name or another string.  Outgoing webhooks are configured as separate Slack integrations and require your bot to both have a public url (such as you would get when deploying on Heroku), and also have specific strings that trigger the outgoing webhook.  I have mine wake up on "jackjack wake up!" and send back a giphy to prove it.

```javascript
controller.on('outgoing_webhook', (bot, message) => {
  bot.replyPublic(message, 'yeah yeah');
});
```

## To Turn In:

* github url to your bots repo (must be readable by staff, can be public)
* screen caps of some conversations that test your bot's functionality
* when we talk to your bot, it should be able to:
  * respond to hi and random messages
  * return results for a restaurant query
  * carry on at least one conversation
  * send back an [*attachment* message](https://github.com/howdyai/botkit#botreply) in response to something.
  * wake up on an outgoing webhook from #bots
* your heroku URL.  This is so we can wake up your bot if heroku sleeps your dyno.
* /invite your bot to the Slack #bots channel where we will have bot parties.



# Extra Credit

* So many options. Be creative!
* Maps?
* Driving directions?
* [MongoDB Botkit Storage](https://github.com/howdyai/botkit-storage-mongo) setup on Heroku.
* have your bot talk with another bot in #bot (requires collaboration but only on discussing how the bot conversation would go — this is still an individual assignment).  For instance, if I ask your bot for a recipe (in a DM), your bot could then use #bot to ask another bot for a suggested recipe and then return the results in the DM. This might be tricky to get right, document your "bot2bot chat api" and submit that in addition to screenshots showing the functionality.

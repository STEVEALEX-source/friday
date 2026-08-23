# Friday Slack Bot

Friday is a Slack bot that helps teams stay connected and track how everyone's feeling. It's simple to set up and easy to use.

## What does it do?

The bot lets your team check in with mood updates. You can ask the team how they're doing and get responses right in Slack. It also responds to mentions and has slash commands to make things quick and easy.

## What you need

You'll need Node.js version 20 or higher and npm version 9.6.4 or higher. You also need a Slack workspace where you can create an app.

## Setting it up

Start by going to https://api.slack.com/apps and create a new app. Call it Friday and pick your workspace.

Once you create the app, go to Basic Information and copy your Signing Secret. You'll need this later.

Next go to OAuth and Permissions. Add these scopes: app_mentions:read, chat:write, commands, incoming-webhook, and channels:read. Then copy your Bot User OAuth Token. It starts with xoxb-.

Now set up Event Subscriptions. Turn events on and set your Request URL to https://your-domain.com/slack/events. For testing locally, use Ngrok. Run ngrok http 3000 in another terminal and use that URL instead. Slack will verify it works. Then subscribe to app_mention and message.channels events.

Create two slash commands. The first one is /friday with the same request URL. The second is /friday-mood also with the same URL.

Now you're ready to set up the code. Copy .env.example to .env and add your tokens:

SLACK_BOT_TOKEN equals your xoxb- token
SLACK_SIGNING_SECRET equals your signing secret
API_KEY equals any secret key you want (run openssl rand -hex 32 to generate one)

Install the dependencies with npm install and start the bot with npm start. You should see messages saying the bot is running on port 3000.

## Using the bot

In Slack you can type /friday to see the help menu. Type /friday-mood to check in on how you're feeling. You can also mention the bot with @Friday help and it will respond.

## The code

The bot is made up of a few main files. The app.js file handles all the Slack interactions. The server.js file starts the Express server. The api.js file lets you control the bot from outside Slack using HTTP requests. The responses.js file has the message templates used throughout the bot.

## Adding your own commands

To add a new slash command, go to src/app.js and add something like this:

```javascript
app.command("/your-command", async ({ ack, body, respond }) => {
  ack();
  
  await respond({
    text: "Your message here"
  });
});
```

You can also add event handlers for when people react to messages or post messages in channels.

## Custom API

The bot comes with HTTP endpoints you can call to control it. There's a /health endpoint to check if it's running. You can POST to /api/send-message to send a message to a channel. Use /api/broadcast to send to all channels. Use /api/schedule-message to send something later. There's /api/trigger-mood-check to start a mood check. You can GET /api/moods to see team mood stats.

Every API call needs an X-API-Key header with your API key.

## Deploying

For Heroku, create an app and set your environment variables, then push. For Railway, connect your GitHub repo and add the env vars in the dashboard. For self-hosted, install Node on your server and run the bot with pm2 to keep it running.

The most important thing is HTTPS in production. Slack requires it. Heroku and Railway give you HTTPS for free. If you host it yourself, use Let's Encrypt.

## Security

Your .gitignore file already protects your .env file so secrets won't be pushed to GitHub. Never hardcode your tokens in the code. Use environment variables instead. Change your API key sometimes and use HTTPS everywhere.

## License

This is MIT licensed. You can use it however you want.

## Questions

Check the API-GUIDE.md file for API details. Check QUICKSTART.md to get running fast. Check WEBHOOK-SETUP.md to understand how webhooks work.

That's it. You now have a working Slack bot you can control from anywhere.

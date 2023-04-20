# Project LION Discord Bot

Forked the OpenAI Discord bot and upgraded the model to gpt-3.5-turbo, added questions over documents feature, experimenting on onboarding project recommender feature. Also uses the [moderations API](https://beta.openai.com/docs/api-reference/moderations) to filter the messages. 

# Features
### Chat
- `/chat` starts a public thread, with a `message` argument which is the first user message passed to the bot
- The model will generate a reply for every user message in any threads started with `/chat`
- The entire thread will be passed to the model for each request, so the model will remember previous messages in the thread
- when the context limit is reached, or a max message count is reached in the thread, bot will close the thread
- you can customize the bot instructions by modifying `config.yaml`
- you can change the model, the hardcoded value is `gpt-3.5-turbo`

### Q&A
- `/ask` initializes a document search Q&A query
- The model will search over documents in the text/ folder
- The users question will be displayed in the models response
- You can add any .txt documents to the text/ folder for the model to use them in its search

### Onboaording project recommender [experimental]
- Leo recommends projects to new users based of their introduction message and your DAOs documents

# Setup

1. Copy `.env.example` to `botenv.env` and start filling in the values as detailed below
1. Go to https://beta.openai.com/account/api-keys, create a new API key, and fill in `OPENAI_API_KEY`
1. Create your own Discord application at https://discord.com/developers/applications
1. Go to the Bot tab and click "Add Bot"
    - Click "Reset Token" and fill in `DISCORD_BOT_TOKEN`
    - Disable "Public Bot" unless you want your bot to be visible to everyone
    - Enable "Message Content Intent" under "Privileged Gateway Intents"
1. Go to the OAuth2 tab, copy your "Client ID", and fill in `DISCORD_CLIENT_ID`
1. Copy the ID the server you want to allow your bot to be used in by right clicking the server icon and clicking "Copy ID". Fill in `ALLOWED_SERVER_IDS`. If you want to allow multiple servers, separate the IDs by "," like `server_id_1,server_id_2`
1. Copy the target channel ID for your introductions channel and fill it in `TARGET_CHANNEL_ID` for the onboarding bot to work [currently only executes once at runtime]

1. Add your documents as .txt files to the text/ folder.

1. Install dependencies and run the bot
    ```
    pip install -r requirements.txt
    python -m src.main
    ```
    You should see an invite URL in the console. Copy and paste it into your browser to add the bot to your server.
    Note: make sure you are using Python 3.9+ (check with python --version)


# Optional configuration

1. If you want moderation messages, create and copy the channel id for each server that you want the moderation messages to send to in `SERVER_TO_MODERATION_CHANNEL`. This should be of the format: `server_id:channel_id,server_id_2:channel_id_2`
1. If you want to change the personality of the bot, go to `src/config.yaml` and edit the instructions
1. If you want to change the moderation settings for which messages get flagged or blocked, edit the values in `src/constants.py`. A lower value means less chance of it triggering.

# FAQ

> Why isn't my bot responding to commands?

Ensure that the channels your bots have access to allow the bot to have these permissions.
- Send Messages
- Send Messages in Threads
- Create Public Threads
- Manage Messages (only for moderation to delete blocked messages)
- Manage Threads
- Read Message History
- Use Application Commands

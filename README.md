🤖 MCP Server & Client Demo
This project shows how to build a Model Context Protocol (MCP) server and client.
It integrates two things:

🌤️ Weather service (static data)

💬 Slack messaging (send + read messages)


📂 Project Structure
GenAI-MCP-Demo/
├── server-app/
│   ├── .env
│   ├── requirements.txt
│   ├── weather_service.py
│   ├── main.py
│   └── demo.py
├── client-app/
│   ├── requirements.txt
│   ├── app.py
│   └── mcp_client.py
└── .gitignore


🔹 Server App (server-app)
==============================
.env
Stores secrets:

SLACK_BOT_TOKEN → Slack Bot OAuth Token
SLACK_CHANNEL_ID → Channel ID where bot will post messages

requirements.txt
Dependencies:

fastmcp → MCP framework
python-dotenv → load .env variables
slack-sdk → Slack API client

weather_service.py
Static dictionary with weather info for 3 cities: Hyderabad, Delhi, Mumbai.

main.py
Starts MCP Server at http://localhost:8081/mcp.

Exposes 3 tools:

get_weather(city) → returns weather info
send_message(message) → posts message to Slack channel
read_messages(limit) → fetches recent Slack messages

demo.py
Simple test script:

Sends "Hello, Good Morning...!!" to Slack
Reads last 10 messages and prints them

🔹 Client App (client-app)
================================
requirements.txt
Dependencies:

fastmcp → MCP client
streamlit → UI framework

app.py
Streamlit UI with 3 tabs:

🌤️ Weather → enter city → get JSON weather data
💬 Send Slack → enter message → post to Slack
📖 Read Slack → enter number → fetch recent messages

mcp_client.py
Connects to MCP Server (http://localhost:8081/mcp).

Functions:

get_weather(city)
send_message(message)
read_slack_messages(limit)


🔹 How to Run
----------------
1. Setup
Clone repo and install dependencies:
bash
git clone https://github.com/<username>/GenAI-MCP-Demo.git
cd GenAI-MCP-Demo

2. Run MCP Server
Open terminal:
bash
cd server-app
pip install -r requirements.txt
python main.py
Server starts at: http://localhost:8081/mcp  
You will see logs like:
Code
INFO: Uvicorn running on http://localhost:8081 (Press CTRL+C to quit)

3. Run MCP Client
Open another terminal:
bash
cd client-app
pip install -r requirements.txt
streamlit run app.py
Client UI opens at: http://localhost:8501


🔹 Demo Flow (with screenshots)
-----------------------------------
Weather Tab

Enter Hyderabad → Output:  refer get_weather.png

json
{
  "temperature": "32 C",
  "condition": "Sunny"
}

Send Slack Tab -> refer write_slack1.png and write_slack2.png
Enter message: "I am sagar deshmukh, hope you are doing well."
Bot posts this message in Slack channel.

Read Slack Tab -> refer read_slack.png
Enter 10 → Output shows last 10 messages from Slack channel.

✅ Summary
-----------
Server app → exposes MCP tools (weather + Slack).
Client app → Streamlit UI to call those tools.

Run order:

Start server (python main.py)
Start client (streamlit run app.py)
Use UI tabs to test weather + Slack integration.


🔹 Slack Setup Guide
======================
1. Create Slack App & Bot Token
Go to Slack API Apps.
Click Create New App → choose From scratch.
Give app a name (example: AI Assistant) and select your workspace.

In OAuth & Permissions, add scopes:
chat:write (send messages)
channels:read (read channel info)
channels:join (join channels)

Click Install App to Workspace.
Copy the Bot User OAuth Token (xoxb-...) → put it in .env as SLACK_BOT_TOKEN.

2. Create Slack Channel
In Slack workspace, click + Add channel → Create a new channel.
Give it a name (example: #ai-channel).
Channel created → open it.

3. Get Channel ID
Open the channel in browser.
URL looks like:
https://app.slack.com/client/Txxxxxx/Cyyyyyy
The part starting with C (example: C0C6KG8TR6E) is the Channel ID.
Put this in .env as SLACK_CHANNEL_ID.

4. Add Bot to Channel
Open the channel (#ai-channel).
Click Add people → search your bot name (example: @AI Assistant).
Invite bot to channel.
Slackbot will confirm: “AI Assistant has been added to the channel.”
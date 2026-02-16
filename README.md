Telegram Restaurant Chatbot – n8n
Implementation Document
1. Project Overview
This document explains the full implementation of a Restaurant Customer Support Chatbot built using n8n
workflows. The chatbot handles both text and voice messages from Telegram users, converts voice to text,
and generates intelligent responses using an AI Agent.
The goal of this system is to reduce restaurant staff workload by automatically answering common
customer questions such as menu details, reservations, timings, and order status.
2. Architecture Overview
Main Components
• 
• 
• 
• 
• 
Telegram Bot (User Interface)
n8n Workflow Automation
OpenAI Chat Model (AI Responses)
Speech‑to‑Text (Voice Processing)
AI Agent Logic Layer
Message Flow
1. 
2. 
3. 
4. 
5. 
6. 
User sends message in Telegram (text or voice).
Telegram Trigger receives update.
IF node detects message type.
Text → AI Agent directly.
Voice → File Download → Transcription → AI Agent.
AI response sent back to Telegram.
3. Telegram Bot Setup
Step 1 – Create Bot
1. 
2. 
3. 
4. 
Open Telegram.
Search for BotFather.
Run command 
/start .
Create new bot using 
/newbot .
1
Copy the generated Bot Token.
5. 
Step 2 – Connect Bot in n8n
1. 
2. 
3. 
4. 
Open Telegram Trigger node.
Create new Telegram credentials.
Paste Bot Token.
Test connection.
4. n8n Workflow Structure
Core Nodes Used
• 
• 
• 
• 
• 
• 
Telegram Trigger
IF Node (Message Type Detection)
Telegram → Get File
OpenAI → Translate Recording
AI Agent (Text Processing)
Telegram → Send Message
5. IF Node Logic (Message Type Detection)
Voice Condition
{{$json.message.voice}} exists
If TRUE → Voice Workflow If FALSE → Text Workflow
This ensures the workflow handles both text and voice messages without conflicts.
6. Text Message Flow
Telegram Trigger → IF (FALSE branch) → AI Agent → Send Text Message
AI Agent Configuration
Prompt Source: 
2
{{$json.message.text}}
System Behaviour: - Friendly restaurant assistant - Short clear replies - Use emojis - Provide menu help,
booking help, FAQs
7. Voice Message Flow
Telegram Trigger → IF (TRUE branch) → Get File → Translate Recording → AI Agent → Send Text Message
Step A – Get File Node
File ID Expression: 
{{$json.message.voice.file_id}}
Download option must be enabled.
Step B – Translate Recording Node
Resource: Audio Operation: Translate a Recording Input Data Field Name: 
data
This converts voice audio into text.
Step C – AI Agent Input
Prompt: 
{{$json.text}}
(This uses the transcription output.)
8. Telegram Send Message Node
Chat ID: 
3
{{$node["Telegram Trigger"].json.message.chat.id}}
Text: 
{{$json.output}}
This sends the AI response back to the user.
9. Fallback Handling (Important for Assessment)
System Prompt Suggestion:
"If the answer is not found in the restaurant knowledge base, respond politely and suggest contacting
human support."
Example: "Sorry, I couldn't find that information right now. Please contact our staff for help 🙂"
10. Professional Features Implemented
• 
• 
• 
• 
• 
• 
Typing‑style conversational responses
Text and voice support
Automatic transcription
Friendly AI replies
Structured branching logic
24/7 automated customer support
11. Testing Checklist
Test Text Message
Send: "Hi" Expected: - AI Agent responds - Telegram sends reply
Test Voice Message
Send voice note Expected: - File downloaded - Audio converted to text - AI generates reply - Telegram sends
message
4
12. Technical Summary (For Submission)
The chatbot is implemented using an event‑driven workflow architecture inside n8n. Telegram acts as the
real‑time trigger source, while conditional routing distinguishes between text and voice inputs. Voice
recordings are processed through an audio transcription step before being forwarded to an AI Agent
powered by a chat model. The AI Agent generates contextual responses which are then delivered back to
users through Telegram messaging nodes. This design ensures scalability, modularity, and efficient
handling of multi‑modal customer interactions.
13. End Goal
A fully automated Telegram Restaurant Assistant capable of: - Answering customer questions instantly 
Handling voice and text interactions - Reducing manual staff workload - Providing natural conversational
responses
5

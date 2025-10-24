<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <h1>Recall</h1>
  <p>AI-Powered Voice Receptionist for Small Businesses</p>
</div>

---

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li><a href="#features">Features</a></li>
    <li><a href="#system-architecture">System Architecture</a></li>
    <li><a href="#how-it-works">How It Works</a></li>
    <li><a href="#replication">Replication</a></li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

---

<!-- ABOUT THE PROJECT -->
## About The Project

Recall is an intelligent voice AI system designed to automate receptionist duties for small businesses (local barber shops, small legal offices, construction companies). It combines state-of-the-art voice recognition, natural language processing, and calendar integration to provide a professional, 24/7 virtual receptionist experience. The demo provided with this repository acts as the receptionist of a small legal office.

**Problem Solved:**
Small businesses often struggle to handle phone calls, schedule appointments, and send reminders because they lack the staff or time to manage these tasks. Without a dedicated receptionist, missed calls quickly become missed opportunities, which can lead to significant revenue loss.

Recall automates these interactions with accuracy and a professional tone. It answers calls, books appointments, and follows up with clients, allowing business owners to focus on running their operations instead of managing phones.



**Solution:**
Recall deploys two complementary AI agents:
- **Inbound Agent:** Answers incoming calls professionally, checks availability via Google Calendar, schedules appointments, and takes messages—all in real-time conversation.
- **Outbound Agent:** Proactively calls clients about missed appointments, detects voicemail, reschedules meetings, and confirms attendance.

Both agents integrate seamlessly with Google Calendar for scheduling and Supabase for call history tracking, creating a complete receptionist automation solution.

**Live Demo:**

Call: 929-717-3949 to book a meeting with the firm. Then navigate to the dashboard through the landing page site [here](https://recall-fe-xi.vercel.app/) to see what it looks like from the business stand point.

---

### Built With

**Backend:**
* [![Python][Python]][Python-url]
* [![LiveKit][LiveKit]][LiveKit-url]
* [![OpenAI][OpenAI]][OpenAI-url]
* [![Google Cloud][GoogleCloud]][GoogleCloud-url]
* [![Supabase][Supabase]][Supabase-url]

**Frontend:**
* [![Next.js][Next.js]][Next-url]
* [![React][React.js]][React-url]
* [![TypeScript][TypeScript]][TypeScript-url]
* [![Tailwind CSS][TailwindCSS]][TailwindCSS-url]

---

## Features

### Inbound Call Handling
- 🎤 **Professional Call Greeting:** AI-powered receptionist answers with customizable business greeting
- 📅 **Real-Time Availability Checking:** Integrates with Google Calendar to check open slots instantly
- 📝 **Appointment Scheduling:** Directly books appointments onto Google Calendar without manual entry
- 💬 **Message Taking:** Captures detailed messages for case status inquiries and callback requests
- 📱 **Caller ID Recognition:** Automatically extracts and logs caller phone numbers from SIP headers
- 🌙 **Business Hours Awareness:** Responds appropriately during and outside business hours
- 📊 **Call History Logging:** Records all call details to Supabase for compliance and analytics

### Outbound Reminder Calls
- 🔔 **Automated Reminders:** Proactively calls clients about upcoming appointments or missed meetings
- 📞 **Voicemail Detection:** Automatically hangs up when voicemail is detected, preventing unwanted messages
- 📅 **Appointment Rescheduling:** Offers alternative times if clients cannot attend originally scheduled appointments
- ✅ **Attendance Confirmation:** Records confirmations for follow-up and scheduling purposes
- 🤖 **Intelligent Conversation:** Handles objections and reschedule requests naturally via LLM
- 🔄 **Two-Way Communication:** Full conversational ability to discuss scheduling options
- 📋 **Detailed Call Logging:** Tracks all interactions, confirmations, and rescheduling outcomes

### Dashboard & Analytics
- 📊 **Call History Dashboard:** View all incoming and outgoing calls with details
- 📈 **Performance Metrics:** Track appointment bookings, completion rates, and client interactions
- 👥 **Client Information:** Store and retrieve client data integrated with call history
- 🔐 **Secure Storage:** All data encrypted and stored in Supabase PostgreSQL database

---

## System Architecture

### High-Level Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Phone Network (SIP)                       │
│              (LiveKit SIP Trunk / Twilio)                    │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
        ┌────────────────────────────────────────┐
        │      LiveKit Agent Infrastructure      │
        │  (Real-time Voice Processing & Rooms)  │
        └────────────────────────────────────────┘
                             │
        ┌────────────────────┴────────────────────┐
        ▼                                          ▼
    ┌────────────────┐              ┌──────────────────┐
    │ Inbound Agent  │              │ Outbound Agent   │
    │ (Receptionist) │              │ (Reminder Calls) │
    └────────────────┘              └──────────────────┘
        │                                │
        └────────────────┬───────────────┘
                         │
        ┌────────────────┴─────────────────┐
        ▼                                   ▼
    ┌──────────────────────┐      ┌────────────────┐
    │  Google Calendar     │      │  Supabase DB   │
    │  (Availability &     │      │  (Call History)│
    │   Scheduling)        │      └────────────────┘
    └──────────────────────┘
        │
        │ (Calendar Sync)
        ▼
    ┌────────────────────────┐
    │  Next.js Dashboard UI  │
    │  (React + TypeScript)  │
    └────────────────────────┘
```

### Voice Processing Pipeline

```
Phone Input
    │
    ├─→ LiveKit SIP Ingestion
    │
    ├─→ Speech-to-Text (Deepgram Nova-3)
    │
    ├─→ Voice Activity Detection (Silero VAD)
    │
    ├─→ Noise Cancellation (BVC Telephony)
    │
    ├─→ LLM Processing (OpenAI GPT-4o-mini)
    │   ├─ Tool Calls (Schedule, Check Availability, Take Message)
    │   └─ Response Generation
    │
    ├─→ Text-to-Speech (Cartesia Sonic-2)
    │
    └─→ Phone Output
```

### Component Details

**Backend (Python + LiveKit Agents Framework)**

| Component | Purpose | Technology |
|-----------|---------|-----------|
| `InboundAgent` | Answers calls, schedules appointments | LiveKit Agents, OpenAI GPT-4o-mini |
| `OutboundAgent` | Reminder calls, rescheduling | LiveKit Agents, OpenAI GPT-4o-mini |
| STT (Speech-to-Text) | Converts speech to text | Deepgram Nova-3 |
| TTS (Text-to-Speech) | Converts text to speech | Cartesia Sonic-2 |
| VAD (Voice Activity Detection) | Detects speech vs silence | Silero VAD |
| Noise Cancellation | Removes background noise | BVC Telephony NC |
| Google Calendar Integration | Checks availability, books appointments | Google Workspace API |
| Supabase Integration | Stores call history and logs | Supabase PostgreSQL |

**Frontend (Next.js + React)**

| Component | Purpose | Technology |
|-----------|---------|-----------|
| Dashboard | Call history and metrics display | Next.js App Router, React 19 |
| Authentication | User login and session management | Supabase Auth |
| Styling | Modern, responsive UI | Tailwind CSS v4 |
| Animations | Smooth transitions and effects | Framer Motion |

---

## How It Works

### Inbound Call Flow

```
1. Incoming SIP Call
   └─→ LiveKit SIP trunk routes to agent

2. Agent Initialization
   └─→ Agent connects to LiveKit room
   └─→ Greeting message plays to caller

3. Speech Recognition
   └─→ Caller speaks
   └─→ Deepgram STT converts to text
   └─→ Silero VAD detects speech boundaries

4. LLM Processing
   └─→ OpenAI GPT-4o-mini processes caller intent
   └─→ Agent decides on next action:
       ├─ Retrieve available appointment slots
       ├─ Schedule appointment
       ├─ Take a message
       └─ Transfer or escalate

5. Function Calling (Agent Tools)
   └─→ get_available_slots()
       └─→ Queries Google Calendar API
       └─→ Returns available time windows

   └─→ schedule_appointment()
       └─→ Creates calendar event
       └─→ Stores appointment in Google Calendar

   └─→ take_message()
       └─→ Records message content
       └─→ Stores in Supabase with caller info

6. Response Generation
   └─→ LLM generates natural response
   └─→ Cartesia TTS converts to audio
   └─→ Audio streams back to caller

7. Call Conclusion
   └─→ Conversation ends naturally
   └─→ Call metadata and transcript logged to Supabase
   └─→ Call history updated in dashboard
```

### Outbound Reminder Call Flow

```
1. Scheduled Reminder Trigger
   └─→ Job scheduler or manual trigger
   └─→ Passes appointment details via job metadata

2. Outbound Call Initiation
   └─→ Twilio SIP trunk creates outbound call
   └─→ Agent joins LiveKit room

3. Call Connection
   └─→ Caller answers
   └─→ Greeting and appointment reminder plays

4. Voicemail Detection
   └─→ Agent listens for voicemail tone
   └─→ If detected: hang up immediately (no message left)
   └─→ Call logged as voicemail encounter

5. Live Conversation (If Answered)
   └─→ Caller responds
   └─→ STT processes response
   └─→ LLM evaluates:
       ├─ Can attend appointment (confirm)
       ├─ Cannot attend (offer rescheduling)
       └─ Need information (provide details)

6. Rescheduling (If Needed)
   └─→ get_available_slots()
       └─→ Retrieves alternative times

   └─→ schedule_appointment()
       └─→ Books new appointment
       └─→ Updates Google Calendar

   └─→ confirm_meeting()
       └─→ Records new confirmation
       └─→ Logs to Supabase

7. Call Closure
   └─→ Graceful goodbye and hang-up
   └─→ Full interaction logged to database
   └─→ Dashboard updated with results
```

### Technology Deep Dive

**Why LiveKit?**
- Real-time, low-latency voice processing
- Built-in SIP trunk integration for phone networks
- Scalable architecture for handling multiple simultaneous calls
- Agent framework with LLM integration

**Why Deepgram + Cartesia?**
- Deepgram Nova-3: Industry-leading accuracy for speech-to-text, optimized for telephony
- Cartesia Sonic-2: Natural-sounding text-to-speech with professional voice options
- Both providers integrate directly with LiveKit agents

**Why OpenAI GPT-4o-mini?**
- Fast inference for real-time conversation
- Strong instruction following for function calling
- Cost-effective for high-volume call handling
- Understands legal/professional context

**Why Google Calendar + Supabase?**
- Google Calendar: Existing system of record for many legal offices
- Supabase: Serverless PostgreSQL with real-time updates, perfect for call history and analytics

---

## Replication

### Prerequisites

Before setting up Recall, you'll need accounts and API keys from:

1. **LiveKit Cloud** (https://cloud.livekit.io)
   - LiveKit URL and API key for voice infrastructure

2. **Google Cloud** (https://cloud.google.com)
   - Google Calendar API enabled
   - Service account created with Calendar API access
   - Service account JSON credentials file downloaded

3. **OpenAI** (https://platform.openai.com)
   - API key with access to GPT-4o-mini

4. **Deepgram** (https://console.deepgram.com)
   - API key for speech-to-text

5. **Cartesia** (https://cartesia.ai)
   - API key for text-to-speech

6. **Twilio** (https://www.twilio.com) [Optional for outbound calls]
   - SIP trunk configured (BYOC - Bring Your Own Carrier)
   - Authorized phone numbers

7. **Supabase** (https://supabase.com)
   - PostgreSQL database created
   - Public API key and secret key
   - Database URL

### Step 1: Clone Repository

```bash
git clone https://github.com/RichardGuo24/Recall-Combined.git
cd Recall-Combined
```

### Step 2: Backend Setup

#### Install Python Dependencies

Navigate to the backend directory:

```bash
cd backend/recall-be
```

Install dependencies using `uv` (modern Python package manager):

```bash
# Install uv if you don't have it
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install project dependencies
uv sync
```

If you prefer pip:

```bash
pip install -r requirements.txt
```

#### Environment Configuration

Create a `.env` file in `backend/recall-be/`:

```env
# LiveKit Configuration
LIVEKIT_URL=wss://your-livekit-url.livekit.cloud
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret

# Google Calendar Configuration
GOOGLE_CREDENTIALS_FILE=/path/to/service-account-key.json
CALENDAR_ID=primary  # or specific calendar ID
DEFAULT_MEETING_DURATION=60

# OpenAI Configuration
OPENAI_API_KEY=sk-your-openai-key

# Deepgram Configuration
DEEPGRAM_API_KEY=your_deepgram_key

# Cartesia Configuration
CARTESIA_API_KEY=your_cartesia_key

# Supabase Configuration
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SK=your_supabase_service_role_key
SUPABASE_PK=your_supabase_public_anon_key

# Twilio Configuration (for outbound calls)
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
OUTBOUND_SIP_TRUNK_ID=your_sip_trunk_id
TWILIO_CALLER_ID=+1234567890

# Business Information
BUSINESS_NAME=Your Legal Firm Name
BUSINESS_HOURS=Monday-Friday 9AM-6PM
BUSINESS_PHONE=+1-555-0123
```

#### Set Up Google Calendar Service Account

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project
3. Enable the Google Calendar API
4. Create a service account and download the JSON key
5. Share your calendar with the service account email address
6. Place the JSON file in an accessible location and set `GOOGLE_CREDENTIALS_FILE` path in `.env`

#### Create Supabase Tables

Create the `call_history` table in your Supabase database:

```sql
CREATE TABLE call_history (
  id BIGSERIAL PRIMARY KEY,
  phone_number VARCHAR(20) NOT NULL,
  caller_name VARCHAR(255),
  call_type VARCHAR(20),  -- 'inbound' or 'outbound'
  meeting_date TIMESTAMP,
  call_notes TEXT,
  transcript TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_call_history_phone ON call_history(phone_number);
CREATE INDEX idx_call_history_date ON call_history(created_at);
```

### Step 3: Frontend Setup

Navigate to the frontend directory:

```bash
cd ../../frontend/recall-fe
```

Install dependencies:

```bash
npm install
```

Create a `.env.local` file:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_public_anon_key
```

### Step 4: Running the Application

#### Start the Backend

From `backend/recall-be/`:

```bash
# Development mode
python main.py

# Or if you have uv installed
uv run python main.py
```

The backend will connect to LiveKit and wait for incoming SIP calls or start listening for job triggers.

#### Start the Frontend

From `frontend/recall-fe/`:

```bash
# Development server (runs on http://localhost:3000)
npm run dev

# Production build
npm run build
npm start
```

#### Verify Everything is Connected

1. **Check LiveKit Connection:**
   - Backend logs should show successful LiveKit connection
   - LiveKit dashboard should show your agent available

2. **Test Google Calendar Integration:**
   - Agent should be able to query calendar availability
   - Test creating a sample appointment in the calendar

3. **Test Supabase Connection:**
   - Run a test inbound call and verify it appears in `call_history` table
   - Check Supabase dashboard for incoming data

4. **Access Dashboard:**
   - Open http://localhost:3000 in your browser
   - You should see call history and analytics

### Step 5: Configure SIP Trunk (Inbound Calls)

For incoming calls, configure your phone provider's SIP trunk to route calls to your LiveKit instance:

**For Twilio (Recommended):**

1. Go to Twilio Console → Voice → Manage SIP Trunks
2. Create or select a SIP trunk
3. Set the Origination URI to: `sips://your-livekit-url:5349`
4. Configure phone numbers to use this trunk
5. Test with an inbound call

**For Other Providers:**
- Route SIP traffic to your LiveKit server URL
- Ensure firewall rules allow port 5349 (TLS) for secure connections

### Step 6: Deploy to Production

**Backend Deployment (e.g., Railway, Render, AWS Lambda):**

```bash
# Create a requirements file from uv
uv pip compile pyproject.toml > requirements.txt

# Deploy using your preferred platform
# Example with Railway:
# 1. Connect your GitHub repo
# 2. Set environment variables
# 3. Deploy
```

**Frontend Deployment (e.g., Vercel, Netlify):**

```bash
# Vercel (recommended for Next.js)
npm install -g vercel
vercel

# Or push to GitHub and connect to Vercel dashboard
```

### Troubleshooting

**Backend won't connect to LiveKit:**
- Verify `LIVEKIT_URL` is correct (should be `wss://` for WebSocket)
- Check API key and secret are correct
- Ensure firewall allows outbound HTTPS connections

**Google Calendar integration not working:**
- Verify service account has been shared with the calendar
- Check `GOOGLE_CREDENTIALS_FILE` path is correct and file is readable
- Look for permission errors in logs

**Incoming calls not reaching agent:**
- Verify SIP trunk is configured correctly in phone provider
- Check LiveKit listening on port 5349
- Review LiveKit logs for incoming SIP traffic

**Supabase writes failing:**
- Verify database credentials in `.env`
- Ensure `call_history` table exists
- Check row-level security policies aren't blocking writes

**STT not recognizing speech:**
- Verify Deepgram API key is valid
- Check audio input levels (use test recordings)
- Ensure noise cancellation isn't too aggressive

---

## Usage

### Making Inbound Calls

1. **Call the configured phone number** for your system
2. **Listen to the greeting** from Recall's receptionist
3. **State your reason** for calling (e.g., "I'd like to schedule an appointment")
4. **Interact naturally** with the AI:
   - Provide your name if asked
   - Respond to availability questions
   - Confirm appointment details
5. **Hang up** when finished (or agent will disconnect after confirmation)

### Making Outbound Reminder Calls

Outbound calls can be triggered via:

**Manual Trigger (via API or dashboard):**
```bash
curl -X POST http://localhost:8000/api/reminder-call \
  -H "Content-Type: application/json" \
  -d '{
    "phone_number": "+1-555-0100",
    "meeting_date": "2025-10-25T10:00:00Z",
    "customer_name": "John Doe",
    "meeting_details": "Client consultation"
  }'
```

**Scheduled Trigger:**
- Set up a cron job or task scheduler to call the endpoint at appropriate times (e.g., 24 hours before appointment)

### Accessing the Dashboard

1. Open http://localhost:3000 (development) or your deployed URL
2. **View Call History:** See all incoming and outgoing calls with transcripts
3. **Analytics:** Track appointment bookings, completion rates, voicemail encounters
4. **Manage Contacts:** View repeated callers and their interaction history
5. **Export Data:** Download call logs for compliance and audit purposes

---

## Contributing

Contributions are welcome! This project is meant to be improved and extended. Here are some areas where help is appreciated:

### Areas for Contribution

- 🔊 **Voice Quality Improvements:** Better VAD tuning, accent handling, background noise robustness
- 🎨 **Frontend Enhancements:** Better dashboards, real-time call monitoring, export features
- 📊 **Analytics Features:** Advanced metrics, predictive analysis, customer satisfaction tracking
- 🔐 **Security:** HIPAA compliance, PCI-DSS readiness for payment processing
- ⚡ **Performance:** Faster response times, cost optimization, multi-call scaling
- 📚 **Documentation:** Better setup guides, video tutorials, architecture docs
- 🧪 **Testing:** Unit tests, integration tests, load testing

### Contribution Workflow

1. **Fork** the repository
2. **Create a feature branch** (`git checkout -b feature/YourFeatureName`)
3. **Make your changes** with clear commit messages
4. **Test thoroughly** before submitting
5. **Push** to your branch (`git push origin feature/YourFeatureName`)
6. **Open a Pull Request** with:
   - Clear description of changes
   - Why the change is needed
   - Screenshots/demos if UI-related
   - Testing methodology

### Code Standards

- **Backend:** Follow PEP 8, add type hints, include docstrings
- **Frontend:** Use TypeScript, follow ESLint rules, add meaningful prop descriptions
- **Documentation:** Keep README updated, add inline comments for complex logic

---

## Contact

**Richard Guo** - [rwg2125@columbia.edu](mailto:rwg2125@columbia.edu)

**Project Links:**
- Repository: [Recall-Combined](https://github.com/RichardGuo24/Recall-Combined)
- Live Demo: [https://recall-fe-xi.vercel.app](https://recall-fe-xi.vercel.app/)
- Issues/Feedback: [GitHub Issues](https://github.com/RichardGuo24/Recall-Combined/issues)

---

## Acknowledgments

Special thanks to:
- **MY AMAZING HACKATHON TEAM*
- **LiveKit** for providing excellent real-time voice infrastructure
- **OpenAI, Deepgram, and Cartesia** for state-of-the-art AI models
- **DivHacks 2025** for the hackathon platform and community support

### Resources Used

- [LiveKit Agents Documentation](https://docs.livekit.io/agents/)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Google Calendar API Guide](https://developers.google.com/calendar)
- [Supabase Documentation](https://supabase.com/docs)
- [Next.js App Router Guide](https://nextjs.org/docs)

---

<p align="center">
  Made with ❤️ for legal offices everywhere
</p>

<!-- Technology Badges -->
[Python]: https://img.shields.io/badge/Python-3.13+-3776ab?style=for-the-badge&logo=python&logoColor=white
[Python-url]: https://www.python.org/

[LiveKit]: https://img.shields.io/badge/LiveKit-Real--time%20Voice-4a90e2?style=for-the-badge&logo=livekit&logoColor=white
[LiveKit-url]: https://livekit.io/

[OpenAI]: https://img.shields.io/badge/OpenAI-GPT--4o%20Mini-000000?style=for-the-badge&logo=openai&logoColor=white
[OpenAI-url]: https://openai.com/

[GoogleCloud]: https://img.shields.io/badge/Google%20Cloud-Calendar%20API-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white
[GoogleCloud-url]: https://cloud.google.com/

[Supabase]: https://img.shields.io/badge/Supabase-PostgreSQL-3fcf8e?style=for-the-badge&logo=supabase&logoColor=white
[Supabase-url]: https://supabase.com/

[Next.js]: https://img.shields.io/badge/Next.js-black?style=for-the-badge&logo=next.js&logoColor=white
[Next-url]: https://nextjs.org/

[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/

[TypeScript]: https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white
[TypeScript-url]: https://www.typescriptlang.org/

[TailwindCSS]: https://img.shields.io/badge/Tailwind%20CSS-06B6D4?style=for-the-badge&logo=tailwind-css&logoColor=white
[TailwindCSS-url]: https://tailwindcss.com/

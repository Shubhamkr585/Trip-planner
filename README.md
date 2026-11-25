<div align="center">
  <h1>🌍 Voyage</h1>
  <p><strong>A collaborative, real-time trip planning platform with voice chat, AI suggestions, and seamless itinerary tracking.</strong></p>

  [![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg)](https://opensource.org/licenses/ISC)
  [![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)](https://reactjs.org/)
  [![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat&logo=node.js&logoColor=white)](https://nodejs.org/)
  [![Socket.io](https://img.shields.io/badge/Socket.io-black?style=flat&logo=socket.io&badgeColor=010101)](https://socket.io/)
  [![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=flat&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
  [![GitHub](https://img.shields.io/badge/GitHub-Shubhamkr585%2FTrip--planner-181717?style=flat&logo=github)](https://github.com/Shubhamkr585/Trip-planner)

  <h3>
    <a href="https://trip-planner-psi-jade.vercel.app/" target="_blank">Live Demo</a>
    <span> | </span>
    <a href="https://trip-planner-oyp4.onrender.com" target="_blank">Backend API</a>
    <span> | </span>
    <a href="https://github.com/Shubhamkr585/Trip-planner" target="_blank">GitHub Repository</a>
  </h3>
</div>

<br />

## 📸 See It In Action
<!-- > **Note:** Replace this placeholder with your actual screenshot or GIF! -->

![Voyage Dashboard Screenshot](https://drive.google.com/uc?export=view&id=1BvDPpHrJQ5e1AEaNdKUZjm0bYapExU6V)

---

## ✨ Features

- **⚡ Real-time synchronization:** Instantly see updates from your travel buddies, powered by Socket.io & Redis caching.
- **🎙️ Live Voice Rooms:** Plan together out loud without leaving the app, powered by LiveKit.
- **🤖 AI Itinerary Suggestions:** Get smart travel suggestions and itineraries powered by Groq/GenAI.
- **📍 Interactive Maps:** Real-time location sharing and place searches using Leaflet & Google Maps.
- **📝 Notes and Memories:** Collaborative, real-time text-based notes for keeping track of ideas and journaling your trip.
- **💰 Shared Expense Tracking:** Easily add and manage group trip costs so everyone knows who owes what.
- **📸 Media Uploads:** Share photos and files directly to your trip dashboard, stored securely on Cloudinary.
- **🔐 Secure Authentication:** Seamless and safe user login flows powered by Clerk.

---

## 🛠️ Tech Stack

### Frontend (Client)
- **React 19** (Vite)
- **Tailwind CSS & Shadcn UI** for styling
- **Clerk React SDK** for authentication
- **Socket.io-client** for real-time state
- **Leaflet & React-Leaflet** for interactive mapping
- **LiveKit React Components** for voice/video rooms

### Backend (Server)
- **Node.js & Express 5**
- **MongoDB & Mongoose** for persistent data
- **Redis** for blazing-fast caching and Pub/Sub
- **Socket.io** for real-time WebSocket communication
- **Clerk Node SDK** for validating authentication
- **LiveKit Server SDK** for voice room management
- **Cloudinary & Multer** for media uploads

---

## 👥 Engineering Team & Technical Ownership

Voyage was built as a collaborative group engineering project with clear separation of concerns across real-time backend systems, cloud pipelines, and interactive client architecture.

### Backend Systems & Cloud Architecture (Owned by Shubham Kumar)
**Shubham Kumar ([@Shubhamkr585](https://github.com/Shubhamkr585)) — Lead Backend & Systems Architect**

- **⚡ Real-Time Synchronization Engine & Caching Layer (`server/socket.js`)**:
  - Architected room-scoped bidirectional event routing with **Socket.io** (live itinerary changes, member locations, trip chat, broadcast notifications).
  - Implemented an in-memory **Redis** caching tier with an LRU trimming policy (`lTrim -100 -1`) to serve the latest 100 messages at sub-millisecond latencies before asynchronously persisting to MongoDB, preventing database write starvation under high message volumes.
- **🛡️ Authentication & User Lifecycle Pipeline (`server/middleware/auth.js`, `server/routes/users.js`)**:
  - Integrated **Clerk Node SDK** to intercept and verify RS256 JWT bearer tokens on all protected API routes.
  - Built automated synchronization between Clerk identities and MongoDB `User` documents, injecting validated user context (`req.dbUser`) into Express route handlers.
- **🤖 Dual-LLM AI Pipeline (`server/services/ai.js`, `server/routes/trips.js`)**:
  - Engineered hybrid AI capabilities: **Groq SDK (Llama 3.1)** for sub-second, low-latency conversational travel assistance and **Google Gemini 2.5 Flash** for multimodal trip finalization and markdown journal generation.
- **🎙️ WebRTC Voice Room Infrastructure (`server/routes/voice.js`)**:
  - Implemented the token exchange service using **LiveKit Server SDK**, issuing cryptographically signed, short-lived JWTs to authenticate clients into scoped WebRTC audio rooms.
- **🗄️ Relational Data Modeling & MongoDB Schemas (`server/models/`)**:
  - Designed the relational data schema in **Mongoose** connecting Trips (unique 6-character join codes, member access tiers), Checkpoints, Notes, Expenses, Messages, and Notifications.
- **📸 Media Pipeline & DevOps (`server/routes/media.js`, `docker-compose.yml`)**:
  - Configured streaming media ingestion via **Multer** directly to **Cloudinary**, preventing disk I/O bottlenecks.
  - Authored production **Docker** containers and **docker-compose** multi-service orchestration.

### Frontend Client & UI/UX Architecture
- **Collaborators:** Deepak Kumar Yadav, Aman Singh, Ayush Gupta
  - Developed the React 19 Single Page Application (Vite, Tailwind CSS, Shadcn UI, Leaflet interactive maps, LiveKit WebRTC client integration).

---

## 🚀 Getting Started

Follow these instructions to set up the project locally.

### Prerequisites
- Node.js (v18+ recommended)
- MongoDB running locally or a MongoDB Atlas URI
- Redis server running locally or a cloud Redis URI (e.g., Upstash)

### 1. Clone the repository
```bash
git clone https://github.com/Shubhamkr585/Trip-planner.git
cd Trip-planner
```

### 2. Install Dependencies

**For the Backend:**
```bash
cd server
npm install
```

**For the Frontend:**
```bash
cd ../client
npm install
```

### 3. Environment Variables

Create a `.env` file in **both** the `server` and `client` directories using the templates below. Fill in the variables with your own API keys.

#### Server (`/server/.env`)
```env
# Core Server Config
PORT=5000
CLIENT_URL=http://localhost:5173

# Database & Caching
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.example.mongodb.net
REDIS_URI=redis://default:<password>@your-redis-url.io:6379

# Clerk Authentication
CLERK_PUBLISHABLE_KEY=pk_test_your_clerk_publishable_key
CLERK_SECRET_KEY=sk_test_your_clerk_secret_key

# LiveKit Voice Calling
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret

# AI APIs
GEMINI_API_KEY=your_gemini_api_key
GROQ_API_KEY=your_groq_api_key

# Cloudinary Media Uploads
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret

# Map Search Pipeline
OPEN_TRIP_MAP_KEY=your_open_trip_map_key

# SMTP / Email via Resend
SMTP_HOST=smtp.resend.com
SMTP_PORT=587
SMTP_USER=resend
SMTP_PASS=your_resend_smtp_password
```

#### Client (`/client/.env`)
```env
VITE_API_URL=http://localhost:5000/api
VITE_CLERK_PUBLISHABLE_KEY=pk_test_your_clerk_publishable_key
VITE_LIVEKIT_URL=wss://your-livekit-url.livekit.cloud
```

### 4. Run the Application

You'll need two terminal windows to run both the frontend and backend simultaneously.

**Terminal 1 (Backend):**
```bash
cd server
npm run dev
```

**Terminal 2 (Frontend):**
```bash
cd client
npm run dev
```

Visit `http://localhost:5173` in your browser to start planning your Voyage!

---

## 🐳 Docker Setup (Recommended for Production)

Voyage includes a complete Dockerized environment following industry best practices (multi-stage Nginx builds for the frontend and minimal Node images for the backend).

### 1. Configure Environment Variables
**Crucial Step:** Docker relies entirely on your `.env` files. Ensure you have created both `/client/.env` and `/server/.env` files with your actual credentials as described in the Environment Variables section above. 

> [!WARNING]
> Never commit your `.env` files to version control! The provided `docker-compose.yml` dynamically reads these files at runtime to keep your secrets perfectly safe.

### 2. Run with Docker Compose
From the root directory of the project, run:
```bash
docker-compose up -d --build
```
This single command will build the frontend and backend images and spin up the containers in the background.

### 3. Access the Application
- **Frontend:** The React app is served securely via Nginx on port `80`. Access it simply at `http://localhost`.
- **Backend API:** The Express server is exposed on port `5000` at `http://localhost:5000`.

---

## 📜 License

This project is licensed under the [ISC License](LICENSE).

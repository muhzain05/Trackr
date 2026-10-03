# Trackr

A job-application tracking prototype with two partially separate application paths:

- a browser UI that stores application data in Firestore and resume variants in local storage
- an Express REST API prototype backed by an in-memory application store

The current frontend is primarily a **Vue application mounted from <code>frontend/src/services/app.js</code>**, even though React packages are also installed in the frontend workspace.

## Browser application

The active browser UI supports:

- application records with company, position, location, source, status, priority, dates, and notes
- Saved / Applied / Interview / Offer / Rejected workflow columns
- filtering, search, sorting, and archived applications
- follow-up dates and reminder calculations
- communication notes/history per application
- a master resume attached to the user's application state
- Firestore persistence for the main application tracker

The current Firestore path used by the application is a single <code>users/currentUser</code> document containing the master resume and application array.

## Resume workspace

The repository also contains a client-side resume workspace that is separate from the main Firestore tracker.

It supports:

- a master resume plus role/company variants
- LaTeX text editing
- localStorage-backed resume tracks
- copying LaTeX to the clipboard
- downloading a <code>.tex</code> file
- sending the current LaTeX snippet to Overleaf
- local snapshot/variant UI

### "AI" helper

The checked-in resume assistant is **not an LLM**. It is an offline, rule/template-based helper that detects keywords in the user's question and returns predefined guidance for tailoring, ATS formatting, cover letters, action verbs, and similar resume topics.

That distinction is intentional here so the README matches the implementation.

## Express API prototype

<code>backend/</code> contains a separate Express 5 API with routes for:

- creating applications
- listing/filtering/searching applications
- fetching an application by ID
- updating and deleting applications
- changing status
- reading status history
- health checks

Its current database module is an in-memory array with pagination and filtering helpers. This API is **not currently the persistence layer used by the main Vue/Firestore frontend**.

## Stack

### Frontend

- Vue 3
- Vite 7
- JavaScript
- Firebase Web SDK / Firestore
- HTML/CSS
- localStorage for resume-track data
- LaTeX-oriented resume editing utilities

React dependencies are present in <code>frontend/package.json</code>, but the current <code>main.jsx</code> imports and mounts the Vue implementation.

### Backend prototype

- Node.js
- Express 5
- CORS
- dotenv
- nodemon
- in-memory application store

## Repository layout

~~~text
Trackr/
├── frontend/
│   ├── src/
│   │   ├── main.jsx
│   │   └── services/
│   │       ├── app.js
│   │       ├── firebase.js
│   │       ├── resumes.js
│   │       ├── ai-chatbot-offline.js
│   │       └── ai-assistant-offline.js
│   └── package.json
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── db/
│   │   ├── routes/
│   │   ├── app.js
│   │   └── server.js
│   └── package.json
└── README.md
~~~

## Frontend setup

~~~bash
git clone https://github.com/muhzain05/Trackr.git
cd Trackr/frontend
npm install
npm run dev
~~~

The Firestore client reads <code>VITE_FIREBASE_API_KEY</code> from the Vite environment. Other Firebase project identifiers are currently present in <code>firebase.js</code>.

## Backend status

The Express side is a prototype and is not ready to present as a production backend. In the current checked-in <code>backend/src/app.js</code>, CORS middleware is invoked before <code>app</code> is initialized; that file needs a small initialization-order fix before the backend can start cleanly.

After that source issue is corrected, its scripts are:

~~~bash
cd backend
npm install
npm run dev
~~~

## Current limitations

- frontend and REST backend use separate storage paths and are not integrated
- Firestore data is stored under a fixed <code>currentUser</code> document rather than authenticated per-user records
- the resume assistant is deterministic/template based
- backend tests are not implemented in the current package script
- the Express backend currently has the initialization-order issue described above

## Author

Muhammad Zain Asad — [GitHub](https://github.com/muhzain05)

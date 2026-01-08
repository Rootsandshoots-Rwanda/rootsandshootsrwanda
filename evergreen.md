# Evergreen — Architecture Diagram & Step-by-Step Gantt Roadmap

**Purpose:** Visual architecture and an 8-week step-by-step roadmap (Gantt-style) for building the Evergreen project as part of the Roots & Shoots Rwanda Digital Platform. This document is designed to be copy-pasted into presentations or used directly for project planning.

---

## 1. One-line summary

Build a mobile-first, offline-capable Evergreen module: public storytelling pages + private Admin PWA for facilitators, coordinators, and admins — integrated with Firebase (Auth, Firestore, Storage), robust offline sync, photo handling, analytics, and secure role-based access.

---

## 2. High-level Architecture Diagram (visualized)

```
[Public Site - Next.js/Static HTML]
           |
           | (Read-only Firebase SDK for Impact data)
           v
      [Firebase Hosting / Vercel]                    [Admin Hosting (PWA)]
           |                                                |
           |                                                v
           |                                      [Admin PWA (React + Workbox)]
           |                                                |
           |                                                |  Auth (Firebase Auth)
           |                                                |  Firestore (reports, forms, users)
           |                                                |  Storage (photos)
           |                                                |  Cloud Functions (optional APIs)
           |                                                |
           v                                                v
    Users / Visitors                                   Field Staff / Coordinators

Offline Layer (Admin PWA): IndexedDB (idb/localForage) + Sync Queue
Security: Firebase Rules, Custom Claims, Storage Rules
Monitoring + Backup: Firebase Crashlytics, Analytics, Scheduled Firestore Export to GCS
```

---

## 3. Components & Responsibilities

* **Public Evergreen pages** (rootsandshoots.rw/evergreen)

  * Purpose: storytelling, recruit volunteers, show impact counters
  * Tech: HTML/Tailwind or Next.js; i18next for EN/FR/RW; read-only Firebase connection for live stats

* **Admin PWA** (admin.rootsandshoots.rw)

  * Purpose: Submit reports (offline), build forms, review & export data
  * Tech: React + Tailwind + ShadCN UI, Workbox service worker, IndexedDB (localForage/idb), React Query/TanStack for caching

* **Backend (Firebase)**

  * Auth: Firebase Auth (email/password, optional Google OAuth)
  * Firestore: projects, forms, reports, users, activityLogs
  * Storage: photos, documents
  * Functions: aggregation endpoints, scheduled exports, approval workflows (optional)

* **Offline & Sync**

  * Local queue for pending reports, automatic background sync with retry & exponential backoff, photo compression before upload

* **Security**

  * Firestore rules enforcing role-based access, Storage rules, enforce email verification and custom claims

---

## 4. 8-Week Step-by-Step Roadmap (Gantt-style table)

Legend: W1..W8 = Week 1..Week 8

| Task ID |                                 Task name |  W1 |  W2 |  W3 |  W4 |  W5 |  W6 |  W7 |  W8 | Outcome / Notes                                |
| ------- | ----------------------------------------: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | ---------------------------------------------- |
| 1       |                Project init & environment |  X  |     |     |     |     |     |     |     | Repo, CI, Firebase project config              |
| 2       |         Public Evergreen pages (skeleton) |  X  |  X  |     |     |     |     |     |     | Static pages + i18next keys                    |
| 3       |          Firebase schema + security rules |  X  |  X  |     |     |     |     |     |     | Firestore structure + rules tested in emulator |
| 4       |       Admin PWA skeleton (Auth + routing) |     |  X  |  X  |     |     |     |     |     | Login, role-based routes                       |
| 5       |                Form Builder (Coordinator) |     |     |  X  |  X  |     |     |     |     | CRUD forms, preview mode                       |
| 6       | ReportForm (Facilitator) + photo compress |     |     |     |  X  |  X  |     |     |     | Dynamic form, camera upload, compression       |
| 7       |               Offline queue & sync engine |     |     |     |     |  X  |  X  |     |     | IndexedDB queue, upload worker, retry logic    |
| 8       |            My Reports & edit window (10h) |     |     |     |     |     |  X  |     |     | Local edits & server update rules              |
| 9       |           Coordinator dashboard + exports |     |     |     |     |     |     |  X  |     | Filters, totals, PDF/Excel export              |
| 10      |     Impact Dashboard integration (public) |     |     |     |     |     |     |  X  |     | Real-time counters from Firestore              |
| 11      |             QA, staging, pilot deployment |     |     |     |     |     |     |     |  X  | UAT with 2 facilitators; fix found issues      |
| 12      |               Production launch & backups |     |     |     |     |     |     |     |  X  | Go-live, schedule backups                      |

---

## 5. Step-by-step checklist (detailed)

### Week 1 — Setup & Foundations

* [ ] Create GitHub repo & branch strategy (main/dev/feature/*)
* [ ] Configure GitHub Actions: lint, test, build, deploy to staging
* [ ] Initialize Firebase project (auth, firestore, storage)
* [ ] Setup Firebase Emulator for local dev
* [ ] Create initial Firestore schema docs (projects:evergreen)

### Week 2 — Public pages & Auth

* [ ] Build static Evergreen pages (index, about, gallery) with i18next placeholders
* [ ] Add meta tags, OG tags, and schema placeholders
* [ ] Implement Firebase Auth sign-in for Admin PWA (email/password)
* [ ] Add role-based routing skeleton

### Week 3 — Forms & Form Builder

* [ ] Design forms collection & UI schema
* [ ] Implement Coordinator Form Builder (create/edit/delete fields)
* [ ] Save form templates to Firestore

### Week 4 — Report Form & Media

* [ ] Implement dynamic ReportForm rendering from form template
* [ ] Add camera/photo upload UI and client-side compression
* [ ] Save drafts to IndexedDB

### Week 5 — Sync Engine & Offline

* [ ] Implement sync queue: enqueue reports + photos
* [ ] Background worker: detect online, upload, update report doc
* [ ] Exponential backoff & failure handling

### Week 6 — My Reports & Edit Window

* [ ] Show facilitator recent reports with 10-hour edit window
* [ ] Implement edit flow for pending reports
* [ ] Add activityLogs entries for create/update actions

### Week 7 — Dashboard & Exports

* [ ] Build Coordinator ReportsTable with filters & totals
* [ ] Implement PDF export (jsPDF) and Excel (SheetJS)
* [ ] Add charts (Recharts) for visual analytics

### Week 8 — Pilot, QA & Launch

* [ ] Deploy to staging and invite 2–3 facilitators for pilot
* [ ] Collect feedback & fix critical issues
* [ ] Deploy to production, enable scheduled Firestore backups

---

## 6. Acceptance Criteria (for each milestone)

* **Auth & Security:** Users can sign in; roles enforced by Firebase rules (test pass)
* **Form Builder:** Coordinator can create a new Evergreen form and it saves to Firestore
* **Offline Flow:** Facilitator can create report offline; it syncs automatically when online
* **Media:** Photos compressed < 500KB and available in Storage URLs embedded in reports
* **Exports:** Coordinator can export filtered reports to PDF and Excel containing images
* **Public Integration:** Impact counters reflect aggregated Firestore data

---

## 7. Deliverables I will prepare next (pick any)

* [ ] Firestore seed JSON for `projects`, `forms`, `users` (Evergreen sample)
* [ ] Firebase security rules file (production-ready template)
* [ ] React ReportForm component scaffold (with compression + IndexedDB hooks)
* [ ] Workbox service worker config + sync worker pseudocode
* [ ] Pilot test checklist & feedback form

---

*End of document — Evergreen Architecture & Roadmap*
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "AIzaSyDuIbCM8ekh1CjBzq7NntPtLekU71skw6Q",
  authDomain: "rs-rwanda-prod.firebaseapp.com",
  projectId: "rs-rwanda-prod",
  storageBucket: "rs-rwanda-prod.firebasestorage.app",
  messagingSenderId: "565764668411",
  appId: "1:565764668411:web:2b0dfdaddc24c091ece95e",
  measurementId: "G-SYD0SYTZ5F"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);

// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "AIzaSyDuIbCM8ekh1CjBzq7NntPtLekU71skw6Q",
  authDomain: "rs-rwanda-prod.firebaseapp.com",
  projectId: "rs-rwanda-prod",
  storageBucket: "rs-rwanda-prod.firebasestorage.app",
  messagingSenderId: "565764668411",
  appId: "1:565764668411:web:4b7250cbfe5e149aece95e",
  measurementId: "G-6D4NG1RTDF"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
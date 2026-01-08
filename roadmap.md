# 🌍 ROOTS & SHOOTS RWANDA DIGITAL PLATFORM 2.0  
### Developer Implementation Roadmap

**Repository:** `rootsandshoots-rwanda`  
**Maintainer:** Ishimwe Manzi Samuel  
**Frameworks:** Next.js, React, Firebase, Tailwind CSS, i18next  
**Last Updated:** 2025-11-11  

---

## 🧭 OVERVIEW

The Roots & Shoots Rwanda platform is divided into two major systems:

1. **Public Experience (Project 1):**  
   A trilingual website (EN/FR/RW) to showcase the organization’s mission, attract donors, and inspire youth participation.

2. **Private Experience (Project 2):**  
   A secure Progressive Web App (PWA) for staff to manage reports, projects, and forms — with offline capabilities and automatic sync.

---

## ⚙️ TECHNOLOGY STACK

| Layer | Tools |
|-------|-------|
| Frontend (Public) | Next.js, Tailwind CSS, Framer Motion, i18next |
| Admin PWA | React, Firebase, IndexedDB, React Router, TanStack Query |
| Database | Firebase Firestore |
| Authentication | Firebase Auth (Email/Password, optional Google OAuth) |
| Hosting | Firebase Hosting / Vercel |
| Analytics | Google Analytics 4, Firebase Crashlytics |
| Testing | Jest, React Testing Library, axe-core |
| DevOps | GitHub Actions (CI/CD) |

---

## 🚀 PHASE 1: PUBLIC WEBSITE

### 🔹 1.1 – Setup & Environment
- [ ] Initialize Next.js project  
- [ ] Install Tailwind CSS and set up custom color palette  
- [ ] Integrate i18next for multilingual support (EN/FR/RW)  
- [ ] Install UI/UX libraries: `framer-motion`, `lucide-react`, `react-scroll`  
- [ ] Configure ESLint, Prettier, and Husky hooks  
- [ ] Setup `.env` for environment variables  

### 🔹 1.2 – Homepage
- [ ] Create animated hero section (4-image carousel with CTAs)  
- [ ] Add responsive navbar with language selector  
- [ ] Add footer with social media links (Instagram, X, YouTube)  
- [ ] Implement smooth scrolling + animations via Framer Motion  

### 🔹 1.3 – Core Pages
- [ ] `/about` – Global & Rwanda mission, Meet the Team (timeline + cards)  
- [ ] `/projects` – Dynamic project gallery (Firestore integration in Phase 3)  
- [ ] `/news` – Blog grid with tag filters (Markdown or Firestore source)  
- [ ] `/contact` – Contact info + embedded Google Map + form (EmailJS/Formspree)  
- [ ] `/donate` – Static donation details (MomoPay, Bank Transfer)  
- [ ] `/impact` – Placeholder dashboard for live Firebase data  

### 🔹 1.4 – UI/UX & SEO
- [ ] Implement dark/light mode toggle  
- [ ] Add meta tags, Open Graph tags, and structured data  
- [ ] Create `sitemap.xml` and `robots.txt`  
- [ ] Optimize images (lazy loading)  
- [ ] Lighthouse audit (Target 90+ performance, accessibility, SEO)  

---

## 🔒 PHASE 2: PRIVATE ADMIN PORTAL (REACT PWA)

### 🔹 2.1 – Setup
- [ ] Initialize React app with TypeScript  
- [ ] Install Firebase SDKs and connect project  
- [ ] Enable PWA support (manifest.json + service worker)  
- [ ] Add routing using React Router v6  

### 🔹 2.2 – Authentication
- [ ] Email/Password login (Firebase Auth)  
- [ ] Role-based redirects (facilitator, coordinator, admin)  
- [ ] Password reset  
- [ ] (Optional) Google OAuth login  
- [ ] Two-Factor Authentication (2FA) for admins  

### 🔹 2.3 – Firestore Structure
```yaml
users:
  - userId
  - name
  - email
  - role
projects:
  - id
  - name
  - description
forms:
  - id
  - title
  - associatedProject
  - questions[]
reports:
  - id
  - facilitatorId
  - projectId
  - createdAt
  - answers[]
activityLogs:
  - id
  - action
  - timestamp
  - userId

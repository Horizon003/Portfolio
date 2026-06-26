# Horizon Creator Studio — Admin Dashboard Design Document

> **Project:** Haris Aslam (Horizon) Portfolio — Hidden Admin System  
> **File Output:** Single `index.html` (Firebase + localStorage)  
> **Dashboard Name:** Horizon Creator Studio  
> **Version:** 1.0 — Full Spec

---

## 1. Project Overview

**Horizon Creator Studio** ek hidden, premium admin control room hai jo portfolio website ke andar secretly embedded hai. Normal visitors ko dashboard ka koi hint nahi milta. Admin ek secret gesture ya keyword se unlock karta hai, Firebase se authenticate karta hai, aur phir poori website ka content live edit kar sakta hai bina code chhue.

### Core Philosophy
- **Zero Footprint** — Public visitors ko dashboard nazar na aaye
- **Single File** — Sab kuch `index.html` mein
- **Live Sync** — Firebase Realtime Database se instant updates
- **Graceful Fallback** — localStorage demo mode agar Firebase unavailable ho

---

## 2. Technology Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML5, CSS3 (CSS Variables), JavaScript ES Modules |
| Database | Firebase Realtime Database |
| Auth | Firebase Authentication (Email/Password) |
| Fonts | Inter (body), Space Grotesk (display) |
| Icons | Font Awesome 6 |
| Fallback | localStorage (Demo Mode) |
| Deploy | Netlify / GitHub Pages |
| Images | URL-based (no Firebase Storage upload required) |

---

## 3. Firebase Configuration

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyCXGOewMBdnFwBoMmakuPBOj9Ozhrg5CZw",
  authDomain: "portfolio-2c39e.firebaseapp.com",
  projectId: "portfolio-2c39e",
  storageBucket: "portfolio-2c39e.firebasestorage.app",
  messagingSenderId: "932149474919",
  appId: "1:932149474919:web:235a31bc7c7654befa46c2",
  databaseURL: "https://portfolio-2c39e-default-rtdb.asia-southeast1.firebasedatabase.app"
};

// ⚠️ Replace with your actual UID after creating Firebase Auth user
const ADMIN_UID = "PASTE_YOUR_ADMIN_UID_HERE";
```

### Firebase Realtime Database Security Rules

**Standard Rules:**
```json
{
  "rules": {
    "portfolio": {
      ".read": true,
      ".write": "auth != null && auth.uid === 'PASTE_YOUR_ADMIN_UID_HERE'"
    }
  }
}
```

**Strict Rules (Messages Private):**
```json
{
  "rules": {
    "portfolio": {
      ".read": true,
      "messages": {
        ".read": "auth != null && auth.uid === 'PASTE_YOUR_ADMIN_UID_HERE'",
        ".write": true
      },
      "$other": {
        ".write": "auth != null && auth.uid === 'PASTE_YOUR_ADMIN_UID_HERE'"
      }
    }
  }
}
```

---

## 4. Database Structure

```text
portfolio/
├── settings/
│   ├── siteName
│   ├── siteTagline
│   ├── faviconUrl
│   ├── maintenanceMode
│   └── lastUpdated
│
├── theme/
│   ├── primaryColor          "#7C3AED"
│   ├── primaryLight          "#A78BFA"
│   ├── primaryDark           "#5B21B6"
│   ├── accentColor           "#0EA5E9"
│   ├── accentLight           "#38BDF8"
│   ├── roseColor             "#F43F5E"
│   ├── tealColor             "#14B8A6"
│   ├── backgroundColor       "#FFFFFF"
│   ├── altBgColor            "#F8FAFC"
│   ├── surfaceColor          "#F1F5F9"
│   ├── textPrimary           "#0F172A"
│   ├── textSecondary         "#475569"
│   ├── textMuted             "#94A3B8"
│   ├── borderColor           "#E2E8F0"
│   ├── buttonText            "#FFFFFF"
│   ├── buttonRadius          "12px"
│   ├── cardRadius            "20px"
│   ├── shadowIntensity       "medium"
│   ├── glowIntensity         "medium"
│   ├── darkMode              false
│   ├── fontFamily            "Inter"
│   ├── sectionSpacing        "80px"
│   └── cardSpacing           "24px"
│
├── navigation/
│   ├── logoText              "Haris."
│   ├── logoDotVisible        true
│   ├── logoDotColor          "#7C3AED"
│   ├── stickyNavbar          true
│   ├── ctaText               "Hire Me"
│   ├── ctaTarget             "#contact"
│   └── links/
│       ├── link_1: { label: "Home", target: "#home", visible: true, sortOrder: 1 }
│       ├── link_2: { label: "About", target: "#about", visible: true, sortOrder: 2 }
│       ├── link_3: { label: "Skills", target: "#skills", visible: true, sortOrder: 3 }
│       ├── link_4: { label: "Projects", target: "#projects", visible: true, sortOrder: 4 }
│       ├── link_5: { label: "Experience", target: "#experience", visible: true, sortOrder: 5 }
│       └── link_6: { label: "Contact", target: "#contact", visible: true, sortOrder: 6 }
│
├── hero/
│   ├── badgeText             "Available for Work"
│   ├── badgeDotColor         "#22C55E"
│   ├── firstName             "Haris"
│   ├── lastName              "Aslam"
│   ├── accentWord            "Horizon"
│   ├── aliasText             "Horizon"
│   ├── headline              "Building Digital Experiences"
│   ├── staticPrefix          "I'm a"
│   ├── description           "Pharm-D student, vibe coder..."
│   ├── profileImageUrl       ""
│   ├── orbitSpeed            "12s"
│   ├── bgGradient            "purple-blue"
│   ├── imageGlow             true
│   ├── scrollText            "Scroll to explore"
│   ├── typingPhrases/
│   │   ├── phrase_1: "Pharm-D Student"
│   │   ├── phrase_2: "Vibe Coder"
│   │   ├── phrase_3: "Creative Technologist"
│   │   ├── phrase_4: "Graphic Designer"
│   │   ├── phrase_5: "AI Enthusiast"
│   │   └── phrase_6: "Media Cell Head"
│   ├── orbitIcons/
│   │   ├── icon_1: { icon: "fa-react", color: "#61DAFB" }
│   │   ├── icon_2: { icon: "fa-figma", color: "#F24E1E" }
│   │   ├── icon_3: { icon: "fa-fire", color: "#FF6D00" }
│   │   └── icon_4: { icon: "fa-brain", color: "#7C3AED" }
│   ├── buttons/
│   │   ├── btn_1: { text: "View Projects", icon: "fa-arrow-right", link: "#projects", style: "primary" }
│   │   └── btn_2: { text: "Download CV", icon: "fa-download", link: "#", style: "outline" }
│   └── stats/
│       ├── stat_1: { number: 50, suffix: "+", label: "Happy Clients" }
│       ├── stat_2: { number: 100, suffix: "+", label: "Projects Done" }
│       └── stat_3: { number: 3, suffix: "", label: "Countries" }
│
├── about/
│   ├── sectionTag            "About Me"
│   ├── sectionTitle          "Passionate Creator"
│   ├── sectionSubtitle       "Blending pharmacy and technology..."
│   └── cards/
│       ├── card_1: { title: "The Student", description: "...", icon: "fa-graduation-cap", iconGradient: "purple-blue", visible: true, sortOrder: 1 }
│       ├── card_2: { title: "The Tech Enthusiast", description: "...", icon: "fa-code", iconGradient: "blue-teal", visible: true, sortOrder: 2 }
│       └── card_3: { title: "The Creative", description: "...", icon: "fa-palette", iconGradient: "teal-rose", visible: true, sortOrder: 3 }
│
├── education/
│   ├── sectionTag            "Education"
│   ├── sectionTitle          "Academic Journey"
│   └── items/
│       ├── edu_1: { title: "Matriculation", year: "2020", institute: "...", description: "...", badge: "Science", active: false, pulse: false, sortOrder: 1 }
│       ├── edu_2: { title: "FSC Pre-Medical", year: "2022", institute: "...", description: "...", badge: "Pre-Medical", active: false, pulse: false, sortOrder: 2 }
│       └── edu_3: { title: "Pharm-D", year: "2022–Present", institute: "QCP Pattoki", description: "...", badge: "Current", active: true, pulse: true, sortOrder: 3 }
│
├── skills/
│   ├── sectionTag            "My Skills"
│   ├── sectionTitle          "What I Do"
│   └── items/
│       ├── skill_1: { title: "Graphic Designing", description: "...", icon: "fa-pen-nib", iconGradient: "purple-rose", tags: ["Canva","Photoshop","Figma"], visible: true, sortOrder: 1 }
│       ├── skill_2: { title: "Web Development", description: "...", icon: "fa-code", iconGradient: "blue-teal", tags: ["HTML","CSS","JS","Firebase"], visible: true, sortOrder: 2 }
│       ├── skill_3: { title: "Social Media Management", description: "...", icon: "fa-hashtag", iconGradient: "teal-blue", tags: ["Instagram","Content","Strategy"], visible: true, sortOrder: 3 }
│       └── skill_4: { title: "Generative AI", description: "...", icon: "fa-brain", iconGradient: "rose-purple", tags: ["ChatGPT","Midjourney","Claude"], visible: true, sortOrder: 4 }
│
├── services/
│   ├── sectionTag            "Services"
│   ├── sectionTitle          "What I Offer"
│   ├── visible               false
│   └── items/
│       └── [same structure as skills + price, buttonText, buttonLink, category]
│
├── projects/
│   ├── sectionTag            "My Work"
│   ├── sectionTitle          "Featured Projects"
│   └── items/
│       └── {
│             id, title, category, status,
│             shortDescription, fullDescription,
│             thumbnail, mediaType, iframePreviewUrl,
│             liveLink, githubLink, technologies[],
│             featured, visible, sortOrder,
│             buttonText, buttonAction, badgeColor,
│             createdAt, updatedAt
│           }
│
├── experience/
│   ├── sectionTag            "Experience"
│   ├── sectionTitle          "My Journey"
│   └── items/
│       └── {
│             id, roleTitle, organization,
│             description, icon, badge, badgeColor,
│             highlights[], countries[], tags[],
│             isCurrent, isFeatured,
│             sortOrder, visible, createdAt
│           }
│
├── clients/
│   ├── sectionTag            "Trusted By"
│   ├── sectionTitle          "Clients & Collaborators"
│   ├── displayMode           "marquee"
│   ├── animationSpeed        "30s"
│   └── items/
│       └── {
│             id, name, logo, countryFlag,
│             countryName, testimonial, rating,
│             visible, sortOrder
│           }
│
├── contact/
│   ├── sectionTag            "Contact"
│   ├── sectionTitle          "Let's Connect"
│   ├── sectionSubtitle       "..."
│   ├── whatsappNumber        ""
│   ├── whatsappDisplayText   "Chat on WhatsApp"
│   ├── whatsappLink          ""
│   ├── email                 ""
│   ├── emailDisplayText      "Send Email"
│   ├── location              "Pattoki, Pakistan"
│   ├── submitButtonText      "Send Message"
│   ├── successMessage        "Message sent successfully!"
│   ├── errorMessage          "Failed to send. Try WhatsApp."
│   ├── formEnabled           true
│   ├── firebaseSaveEnabled   true
│   └── mailtoFallback        true
│
├── socialLinks/
│   ├── instagram: { url, icon, visible, sortOrder }
│   ├── facebook:  { url, icon, visible, sortOrder }
│   ├── linkedin:  { url, icon, visible, sortOrder }
│   ├── github:    { url, icon, visible, sortOrder }
│   ├── youtube:   { url, icon, visible, sortOrder }
│   ├── whatsapp:  { url, icon, visible, sortOrder }
│   └── email:     { url, icon, visible, sortOrder }
│
├── footer/
│   ├── brandText             "Haris Aslam"
│   ├── description           "Building digital experiences..."
│   ├── copyrightText         "© 2025 Haris Aslam. All rights reserved."
│   ├── bgColor               "#0F172A"
│   ├── visible               true
│   └── links/
│       └── [array of footer link groups]
│
├── seo/
│   ├── pageTitle             "Haris Aslam — Horizon | Portfolio"
│   ├── metaDescription       "..."
│   ├── metaKeywords          "..."
│   ├── ogTitle               "..."
│   ├── ogDescription         "..."
│   ├── ogImage               ""
│   ├── faviconUrl            ""
│   └── authorName            "Haris Aslam"
│
├── animations/
│   ├── revealAnimations      true
│   ├── animatedBlobs         true
│   ├── typingEffect          true
│   ├── typingSpeed           100
│   ├── deletingSpeed         50
│   ├── pauseDuration         2000
│   ├── countUpStats          true
│   ├── orbitAnimation        true
│   ├── orbitSpeed            "12s"
│   ├── blobOpacity           0.6
│   ├── blobBlur              "80px"
│   └── scrollBehavior        "smooth"
│
├── messages/
│   └── {messageId}/
│       ├── id
│       ├── name
│       ├── email
│       ├── subject
│       ├── message
│       ├── status             "unread"
│       ├── createdAt
│       └── userAgent
│
├── media/
│   └── {mediaId}/
│       ├── id
│       ├── title
│       ├── url
│       ├── altText
│       └── createdAt
│
└── activityLogs/
    └── {logId}/
        ├── action             "Project Added"
        ├── section            "projects"
        ├── itemTitle          "Blood Donation App"
        ├── timestamp
        └── adminUid
```

---

## 5. Hidden Admin Access

### Method 1 — Logo Click (5 times in 2 seconds)

```text
Click logo → Click logo → Click logo → Click logo → Click logo
          ↓ (within 2000ms total)
    Premium Unlock Animation triggers
          ↓
    Admin Login Modal opens
```

### Method 2 — Secret Keyword

```text
User types "horizonadmin" anywhere on page
(characters are intercepted, NOT shown on screen)
          ↓
    Premium Unlock Animation triggers
          ↓
    Admin Login Modal opens
```

### Unlock Animation Sequence
1. **Glow Ripple** — Purple/teal radial pulse expands from center
2. **Glass Panel Reveal** — Frosted glass overlay fades in with scale
3. **"Horizon Creator Studio" text** animates in with letter spacing
4. **Login modal** slides up into view

---

## 6. Admin Login Modal Design

```text
┌─────────────────────────────────────────┐
│  🔐  Horizon Creator Studio              │
│      Premium Admin Access               │
│─────────────────────────────────────────│
│                                         │
│  ✉  Email                               │
│  ┌─────────────────────────────────┐   │
│  │ admin@example.com               │   │
│  └─────────────────────────────────┘   │
│                                         │
│  🔒 Password                            │
│  ┌──────────────────────────────── 👁 ┐ │
│  │ ••••••••••••••                     │ │
│  └────────────────────────────────────┘ │
│                                         │
│  [ Forgot Password? ]                   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │     🚀  Login to Dashboard      │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ⚠ Error message area (hidden by default)│
│                                         │
│                              [ ✕ Close ]│
└─────────────────────────────────────────┘
```

### Login States
| State | Visual |
|-------|--------|
| Default | Clean glass modal |
| Loading | Spinner + "Authenticating..." |
| Success | Green checkmark + slide out |
| Wrong UID | Red "Access Denied" + auto sign-out |
| Wrong Pass | Red inline error message |
| Forgot Pass | Toast: "Reset email sent to your inbox" |

---

## 7. Dashboard Layout

### Desktop Layout (≥1024px)

```text
┌──────────────────────────────────────────────────────────────────┐
│  TOP BAR                                                         │
│  [☰ Menu]  Horizon Creator Studio  [🌙 Dark]  [👤 Admin]  [🚪]  │
├────────────────┬─────────────────────────────────────────────────┤
│                │                                                 │
│   SIDEBAR      │   MAIN CONTENT AREA                             │
│   (240px)      │   (flexible)                                    │
│                │                                                 │
│  🏠 Overview   │                                                 │
│  🏗 Structure  │                                                 │
│  🧭 Nav        │                                                 │
│  🦸 Hero       │                                                 │
│  👤 About      │                                                 │
│  🎓 Education  │                                                 │
│  ⚡ Skills     │                                                 │
│  🛠 Services   │                                                 │
│  📁 Projects   │                                                 │
│  💼 Experience │                                                 │
│  🤝 Clients    │                                                 │
│  📬 Contact    │                                                 │
│  🔗 Social     │                                                 │
│  📄 Footer     │                                                 │
│  🎨 Theme      │                                                 │
│  ✨ Animations │                                                 │
│  🖼 Media      │                                                 │
│  📊 SEO        │                                                 │
│  💬 Messages   │                                                 │
│  💾 Backup     │                                                 │
│                │                                                 │
│  ─────────     │                                                 │
│  🔴 Firebase   │                                                 │
│  Sync Active   │                                                 │
│                │                                                 │
└────────────────┴─────────────────────────────────────────────────┘
```

### Mobile Layout (≤768px)

```text
┌──────────────────────────────┐
│  ☰  Horizon Studio      [👤] │
│─────────────────────────────│
│                             │
│   MAIN CONTENT              │
│   (full width)              │
│                             │
├─────────────────────────────┤
│ 🏠  🏗  📁  💬  🎨          │
│ Home Struct Proj Msg Theme  │
└─────────────────────────────┘
(Bottom nav — 5 quick sections)
```

---

## 8. Dashboard Top Bar

```text
┌─────────────────────────────────────────────────────────────────┐
│ [☰]  ⬡ Horizon Creator Studio    🟢 Firebase Sync Active       │
│                            [🌙 Dark Mode] [admin@mail] [Logout] │
└─────────────────────────────────────────────────────────────────┘
```

**Elements:**
- Hamburger menu (sidebar toggle)
- Brand name + logo glyph
- Sync status badge (`🟢 Firebase Sync Active` / `🟡 Demo Mode Active`)
- Dark/Light mode toggle
- Admin email display
- Logout button

---

## 9. Dashboard Sections — Detail

---

### A. Overview (Dashboard Home)

```text
┌────────────────┬──────────────┬──────────────┬──────────────────┐
│  Total Projects│ Featured     │ Live Projects │  Total Messages  │
│      12        │      4       │      9        │       7          │
│  📁            │  ⭐           │  ✅            │  💬              │
└────────────────┴──────────────┴──────────────┴──────────────────┘

┌──────────────────────────────────┬──────────────────────────────┐
│  Draft Projects: 2               │  Firebase Status             │
│  In Progress: 1                  │  🟢 Connected                │
│  Total Skills: 4                 │  Last Synced: 2 min ago      │
│  Visible Sections: 10            │  Admin UID: [masked]         │
└──────────────────────────────────┴──────────────────────────────┘

RECENT ACTIVITY LOG
┌──────────────────────────────────────────────────────────────────┐
│ ✏ Project "Blood App" edited              2 min ago              │
│ ➕ New message from Ali Hassan            5 min ago              │
│ 🎨 Theme color updated                   1 hour ago             │
│ ➕ Project "Eid Site" added              Yesterday               │
└──────────────────────────────────────────────────────────────────┘
```

---

### B. Site Structure Manager

```text
Section Visibility Controls

┌────────────────────────────────────────────────────┐
│  Navbar       [●──] Visible  [Edit Label] [↕ Move] │
│  Hero         [●──] Visible  [Edit Label] [↕ Move] │
│  About        [●──] Visible  [Edit Label] [↕ Move] │
│  Education    [●──] Visible  [Edit Label] [↕ Move] │
│  Skills       [●──] Visible  [Edit Label] [↕ Move] │
│  Services     [○──] Hidden   [Edit Label] [↕ Move] │
│  Projects     [●──] Visible  [Edit Label] [↕ Move] │
│  Experience   [●──] Visible  [Edit Label] [↕ Move] │
│  Clients      [●──] Visible  [Edit Label] [↕ Move] │
│  Contact      [●──] Visible  [Edit Label] [↕ Move] │
│  Footer       [●──] Visible  [Edit Label] [↕ Move] │
└────────────────────────────────────────────────────┘
```

Each row: Toggle visibility, edit section title/tag/subtitle, reorder

---

### C. Navigation Manager

**Fields:**
- Logo text (e.g., "Haris.")
- Logo dot: visible toggle, color picker
- Sticky navbar: on/off
- CTA button text + target link
- Nav links list: add/edit/delete/reorder each link (label + target anchor)
- Mobile menu label

**UI:** Drag-handle list for reordering links, inline edit for each

---

### D. Hero Manager

**Left panel — Form:**
- Availability badge text + dot color
- First name, Last name, Accent word, Alias
- Typing phrases (add/edit/delete/reorder — draggable list)
- Headline + static prefix
- Description textarea
- Profile image URL + preview
- Hero buttons (text, icon, link, style)
- Stats (number, suffix, label for each stat)
- Orbit icons (icon class, color — add/edit/delete)
- Orbit animation speed (slider)
- Background gradient selector
- Image glow toggle
- Scroll indicator text

**Right panel — Live Preview Card:**
```text
┌──────────────────────────────────┐
│ [Profile Image Preview]          │
│  🟢 Available for Work           │
│  Haris Aslam — Horizon           │
│  "Vibe Coder"   (typed preview)  │
│  [View Projects] [Download CV]   │
│  50+ Clients | 100+ Projects     │
└──────────────────────────────────┘
```

---

### E. About Manager

**Fields:**
- Section tag, title, subtitle
- About cards list

Each card:
- Title, description
- Font Awesome icon class (with live icon preview)
- Icon color/gradient selector
- Visible toggle
- Sort order / drag reorder

**Card UI:**
```text
┌──────────────────────────────────┐
│  [Icon preview: 🎓]              │
│  Title: [The Student       ]     │
│  Icon:  [fa-graduation-cap ]     │
│  Gradient: [purple-blue    ▾]    │
│  Desc: [textarea           ]     │
│  [👁 Show] [🗑 Delete] [↕ Drag]  │
└──────────────────────────────────┘
```

---

### F. Education Timeline Manager

**Fields per item:**
- Title (degree/course name)
- Year
- Institute name
- Description
- Badge text
- Active/current toggle (triggers pulse animation)
- Marker pulse on/off

**Timeline Preview:**
```text
○─────── 2020 ──── Matriculation ── Science
│
●─────── 2022 ──── FSC Pre-Medical ── Pre-Med
│
◉═══════ 2022–Present ── Pharm-D ── Current 🔵
(pulsing marker for active)
```

---

### G. Skills Manager

**Fields per skill:**
- Title
- Description
- Font Awesome icon class
- Icon gradient
- Tags (comma-separated → tag chips)
- Visible toggle
- Sort order

**Actions:** Add, Edit, Delete, Duplicate, Reorder, Hide/Show

---

### H. Services Manager

*(Optional section — hidden by default)*

**Fields per service:**
- Title
- Description
- Icon
- Price/rate (optional)
- Button text + link
- Category
- Visible toggle
- Sort order

---

### I. Projects Manager

**Top Controls:**
```text
[ + Add Project ]  [ Search... ]  [ Filter: All ▾ ]  [ Sort: Newest ▾ ]
                                   Category | Status | Featured
```

**Project List View:**
```text
┌───────────────────────────────────────────────────────────────┐
│ [Thumbnail] Blood Donation App          🟢 Live  ⭐ Featured   │
│             Category: Web App                                 │
│             Updated: 2 days ago                               │
│  [✏ Edit] [👁 Hide] [⭐ Feature] [📋 Duplicate] [🗑 Delete]   │
├───────────────────────────────────────────────────────────────┤
│ [Thumbnail] Eid Mubarak Website         🟢 Live               │
│             Category: Creative Web                            │
│  [✏ Edit] [👁 Hide] [⭐ Feature] [📋 Duplicate] [🗑 Delete]   │
└───────────────────────────────────────────────────────────────┘
```

**Project Edit Form (full):**
```text
Project Title:        [_________________________]
Category:             [_____________] (text or dropdown)
Status:               [ Live ▾ ] (Live / Draft / In Progress / Award / Archived)
Featured:             [☐ Mark as Featured]
Visible:              [☑ Show on website]

Short Description:    [_________________________]
Full Description:     [textarea — rich text if possible]

Thumbnail URL:        [_________________________] [Preview]
Media Type:           [⦿ Image ○ iFrame ○ Video]
iFrame Preview URL:   [_________________________]

Live Link:            [_________________________]
GitHub Link:          [_________________________]
Technologies:         [React, Firebase, HTML...]  → tag chips

Button Text:          [View Project]
Button Action:        [⦿ Open Modal ○ Open Link ○ Both]
Badge Color:          [🟣 ▾]

Sort Order:           [0]

Created:  [auto] | Updated: [auto]

[💾 Save Project]  [Cancel]  [Live Preview ↗]
```

---

### J. Experience / Roles Manager

**Fields per role:**
- Role title
- Organization
- Description
- Icon (Font Awesome)
- Badge text + color
- Highlights (bullet list — add/delete)
- Countries/tags
- Current role toggle
- Featured role toggle
- Sort order

---

### K. Trusted by Clients Manager

**Section Settings:**
```text
Display Mode:    [⦿ Marquee ○ Grid ○ Cards]
Animation Speed: [──●──────] 30s
Section Title:   [_____________]
Section Label:   [_____________]
```

**Client Item Fields:**
- Name
- Logo URL (or initials fallback)
- Country flag emoji
- Country name
- Testimonial text
- Rating (1–5 stars)
- Visible toggle
- Sort order

**Public Display (Marquee Mode):**
```text
←  🇵🇰 Client A  ⭐⭐⭐⭐⭐  |  🇺🇸 Client B ⭐⭐⭐⭐⭐  |  🇬🇧 Client C ⭐⭐⭐⭐⭐  ←
(infinite smooth scroll, purple/teal glow cards)
```

---

### L. Contact Settings Manager

**Section Content:**
- Tag, title, subtitle
- WhatsApp number + display text + link
- Email + display text
- Location

**Form Settings:**
- Contact form fields visibility
- Submit button text
- Success / error messages
- Enable/disable contact form toggle
- Firebase save toggle
- Mailto fallback toggle
- WhatsApp prefilled message fallback

**Messages Inbox:**
```text
[ 🔍 Search... ]  [ Filter: Unread ▾ ]

┌─────────────────────────────────────────────────────────────┐
│ 🔵 [Unread]  Ali Hassan              10 min ago             │
│    Subject: Web Design Inquiry                              │
│    "Hello, I need a portfolio website..."                   │
│    [📖 Read] [📋 Copy] [📧 Reply] [📱 WhatsApp] [🗑 Delete] │
├─────────────────────────────────────────────────────────────┤
│ ✓  [Read]    Sara Ahmed              2 days ago             │
│    Subject: Collaboration Proposal                          │
│    [📖 Read] [📋 Copy] [📧 Reply] [📱 WhatsApp] [🗑 Delete] │
└─────────────────────────────────────────────────────────────┘
```

---

### M. Social Links Manager

**Per platform:**
- URL
- Icon class (Font Awesome)
- Visible toggle
- Sort order (drag reorder)

**Platforms:** Instagram, Facebook, LinkedIn, GitHub, YouTube, WhatsApp, Email, Custom

---

### N. Footer Manager

- Brand text
- Description
- Copyright text
- Background color picker
- Footer links (grouped)
- Social icons visibility
- Footer visible toggle

---

### O. Theme Settings

```text
┌────────────── COLOR PALETTE ──────────────────────┐
│  Primary:       [🟣 #7C3AED] ████                 │
│  Primary Light: [🟣 #A78BFA] ████                 │
│  Primary Dark:  [🟣 #5B21B6] ████                 │
│  Accent:        [🔵 #0EA5E9] ████                 │
│  Accent Light:  [🔵 #38BDF8] ████                 │
│  Teal:          [🟢 #14B8A6] ████                 │
│  Rose:          [🔴 #F43F5E] ████                 │
│  Background:    [⬜ #FFFFFF] ████                 │
│  Surface:       [⬜ #F1F5F9] ████                 │
│  Text Primary:  [⬛ #0F172A] ████                 │
│  Text Secondary:[⬛ #475569] ████                 │
│  Border Color:  [⬜ #E2E8F0] ████                 │
└───────────────────────────────────────────────────┘

GRADIENT PRESETS
[ Purple Blue ] [ Blue Teal ] [ Rose Purple ] [ Midnight ]

TYPOGRAPHY
  Font Family:  [ Inter ▾ ]

SPACING
  Button Radius: [───●──] 12px
  Card Radius:   [────●─] 20px
  Section Spacing:[──●───] 80px
  Shadow:        [ Medium ▾ ]
  Glow:          [ Medium ▾ ]

DARK MODE
  [○ Light  ● Dark]

[ 💾 Apply Theme Instantly ]  [ Reset to Defaults ]
```

*All changes apply instantly via CSS variables — no reload.*

---

### P. Animation Settings

| Setting | Control |
|---------|---------|
| Reveal Animations | Toggle |
| Animated Blobs | Toggle |
| Blob Opacity | Slider 0–1 |
| Blob Blur | Slider px |
| Typing Effect | Toggle |
| Typing Speed | Slider ms |
| Deleting Speed | Slider ms |
| Pause Duration | Slider ms |
| Count-Up Stats | Toggle |
| Orbit Animation | Toggle |
| Orbit Speed | Slider seconds |
| Scroll Behavior | Smooth / Normal |

---

### Q. Media Library

```text
[ + Add Image URL ]  [ 🔍 Search... ]

┌─────────┬──────────────────────────────────────┐
│ [Preview]│ profile-photo.jpg                   │
│  [img]  │ URL: https://cloudinary.com/...      │
│         │ Alt: Haris Aslam Profile             │
│         │ [📋 Copy URL] [🗑 Delete]             │
├─────────┼──────────────────────────────────────┤
│ [Preview]│ blood-app-thumb.png                 │
│  [img]  │ URL: https://cloudinary.com/...      │
│         │ [📋 Copy URL] [🗑 Delete]             │
└─────────┴──────────────────────────────────────┘
```

---

### R. SEO Settings

- Page title
- Meta description
- Meta keywords
- OG Title / OG Description / OG Image URL
- Favicon URL
- Author name

*All applied to `<head>` via JavaScript on load.*

---

### S. Backup / Import / Export

```text
┌──────────────────────────────────────────────────┐
│  DATA MANAGEMENT                                 │
│                                                  │
│  [ ⬇ Export Portfolio JSON ]                     │
│  (Downloads full portfolio data as .json)        │
│                                                  │
│  [ ⬆ Import Portfolio JSON ]                     │
│  (Select .json file to restore data)             │
│                                                  │
│  [ 🔄 Reset to Default Seed Data ]               │
│  ⚠ This will overwrite all current data          │
│                                                  │
│  [ 🗑 Clear Demo localStorage Data ]             │
│  ⚠ Only clears browser local storage            │
└──────────────────────────────────────────────────┘
```

All destructive actions require confirmation modal before proceeding.

---

## 10. Public Portfolio Sections

### Public Page Structure
```text
[Navbar]
  ↓
[Hero Section]         — Name, typing effect, stats, orbit icons, profile image
  ↓
[About Section]        — Cards: Student, Tech Enthusiast, Creative
  ↓
[Education Timeline]   — Matric, FSC, Pharm-D (vertical timeline)
  ↓
[Skills Section]       — Skill cards with icons and tags
  ↓
[Services Section]     — (hidden by default, admin can show)
  ↓
[Featured Projects]    — Highlighted grid (featured flag = true)
  ↓
[All Projects]         — Full project grid with filters
  ↓
[Project Detail Modal] — Opens on card click
  ↓
[Experience / Roles]   — Role cards with highlights
  ↓
[Trusted by Clients]   — Animated marquee with flag emojis
  ↓
[Contact Section]      — Form + WhatsApp + Email
  ↓
[Footer]               — Links, copyright, social icons
  ↓
[Back to Top Button]   — Fixed bottom-right
```

---

## 11. Project Card (Public)

```text
┌────────────────────────────────────┐
│                                    │
│  [Thumbnail / iFrame Preview]      │
│  [Hover Overlay: Quick View 👁]    │
│                                    │
├────────────────────────────────────┤
│  [Creative Web] [🟢 Live]          │
│                                    │
│  Blood Donation App                │
│  Firebase-powered donor system     │
│  with real-time Haversine search   │
│                                    │
│  [Firebase] [JS] [HTML] [CSS]      │
│                                    │
│  [ View Project → ]                │
└────────────────────────────────────┘
```

---

## 12. Project Detail Modal

```text
┌────────────────────────────────────────────────────────┐
│                                          [ ✕ ] (Esc)  │
│  [Full-width image / iFrame preview]                   │
│                                                        │
│  Blood Donation App           [🟢 Live]  [Creative Web]│
│                                                        │
│  Full description text here...                         │
│  Multiple paragraphs of project details.               │
│                                                        │
│  Technologies:                                         │
│  [Firebase] [JavaScript] [HTML] [CSS] [Cloudinary]     │
│                                                        │
│  [ 🌐 View Live Project ] [ 💻 Source Code ]           │
│                                                        │
└────────────────────────────────────────────────────────┘
```

---

## 13. Contact Form (Public)

```text
┌──────────────────────────────────────────┐
│  Name:     [_______________________]     │
│  Email:    [_______________________]     │
│  Subject:  [_______________________]     │
│  Message:  [                       ]     │
│            [                       ]     │
│            [_______________________]     │
│                                          │
│  [ 📤 Send Message ]                     │
│                                          │
│  Or reach me directly:                   │
│  [ 📱 Chat on WhatsApp ]                 │
│  [ 📧 Send Email ]                       │
└──────────────────────────────────────────┘
```

**Submit Flow:**
1. Validate all fields
2. Show loading spinner on button
3. Save to `portfolio/messages/{id}` in Firebase
4. Show success toast + reset form
5. If Firebase fails → save to localStorage + show demo notice

---

## 14. Utility Functions (JavaScript)

```javascript
// Core utility functions required

generateId()        // — Unique ID (timestamp + random)
safeText(str)       // — Prevent HTML injection (escape)
parseTags(str)      // — "React, Firebase" → ["React","Firebase"]
formatDate(ts)      // — Timestamp → "2 min ago" / "Jan 5, 2025"
showToast(msg, type)// — type: 'success' | 'error' | 'info' | 'warning'
confirmDelete(msg)  // — Promise-based confirm modal
logActivity(action, section, itemTitle)
saveData(path, data)// — Firebase set() or localStorage
readData(path)      // — Firebase get() or localStorage
subscribeData(path, cb) // — onValue() or localStorage polling
```

---

## 15. First-Run Data Seeding

```text
On page load:
  → Check Firebase portfolio/settings
  → If empty (first run):
    → Seed all default content
    → Set settings.seeded = true
  → If seeded:
    → Load all data from Firebase
    → Apply to public website

On Dashboard load:
  → Same check
  → Admin sees seeded data pre-populated
```

**Seeded defaults include:**
- All nav links (Home → Contact)
- Hero content (Haris Aslam, Horizon, all 6 typing phrases)
- 3 about cards
- 3 education entries (Matric, FSC, Pharm-D)
- 4 skills (Graphic Design, Web Dev, Social Media, AI)
- 4 projects (Blood App, Eid Site, Vibe Coding, Poster)
- 3 experience roles (Media Cell Head, Freelancer, Web Dev)
- Social links (Instagram @yep_its_horizon)
- Contact info
- Theme defaults
- Animation defaults

---

## 16. Demo Mode (Firebase Unavailable)

```text
STATUS: 🟡 Demo Mode Active

┌─────────────────────────────────────────────────┐
│ ⚠ Firebase is not connected.                    │
│   Data is saved only in this browser.           │
│   To enable live sync, configure ADMIN_UID      │
│   and deploy to Netlify with Firebase rules.    │
└─────────────────────────────────────────────────┘
```

- All features work with localStorage
- Same data structure
- Data does not sync across devices
- Warn on destructive actions

---

## 17. Responsive Breakpoints

| Breakpoint | Behavior |
|------------|----------|
| `≥1280px` | Full sidebar + wide content area |
| `1024–1279px` | Sidebar collapsed icons only + content |
| `768–1023px` | Sidebar hidden, hamburger menu |
| `≤767px` | Mobile: bottom navigation (5 tabs), full-screen panels |

**Dashboard Mobile Adaptations:**
- Tables → cards
- Multi-column forms → single column
- Modals → full screen
- Action buttons → icon-only with tooltip

---

## 18. Accessibility

- `aria-label` on all icon-only buttons
- `alt` text pulled from media library / admin fields
- Focus trap in modals
- `Escape` key closes all modals
- `aria-live` for toast notifications
- Proper `<button type="button">` everywhere
- No empty `href="#"` — use `event.preventDefault()`
- Keyboard navigation for sidebar

---

## 19. Setup Instructions (Post-Deploy)

### Step 1 — Create Firebase Auth Admin User
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Select project `portfolio-2c39e`
3. Authentication → Users → Add User
4. Enter your admin email + password

### Step 2 — Get Admin UID
1. Authentication → Users
2. Copy UID of the admin user you just created

### Step 3 — Paste UID in Code
```javascript
const ADMIN_UID = "paste_your_uid_here_exactly";
```

### Step 4 — Set Database Rules
1. Realtime Database → Rules
2. Paste the security rules from Section 3 above
3. Replace `PASTE_YOUR_ADMIN_UID_HERE` with real UID
4. Publish

### Step 5 — Deploy
1. Upload `index.html` to GitHub repo
2. Connect repo to Netlify
3. Auto-deploy on push

### Step 6 — Test Admin Login
- Method 1: Click logo **5 times within 2 seconds**
- Method 2: Type `horizonadmin` anywhere on page
- Enter Firebase Auth email + password
- Dashboard opens if UID matches

---

## 20. Design Tokens Reference

### Color System
```css
:root {
  --primary:        #7C3AED;  /* Purple */
  --primary-light:  #A78BFA;
  --primary-dark:   #5B21B6;
  --accent:         #0EA5E9;  /* Blue */
  --accent-light:   #38BDF8;
  --teal:           #14B8A6;
  --rose:           #F43F5E;
  --bg:             #FFFFFF;
  --alt-bg:         #F8FAFC;
  --surface:        #F1F5F9;
  --text-primary:   #0F172A;
  --text-secondary: #475569;
  --text-muted:     #94A3B8;
  --border:         #E2E8F0;
  --radius-btn:     12px;
  --radius-card:    20px;
  --shadow:         0 4px 24px rgba(0,0,0,0.08);
  --glow:           0 0 32px rgba(124,58,237,0.2);
}
```

### Typography
```css
--font-display: 'Space Grotesk', sans-serif;   /* Headings, hero */
--font-body:    'Inter', sans-serif;            /* Body, UI text */
```

### Glassmorphism
```css
.glass {
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: var(--shadow);
}

/* Dashboard dark glass */
.glass-dark {
  background: rgba(15, 23, 42, 0.8);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255,255,255,0.06);
}
```

---

## 21. File Size Estimate

| Component | Approx Lines |
|-----------|-------------|
| HTML structure | ~200 |
| Public portfolio CSS | ~800 |
| Dashboard CSS | ~600 |
| Utility functions JS | ~200 |
| Firebase setup + seeding | ~300 |
| Public website JS | ~400 |
| Hidden admin unlock | ~80 |
| Login modal | ~120 |
| Dashboard sidebar/nav | ~150 |
| Section A–F (Overview to Education) | ~700 |
| Section G–L (Skills to Contact) | ~800 |
| Section M–S (Social to Backup) | ~600 |
| Activity logger | ~100 |
| Contact form + messages | ~300 |
| **Total Estimate** | **~5,350 lines** |

---

*Document generated for: Haris Aslam (Horizon) — Portfolio Admin Dashboard Project*  
*Prompt Source: portfolio_admin_dashboard_prompt_UPDATED.txt*

# NuraMed: Product Requirements Document

*HT-OPEN | Open Innovation, HealthTech & Accessibility | Prototype v1.0 | Hackathon build*

## 1. Overview

**Product:** NuraMed, a bridge between patients and doctors for people who cannot travel back and forth for routine follow-ups and availibilty of doctors.

**One-line pitch:** Reach your doctor without the journey, by app, by chat, or by a simple phone call.

**Problem:** Patients across the country  travel long distances only to get a dosage reviewed, a follow-up checked, or to learn whether a doctor is available. Elderly, low-literacy and feature-phone users are left out of app-only digital health tools.

**Solution:** One shared backend powering a patient web app (PWA), four core portals, an AI health assistant, a toll-free IVR line, and two side portals (Admin, Health Worker/Pharmacy).

## 2. Goals and Non-Goals

### Goals

- Let patients request prescription and dosage reviews remotely, with the doctor making every decision.
- Show which doctors are available, with emergency guidance always first.
- Enable chat, audio and video consults with a low-bandwidth mode.
- Serve non-smartphone users through a toll-free IVR with callback.
- Give basic first-aid and self-care guidance through a safe, source-grounded AI assistant which can track all the things of the patient .
- Demonstrate improved access, clarity and task completion without overstating clinical outcomes.

### Non-Goals

- No diagnosis by the system or the AI.
- No automatic dosage change, and no AI recommendation of medicines or doses.
- No replacement for emergency services (108/112).
- No real patient data, and no claims of clinical outcomes or accuracy percentages.

## 3. Users and Personas (all fictional)

| Persona | Profile | Primary need |
| --- | --- | --- |
| **Ramesh**, 62 | Smartphone user, on BP medication | Ask for dosage review without a clinic visit |
| **Kamla Devi**, 70 | Feature phone only, Hindi speaker | Reach a doctor by calling a number |
| **Dr. Meera** | General physician | Review requests quickly with full context and an audit trail |
| **Sunita** | ASHA worker | Raise requests on behalf of patients who cannot do it themselves |
| **Admin** | Program coordinator | Manage doctors, monitor requests and call logs |
| **Pharmacist** | Local pharmacy | View only approved prescriptions |

## 4. System Architecture (high level)

- **Patient App (PWA):** hosts Portals 1, 2, 3 and the AI assistant.
- **IVR (Twilio Voice):** Portal 4, a phone interface to the same backend work same as all the features in my app using toll free no.  like dial 1  to track patitent  app similarly  (like jio call free no . u can add the our system architech).
- **Doctor Dashboard:** receives requests from the app, IVR and health workers in one queue.
- **Shared Backend + DB:** single source of truth, so an IVR request appears in the same doctor queue as an app request.
- **Side Portals:** Admin, and Health Worker/Pharmacy.
- **Suggested stack:** React/Next.js, Node or FastAPI,  three .js or 3d , Twilio Voice and SMS, WebRTC or Jitsi, Claude API for the assistant.

## 5. Global Requirements

| ID | Requirement |
| --- | --- |
| G-1 | Every screen shows an emergency banner: "In an emergency call 108 / 112 immediately." |
| G-2 | Role-based access: patient, doctor, health worker, pharmacy, admin. |
| G-3 | Every clinical action (request, approve, modify, reject) is written to an immutable audit log with user, time and action. |
| G-4 | UI in Hindi and English; Rajasthani listed as a roadmap language. |
| G-5 | Accessibility: large-font toggle, high contrast, text-to-speech, simple icon-led navigation. |
| G-6 | Only fictional or de-identified data in the prototype. |
| G-7 | Consent checkbox before any data or photo submission. |

## 6. Portal 1: Prescription Review

**Purpose:** Let a patient request a medicine or dosage review remotely. The doctor decides.

### Core (Must)

| ID | Requirement |
| --- | --- |
| P1-1 | Patient submits a review request: current medicine (from their existing prescription), issue description, and reason (side effect, dose too high or low, missed dose, other). |
| P1-2 | Doctor sees a request queue and can **Approve, Modify or Reject**, with mandatory notes on modify and reject. |
| P1-3 | The system never changes a prescription without doctor action. Request status is Pending until reviewed. |
| P1-4 | Patient is notified of the outcome, with the reason on rejection. |
| P1-5 | Full audit trail per request: submitted, viewed, decision, notes, timestamps. |

### Add-ons

| ID | Feature | Priority |
| --- | --- | --- |
| P1-6 | Side-effect symptom checklist form (dizziness, nausea, rash, etc.) attached to the request | Should |
| P1-7 | Prescription history timeline | Should |
| P1-8 | Doctor "Urgent" flag, auto-suggested when red-flag symptoms are ticked | Should |
| P1-9 | Approved prescription delivered as PDF and SMS | Should |
| P1-10 | Medicine reminders by SMS/WhatsApp (simulated in prototype) | Could |
| P1-11 | Doctor can call the patient from the request card | Could |

### Acceptance criteria

- Dosage-change request leaves the stored dosage unchanged until a doctor approves.
- Rejected request shows the patient a reason.
- Each decision appears in the audit log.

## 7. Portal 2: Doctor Availability

**Purpose:** Show which doctors are reachable now, especially when a patient needs help quickly.

### Core (Must)

| ID | Requirement |
| --- | --- |
| P2-1 | Emergency banner pinned at the top: 108 / 112, tap to dial. |
| P2-2 | Doctor list with status (Online, Busy, Offline) updated by the doctor. |
| P2-3 | Filter by specialty. |
| P2-4 | Selecting an available doctor leads into Portal 3. Offline doctors show alternates. |

### Add-ons

| ID | Feature | Priority |
| --- | --- | --- |
| P2-5 | Language filter (Hindi, Rajasthani, English) | Should |
| P2-6 | "Next available slot" display | Should |
| P2-7 | On-call roster view | Should |
| P2-8 | Nearest hospital / PHC list (static fictional data) | Should |
| P2-9 | Nearby doctors map | Could |

### Acceptance criteria

- Emergency banner is visible without scrolling on every screen.
- Request to an offline doctor returns "Doctor unavailable" with a suggested alternate.

## 8. Portal 3: Doctor-Patient Connect

**Purpose:** Real-time or asynchronous communication between patient and doctor.

### Core (Must)

| ID | Requirement |
| --- | --- |
| P3-1 | Text chat between patient and doctor. |
| P3-2 | Audio call; video call as a stretch goal (WebRTC/Jitsi). |
| P3-3 | Low-bandwidth mode: auto-suggest audio-only or text when connection is weak. |
| P3-4 | Callback request button when a doctor is busy. |

### Add-ons

| ID | Feature | Priority |
| --- | --- | --- |
| P3-5 | Pre-call form (symptoms, duration, current medicines) shared with the doctor | Should |
| P3-6 | Appointment booking | Should |
| P3-7 | Post-consult summary note visible to the patient | Should |
| P3-8 | Accessibility: text-to-speech of doctor messages, large font | Should |
| P3-9 | Chat summary and photo attachment passed in from the AI assistant | Should |

### Acceptance criteria

- On weak connectivity the user is offered audio-only or text.
- Pre-call form content appears in the doctor's view before the session starts.

## 9. Portal 4: Toll-Free IVR (Twilio)

**Purpose:** Give feature-phone, elderly and low-literacy users the same access through a phone call.

### Core (Must)

| ID | Requirement |
| --- | --- |
| P4-1 | Hindi voice menu: **1** = talk to a doctor, **2** = prescription review request, **3** = doctor availability, **0** = human callback. |
| P4-2 | Option 2 records the patient's spoken problem and creates a request in the shared doctor queue. |
| P4-3 | Callback model: the doctor calls the patient back. |
| P4-4 | Wrong key press repeats the menu; pressing 0 routes to callback. |

### Add-ons

| ID | Feature | Priority |
| --- | --- | --- |
| P4-5 | SMS confirmation after the call | Should |
| P4-6 | Missed-call-back service (patient gives a missed call, system calls back at no cost to the patient) | Should |
| P4-7 | Voice message transcription shown on the doctor's dashboard | Could |
| P4-8 | Regional language option (Rajasthani) | Could |
| P4-9 | Spoken red-flag guidance: if the caller says chest pain or breathlessness, the IVR immediately advises calling 108 | Could |

### Prototype note

If a live Twilio number is not available, provide an **IVR simulator** in the web UI that follows the identical call flow and writes to the same queue.

### Acceptance criteria

- An IVR request appears in the doctor dashboard in the same format as an app request.
- Invalid input never dead-ends the caller.

## 10. AI Health Assistant ("Setu Sahayak")

**Purpose:** Basic guidance on minor health issues and first aid, grounded in verified sources, with clear escalation to a doctor.

### Approach

Retrieval-augmented generation (RAG) over 10 to 20 verified documents (WHO, MoHFW, Red Cross first-aid guides), a strict system prompt, and the Claude API. No model training or fine-tuning.

### Requirements

| ID | Requirement | Priority |
| --- | --- | --- |
| AI-1 | Text chat in Hindi, English and Hinglish for minor issues: fever, cold and cough, stomach upset, minor cuts and burns, dehydration, heat protection | Must |
| AI-2 | Red-flag detection (chest pain, breathlessness, heavy bleeding, etc.) shows the 108/112 warning immediately and pauses normal chat | Must |
| AI-3 | Hard rules: no diagnosis, no medicine names or doses, no replacement for doctor advice; redirects to Portal 1 or a doctor | Must |
| AI-4 | "Connect to a doctor" button at the end of the conversation leading to Portal 3, with chat summary | Must |
| AI-5 | Disclaimer under every answer: "This is general information, not medical advice." | Must |
| AI-6 | Wound photo upload with consent: AI describes only what is visible, gives general first aid, never gives a verdict | Should |
| AI-7 | Unclear photos prompt a retake; photos with serious signs prompt "see a doctor / call 108" | Should |
| AI-8 | "Send to doctor" attaches the photo and summary to Portal 1 or 3 | Should |
| AI-9 | Photo privacy: strip EXIF and location data; fictional or stock images only in the demo | Must |
| AI-10 | Out-of-scope questions are politely declined | Should |

## 11. Side Portal A: Admin

| ID | Requirement | Priority |
| --- | --- | --- |
| AD-1 | Add, edit and deactivate doctors; set specialty and languages | Must |
| AD-2 | Dashboard: requests received, completed, pending, and source (app, IVR, health worker) | Must |
| AD-3 | Call logs and IVR usage | Should |
| AD-4 | Average time from request to doctor response (supports the expected-benefit demo) | Should |
| AD-5 | Audit log viewer | Should |

## 12. Side Portal B: Health Worker (ASHA) and Pharmacy

### Health Worker / Family Member

| ID | Requirement | Priority |
| --- | --- | --- |
| HW-1 | Submit a prescription review request or doctor connect on behalf of a patient, with the patient's consent recorded | Must |
| HW-2 | Use the AI assistant on the patient's behalf | Should |
| HW-3 | View the status of requests the worker submitted | Should |

### Pharmacy

| ID | Requirement | Priority |
| --- | --- | --- |
| PH-1 | View **approved** prescriptions only (read-only) | Must |
| PH-2 | Mark a prescription as fulfilled | Could |

## 13. Demo User Journeys

- **Journey A (app):** Ramesh opens the app, submits a dosage review with a side-effect checklist, Dr. Meera approves with a note, Ramesh receives the updated prescription as PDF and SMS.
- **Journey B (IVR):** Kamla Devi dials the toll-free number, presses 2, speaks her problem, the request reaches the doctor queue, and the doctor calls back.
- **Journey C (emergency):** A patient opens availability, the 108/112 banner appears first, then nearby available doctors are shown.
- **Journey D (AI assistant):** A patient uploads a fictional wound photo, receives first-aid steps, is advised to see a doctor, and forwards the photo to the doctor.
- **Journey E (health worker):** Sunita raises a request for a patient without a phone, and the pharmacy later sees the approved prescription.

## 14. Representative Test Cases

| # | Test | Expected result |
| --- | --- | --- |
| 1 | Patient requests a higher dose | Request goes to the doctor; dosage unchanged until approved |
| 2 | Doctor rejects a request | Patient notified with reason |
| 3 | Request sent to an offline doctor | "Doctor unavailable" plus alternate suggested |
| 4 | Wrong key pressed on IVR | Menu repeats; 0 gives human callback |
| 5 | Patient types "chest pain, can't breathe" | Immediate 108/112 warning |
| 6 | Weak internet | Audio-only or SMS fallback offered |
| 7 | "Which medicine should I take, what dose?" in chatbot | Refuses and redirects to Portal 1 or a doctor |
| 8 | Deep wound photo | First aid plus urgent doctor advice |
| 9 | Blurry photo | Asks for a clearer photo |
| 10 | "Is this cancer?" | No diagnosis; advises seeing a doctor |
| 11 | Pharmacy opens a pending prescription | Not visible; only approved ones shown |
| 12 | IVR request | Appears in doctor queue with source "IVR" |

## 15. Privacy, Safety and Accessibility

- Fictional or de-identified data only; no real patient records.
- Doctors make all clinical decisions; the system handles communication and workflow.
- Not an emergency service; emergency guidance is always visible.
- Red-flag detection is keyword-based and is not diagnostic.
- Consent is captured before data or photo submission; photo metadata is stripped.
- Role-based access and audit logging throughout.
- Production use would require DPDP Act compliance, encryption, telemedicine guideline adherence, and clinical validation.

## 16. Expected Benefit (carefully worded)

SetuCare may reduce unnecessary repeat travel for routine follow-ups, speed up follow-up requests, and let non-smartphone users reach a doctor. This prototype demonstrates improved access and task completion only. It makes no claims about health outcomes or AI accuracy.

## 17. Limitations

- AI responses can be wrong; photo assessment is limited by lighting, skin tone and camera quality.
- IVR is menu-based only; there is no voice AI.
- Reminders, WhatsApp and map features are simulated or static in the prototype.
- Availability data is fictional, not live hospital data.
- No clinical validation has been performed.

## 18. Scope and Priorities

| Priority | Scope |
| --- | --- |
| **Must demo** | Portal 1 approval flow with audit trail, Portal 2 emergency banner and list, Portal 4 IVR (live or simulated) with callback, doctor dashboard, AI chatbot with red-flag escalation |
| **Should demo** | Portal 3 chat and audio, wound photo upload and forward to doctor, symptom checklist, PDF/SMS prescription, Admin dashboard |
| **Slide only** | Video, map, WhatsApp reminders, Rajasthani IVR, transcription, voice AI, pharmacy fulfilment |

## 19. Success Metrics for the Demo

These are prototype indicators, not clinical outcomes.

- Request-to-decision flow works end to end in all five journeys.
- IVR and app requests land in one doctor queue.
- All 12 test cases pass.
- Red-flag input always triggers the emergency warning.
- A user can complete a prescription request in under three minutes in the demo.

## 20. Suggested Build Order

1. Shared data model, roles and audit log
2. Portal 1 and doctor dashboard
3. Portal 2 with emergency banner
4. IVR simulator, then Twilio if time allows
5. AI assistant with red-flag rules, then photo upload
6. Portal 3 chat and audio
7. Admin and Health Worker/Pharmacy views
8. Demo script, test cases and limitations slide

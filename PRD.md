# HireLoop - Project Requirements (Phase 0)

Status: DRAFT. Fill every line marked  >> YOUR ANSWER <<  one step at a time.

## 1. What HireLoop does (the 4 jobs)
1. FINDS jobs (recruiter emails + job alerts)
2. CHECKS if a job suits me
3. SENDS my resume (apply)
4. REMEMBERS what I applied to (tracker)

## 2. Design decisions (updated)

| Topic | Decision | Why |
|---|---|---|
| Resume | Pre-build 4-6 DOMAIN resumes (Backend, Data, QA, ...). Bot picks the best one per JD. No new resume per job. | Cheaper, no strong model needed, more reliable |
| Domain detection | Keyword scoring first (free). Cheap LLM only for borderline jobs. | Saves API cost |
| Low match | Skip the job or ask me. Never invent a resume. | Keeps resume truthful |
| Applying | 4 tiers: (1) apply by email, (2) adapters per system (Greenhouse, Lever, Workday), (3) smart form filler, (4) assisted apply (I do login/CAPTCHA/OTP/Submit) | Corporate portals (TCS, HCLTech) are hard; assisted mode covers them |
| Form filler | Read form -> find answer (profile -> answer memory -> cheap LLM -> ask me on Telegram) -> fill by field type | Bot asks once, remembers forever |
| Autonomy | Start at Level 2: bot prepares, I approve | Safest |
| Platforms order | Email alerts first, then public job boards, then Naukri/Indeed, LinkedIn last | Lowest ban risk first |
| CAPTCHA / OTP | Bot stops and asks me. Never bypass. | Avoid bans |

## 3. Success metrics (draft - change if you want)
- 20+ relevant jobs found per week
- 80%+ of alerts are relevant to me
- Under 2 minutes of my time per application
- Zero duplicate applications

## 4. My target job profile  (Phase 0, Step 1)  - ANSWERED
- Target roles / domains: AI, ML, Deep Learning, Backend, API Design, FDE (Forward Deployed Engineer)
- Must-have skills: taken from my resume. Matching happens in 2 levels:
  first pick the DOMAIN from the JD, then match skills inside that domain's resume.
- Experience (years): FRESHER (0-1 years). Filter out jobs asking for more. Kept as a setting.
- Preferred locations / remote: set by the user in config
- Minimum salary: none (no salary filter)
- Words / companies to avoid: generic words and emotional words in JDs are ignored
  (for example "rockstar", "passionate", "fast-paced"), and the user can add companies to avoid in config
  (assumption - to be confirmed)
- Domain resumes: 4 by default, the user can add up to 6
  FINAL: 5 domain resumes I already have:
    (1) AI/ML   (2) Deep Learning   (3) Backend   (4) OpenCV / Computer Vision   (5) DA (Data Analyst - assumed)
  Domains in my target list that have no resume of their own are MAPPED to the closest resume
  (assumption, changeable in settings.yaml):
    API Design  -> Backend resume
    FDE         -> Backend resume (or AI/ML if the JD is AI-heavy)
  One slot is still free (max 6) if I want a separate FDE resume later.

### Design note: everything is a SETTING
Experience, locations, salary, avoided words and the domain list are all user settings in
config/settings.yaml. Nothing is hard-coded, so the same bot works for any user.

## 5. Platforms I want first  (Phase 0, Step 4) - ANSWERED
USE:
- Gmail (recruiter emails and job-alert emails)
- Google Careers (confirmed)
- Amazon jobs website
- Other company career pages ("etc"), added one at a time

DO NOT AUTOMATE: Naukri, Indeed, LinkedIn (their rules do not allow automation, so we avoid account-ban risk)

Notes:
- Reading my own Gmail is fine. If Naukri/Indeed/LinkedIn alert emails arrive in my Gmail, the bot can
  only TELL me about them; I would apply by hand. (Optional, my choice.)
- Big company sites (Google, Amazon) need a login and often a CAPTCHA, so they use "assisted apply":
  the bot fills the form, I log in and press Submit.
- Before automating any career site, read its terms first.

## 6. Budget  (Phase 0, Step 6) - ANSWERED
Monthly budget: Rs 1,000 - 3,000
This allows: cheap AI model calls for hard cases (domain check, odd form questions) and a small
always-on server so the bot works while I sleep. Rule: use free keyword rules first, AI only when needed.

## 7. Phase 0 checklist
- [x] Target job profile filled (section 4)
- [x] Autonomy level confirmed (Level 2)
- [x] Platforms chosen (section 5)
- [x] Budget chosen (section 6)
- [x] Task board created (simple notes file: docs/TASKS.md)
- [x] Phase 0 done -> start Phase 1

## 8. Other answers (Phase 0 questions)
- Experience level: fresher
- Resumes: I already have 5 (AI/ML, Deep Learning, Backend, OpenCV, DA)
- Where it runs: backend runs on my LAPTOP; I control it and get notifications from my MOBILE (Telegram).
  Limit: the bot only works while the laptop is on and awake. Later it can move to a small server.
- Job alerts: not set up yet. Set up email alerts first (Phase 3 task), then Gmail reads them.
- Task board: simple notes file (docs/TASKS.md)
- Timeline goal: 1 to 1.5 months, building with AI help. Realistic target in section 9.
- Users: only me first. Later maybe share (free or paid). So keep all personal data in settings/config files
  and never hard-code my details, but do NOT build multi-user features now.

## 9. Realistic 6-week target (because we use pre-built domain resumes, no per-job resume writing)
- Weeks 1-2: foundation, database, my details, domain resumes list, Telegram bot hello
- Weeks 3-4: read job-alert emails, pick the domain, score the match, alert on phone (MVP-1)
- Weeks 5-6: apply by email + assisted form filling for ONE portal (Google Careers), tracker
- After week 6: more portals, daily summary, hardening

## 10. Open items
- Hours per week I can give

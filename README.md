# Namoza Developer Assignment - Krishna Kashyap

Submission for **Developer - Position 1 (Client Web + Martech)**.

## Project overview

This repository contains all 3 requested deliverables for the OrthoNow onboarding scenario:
- Task 01: GTM event schema and booking funnel tracking design
- Task 02: Conversion-focused consultation landing page built with HTML, CSS, and vanilla JavaScript
- Task 03: Written integration architecture for HubSpot, Karix WhatsApp API, and Google Ads conversion tracking

## Repository contents

```text
.
├── README.md
├── task-01-gtm-schema.md
├── ortho-consultation-landing.html
├── task-03-integration-design.md
├── pagespeed-notes.md
└── assets/
    └── pagespeed-mobile-score.png   # add this before final submission
```

## Deliverables mapping

### Task 01 - GTM Event Schema
File: `task-01-gtm-schema.md`

Includes:
- full GTM event schema in table format
- trigger types for each event
- minimum 3 parameters per event
- GA4 report / audience use for each event
- exact `dataLayer.push()` JSON for the 3-step booking funnel
- GA4 Funnel Exploration setup for step-level drop-off
- recommended Google Ads conversion import and justification

### Task 02 - Landing Page Build
File: `ortho-consultation-landing.html`

Build decisions:
- single self-contained HTML file
- HTML, CSS, and vanilla JS only
- mobile-first responsive structure
- 2 visible fields only: Name and Phone
- clear trust section near the form
- primary CTA above the fold
- thank-you state rendered without page reload
- `window.dataLayer.push()` fires only on valid form submit

#### How to test locally
1. Download or clone the repo.
2. Open `ortho-consultation-landing.html` in a browser.
3. Open browser DevTools Console.
4. Submit the form with a valid name and 10-digit phone number.
5. Run `window.dataLayer` in the console.
6. Confirm the array contains:
   - `consultation_form_start`
   - `consultation_form_submitted`
   - `thank_you_state_view`

### Task 03 - Integration Design
File: `task-03-integration-design.md`

Includes:
- end-to-end architecture with chosen integration method
- explicit handling of HubSpot phone-number deduplication risk
- biggest failure point and fallback design
- WhatsApp 2-minute SLA risk analysis and monitoring approach

## Form field decision

The assignment includes a tension between:
- Task 02: keep the landing page form minimal with **2 fields only**
- Task 03: send **Clinic Preference** into HubSpot

For this submission, I prioritized the explicit landing-page requirement and kept the page to **2 visible fields only**:
- Name
- Phone

Operationally, clinic preference can be collected in the callback flow, inferred from campaign / geo routing, or added in a second-step CRM workflow after lead capture. This preserves the conversion goal of the landing page while keeping the integration design realistic.

## PageSpeed requirement

Before final submission, host the landing page on a temporary live URL and add a screenshot file:
- `assets/pagespeed-mobile-score.png`

Recommended quick hosting options:
- GitHub Pages
- Netlify Drop
- Vercel static deployment

Then test the live URL in **PageSpeed Insights Mobile** and capture the score screenshot.

## Recommended Loom walkthrough

### 0:00 - 2:00
- Introduce the OrthoNow scenario
- Walk through the GTM schema
- Explain why multi-step funnel tracking needs front-end `dataLayer.push()` events and not just GTM auto-detection

### 2:00 - 5:00
- Open the landing page
- Explain the conversion choices: reduced friction, one CTA, trust placement, mobile-first layout
- Open DevTools Console
- Submit the form live
- Show `window.dataLayer` and the thank-you state without page reload

### 5:00 - 8:00
- Walk through the integration architecture
- Highlight the HubSpot deduplication issue on phone numbers
- Explain WhatsApp SLA monitoring and retry / alerting logic

## Final submission checklist

- [ ] Push code to GitHub repo
- [ ] Add PageSpeed Mobile screenshot to `assets/`
- [ ] Record Loom walkthrough under 8 minutes
- [ ] Verify form demo works live in the browser console
- [ ] Send GitHub repo link + Loom link to `naman@namoza.com`
- [ ] Use subject line: `Developer Assignment - Krishna Kashyap`

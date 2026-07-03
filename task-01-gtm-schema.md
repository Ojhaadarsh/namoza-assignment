# Task 01 - GTM Event Schema

## Event schema

| Event Name | Trigger Type | Key Parameters (min 3) | GA4 Report / Audience Use |
|---|---|---|---|
| `booking_step_view` | Custom Event trigger from `dataLayer` on each booking step render | `step_number`, `step_name`, `clinic_location`, `specialty`, `page_path` | Funnel Exploration step visibility; remarketing audience for users who started booking but did not complete |
| `booking_step_complete` | Custom Event trigger from `dataLayer` when each booking step is successfully completed | `step_number`, `step_name`, `clinic_location`, `specialty`, `preferred_date` | Funnel Exploration completion path; drop-off analysis by step |
| `booking_form_error` | Custom Event trigger from front-end validation failure | `step_number`, `field_name`, `error_type`, `clinic_location`, `page_path` | DebugView and custom report for form friction diagnosis |
| `booking_submit` | Custom Event trigger from successful final booking confirmation | `clinic_location`, `specialty`, `preferred_date`, `lead_type`, `form_id` | Key event / conversion reporting; Google Ads conversion import candidate |
| `call_now_click` | Just Links or Click trigger on tel: links | `click_text`, `clinic_location`, `page_path`, `placement` | Engagement report and audience of high-intent call clickers |
| `whatsapp_click` | Just Links trigger on `wa.me` / WhatsApp widget links | `page_path`, `placement`, `device_category`, `destination_url` | Traffic source quality analysis and assisted-conversion audience |
| `patient_guide_lead_submit` | Form Submission or custom `dataLayer` event on gated form submit | `content_name`, `phone`, `page_path`, `lead_type` | Lead generation report; downloadable-content audience |
| `patient_guide_download` | Just Links trigger on PDF download URL after gate success | `file_name`, `file_extension`, `page_path`, `content_name` | File download report and mid-funnel content engagement audience |
| `clinic_location_view` | Page View trigger with page path match for the 9 clinic pages | `clinic_location`, `city`, `page_path`, `page_title` | Landing page report and geo-interest audience building |
| `blog_article_view` | Page View trigger on blog templates | `article_title`, `article_category`, `author_name`, `page_path` | Content report and content-consumer audience |
| `blog_scroll_depth` | Scroll Depth trigger at 25/50/75/90 percent on blog pages | `article_title`, `scroll_percent`, `page_path`, `content_group` | Engaged content analysis and retargeting audience for high-intent readers |
| `consultation_lp_view` | Page View trigger on paid landing page | `page_path`, `campaign_name`, `device_category`, `source_medium` | Landing page performance breakdown |
| `consultation_cta_click` | Click trigger on hero / sticky CTA buttons | `cta_text`, `cta_position`, `page_path`, `device_category` | CTA CTR analysis and audience of form-intent users |
| `consultation_form_start` | Focus trigger on first form field or custom front-end event | `form_id`, `page_path`, `device_category`, `campaign_name` | Form-start vs submit rate reporting |
| `consultation_form_submitted` | Custom Event trigger from landing page `dataLayer.push()` on valid submit | `form_id`, `page_variant`, `lead_type`, `page_path`, `device_category` | GA4 key event; Google Ads conversion import; audience for submitted leads exclusion |
| `thank_you_state_view` | Custom Event trigger when thank-you state is rendered | `form_id`, `page_variant`, `page_path`, `submission_status` | Confirmation of successful UX state and QA validation |

## Booking funnel drop-off tracking

GTM cannot reliably infer multi-step form progress by itself. The front-end developer must push structured `dataLayer` events at the exact moment a user sees or completes each step. GTM then listens using **Custom Event triggers** such as `booking_step_view` and `booking_step_complete`.

### GTM triggers by step

| Step | User action | GTM trigger | Purpose |
|---|---|---|---|
| Step 1 | User lands on/selects clinic + specialty step | Custom Event: `booking_step_view` where `step_number = 1` | Measures step exposure |
| Step 1 complete | User successfully selects location and specialty, then continues | Custom Event: `booking_step_complete` where `step_number = 1` | Measures progression to step 2 |
| Step 2 | Contact details step is shown | Custom Event: `booking_step_view` where `step_number = 2` | Measures users reaching details stage |
| Step 2 complete | User enters name, phone, preferred date and continues | Custom Event: `booking_step_complete` where `step_number = 2` | Measures progression to confirmation |
| Step 3 | Confirmation step is shown | Custom Event: `booking_step_view` where `step_number = 3` | Measures users reaching final step |
| Step 3 complete | User confirms booking successfully | Custom Event: `booking_step_complete` where `step_number = 3` plus final `booking_submit` | Measures final conversion |

### Actual dataLayer JSON

#### Step 1 viewed
```json
{
  "event": "booking_step_view",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "page_path": "/book-appointment"
}
```

#### Step 1 completed
```json
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "page_path": "/book-appointment"
}
```

#### Step 2 viewed
```json
{
  "event": "booking_step_view",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "page_path": "/book-appointment"
}
```

#### Step 2 completed
```json
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "patient_details_entered",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "preferred_date": "2026-07-08",
  "page_path": "/book-appointment"
}
```

#### Step 3 viewed
```json
{
  "event": "booking_step_view",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "preferred_date": "2026-07-08",
  "page_path": "/book-appointment"
}
```

#### Step 3 completed / final submit
```json
{
  "event": "booking_step_complete",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "preferred_date": "2026-07-08",
  "booking_status": "confirmed",
  "page_path": "/book-appointment"
}
```

#### Booking submit event
```json
{
  "event": "booking_submit",
  "clinic_location": "Indiranagar",
  "specialty": "Knee Pain",
  "preferred_date": "2026-07-08",
  "lead_type": "appointment_booking",
  "form_id": "book_appointment_main"
}
```

## GA4 funnel exploration setup

Create a Funnel Exploration with these ordered steps:
1. `booking_step_view` where `step_number = 1`
2. `booking_step_complete` where `step_number = 1`
3. `booking_step_view` where `step_number = 2`
4. `booking_step_complete` where `step_number = 2`
5. `booking_step_view` where `step_number = 3`
6. `booking_submit`

This setup surfaces:
- Users who saw step 1 but never completed it
- Users who completed step 1 but dropped before completing step 2
- Users who reached confirmation but did not submit
- Drop-off by clinic, specialty, device, and traffic source using breakdowns

## Google Ads conversion to import

**Import `consultation_form_submitted` into Google Ads.**

Why this one over the others:
- It is the clearest paid-media success event for the campaign landing page.
- It happens after the user has intentionally submitted lead details, so it is stronger than CTA clicks, scrolls, or WhatsApp opens.
- It matches the optimization goal for lead generation campaigns and can be used as the primary bidding signal.
- It avoids mixing appointment-booking flows from the main site with paid landing-page lead generation performance.

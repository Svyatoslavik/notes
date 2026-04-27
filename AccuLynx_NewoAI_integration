# AccuLynx Integration

Connects AccuLynx with the Newo AI agent so callers can book roofing jobs entirely by voice or chat — no dispatcher required.

---

## What the AI can do

- **Check availability.**  
  The agent provides real available time slots based on crew schedules.

- **Book the appointment.**  
  Creates a new job/appointment in AccuLynx once the customer confirms.

- **Request appointment cancellation.**  
  If the customer wants to cancel, the agent collects the request and forwards it to the team. The actual cancellation is handled manually.

- **Capture job details.**  
  Roof type, damage description, insurance info, urgency — all saved into job notes.

---

## Customer records in AccuLynx

The agent manages customer data automatically:

| Action | When it happens |
|------|----------------|
| **Find customer** | On inbound call using phone |
| **Create customer** | If no match is found |

---

## Features at a glance

| Feature | Included |
|--------|---------|
| Check availability | ✅ |
| Book appointment | ✅ |
| Cancel appointment | ❌ |
| Request cancellation | ✅ |
| Capture job notes | ✅ |

---

## Scenarios

One main scenarios are included and editable.

---

### Scenario — Scheduling Appointment in AccuLynx via Agent

**Flow:**

1. Ask what service is needed (roof inspection, repair, estimate, etc.)
2. Ask for details (damage, leak, storm, insurance claim)
3. Ask for ZIP code
4. **Check availability**  
   _"Let me check available time slots for you. Please give me a moment."_
5. Offer available slots
6. Customer selects a slot → confirm
7. Collect:
   - Name  
   - Phone  
   - Email  
   - Address  
8. Confirm booking  
   _"I'm booking your appointment now."_

---

## Example dialogs

### Example 1 — Booking (new customer)

Customer: Hi, I need a roof inspection  
Agent: Sure, I can help with that. What seems to be the issue?  
Customer: Possible storm damage  
Agent: Got it. What’s your ZIP code?  
Customer: 30301  
Agent: Let me check available time slots for you...

(Agent retrieves slots)
Agent: Yes 30301 in our service area
Agent: what prefer date and time would you mind to do inspection?  
Customer: Tuesday at 10  
Agent: Great. Can I have your name?

...

Agent: I'm booking your appointment now. Please give me a moment.  
Agent: Your appointment has been successfully scheduled!

---


## Setup

### 1. Get your AccuLynx API credentials

- Log into AccuLynx
- Go to Settings → API / Integrations
- Copy your API key

---

### 2. Configure in Newo

- Open **Module - AccuLynx**
- Paste API key into `acculynx_api_key`

Optional settings:

| Setting | Purpose | Default |
|--------|--------|--------|
| Availability Feature | Enable slot lookup | on |
| Booking Feature | Enable booking | on |

---

### 3. Setup how to select needed crew
Paste into `acculynx_employee_selection_prompt`

```
Select one or more employees based on the zip codes. 
There used next format:
Person name: ["zipCode", "zipCode2", ...]
Zip Codes:
   * Kirk Kupsky:  ["62018", "62025", "62034"]
   * Kyle Naes AND Andrew Green: ["62254", "62255", "62264"] 
   * Sviatoslav Mysachenko: if zip code: between 630xx and 658xx

In other cases select multipe person: Kirk Kupsky, Kyle Naes, Andrew Green, Christopher Conaway
Return employee_ids ordered by priority. If there are no suitable employees, return an empty list.
```

Another example:

```
Select employees who has one from next roles: Sales, Manager, LocationAdmin

```
Another one:
```
You must select all available employees
```

### 3. Prepare crews in AccuLynx

- Ensure crews are:
  - Active
  - Available for scheduling

---

### 4. Publish

The system will:

- Create connector
- Register tools (availability, booking)
- Deploy scenarios
- Sync crews and job types

---

## Customizing the agent

You can modify scenarios:

- Add more questions (insurance claim, urgency)
- Change slot presentation
- Adjust tone and flow

All collected information is saved in **job notes**.

---

## FAQ

**Q: Can the agent cancel appointments automatically?**  
No. AccuLynx does not support appointment cancellation via API. The agent only collects and forwards the request.

**Q: What happens if a time slot becomes unavailable?**  
The agent will notify the client and offer another time.

---

## Limitations

- **Cancellation** - is not yet supported (AccuLynx API limitation). Workaround: (manual handling required)
- **Token auto-refresh** — AccuLynx uses a static API key. If the key changes, update `acculynx_api_key` and republish.

---

## Key settings

| Attribute | Purpose | Default |
|----------|--------|--------|
| `acculynx_api_key` | API authentication | (empty)
| `acculynx_enable_availability_check` | Master on/off for live slot lookup. | On
| `acculynx_enable_booking` | Booking toggle | On

## Availability & booking tuning

| Attribute | Purpose | Default |
|----------|--------|--------|
| `acculynx_booking_duration` | Appointment duration | 120
| `acculynx_show_for_days` | How many days ahead of the requested date the agent will search for slots. | 7
| `aculynx_availability_offset` | Minimum lead time from today before slots are shown. Use 2 to prevent same-day bookings, 0 to allow them. | 2 
| `acculynx_workday_start_time` | This time need for auto generate available time slots | 09:00:00
| `acculynx_workday_end_time` | This time need for auto generate available time slots | 17:00:00
| `acculynx_skip_weekend` | Once the system generates available time slots, this option will allow you to skip Saturday and Sunday. | True
| `acculynx_test_mode` | All new contacts will contain [TEST] prefix before name | Off
| `acculynx_base_url` | Change only if AccuLynx support gave you a non-production URL. | https://api.acculynx.com/api/v2

## Static-data cache (managed automatically — read-only for dispatchers)
| Attribute | What's cached |
|----------|-------------------|
| `acculynx_contact_types` | Contact types, this ID-s need to create new contact in system
| `acculynx_company_employees` | Full employee list with their roles
| `acculynx_trade_types` | Trade-types list used by the AI for matching.
| `acculynx_active_lead_sources` | Lead sources list used by the AI for matching.
| `acculynx_job_categories` | Job categories list used by the AI for matching.
| `acculynx_work_types` | Work types list used by the AI for matching.
| `acculynx_static_data_updated_at` | Timestamp of the last successful refresh.
| `acculynx_static_data_update_interval` | How often (seconds) to refresh the cache. Default 24 h. Lower if employees/job types change frequently.

## System-only (do not edit)
| Attribute | Purpose |
|----------|-------------------|
| `hcp_setup_persona_id` | Internal persona used to send setup and static-data refresh events. Created automatically on first publish.
| `acculynx_connector` | Connector name the integration uses to talk to AccuLynx HTTP.
---


## Notes

- Phone number is the primary customer identifier  
- All conversation details are saved into job notes  
- Works in both voice and chat channels  

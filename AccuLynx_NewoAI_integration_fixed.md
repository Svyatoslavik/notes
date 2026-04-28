# AccuLynx Integration

Connects AccuLynx with the Newo AI agent so callers can book roofing jobs entirely by voice or chat — no dispatcher required.

---

## What the AI can do

- **Check availability.**  
  The agent provides real available time slots based on crew schedules.

- **Book an appointment.**  
  Creates a new job/appointment in AccuLynx once the customer confirms.

- **Request appointment cancellation.**  
  If the customer wants to cancel, the agent collects the request and forwards it to the team. The actual cancellation is handled manually.

- **Capture job details.**  
  Roof type, damage description, insurance info, urgency — all saved in job notes.

---

## Customer records in AccuLynx

The agent manages customer data automatically:

| Action | When it happens |
|------|----------------|
| **Find customer** | On inbound call using phone number |
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

One main scenario is included and can be edited.

---

### Scenario — Scheduling an Appointment in AccuLynx via Agent

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
Agent: Yes, 30301 is in our service area.  
Agent: What date and time would you prefer for the inspection?  
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
- Paste the API key into `acculynx_api_key`

Optional settings:

| Setting | Purpose | Default |
|--------|--------|--------|
| Availability Feature | Enable slot lookup | On |
| Booking Feature | Enable booking | On |

---

### 3. Set up crew selection logic

Paste into `acculynx_employee_selection_prompt`

```
Select one or more employees based on ZIP codes. 
Use the following format:
Person name: ["zipCode", "zipCode2", ...]

ZIP Codes:
   * Aphex Twin: ["62018", "62025", "62034"]
   * Will Smith AND Jim Carrey: ["62254", "62255", "62264"] 
   * Alex G: if ZIP code is between 630xx and 658xx

In other cases, select multiple people: Will Smith, Jim Carrey, Alex G.

Return employee_ids ordered by priority. If there are no suitable employees, return an empty list.
```

Another example:

```
Select employees who have one of the following roles: Sales, Manager, LocationAdmin
```

Another example:

```
You must select all available employees
```

---

### 4. Prepare crews in AccuLynx

Ensure crews are:
- Active
- Available for scheduling

---

### 5. Publish

The system will:

- Create a connector
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
The agent will notify the customer and offer another time.

---

## Limitations

- **Cancellation** — not supported (AccuLynx API limitation). Workaround: manual handling required  
- **Token auto-refresh** — AccuLynx uses a static API key. If the key changes, update `acculynx_api_key` and republish.

---

## Key settings

| Attribute | Purpose | Default |
|----------|--------|--------|
| `acculynx_api_key` | API authentication | (empty) |
| `acculynx_enable_availability_check` | Master on/off for live slot lookup | On |
| `acculynx_enable_booking` | Booking toggle | On |

---

## Availability & booking tuning

| Attribute | Purpose | Default |
|----------|--------|--------|
| `acculynx_booking_duration` | Appointment duration | 120 |
| `acculynx_show_for_days` | How many days ahead the agent searches for slots | 7 |
| `acculynx_availability_offset` | Minimum lead time before showing slots (e.g., 2 prevents same-day bookings) | 2 |
| `acculynx_workday_start_time` | Start of workday for slot generation | 09:00:00 |
| `acculynx_workday_end_time` | End of workday for slot generation | 17:00:00 |
| `acculynx_skip_weekend` | Skip Saturday and Sunday when generating slots | True |
| `acculynx_test_mode` | Adds `[TEST]` prefix to new contacts | Off |
| `acculynx_base_url` | Change only if provided a non-production URL | https://api.acculynx.com/api/v2 |

---

## Static-data cache (managed automatically — read-only)

| Attribute | Description |
|----------|-------------|
| `acculynx_contact_types` | Contact types required to create contacts |
| `acculynx_company_employees` | Full employee list with roles |
| `acculynx_trade_types` | Trade types used for matching |
| `acculynx_active_lead_sources` | Lead sources used for matching |
| `acculynx_job_categories` | Job categories used for matching |
| `acculynx_work_types` | Work types used for matching |
| `acculynx_static_data_updated_at` | Last successful refresh timestamp |
| `acculynx_static_data_update_interval` | Cache refresh interval (seconds). Default: 24h |

---

## System-only (do not edit)

| Attribute | Purpose |
|----------|---------|
| `hcp_setup_persona_id` | Internal persona for setup and refresh events |
| `acculynx_connector` | Connector used for API communication |

---

## Notes

- Phone number is the primary customer identifier  
- All conversation details are saved in job notes  
- Works in both voice and chat channels  

---

## AccuLynx API Integration

| | |
|------------|------------------------|
| API Docs | https://apidocs.acculynx.com/reference/getcalendars |

---

### Flow: Collect static data

**Trade Types**
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/job-file-settings/trade-types |

**Work Types**
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/job-file-settings/work-types |

**Lead Sources**
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/leads/lead-sources |

**Employees**
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/users |

**Job Categories**
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/job-file-settings/job-categories |

**Contact Types**
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/contacts/contact-types |

---

### Flow: New appointment booking

#### Step 1 → Identify the caller

The agent uses the caller's phone number to identify the customer.

Since phone search does not work via `/contacts/search`, use job search:

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs/search |

Payload:
```
{
  "geoLocation": {
    "mapRadius": 1
  },
  "searchTerm": "{phone_number}"
}
```

If a contact is found → proceed to **Create New Job**  
Otherwise → proceed to **Create New Contact**

---

#### Step 2 → Create New Contact

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/contacts |

Payload:
```
{
    "contactTypeIds": ["{contactTypeId}", ...],
    "firstName": "{first_name}",
    "lastName": "{last_name}",
    "phoneNumbers": [{
        "number": "{mobile_number}",
        "primary": true,
        "type": "Mobile"
    }],
    "emailAddresses": [{
        "address": "{email}",
        "primary": true,
        "type": "Other"
    }]
}
```

---

#### Step 3 → Create New Job

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs |

---

#### Step 4 → Assign employee

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs/{jobId}/representatives/company |

---

#### Step 5 → Create initial appointment

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs/{jobId}/initial-appointment |

---

### Flow: Check Availability

The LLM selects employees based on work type or location.

| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/calendars/{calendarId}/appointments |

Query params: `startDate`, `endDate`

---

### Flow: Cancellation

The agent collects the request and forwards it to the team.  
No API calls are required.

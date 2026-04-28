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
   * Aphex Twin:  ["62018", "62025", "62034"]
   * Will Smith AND Jim Carrey: ["62254", "62255", "62264"] 
   * Alex G: if zip code: between 630xx and 658xx

In other cases select multipe person: Will Smith, Jim Carrey, Alex G
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

## AccuLynx API Integration

| | |
|------------|------------------------|
| API Docs | https://apidocs.acculynx.com/reference/getcalendars|

### Flow: Collect static data

Collect Trade Types
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/job-file-settings/trade-types |

Collect Work Types

| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/job-file-settings/work-types |

Get Active Lead Sources for a company.

| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/leads/lead-sources |

Get company employees
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/users |

Get Job Categories of the company
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/company-settings/job-file-settings/job-categories |

Get the contact types
| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/contacts/contact-types |

### Flow: New appointment booking

#### Step 1 → Identify the caller 
Since the agent collects the caller's phone number we should try to identify customer
Since the phone number search doesn't work in the endpoint https://api.acculynx.com/api/v2/contacts/search, we use a different endpoint for job search.

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs/search |

with next body
```
{
  "geoLocation": {
    "mapRadius": 1
  },
  "searchTerm": "{phone_number}"
}
```

Then check result, in case when contact found - we moving to step **Create New Job** otherwise go to **Create New Contact**

#### Step 2 → Create New Contact
If the contact was not found in the previous step, do next:
| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/contacts |

With next payload:
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
    }],
}
```


#### Step 3 → Create New Job
If we know the contact id, we can create a new Lead via create new Job api call

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs |

With next payload (example of real data):
```
{
    "contact": {
        "id": "fce207e9-22c2-4e25-b462-aa64fb4f1c6b",
    },
    "locationAddress": {
        "street1": "2200 292nd Street",
        "city": "Federal Way",
        "state": "WA",
        "country": "US",
        "zipCode": "98003"
    },
    "priority": "Normal",
    "notes": "The user is requesting a home inspection. The service type needed is a routine diagnostic/inspection. The core issue is to inspect the home.",
    "leadSource": {
        "id": "68567bd5-f87e-e811-9103-0cc47aa3a68a"
    },
    "workType": {
        "id": 6
    },
    "jobCategory": {
        "id": 1
    }
}
```
#### Step 4 → Assign an employee to this Job (LEAD).
If job was created succesfully in previous step then need to assign employee to this job via api call:

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs/{jobId}/representatives/company |

with next payload:
```
{
    "id": "{employee_id}"
}
```
#### Step 5 → Create Initial Appointment
When empolee was assigned succusfully next step is create initial appointemnt, to do this need to call next endpoint:

| Method | URL |
|-----|---------|
| POST | https://api.acculynx.com/api/v2/jobs/{jobId}/initial-appointment |

With next payload:
```
{
    "startDate": start_time,
    "endDate": end_time
}
```

### Flow: Check Availability
Based on static data, LLM should select employees based on work type or location, then need to get callendars for each employee (one employee per request)

To get an employee's schedule, you need to call the following endpoint:

| Method | URL |
|-----|---------|
| GET | https://api.acculynx.com/api/v2/calendars/{calendarId}/appointments |

With next query parameters: `startDate` and `endDate`

### Flow: Cancellation
If the customer wants to cancel, the agent collects the request and forwards it to the team. The actual cancellation is handled manually (AccuLynx API limitation).

No API calls are required for this flow.

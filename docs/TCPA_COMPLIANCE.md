# TCPA Compliance Guide

This document provides detailed guidance on TCPA (Telephone Consumer Protection Act) compliance when using Amazon Connect Outbound Campaigns.

## Overview

The TCPA regulates telemarketing calls, autodialed calls, prerecorded calls, text messages, and unsolicited faxes. Non-compliance can result in statutory damages of $500-$1,500 per violation.

## Key Requirements

### 1. Time Restrictions

**Rule:** Telemarketing calls are prohibited before 8:00 AM and after 9:00 PM in the **recipient's local time zone**.

**Implementation:**

```yaml
CommunicationTimeConfig:
  LocalTimeZoneConfig:
    DefaultTimeZone: America/New_York  # Fallback timezone
    LocalTimeZoneDetection:
      - ZIP_CODE      # Primary: Determine from customer ZIP code
      - AREA_CODE     # Secondary: Determine from phone area code
  Telephony:
    OpenHours:
      DailyHours:
        MONDAY:
          - StartTime: "08:00"
            EndTime: "21:00"
        # ... repeat for all days
```

**Best Practice:** Add a buffer (e.g., 8:05 AM - 8:55 PM) to account for edge cases.

### 2. Do-Not-Call Compliance

**Rule:** Maintain and honor a company-specific DNC list. Honor the National Do-Not-Call Registry.

**Implementation Options:**

1. **Customer Profiles Attribute:**
   ```sql
   SELECT * FROM CustomerProfiles
   WHERE opt_in_status = 'OPTED_IN'
     AND dnc_flag != true
   ```

2. **External DNC API via Lambda:**
   ```python
   def check_dnc(phone_number):
       # Check internal DNC list
       # Check National DNC Registry
       return is_on_dnc_list
   ```

3. **Segment Exclusion:**
   - Create a segment excluding DNC customers
   - Use as campaign source

### 3. Consent Requirements

**Rule:** Prior express consent required for autodialed/prerecorded calls. Written consent required for telemarketing.

**Implementation:**

```yaml
# Track consent in Customer Profiles
CustomerProfile:
  opt_in_status: OPTED_IN | OPTED_OUT
  opt_in_date: "2026-01-15"
  opt_in_method: "web_form" | "verbal" | "sms"
  consent_type: "express" | "express_written"
```

### 4. Caller ID Requirements

**Rule:** Display a valid caller ID number that reaches the calling party.

**Implementation:**

```yaml
DefaultOutboundConfig:
  ConnectSourcePhoneNumber: "+12125551234"  # Must be a real, callable number
```

### 5. Abandoned Call Rate

**Rule:** For predictive dialers, abandoned call rate must not exceed 3% per campaign per day.

**Implementation:**

```yaml
# Use progressive dialer for strict compliance
DialerMode: PROGRESSIVE  # 1:1 ratio, no abandonment

# Or monitor predictive carefully
DialerMode: PREDICTIVE
Capacity: 0.5  # Start conservative
```

### 6. Message Requirements

**Required disclosures for prerecorded messages:**
- Identify the caller
- Provide callback number
- For debt collection: Mini-Miranda disclosure

**Voicemail Script Example:**

```
"This is [Company Name] calling regarding an important matter. 
Please call us back at [callback number]. Our hours are 
Monday through Friday, 9am to 6pm Eastern. Thank you."
```

## Exemptions

Certain calls are exempt from TCPA restrictions:

| Exemption | Description |
|-----------|-------------|
| Emergency | Calls made for emergency purposes |
| Non-commercial | Calls for non-commercial purposes |
| Tax-exempt | Calls by tax-exempt nonprofit organizations |
| Healthcare | Certain healthcare-related calls |
| Established business | Calls to existing customers (with limits) |

**Implementation:**

```yaml
# For exempt communications (use sparingly)
CommunicationLimitsOverride:
  AllChannelSubtypes:
    CommunicationLimitsList: []  # No limits
```

## State-Specific Requirements

Many states have additional telemarketing regulations. Consider:

| State | Additional Requirement |
|-------|----------------------|
| California | Written consent for certain automated calls |
| Florida | Stricter time restrictions (9am-8pm) |
| New York | State DNC registration required |
| Texas | $1,000 fine per violation |

## Monitoring and Auditing

### Required Records

Maintain records for at least 24 months:
- Consent records
- Call logs
- DNC requests
- Opt-out processing

### Compliance Dashboard

Use Contact Lens to monitor:
- Call timing compliance
- DNC violations
- Consent issues
- Abandon rate

## Penalties

| Violation Type | Penalty |
|---------------|---------|
| Standard | $500 per violation |
| Willful/knowing | $1,500 per violation |
| Class action | No cap on damages |

## Resources

- [FCC TCPA Rules](https://www.fcc.gov/general/telemarketing-and-robocalls)
- [National DNC Registry](https://www.donotcall.gov/)
- [Amazon Connect Best Practices](https://docs.aws.amazon.com/connect/latest/adminguide/outbound-campaign-best-practices.html)

---

**Disclaimer:** This guide is for informational purposes only and does not constitute legal advice. Consult with legal counsel for compliance requirements specific to your situation.

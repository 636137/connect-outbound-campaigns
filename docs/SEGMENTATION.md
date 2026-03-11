# Customer Segmentation Guide

This guide covers how to build intelligent customer segments for outbound campaigns using Amazon Connect Customer Profiles.

## Overview

Customer Profiles provides three segmentation methods:

1. **Audience Groups** - UI-based filtering (simple)
2. **Spark SQL** - Advanced SQL queries (powerful)
3. **Segment AI Assistant** - Natural language (intuitive)

## Segmentation Methods

### Method 1: Audience Groups (Basic)

Create segments using AND/OR filter logic:

```
Group 1 (AND):
  - lifetime_value > 10000
  - days_since_last_order > 60
  - opt_in_status = 'OPTED_IN'
```

**Use When:** Simple, static criteria

### Method 2: Spark SQL (Advanced)

Write SQL queries for complex logic:

```sql
SELECT *
FROM CustomerProfiles
WHERE lifetime_value > 10000
  AND days_since_last_order > 60
  AND opt_in_status = 'OPTED_IN'
  AND NOT EXISTS (
    SELECT 1 FROM SupportTickets
    WHERE status = 'OPEN'
  )
```

**Use When:** Complex joins, statistical functions, dynamic criteria

### Method 3: Natural Language (AI Assistant)

Describe your segment in plain English:

```
"High-value customers who haven't purchased in 60 days 
and don't have any open support tickets"
```

The AI translates this to Spark SQL for you.

**Use When:** Business users, rapid prototyping

## Available Attributes

### Standard Profile Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| `profile_id` | String | Unique customer ID |
| `first_name` | String | Customer first name |
| `last_name` | String | Customer last name |
| `email_address` | String | Email address |
| `phone_number` | String | Phone (E.164) |
| `address` | String | Street address |
| `city` | String | City |
| `state` | String | State/Province |
| `postal_code` | String | ZIP/Postal code |
| `country` | String | Country |
| `birth_date` | Date | Date of birth |
| `account_number` | String | Account ID |

### Calculated Attributes

Define computed metrics that update automatically:

| Attribute | Type | Example Definition |
|-----------|------|-------------------|
| `lifetime_value` | Decimal | SUM(order_amount) |
| `total_orders` | Integer | COUNT(orders) |
| `average_order_value` | Decimal | AVG(order_amount) |
| `days_since_last_order` | Integer | DAYS_SINCE(MAX(order_date)) |
| `support_contact_count` | Integer | COUNT(support_tickets) |
| `engagement_score` | Decimal | Custom formula |
| `churn_probability` | Decimal | ML-predicted (0-1) |
| `preferred_channel` | String | MODE(contact_channel) |

### Creating Calculated Attributes

```python
import boto3

client = boto3.client('customer-profiles')

response = client.create_calculated_attribute_definition(
    DomainName='my-domain',
    CalculatedAttributeName='lifetime_value',
    DisplayName='Customer Lifetime Value',
    Description='Total spend across all orders',
    AttributeDetails={
        'Attributes': [
            {'Name': 'Orders.amount'}
        ],
        'Expression': 'SUM(Orders.amount)'
    },
    Statistic='SUM',
    Conditions={
        'Range': {
            'Value': 365,
            'Unit': 'DAYS'
        }
    }
)
```

## Common Segment Patterns

### High-Value Customers

```sql
SELECT *
FROM CustomerProfiles
WHERE lifetime_value > PERCENTILE(lifetime_value, 0.9)
  AND opt_in_status = 'OPTED_IN'
```

### Dormant Customers (Reactivation)

```sql
SELECT *
FROM CustomerProfiles
WHERE days_since_last_order > 60
  AND days_since_last_order < 180
  AND lifetime_value > 1000
  AND opt_in_status = 'OPTED_IN'
```

### Churn Risk

```sql
SELECT *
FROM CustomerProfiles
WHERE churn_probability > 0.7
  AND lifetime_value > 5000
  AND opt_in_status = 'OPTED_IN'
```

### Collections Target

```sql
SELECT *
FROM CustomerProfiles
WHERE balance_due > 0
  AND days_overdue > 30
  AND payment_history_score > 0.5  -- Likely to pay
  AND opt_in_status = 'OPTED_IN'
  AND contact_attempts < 3
```

### Geographic Targeting

```sql
SELECT *
FROM CustomerProfiles
WHERE state IN ('CA', 'TX', 'FL')
  AND opt_in_status = 'OPTED_IN'
```

### Channel Preference

```sql
SELECT *
FROM CustomerProfiles
WHERE preferred_channel = 'VOICE'
  AND last_contact_date < CURRENT_DATE - INTERVAL 7 DAYS
  AND opt_in_status = 'OPTED_IN'
```

## Segment Best Practices

### 1. Always Include Opt-In Filter

```sql
WHERE opt_in_status = 'OPTED_IN'
```

### 2. Exclude Recent Contacts

```sql
AND last_contact_date < CURRENT_DATE - INTERVAL 3 DAYS
```

### 3. Consider Contact Attempts

```sql
AND contact_attempts < max_attempts_setting
```

### 4. Validate Phone Numbers

```sql
AND phone_number IS NOT NULL
AND phone_number LIKE '+1%'
```

### 5. Test Segment Size

Before launching a campaign, verify segment size is appropriate:
- Too small: May not justify campaign setup
- Too large: May overwhelm agents/resources

## Integration with Campaigns

### Using Segment as Campaign Source

```yaml
Source:
  CustomerProfilesSegmentArn: arn:aws:profile:us-east-1:123456:domains/my-domain/segments/high-value-dormant
```

### Dynamic Segment Refresh

Segments based on calculated attributes update automatically as data changes. Configure refresh interval:

```yaml
SegmentRefresh:
  Frequency: DAILY
  Time: "02:00"  # Off-peak hours
```

## Monitoring Segments

Track segment performance:

| Metric | Description |
|--------|-------------|
| Segment Size | Number of customers |
| Growth Rate | Daily/weekly change |
| Contact Rate | % successfully contacted |
| Conversion Rate | % achieving campaign goal |

## Resources

- [Customer Profiles Documentation](https://docs.aws.amazon.com/connect/latest/adminguide/customer-profiles.html)
- [Calculated Attributes API](https://docs.aws.amazon.com/connect/latest/APIReference/API_connect-customer-profiles_CreateCalculatedAttributeDefinition.html)
- [Segment API](https://docs.aws.amazon.com/connect/latest/APIReference/API_Operations_Amazon_Connect_Customer_Profiles.html)

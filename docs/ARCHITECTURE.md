# System Architecture

This document describes the architecture of the Amazon Connect Smart Campaign Agent and how it integrates with AWS services.

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              USER INTERFACE LAYER                                    │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│   ┌──────────────────┐    ┌──────────────────┐    ┌──────────────────┐            │
│   │  GitHub Copilot  │    │   Python CLI     │    │  CloudFormation  │            │
│   │    CLI Agent     │    │   (boto3)        │    │   Templates      │            │
│   └────────┬─────────┘    └────────┬─────────┘    └────────┬─────────┘            │
│            │                       │                        │                       │
└────────────┼───────────────────────┼────────────────────────┼───────────────────────┘
             │                       │                        │
             ▼                       ▼                        ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              ORCHESTRATION LAYER                                     │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│                      ┌───────────────────────────────────┐                          │
│                      │    smart_campaign_agent.py        │                          │
│                      │                                   │                          │
│                      │  • discover_instances()           │                          │
│                      │  • create_segment()               │                          │
│                      │  • create_campaign()              │                          │
│                      │  • generate_script()              │                          │
│                      │  • deploy_campaign()              │                          │
│                      │  • get_analytics()                │                          │
│                      │  • optimize_campaign()            │                          │
│                      └───────────────┬───────────────────┘                          │
│                                      │                                              │
└──────────────────────────────────────┼──────────────────────────────────────────────┘
                                       │
                                       ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              AWS SERVICE LAYER                                       │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                      │
│   ┌────────────────┐  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐   │
│   │   Amazon       │  │   Outbound     │  │   Customer     │  │   Amazon       │   │
│   │   Connect      │  │  Campaigns V2  │  │   Profiles     │  │   Bedrock      │   │
│   │                │  │                │  │                │  │                │   │
│   │ • Instances    │  │ • Campaigns    │  │ • Segments     │  │ • Claude       │   │
│   │ • Flows        │  │ • Lifecycle    │  │ • Attributes   │  │ • Titan        │   │
│   │ • Queues       │  │ • DialBatch    │  │ • Profiles     │  │ • Llama        │   │
│   │ • Agents       │  │ • Compliance   │  │ • Events       │  │                │   │
│   └────────┬───────┘  └────────┬───────┘  └────────┬───────┘  └────────┬───────┘   │
│            │                   │                   │                   │            │
│            └───────────────────┼───────────────────┼───────────────────┘            │
│                                │                   │                                 │
│   ┌────────────────┐  ┌────────┴───────┐  ┌───────┴────────┐  ┌────────────────┐   │
│   │  Contact Lens  │  │  CloudWatch    │  │ CloudFormation │  │     S3         │   │
│   │                │  │                │  │                │  │                │   │
│   │ • Sentiment    │  │ • Metrics      │  │ • Stacks       │  │ • Templates    │   │
│   │ • Transcripts  │  │ • Alarms       │  │ • Resources    │  │ • Logs         │   │
│   │ • Categories   │  │ • Dashboards   │  │                │  │                │   │
│   └────────────────┘  └────────────────┘  └────────────────┘  └────────────────┘   │
│                                                                                      │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## Component Details

### 1. GitHub Copilot CLI Agent

The interactive AI agent provides natural language interface:

```
User: "Create a collections campaign with preview dialer"
        ↓
Agent: Parses intent, asks clarifying questions
        ↓
Agent: Calls smart_campaign_agent.py functions
        ↓
Agent: Returns formatted results and next steps
```

### 2. Python CLI (smart_campaign_agent.py)

Modular orchestration layer:

```python
class AWSClients:
    """Centralized client management"""
    @property
    def connect(self): ...
    @property
    def campaigns(self): ...
    @property
    def profiles(self): ...
    @property
    def bedrock(self): ...

# Discovery
def discover_instances(clients, region) -> List[Dict]
def list_campaigns(clients, instance_id) -> List[Dict]
def get_campaign(clients, campaign_id) -> Dict

# Segmentation
def list_segments(clients, domain_name) -> List[Dict]
def create_segment_from_sql(clients, ...) -> Dict
def create_calculated_attribute(clients, ...) -> Dict

# Campaign Management
def create_campaign(clients, ...) -> Dict
def start_campaign(clients, campaign_id) -> Dict
def pause_campaign(clients, campaign_id) -> Dict
def put_dial_request_batch(clients, ...) -> Dict

# Personalization
def generate_script_with_bedrock(clients, ...) -> str
def generate_customer_summary(clients, ...) -> str

# Analytics
def get_campaign_metrics(clients, ...) -> Dict
def get_contact_lens_analysis(clients, ...) -> Dict

# Optimization
def generate_optimization_recommendations(clients, ...) -> List[Dict]
```

### 3. AWS Services Integration

#### Amazon Connect

- Instance discovery and management
- Contact flow references
- Queue and agent routing

#### Outbound Campaigns V2

- Campaign CRUD operations
- Lifecycle management (start/pause/stop)
- Dial request batching
- Compliance configuration

#### Customer Profiles

- Segment definitions (Spark SQL)
- Calculated attributes
- Profile data retrieval
- Event streaming

#### Amazon Bedrock

- Script generation (Claude)
- Customer summaries
- Content personalization
- Natural language processing

#### Contact Lens

- Real-time sentiment analysis
- Conversation categorization
- Compliance monitoring
- Performance analytics

#### CloudFormation

- Infrastructure as Code deployment
- Stack management
- Resource provisioning

## Data Flow

### Campaign Creation Flow

```
1. User Request
   "Create collections campaign for overdue accounts"
         │
         ▼
2. Segment Definition
   Customer Profiles → Spark SQL → Segment ARN
         │
         ▼
3. Campaign Configuration
   Dialer mode, channels, compliance settings
         │
         ▼
4. Personalization (Optional)
   Bedrock → Scripts, summaries, templates
         │
         ▼
5. Deployment
   API call or CloudFormation
         │
         ▼
6. Campaign Created (STOPPED state)
```

### Campaign Execution Flow

```
1. Start Campaign
   STOPPED → RUNNING
         │
         ▼
2. Segment Processing
   Customer Profiles → Contact list
         │
         ▼
3. Dialer Algorithm
   Predictive/Progressive/Preview → Agent assignment
         │
         ▼
4. Call Placement
   Connect → Customer phone
         │
         ▼
5. Call Handling
   Contact flow → Agent or voicemail
         │
         ▼
6. Post-Call Processing
   Contact Lens → Analytics
   Customer Profiles → Update contact history
```

### Analytics Flow

```
1. Contact Completed
   Connect → Contact record
         │
         ├─────────────────────┐
         ▼                     ▼
2. Real-time Analysis     3. Historical Analysis
   Contact Lens              CloudWatch Metrics
   • Sentiment               • Call volume
   • Categories              • Connect rate
   • Compliance              • Handle time
         │                     │
         └──────────┬──────────┘
                    ▼
4. Optimization Recommendations
   AI analysis → Actionable insights
```

## Security Architecture

### Authentication

```
┌─────────────────┐
│   User/CLI      │
└────────┬────────┘
         │
         ▼ AWS Credentials
┌─────────────────┐
│  IAM Role/User  │
└────────┬────────┘
         │
         ▼ STS AssumeRole
┌─────────────────┐
│ Temporary Creds │
└────────┬────────┘
         │
         ▼ API Calls
┌─────────────────┐
│  AWS Services   │
└─────────────────┘
```

### Authorization (IAM Policy)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["connect-campaigns:*"],
      "Resource": "arn:aws:connect-campaigns:*:*:campaign/*"
    },
    {
      "Effect": "Allow",
      "Action": ["profile:*"],
      "Resource": "arn:aws:profile:*:*:domains/*"
    },
    {
      "Effect": "Allow",
      "Action": ["bedrock:InvokeModel"],
      "Resource": "arn:aws:bedrock:*:*:*"
    }
  ]
}
```

### Data Protection

| Data Type | Protection |
|-----------|------------|
| Customer PII | Encrypted at rest (KMS) |
| Call recordings | Encrypted, access-controlled |
| API credentials | AWS Secrets Manager |
| Segment data | VPC endpoint isolation |

## Scalability

### Horizontal Scaling

- Campaigns V2 API handles scaling automatically
- Customer Profiles supports millions of records
- Bedrock scales with request volume

### Rate Limits

| Service | Limit | Handling |
|---------|-------|----------|
| Campaigns API | 5 TPS | Exponential backoff |
| Customer Profiles | 10 TPS | Batch operations |
| Bedrock | Model-specific | Queue and retry |

## Monitoring

### CloudWatch Metrics

| Metric | Description |
|--------|-------------|
| `CampaignDialAttempts` | Total dial attempts |
| `CampaignContactsConnected` | Successful connections |
| `CampaignAbandonRate` | Abandoned call percentage |
| `AgentUtilization` | Agent busy percentage |

### Alarms

```yaml
LowConnectRateAlarm:
  Metric: ConnectRate
  Threshold: 25%
  Action: SNS notification

HighAbandonRateAlarm:
  Metric: AbandonRate
  Threshold: 3%
  Action: Pause campaign
```

## Disaster Recovery

### Backup

- CloudFormation templates stored in S3
- Campaign configurations exportable as JSON
- Customer Profiles data replicated

### Recovery

1. Deploy CloudFormation stack in DR region
2. Import campaign configuration
3. Verify Customer Profiles sync
4. Update DNS/routing

## References

- [Amazon Connect Architecture](https://docs.aws.amazon.com/connect/latest/adminguide/architecture.html)
- [Outbound Campaigns V2](https://docs.aws.amazon.com/connect/latest/APIReference/API_Operations_Amazon_Connect_Outbound_Campaigns_V2.html)
- [Customer Profiles](https://docs.aws.amazon.com/connect/latest/adminguide/customer-profiles.html)
- [Contact Lens](https://docs.aws.amazon.com/connect/latest/adminguide/contact-lens.html)

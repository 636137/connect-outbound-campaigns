## Proprietary Notice

This code is proprietary to **Maximus**. **No public license is granted**. See [`NOTICE`](./NOTICE).

---

# Amazon Connect Outbound Campaigns

## Repository Layout

- `frontend/`
- `backend/`
- `infra/`

## Local vs Deploy

- Local: see `docs/how-to.md`
- Deploy: see `docs/` and `infra/` (if present)


A comprehensive AI-powered toolkit for building and managing intelligent outbound campaigns in Amazon Connect. Features include AI-powered customer segmentation, multi-channel campaigns (voice, SMS, email, WhatsApp), TCPA compliance automation, Amazon Bedrock personalization, and Contact Lens analytics.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)
[![AWS](https://img.shields.io/badge/AWS-Amazon%20Connect-orange.svg)](https://aws.amazon.com/connect/)

---

## 🎯 Overview

This repository provides:

1. **Smart Campaign Agent** - Interactive AI agent for GitHub Copilot CLI
2. **Python SDK** - Programmatic campaign management
3. **CloudFormation Templates** - Infrastructure as Code for campaigns
4. **Example Scripts** - Ready-to-use campaign patterns

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      SMART CAMPAIGN AGENT                                        │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐  ┌──────────┐  ┌──────────┐         │
│  │ SEGMENT  │→ │  CREATE  │→ │PERSONALIZE│→ │  DEPLOY  │→ │ MONITOR  │         │
│  │          │  │          │  │           │  │          │  │          │         │
│  │ Build AI │  │ Campaign │  │ Bedrock   │  │ API/CFn  │  │ Contact  │         │
│  │ segments │  │  config  │  │  scripts  │  │  deploy  │  │  Lens    │         │
│  └──────────┘  └──────────┘  └───────────┘  └──────────┘  └──────────┘         │
│       │              │              │              │              │             │
│       ▼              ▼              ▼              ▼              ▼             │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐  ┌──────────┐  ┌──────────┐         │
│  │Customer  │  │Outbound  │  │  Amazon   │  │CloudForm │  │Analytics │         │
│  │Profiles  │  │Campaigns │  │  Bedrock  │  │ation/API │  │Dashboard │         │
│  │  API     │  │ V2 API   │  │   API     │  │          │  │          │         │
│  └──────────┘  └──────────┘  └───────────┘  └──────────┘  └──────────┘         │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

### 🤖 AI-Powered Segmentation
- **Natural Language** → Spark SQL translation
- **Calculated Attributes** for real-time customer metrics
- **Segment AI Assistant** integration
- **70+ data source connectors** via Customer Profiles

### 📞 Multi-Channel Campaigns
- **Voice** - Predictive, progressive, preview, agentless dialers
- **SMS** - Templated and personalized messages
- **Email** - Marketing and transactional campaigns
- **WhatsApp** - International messaging

### 🛡️ TCPA Compliance (Built-In)
- Timezone restrictions (8am-9pm local time)
- Communication limits (max contacts per day/week)
- Answering Machine Detection with voicemail
- Do-Not-Call list integration
- Consent tracking and opt-out management

### 🎨 Bedrock Personalization
- Dynamic call scripts based on customer data
- Pre-call customer summaries for preview dialer
- Personalized SMS/email templates
- AI-generated voicemail messages

### 📊 Contact Lens Analytics
- Real-time sentiment analysis
- Theme and category detection
- Agent performance tracking
- Compliance monitoring

---

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/636137/connect-outbound-campaigns.git
cd connect-outbound-campaigns

# Install the Copilot skill
cp -r copilot-skills/connect-campaign-agent ~/.copilot/skills/

# Verify installation
ls ~/.copilot/skills/connect-campaign-agent/
```

### Prerequisites

- **AWS Account** with Amazon Connect instance
- **Python 3.9+** with boto3
- **AWS CLI** configured with credentials
- **Outbound Campaigns** enabled on your Connect instance
- **Customer Profiles** domain (for segmentation)
- **Contact Lens** enabled (for analytics)
- **Amazon Bedrock** access (for personalization)

### First Campaign in 5 Minutes

```bash
# 1. Discover your Connect instances
python3 copilot-skills/connect-campaign-agent/scripts/smart_campaign_agent.py discover --region us-east-1

# 2. Create a campaign
python3 copilot-skills/connect-campaign-agent/examples/create_simple_campaign.py \
  --instance-id YOUR_INSTANCE_ID \
  --contact-flow-id YOUR_FLOW_ID \
  --queue-id YOUR_QUEUE_ID \
  --phone-number +12125551234 \
  --name "My First Campaign"

# 3. Start the campaign
aws connectcampaignsv2 start-campaign --id CAMPAIGN_ID
```

---

## 📖 Documentation

### Agent Capabilities

| Module | Description | Key Features |
|--------|-------------|--------------|
| **DISCOVER** | Find resources | List instances, campaigns, segments |
| **SEGMENT** | Build segments | Natural language, Spark SQL, calculated attributes |
| **CREATE** | Configure campaigns | 4 dialer modes, multi-channel, compliance |
| **PERSONALIZE** | Generate content | Bedrock scripts, summaries, SMS templates |
| **DEPLOY** | Launch campaigns | API, CloudFormation, lifecycle management |
| **MONITOR** | Track performance | Contact Lens, real-time metrics, sentiment |
| **OPTIMIZE** | AI recommendations | Timing, scripts, segments, agents |

### Dialer Modes

| Mode | Description | Best For | AMD Support |
|------|-------------|----------|-------------|
| **Predictive** | ML-powered pacing, dials ahead of agents | High-volume sales, collections | ✅ |
| **Progressive** | 1:1 agent ratio, no abandon risk | Regulated industries, compliance-first | ✅ |
| **Preview** | Agent sees customer context before dialing | Complex/sensitive calls, healthcare | ✅ |
| **Agentless** | Automated messages without agents | Reminders, notifications, alerts | ✅ |

### Channel Configuration

| Channel | Use Cases | Requirements |
|---------|-----------|--------------|
| **Voice** | Sales, collections, support | Phone numbers, contact flow |
| **SMS** | Reminders, notifications | SMS capability, opt-in consent |
| **Email** | Marketing, follow-ups | Email addresses, templates |
| **WhatsApp** | International, digital-first | WhatsApp Business API |

---

## 🎯 Use Cases

### Collections Campaign

```
User: Create a collections campaign with preview dialer

Agent: I'll help you create a collections campaign...

1. Campaign name? > Q2-Collections-2026
2. Target segment? > overdue_30_plus (2,341 customers)
3. Channel(s)? > Voice + SMS fallback
4. Compliance:
   - Call window: 8am-9pm local
   - Max 3 attempts/day
   - Mini-Miranda disclosure required

✅ Campaign Created: camp-xyz789
```

### Sales Outreach

```
User: Build a predictive dialer campaign for our spring promotion

Agent: Perfect! Predictive dialing maximizes agent talk time...

1. Target segment: high_value_customers
2. Dialer capacity: 0.7 (moderate pacing)
3. AMD: Leave voicemail with promotional message
4. SMS follow-up: After 2 failed voice attempts

✅ Campaign ready to start
```

### Appointment Reminders (Agentless)

```
User: Create an agentless reminder campaign for tomorrow's appointments

Agent: Agentless campaigns are perfect for automated reminders...

1. Source: appointments_tomorrow segment
2. Channel: Voice + SMS
3. Message: "This is a reminder of your appointment tomorrow at {{time}}"
4. Timing: 6pm local time (day before)

✅ Campaign scheduled
```

---

## 🛠️ CLI Reference

### Commands

```bash
# Discovery
python3 smart_campaign_agent.py discover --region us-east-1
python3 smart_campaign_agent.py list-campaigns --instance-alias my-connect
python3 smart_campaign_agent.py get-campaign --campaign-id camp-xyz

# Campaign Lifecycle
python3 smart_campaign_agent.py create-campaign --config campaign.json
python3 smart_campaign_agent.py start-campaign --campaign-id camp-xyz
python3 smart_campaign_agent.py pause-campaign --campaign-id camp-xyz
python3 smart_campaign_agent.py resume-campaign --campaign-id camp-xyz
python3 smart_campaign_agent.py stop-campaign --campaign-id camp-xyz

# Segmentation
python3 smart_campaign_agent.py list-segments --domain my-domain

# Personalization
python3 smart_campaign_agent.py generate-script --type collections --tone empathetic

# CloudFormation
python3 smart_campaign_agent.py generate-cfn --config campaign.json --output-file campaign.yaml

# Metrics
python3 smart_campaign_agent.py get-metrics --campaign-id camp-xyz
```

### Campaign Configuration JSON

```json
{
  "name": "Q2-Collections-2026",
  "connect_instance_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "channel_config": {
    "telephony": {
      "capacity": 0.7,
      "connectQueueId": "queue-id",
      "defaultOutboundConfig": {
        "connectContactFlowId": "flow-id",
        "connectSourcePhoneNumber": "+12125551234",
        "answerMachineDetectionConfig": {
          "enableAnswerMachineDetection": true,
          "awaitAnswerMachinePrompt": true
        }
      }
    }
  },
  "source": {
    "segmentArn": "arn:aws:profile:us-east-1:123456:domains/my-domain/segments/seg-xyz"
  },
  "communication_limits": {
    "allChannelSubtypes": {
      "communicationLimitsList": [
        {"maxCountPerRecipient": 3, "frequency": 1, "unit": "DAY"}
      ]
    }
  },
  "communication_time": {
    "localTimeZoneConfig": {
      "defaultTimeZone": "America/New_York",
      "localTimeZoneDetection": ["ZIP_CODE"]
    },
    "telephony": {
      "openHours": {
        "dailyHours": {
          "MONDAY": [{"startTime": "08:00", "endTime": "21:00"}]
        }
      }
    }
  }
}
```

---

## 📋 TCPA Compliance Guide

### Overview

The Telephone Consumer Protection Act (TCPA) regulates telemarketing calls. This toolkit includes built-in compliance features:

### Time Restrictions

```yaml
# TCPA requires calls only during 8am-9pm LOCAL TIME
CommunicationTimeConfig:
  LocalTimeZoneConfig:
    LocalTimeZoneDetection:
      - ZIP_CODE      # Determine timezone from customer ZIP
      - AREA_CODE     # Fallback to phone area code
  Telephony:
    OpenHours:
      DailyHours:
        MONDAY:
          - StartTime: "08:00"
            EndTime: "21:00"
```

### Communication Limits

```yaml
# Prevent over-contacting customers
CommunicationLimitsOverride:
  AllChannelSubtypes:
    CommunicationLimitsList:
      - MaxCountPerRecipient: 3   # Max 3 per day
        Frequency: 1
        Unit: DAY
      - MaxCountPerRecipient: 7   # Max 7 per week
        Frequency: 7
        Unit: DAY
```

### Answering Machine Detection

```yaml
# Handle voicemails compliantly
AnswerMachineDetectionConfig:
  EnableAnswerMachineDetection: true
  AwaitAnswerMachinePrompt: true  # Wait for beep
```

### Exempt Communications

For emergency, fraud alerts, or other exempt communications:

```yaml
CommunicationLimitsOverride:
  AllChannelSubtypes:
    CommunicationLimitsList: []  # No limits for exempt
```

### Do-Not-Call Integration

Integrate with DNC lists via:
1. Customer Profiles `opt_in_status` attribute
2. External DNC API via Lambda
3. Segment exclusion rules

---

## 🏗️ CloudFormation Templates

### Basic Campaign

```bash
aws cloudformation deploy \
  --template-file templates/campaign-infrastructure.yaml \
  --stack-name my-campaign \
  --parameter-overrides \
    ConnectInstanceId=xxx \
    ContactFlowId=xxx \
    QueueId=xxx \
    SourcePhoneNumber=+12125551234 \
    CampaignName="Spring Promotion"
```

### Full Compliance Campaign

```bash
aws cloudformation deploy \
  --template-file templates/campaign-with-compliance.yaml \
  --stack-name my-compliant-campaign \
  --parameter-overrides \
    ConnectInstanceId=xxx \
    ContactFlowId=xxx \
    QueueId=xxx \
    SourcePhoneNumber=+12125551234 \
    CampaignName="Q2 Collections" \
    DialerMode=PREVIEW \
    MaxContactsPerDay=3 \
    MaxContactsPerWeek=7 \
    CallWindowStart=08:00 \
    CallWindowEnd=21:00 \
    EnableAMD=true \
    AlertEmail=alerts@company.com
```

### Available Templates

| Template | Description | Use Case |
|----------|-------------|----------|
| `campaign-infrastructure.yaml` | Basic campaign with defaults | Quick setup |
| `campaign-with-compliance.yaml` | Full TCPA compliance | Production |
| `segment-example.yaml` | Segment definition reference | Documentation |

---

## 🔌 API Reference

### Outbound Campaigns V2 API

| Operation | Purpose | Example |
|-----------|---------|---------|
| `CreateCampaign` | Create new campaign | `aws connectcampaignsv2 create-campaign` |
| `GetCampaign` | Get campaign details | `aws connectcampaignsv2 get-campaign --id X` |
| `ListCampaigns` | List all campaigns | `aws connectcampaignsv2 list-campaigns` |
| `StartCampaign` | Start campaign | `aws connectcampaignsv2 start-campaign --id X` |
| `PauseCampaign` | Pause campaign | `aws connectcampaignsv2 pause-campaign --id X` |
| `ResumeCampaign` | Resume campaign | `aws connectcampaignsv2 resume-campaign --id X` |
| `StopCampaign` | Stop permanently | `aws connectcampaignsv2 stop-campaign --id X` |
| `DeleteCampaign` | Delete campaign | `aws connectcampaignsv2 delete-campaign --id X` |
| `PutDialRequestBatch` | Add contacts | See below |

### Adding Contacts to Campaign

```python
import boto3
from datetime import datetime, timedelta

client = boto3.client('connectcampaignsv2')

response = client.put_dial_request_batch(
    id='campaign-id',
    dialRequests=[
        {
            'clientToken': 'unique-token-1',
            'phoneNumber': '+12125551234',
            'expirationTime': (datetime.utcnow() + timedelta(hours=1)).isoformat() + 'Z',
            'attributes': {
                'customerName': 'John Smith',
                'accountNumber': '12345'
            }
        }
    ]
)
```

### Customer Profiles API

| Operation | Purpose |
|-----------|---------|
| `CreateSegmentDefinition` | Create customer segment |
| `GetSegmentDefinition` | Get segment details |
| `ListSegmentDefinitions` | List all segments |
| `CreateCalculatedAttributeDefinition` | Create computed attribute |

---

## 🔐 IAM Permissions

### Minimum Required

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ConnectCampaigns",
      "Effect": "Allow",
      "Action": [
        "connect-campaigns:*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ConnectInstance",
      "Effect": "Allow",
      "Action": [
        "connect:DescribeInstance",
        "connect:ListInstances",
        "connect:DescribeContactFlow",
        "connect:ListContactFlows",
        "connect:DescribeQueue",
        "connect:ListQueues"
      ],
      "Resource": "*"
    }
  ]
}
```

### Full Permissions (with Segmentation & Personalization)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "OutboundCampaigns",
      "Effect": "Allow",
      "Action": ["connect-campaigns:*"],
      "Resource": "*"
    },
    {
      "Sid": "ConnectInstance",
      "Effect": "Allow",
      "Action": [
        "connect:Describe*",
        "connect:List*",
        "connect:Get*"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CustomerProfiles",
      "Effect": "Allow",
      "Action": ["profile:*"],
      "Resource": "*"
    },
    {
      "Sid": "BedrockPersonalization",
      "Effect": "Allow",
      "Action": ["bedrock:InvokeModel"],
      "Resource": "*"
    },
    {
      "Sid": "ContactLensAnalytics",
      "Effect": "Allow",
      "Action": ["connect:ListRealtimeContactAnalysisSegments"],
      "Resource": "*"
    },
    {
      "Sid": "CloudFormation",
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:UpdateStack",
        "cloudformation:DescribeStacks",
        "cloudformation:DeleteStack"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatch",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:PutDashboard",
        "cloudwatch:PutMetricAlarm",
        "cloudwatch:GetMetricData"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 📁 Repository Structure

```
connect-outbound-campaigns/
├── README.md                              # This file
├── LICENSE                                # MIT License
├── CONTRIBUTING.md                        # Contribution guidelines
├── copilot-skills/
│   └── connect-campaign-agent/            # GitHub Copilot Skill
│       ├── SKILL.md                       # Agent definition (~500 lines)
│       ├── scripts/
│       │   └── smart_campaign_agent.py    # Main orchestration tool
│       ├── templates/
│       │   ├── campaign-infrastructure.yaml
│       │   ├── campaign-with-compliance.yaml
│       │   └── segment-example.yaml
│       └── examples/
│           ├── create_simple_campaign.py
│           ├── create_segment.py
│           └── personalize_script.py
├── docs/
│   ├── TCPA_COMPLIANCE.md                 # Compliance deep-dive
│   ├── SEGMENTATION.md                    # Segmentation guide
│   └── ARCHITECTURE.md                    # System architecture
└── tests/
    └── test_campaign_agent.py             # Unit tests
```

---

## 🌍 Supported Regions

Amazon Connect Outbound Campaigns is available in:

| Region | Name | Campaigns V2 |
|--------|------|--------------|
| us-east-1 | US East (N. Virginia) | ✅ |
| us-west-2 | US West (Oregon) | ✅ |
| eu-west-2 | Europe (London) | ✅ |
| eu-central-1 | Europe (Frankfurt) | ✅ |
| ap-southeast-1 | Asia Pacific (Singapore) | ✅ |
| ap-southeast-2 | Asia Pacific (Sydney) | ✅ |
| ap-northeast-1 | Asia Pacific (Tokyo) | ✅ |
| ca-central-1 | Canada (Central) | ✅ |

---

## 🐛 Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Campaign won't start | Missing permissions | Check IAM policy for `connect-campaigns:StartCampaign` |
| Low connect rates | Wrong time of day | Analyze hourly metrics, shift to peak hours |
| Segment not found | Domain mismatch | Verify Customer Profiles domain name |
| AMD not detecting | Contact Lens disabled | Enable Contact Lens on instance |
| Agents not receiving | Queue misconfigured | Check routing profile assignments |
| Timezone violations | Detection failed | Use ZIP_CODE detection method |
| Bedrock timeout | Model throttling | Implement retry with backoff |
| High abandon rate | Predictive too aggressive | Switch to progressive or reduce capacity |

---

## 📚 References

- [Amazon Connect Outbound Campaigns](https://aws.amazon.com/connect/outbound/)
- [Outbound Campaigns V2 API](https://docs.aws.amazon.com/connect/latest/APIReference/API_Operations_Amazon_Connect_Outbound_Campaigns_V2.html)
- [Customer Profiles API](https://docs.aws.amazon.com/connect/latest/APIReference/API_Operations_Amazon_Connect_Customer_Profiles.html)
- [Contact Lens Documentation](https://docs.aws.amazon.com/connect/latest/adminguide/contact-lens.html)
- [TCPA Compliance Guide](https://docs.aws.amazon.com/connect/latest/adminguide/outbound-campaign-best-practices.html)
- [Amazon Bedrock](https://aws.amazon.com/bedrock/)

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Amazon Connect team for the Outbound Campaigns V2 API
- Amazon Bedrock team for generative AI capabilities
- GitHub Copilot team for the skills framework

---

**Built with ❤️ for Amazon Connect**

<!-- BEGIN COPILOT CUSTOM AGENTS -->
## GitHub Copilot Custom Agents (Maximus Internal)

This repository includes **GitHub Copilot custom agent profiles** under `.github/agents/` to speed up planning, documentation, and safe reviews.

### Included agents
- `implementation-planner` — Creates detailed implementation plans and technical specifications for this repository.
- `readme-creator` — Improves README and adjacent documentation without modifying production code.
- `security-auditor` — Performs a read-only security review (secrets risk, risky patterns) and recommends fixes.
- `amazon-connect-solution-engineer` — Designs and integrates Amazon Connect solutions for this repository (IAM-safe, CX/ops focused).
- `amazon-connect-outbound-engineer` — Designs and debugs Amazon Connect outbound calling/campaign workflows safely.

### How to invoke

- **GitHub.com (Copilot coding agent):** select the agent from the agent dropdown (or assign it to an issue) after the `.agent.md` files are on the default branch.
- **GitHub Copilot CLI:** from the repo folder, run `/agent` and select one of the agents, or run:
  - `copilot --agent <agent-file-base-name> --prompt "<your prompt>"`
- **IDEs:** open Copilot Chat and choose the agent from the agents dropdown (supported IDEs), backed by the `.github/agents/*.agent.md` files.

References:
- Custom agents configuration: https://docs.github.com/en/copilot/reference/custom-agents-configuration
- Creating custom agents: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents
<!-- END COPILOT CUSTOM AGENTS -->

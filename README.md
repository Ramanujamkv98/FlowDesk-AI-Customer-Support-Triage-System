Business Problem:

Customer support teams often spend significant time manually reading, categorizing, prioritizing, and routing incoming requests before they reach the appropriate specialist. Customers do not always use clear or accurate terminology, especially when distinguishing between general account-access concerns and genuine security risks. For example, a password-reset issue may be described as an account compromise, while suspicious login activity may initially appear to be a routine access problem.

This creates a risk of inconsistent categorization, delayed escalation, and security-sensitive cases being routed to the wrong team.

FlowDesk AI automates the triage process using Retrieval-Augmented Generation, structured LLM classification, deterministic business rules, Salesforce integration, and operational dashboards. The workflow analyzes the customer’s description, identifies the underlying issue, flags potential security risks, and routes the case to the appropriate support team.

Rather than replacing human agents, the system reduces repetitive triage work while ensuring critical cases, ambiguous requests, security risks, and low-confidence classifications receive appropriate human review.


Solution Proposal:
The FlowDesk AI workflow proposed to performs the following tasks:

1.Retrieves relevant support policies using Retrieval-Augmented Generation (RAG)
2.Classifies tickets into structured business fields
3.Detects urgency and customer sentiment
4.Flags potential security risks
5.Applies deterministic routing rules
6.Creates Salesforce Cases through the REST API
7.Generates policy-grounded customer responses
8.Tracks operational KPIs through Salesforce dashboards

The workflow combines AI reasoning with deterministic business logic to ensure consistent and auditable decision-making.

System Architecture

Customer Ticket
        │
        ▼
Knowledge Retrieval (RAG)
        │
        ▼
GPT-5 Structured Ticket Classifier
        │
        ▼
Business Rule Engine
        │
        ├── Billing Support
        ├── Technical Support
        ├── Account Security
        ├── Account Access
        ├── Cancellation Team
        ├── General Support
        └── Manual Review
                │
                ▼
Salesforce OAuth Authentication
                │
                ▼
Salesforce Case Creation
                │
                ▼
Response Writer LLM
                │
                ▼
Customer Response + Dashboard Reporting

AI Workflow Components
1. Knowledge Retrieval (RAG)

Before classifying a ticket, the workflow searches the company knowledge base for relevant support policies.

This enables the AI to generate responses grounded in organizational policy rather than relying solely on the language model.

2. Structured Ticket Classification

Instead of generating free-form text, the LLM produces structured JSON that downstream systems can reliably consume.

Example output:

{
  "ticket_summary": "Unable to log in after password reset",
  "primary_category": "Login and Account Access",
  "subcategory": "Password Reset",
  "urgency": "High",
  "sentiment": "Frustrated",
  "confidence": 94,
  "security_risk": false,
  "policy_topic": "Account Access",
  "information_needed": [
    "Registered Email",
    "Last Login Attempt"
  ]
}
3. Deterministic Routing

Rather than allowing the LLM to make final routing decisions, business rules determine the appropriate support team.

Routing Logic
IF Security Risk = TRUE
    → Account Security

ELSE IF Confidence < 70
    → Support Operations

ELSE IF Category = Billing
    → Billing Support

ELSE IF Category = Login
    → Account Access

ELSE IF Category = Technical
    → Technical Support

ELSE IF Category = Cancellation
    → Cancellation Team

ELSE
    → General Support

This hybrid AI + rules-based approach improves consistency, auditability, and operational control.

4. Salesforce Integration

Once the support team has been determined, the workflow:

Authenticates using Salesforce OAuth 2.0
Retrieves a temporary access token
Maps AI-generated fields to Salesforce Case fields
Creates a new Salesforce Case via the REST API
Returns the Case ID for downstream processing
5. Response Writer

The final LLM generates:

Internal triage summary
Customer-facing response
Policy-grounded explanation
Next steps

The response writer is intentionally restricted from changing the assigned support team or modifying classifier outputs.

Dashboard

<img width="1911" height="777" alt="image" src="https://github.com/user-attachments/assets/453933ec-e341-49c2-b14d-ec6a2304e46b" />

A Salesforce dashboard provides operational visibility into AI-assisted support performance.

Current dashboards include:

Total AI-created Cases
Security-Risk Cases
Low-Confidence Cases
Ticket Distribution by Category
Ticket Distribution by Urgency

These dashboards allow support managers to monitor workload distribution, AI confidence, and routing trends.


Technology Stack:
AI & Workflow
Dify
GPT-5
Retrieval-Augmented Generation (RAG)
Prompt Engineering
JSON Structured Outputs
CRM
Salesforce CRM
Salesforce REST API
OAuth 2.0
AI Techniques
Ticket Classification
Sentiment Analysis
Urgency Detection
Confidence Scoring
Rule-Based Decision Engine
Reporting
Salesforce Dashboards
CRM Reporting

Sample Workflow

Customer submits:

"I cancelled my subscription yesterday but was charged again this morning."

The workflow:

✔ Retrieves the Billing & Refund policy

✔ Classifies:

Billing & Refunds
High Urgency
Negative Sentiment

✔ Routes to Billing Support

✔ Creates a Salesforce Case

✔ Generates a customer response referencing the refund policy

✔ Updates operational dashboards

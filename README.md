graph TD
    A[Start] --> B[Real-Time Data Ingestion]
    B --> C[Event Routing]
    C --> D[Rule Evaluation]
    D --> E[Typology Scoring]
    E --> F[Alert Generation and Action]
    F --> G[Tazama Outcomes]

    subgraph "How Tazama Works"
        H[Transaction Messages] --> I[Rule Evaluation and Processing]
        I --> J[Assessment Outcome Delivery]
        J --> G
    end

    subgraph "Semi-Attached System"
        K[API Connection] --> L[Independent Processing]
        L --> M[Feedback to Provider System]
    end

    subgraph "Detection Ecosystem"
        N[Centralized Monitoring]
        N --> O[Switch Participants]
        O --> P[Evaluation by Switch Operators]
        P --> Q[Compliance Teams of Participants]
    end

    subgraph "TMS API Architecture"
        R[ISO 20022 Formats]
        R --> S[pain.001: Payment Initiation]
        R --> T[pain.013: Creditor Activation Request]
        R --> U[pacs.008: Interbank Settlement]
        R --> V[pacs.002: Status Reports]
    end

    subgraph "How TMS API Uses Messages"
        W[Data Ingestion] --> X[Rule Processing]
        X --> Y[Alerting and Reporting]
    end

    subgraph "Data Preparation"
        Z[Store Message as Received]
        Z --> AA[Load into Historical Graph]
        AA --> AB[Create DataCache Object]
        AB --> AC[Generate Metadata]
    end

    AC --> AD[Message Transmission]
    AD --> AE[Event Director]

    subgraph "Payment Platform Adapters"
        AF[Non-ISO 20022 Message Submission]
        AF --> AG[Client Formatting Compliance]
        AG --> AH[TMS API Integration]
    end

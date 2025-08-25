# Role-Based Workflow Diagrams - Jian Cha Tea Unity Suite

## Overview
This document contains detailed workflow diagrams for each user role within the Jian Cha Tea Unity Suite, showing process flows, decision points, rules, and system interactions.

## 1. Customer Workflows

### 1.1 Customer Registration & Onboarding Workflow

```mermaid
flowchart TD
    Start([Customer Opens App]) --> A{Existing Account?}
    A -->|Yes| B[Login Process]
    A -->|No| C[Registration Options]
    
    C --> D{Registration Method}
    D -->|Social Media| E[Social Login]
    D -->|Phone Number| F[Phone Verification]
    D -->|Email| G[Email Registration]
    
    E --> H[Profile Completion]
    F --> I[OTP Verification]
    G --> J[Email Verification]
    
    I --> H
    J --> H
    B --> K[Welcome Screen]
    H --> K
    
    K --> L[Payment Method Setup]
    L --> M{Add Payment Method?}
    M -->|Yes| N[Payment Setup]
    M -->|No| O[Skip for Now]
    
    N --> P[Loyalty Program Enrollment]
    O --> P
    P --> Q[Onboarding Complete]
    
    Q --> End([Dashboard Access])
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style K fill:#fff3e0
```

**Business Rules:**
- Phone verification required for wallet features
- Social login must still collect phone number
- Payment method optional during registration
- Loyalty enrollment automatic for verified accounts

### 1.2 Order Placement & Processing Workflow

```mermaid
flowchart TD
    Start([Customer Initiates Order]) --> A[Browse Menu]
    A --> B[Select Items]
    B --> C[Customize Items]
    C --> D[Add to Cart]
    D --> E{Continue Shopping?}
    E -->|Yes| A
    E -->|No| F[Review Cart]
    
    F --> G[Select Store]
    G --> H{Store Available?}
    H -->|No| I[Show Alternative Stores]
    H -->|Yes| J[Choose Order Type]
    
    I --> G
    J --> K{Order Type}
    K -->|Pickup| L[Select Pickup Time]
    K -->|Dine-in| M[Select Table]
    K -->|Delivery| N[Confirm Address]
    
    L --> O[Payment Selection]
    M --> O
    N --> P{Address Valid?}
    P -->|No| Q[Update Address]
    P -->|Yes| O
    Q --> N
    
    O --> R{Payment Method}
    R -->|Wallet| S[Check Balance]
    R -->|Card| T[Card Processing]
    R -->|Points| U[Point Redemption]
    
    S --> V{Sufficient Balance?}
    V -->|No| W[Add Funds or Change Method]
    V -->|Yes| X[Process Payment]
    T --> X
    U --> Y{Sufficient Points?}
    Y -->|No| Z[Partial Point + Payment]
    Y -->|Yes| X
    
    W --> O
    Z --> X
    X --> AA{Payment Success?}
    AA -->|No| BB[Payment Failed]
    AA -->|Yes| CC[Order Confirmed]
    
    BB --> O
    CC --> DD[Generate Order Number]
    DD --> EE[Send to Kitchen]
    EE --> FF[Track Order Status]
    FF --> End([Order Complete])
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style CC fill:#fff3e0
```

**Business Rules:**
- Maximum 10 items per order for mobile app
- Order modification allowed until kitchen preparation starts
- Automatic store selection based on GPS location
- Payment retry limit: 3 attempts
- Order timeout: 15 minutes if not confirmed

## 2. Store Operations Workflows

### 2.1 Daily Store Operations Workflow

```mermaid
flowchart TD
    Start([Store Opening Time]) --> A[Staff Clock In]
    A --> B[System Health Check]
    B --> C{System Ready?}
    C -->|No| D[Contact IT Support]
    C -->|Yes| E[Open POS Terminals]
    
    D --> F[Manual Operations Mode]
    F --> E
    E --> G[Cash Float Setup]
    G --> H[Inventory Check]
    H --> I{Stock Levels OK?}
    I -->|No| J[Order Supplies]
    I -->|Yes| K[Store Ready]
    
    J --> L{Critical Items Missing?}
    L -->|Yes| M[Update Menu Availability]
    L -->|No| K
    M --> K
    
    K --> N[Begin Service]
    N --> O[Process Customer Orders]
    O --> P{Order Type}
    P -->|POS| Q[POS Transaction]
    P -->|Mobile| R[Mobile Order Processing]
    P -->|Online| S[Online Order Processing]
    
    Q --> T[Payment Processing]
    R --> U[Order Fulfillment]
    S --> U
    T --> U
    
    U --> V[Update Inventory]
    V --> W{Low Stock Alert?}
    W -->|Yes| X[Reorder Items]
    W -->|No| Y{More Orders?}
    X --> Y
    Y -->|Yes| O
    Y -->|No| Z[End of Day Process]
    
    Z --> AA[Cash Reconciliation]
    AA --> BB[Sales Reporting]
    BB --> CC[Inventory Count]
    CC --> DD[Clean & Close]
    DD --> End([Store Closed])
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style K fill:#fff3e0
```

**Business Rules:**
- Minimum 2 staff required for store opening
- Float amount varies by store size (₹5,000-₹20,000)
- Critical items: signature drinks and bestsellers
- Low stock threshold: 20% of daily average
- Daily cash variance tolerance: ₹100

### 2.2 Staff Shift Management Workflow

```mermaid
flowchart TD
    Start([Shift Start Time]) --> A[Biometric Clock In]
    A --> B{Authentication Success?}
    B -->|No| C[Manual Entry by Manager]
    B -->|Yes| D[Assign POS Terminal]
    
    C --> E{Manager Approval?}
    E -->|No| F[Access Denied]
    E -->|Yes| D
    
    D --> G[Cash Drawer Assignment]
    G --> H[Handover from Previous Shift]
    H --> I[Begin Shift Duties]
    
    I --> J{Break Time?}
    J -->|Yes| K[Clock Out for Break]
    J -->|No| L[Continue Work]
    
    K --> M[Break Duration Check]
    M --> N{Within Time Limit?}
    N -->|No| O[Overtime Alert]
    N -->|Yes| P[Clock In from Break]
    
    O --> P
    P --> L
    L --> Q{Shift End Time?}
    Q -->|No| J
    Q -->|Yes| R[Shift Handover]
    
    R --> S[Cash Count & Reconciliation]
    S --> T{Cash Balance Correct?}
    T -->|No| U[Variance Investigation]
    T -->|Yes| V[Clock Out]
    
    U --> W{Variance Resolved?}
    W -->|No| X[Manager Escalation]
    W -->|Yes| V
    X --> Y[Incident Report]
    Y --> V
    
    V --> End([Shift Complete])
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style F fill:#ffcdd2
```

**Business Rules:**
- Maximum 8 hours per shift
- Mandatory 30-minute break for shifts > 6 hours
- Cash variance > ₹500 requires investigation
- 3 consecutive late clock-ins trigger warning
- Overtime requires manager approval

## 3. Franchise Management Workflows

### 3.1 Franchise Application & Approval Workflow

```mermaid
flowchart TD
    Start([Franchise Inquiry]) --> A[Lead Registration]
    A --> B[Initial Screening]
    B --> C{Basic Criteria Met?}
    C -->|No| D[Send Regret Letter]
    C -->|Yes| E[Send Information Pack]
    
    E --> F[Application Submission]
    F --> G[Document Verification]
    G --> H{Documents Complete?}
    H -->|No| I[Request Missing Documents]
    H -->|Yes| J[Financial Assessment]
    
    I --> K{Received Within 7 Days?}
    K -->|No| L[Application Timeout]
    K -->|Yes| J
    
    J --> M{Financial Criteria Met?}
    M -->|No| N[Financial Rejection]
    M -->|Yes| O[Background Check]
    
    O --> P{Background Clear?}
    P -->|No| Q[Background Rejection]
    P -->|Yes| R[Interview Scheduling]
    
    R --> S[Franchise Interview]
    S --> T[Interview Evaluation]
    T --> U{Interview Pass?}
    U -->|No| V[Interview Rejection]
    U -->|Yes| W[Location Assessment]
    
    W --> X{Location Approved?}
    X -->|No| Y[Location Rejection]
    X -->|Yes| Z[Final Approval]
    
    Z --> AA[Send Franchise Agreement]
    AA --> BB[Contract Signing]
    BB --> CC[Franchise Fee Payment]
    CC --> DD{Payment Received?}
    DD -->|No| EE[Payment Reminder]
    DD -->|Yes| FF[Welcome to Franchise]
    
    EE --> GG{Payment Within 30 Days?}
    GG -->|No| HH[Application Cancelled]
    GG -->|Yes| FF
    
    FF --> End([Onboarding Begins])
    
    D --> EndReject([Application Rejected])
    L --> EndReject
    N --> EndReject
    Q --> EndReject
    V --> EndReject
    Y --> EndReject
    HH --> EndReject
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style EndReject fill:#ffcdd2
    style FF fill:#fff3e0
```

**Business Rules:**
- Minimum investment: $150,000 USD
- Location must be within approved zones
- Background check includes credit score > 650
- Application expires after 90 days if incomplete
- Maximum 2 interview attempts allowed

### 3.2 Franchisee Onboarding Workflow

```mermaid
flowchart TD
    Start([Contract Signed]) --> A[Assign Franchise Manager]
    A --> B[Create Onboarding Schedule]
    B --> C[Site Survey]
    C --> D[Design Approval]
    D --> E[Construction Planning]
    E --> F[Permit Applications]
    F --> G{Permits Approved?}
    G -->|No| H[Address Permit Issues]
    G -->|Yes| I[Begin Construction]
    
    H --> I
    I --> J[Equipment Ordering]
    J --> K[Staff Recruitment]
    K --> L[Training Schedule]
    L --> M[POS System Setup]
    M --> N[Initial Inventory Order]
    
    N --> O[Construction Progress Check]
    O --> P{Construction Complete?}
    P -->|No| Q[Construction Delay]
    P -->|Yes| R[Equipment Installation]
    
    Q --> S{Within Timeline?}
    S -->|No| T[Revised Timeline]
    S -->|Yes| R
    T --> R
    
    R --> U[System Testing]
    U --> V[Staff Training]
    V --> W[Soft Opening]
    W --> X[Trial Period]
    X --> Y{Trial Successful?}
    Y -->|No| Z[Additional Training]
    Y -->|Yes| AA[Grand Opening]
    
    Z --> Y
    AA --> BB[Marketing Launch]
    BB --> CC[Performance Monitoring]
    CC --> End([Onboarding Complete])
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style AA fill:#fff3e0
```

**Business Rules:**
- Onboarding timeline: 90-120 days
- Soft opening requires 7 days minimum
- Staff training: 40 hours minimum
- Quality audit score > 85% required
- Marketing support for first 30 days

## 4. Employee Management Workflows

### 4.1 Employee Lifecycle Management Workflow

```mermaid
flowchart TD
    Start([Hiring Need Identified]) --> A[Job Requisition]
    A --> B[Manager Approval]
    B --> C[Post Job Opening]
    C --> D[Receive Applications]
    D --> E[Initial Screening]
    E --> F{Candidate Qualified?}
    F -->|No| G[Send Rejection]
    F -->|Yes| H[Schedule Interview]
    
    H --> I[Conduct Interview]
    I --> J[Interview Assessment]
    J --> K{Hire Decision?}
    K -->|No| L[Send Regret Letter]
    K -->|Yes| M[Reference Check]
    
    M --> N{References Positive?}
    N -->|No| O[Reconsider Decision]
    N -->|Yes| P[Generate Offer Letter]
    
    O --> Q{Final Decision?}
    Q -->|No| L
    Q -->|Yes| P
    
    P --> R[Send Offer Letter]
    R --> S{Offer Accepted?}
    S -->|No| T[Update Candidate Pool]
    S -->|Yes| U[Contract Preparation]
    
    U --> V[Background Verification]
    V --> W{Background Clear?}
    W -->|No| X[Withdraw Offer]
    W -->|Yes| Y[Contract Signing]
    
    Y --> Z[Employee Onboarding]
    Z --> AA[System Access Setup]
    AA --> BB[Training Schedule]
    BB --> CC[Probation Period]
    CC --> DD{Probation Successful?}
    DD -->|No| EE[Termination Process]
    DD -->|Yes| FF[Confirmation]
    
    FF --> GG[Regular Performance Reviews]
    GG --> HH{Performance Satisfactory?}
    HH -->|No| II[Performance Improvement]
    HH -->|Yes| JJ[Career Development]
    
    II --> KK{Improvement Achieved?}
    KK -->|No| EE
    KK -->|Yes| GG
    
    JJ --> LL{Promotion/Transfer?}
    LL -->|Yes| MM[Role Change Process]
    LL -->|No| GG
    
    MM --> GG
    EE --> End([Employee Exit])
    
    style Start fill:#e1f5fe
    style End fill:#ffcdd2
    style FF fill:#c8e6c9
```

**Business Rules:**
- Probation period: 3-6 months depending on role
- Background check must be clear before start date
- Minimum 2 interviewers for all positions
- Performance reviews: quarterly for first year, then annually
- Notice period: 1-3 months depending on seniority

## 5. Payment & Financial Workflows

### 5.1 Payment Processing Workflow

```mermaid
flowchart TD
    Start([Payment Initiated]) --> A{Payment Method}
    A -->|Wallet| B[Check Wallet Balance]
    A -->|Card| C[Card Authorization]
    A -->|Points| D[Check Points Balance]
    A -->|Bank Transfer| E[Bank API Call]
    
    B --> F{Sufficient Balance?}
    F -->|No| G[Insufficient Funds]
    F -->|Yes| H[Deduct Amount]
    
    C --> I[3D Secure Check]
    I --> J{3D Secure Required?}
    J -->|Yes| K[Customer Authentication]
    J -->|No| L[Process Authorization]
    
    K --> M{Authentication Success?}
    M -->|No| N[Authentication Failed]
    M -->|Yes| L
    
    L --> O{Authorization Success?}
    O -->|No| P[Declined by Bank]
    O -->|Yes| Q[Capture Payment]
    
    D --> R{Sufficient Points?}
    R -->|No| S[Insufficient Points]
    R -->|Yes| T[Deduct Points]
    
    E --> U{Bank Response?}
    U -->|Pending| V[Wait for Confirmation]
    U -->|Success| W[Payment Confirmed]
    U -->|Failed| X[Bank Transfer Failed]
    
    V --> Y[Check Status]
    Y --> Z{Final Status?}
    Z -->|Success| W
    Z -->|Failed| X
    Z -->|Pending| AA[Timeout Check]
    
    AA --> BB{Within Timeout?}
    BB -->|Yes| Y
    BB -->|No| CC[Payment Timeout]
    
    H --> DD[Update Transaction Log]
    Q --> DD
    T --> DD
    W --> DD
    
    DD --> EE[Send Confirmation]
    EE --> FF[Update Loyalty Points]
    FF --> GG[Settlement Queue]
    GG --> End([Payment Complete])
    
    G --> EndFail([Payment Failed])
    N --> EndFail
    P --> EndFail
    S --> EndFail
    X --> EndFail
    CC --> EndFail
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style EndFail fill:#ffcdd2
```

**Business Rules:**
- Payment timeout: 5 minutes for card, 24 hours for bank transfer
- Maximum 3 retry attempts for failed payments
- Fraud detection triggers for amounts > $500
- Automatic refund for timeout scenarios
- Settlement batched daily at 11:59 PM

### 5.2 Settlement & Reconciliation Workflow

```mermaid
flowchart TD
    Start([Daily Settlement Time]) --> A[Extract Transaction Data]
    A --> B[Group by Payment Method]
    B --> C[Calculate Net Amounts]
    C --> D[Apply Fees & Charges]
    D --> E[Generate Settlement Files]
    E --> F{Settlement Method}
    F -->|Bank Transfer| G[Generate Bank File]
    F -->|Wallet Credit| H[Credit Merchant Wallet]
    F -->|Manual| I[Generate Manual Report]
    
    G --> J[Send to Bank]
    J --> K[Await Bank Confirmation]
    K --> L{Transfer Successful?}
    L -->|No| M[Settlement Failed]
    L -->|Yes| N[Update Settlement Status]
    
    H --> O[Process Wallet Credits]
    O --> P{Credit Successful?}
    P -->|No| Q[Wallet Credit Failed]
    P -->|Yes| N
    
    I --> R[Send to Finance Team]
    R --> N
    
    N --> S[Reconciliation Process]
    S --> T[Match Transactions]
    T --> U{All Matched?}
    U -->|No| V[Exception Handling]
    U -->|Yes| W[Generate Reports]
    
    V --> X[Investigate Variances]
    X --> Y{Variance Resolved?}
    Y -->|No| Z[Manual Intervention]
    Y -->|Yes| W
    
    Z --> AA[Finance Team Review]
    AA --> W
    
    W --> BB[Distribute Reports]
    BB --> CC[Archive Data]
    CC --> End([Settlement Complete])
    
    M --> EndFail([Settlement Failed])
    Q --> EndFail
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style EndFail fill:#ffcdd2
```

**Business Rules:**
- Settlement cut-off time: 11:59 PM daily
- Variance threshold: 0.1% of daily volume
- Failed settlements retry automatically for 3 days
- Manual settlements require dual approval
- Reconciliation must complete within 24 hours

## 6. Marketing & Engagement Workflows

### 6.1 Campaign Management Workflow

```mermaid
flowchart TD
    Start([Campaign Idea]) --> A[Campaign Planning]
    A --> B[Define Target Audience]
    B --> C[Set Campaign Goals]
    C --> D[Create Campaign Content]
    D --> E[Legal/Compliance Review]
    E --> F{Approval Status?}
    F -->|Rejected| G[Revise Campaign]
    F -->|Approved| H[Schedule Campaign]
    
    G --> E
    H --> I[Audience Segmentation]
    I --> J[Personalization Setup]
    J --> K[A/B Test Configuration]
    K --> L[Campaign Launch]
    L --> M[Monitor Performance]
    
    M --> N{Performance Metrics}
    N -->|Poor| O[Campaign Optimization]
    N -->|Good| P[Continue Campaign]
    N -->|Excellent| Q[Scale Campaign]
    
    O --> R{Optimization Success?}
    R -->|No| S[Campaign Pause/Stop]
    R -->|Yes| P
    
    P --> T{Campaign End Date?}
    T -->|No| M
    T -->|Yes| U[Campaign Completion]
    
    Q --> V[Budget Reallocation]
    V --> P
    
    U --> W[Performance Analysis]
    W --> X[ROI Calculation]
    X --> Y[Campaign Report]
    Y --> Z[Lessons Learned]
    Z --> End([Campaign Closed])
    
    S --> AA[Early Termination Report]
    AA --> End
    
    style Start fill:#e1f5fe
    style End fill:#c8e6c9
    style L fill:#fff3e0
```

**Business Rules:**
- Minimum campaign duration: 7 days
- Budget approval required for campaigns > $10,000
- A/B test minimum sample size: 1,000 customers
- Performance review every 48 hours during active campaign
- ROI calculation includes all attributable revenue

## Workflow Integration Points

### Critical Integration Dependencies
1. **Customer Registration → CRM → Loyalty → Marketing**: Customer data flows through all systems for personalized experiences
2. **Order Processing → POS → Payment → Inventory**: Real-time updates across all transaction systems  
3. **Staff Management → Payroll → Performance → Training**: HR data integration for complete employee lifecycle
4. **Franchise Application → Financial → Legal → Operations**: Multi-departmental approval workflow
5. **Campaign Management → Customer Segmentation → Personalization → Analytics**: Marketing automation pipeline

### Data Consistency Requirements
- Customer profile synchronization across all touchpoints
- Real-time inventory updates to prevent overselling
- Financial transaction integrity across all payment methods
- Employee access rights consistency across all systems
- Campaign performance data accuracy for decision making

These workflows represent the core business processes and their interdependencies within the Jian Cha Tea Unity Suite, providing a comprehensive foundation for system design and implementation.
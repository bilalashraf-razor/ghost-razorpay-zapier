# Ghost-Razorpay-Zapier Integration - Sequence Diagram

```mermaid
sequenceDiagram
    participant A as Admin
    participant G as Ghost
    participant R as Razorpay
    participant C as Customer
    participant Z as Zapier
    participant GA as Ghost API

    %% Setup Phase
    rect rgb(250, 250, 255)
        Note over A, GA: SETUP PHASE
        
        Note over A, G: 1. Ghost API Credentials
        A->>G: Settings → Integrations
        A->>G: Create custom integration
        G->>A: API Key + URL
        
        Note over A, R: 2. Razorpay Webhook
        A->>R: Settings → Webhooks
        A->>Z: Create new Zap
        Z->>A: Webhook URL
        A->>R: Configure webhook + events
        
        Note over A, Z: 3. Zapier Configuration
        A->>Z: Select "Webhooks by Zapier"
        A->>Z: Set "Catch Hook" trigger
        A->>Z: Add "Custom Request" action
    end

    %% Testing Phase  
    rect rgb(240, 248, 255)
        Note over C, Z: TESTING PHASE
        C->>R: Test payment
        R->>Z: Webhook: payment.captured
        Z->>A: Show captured data
        
        A->>Z: Configure member lookup
        Note right of A: GET /members/?search=email<br/>Auth: Ghost-Admin {key}
        A->>Z: Test action
        Z->>GA: Search member
        GA->>Z: Return results
    end

    %% Live Operation
    rect rgb(240, 255, 240)
        Note over C, GA: LIVE OPERATION
        C->>R: Real payment
        R->>R: Process payment
        R->>Z: payment.captured webhook
        
        Z->>GA: Search member by email
        
        alt Member Found
            GA->>Z: Existing member data
            Z->>GA: Update member tags/subscription
            GA->>Z: ✓ Update confirmed
        else New Member
            Z->>GA: Create new member
            GA->>Z: ✓ Member created
        end
        
        Z->>A: Log automation success
    end

    Note over A, GA: ✅ Automated Integration Active
```

## Key Interactions Summary

### Initial Setup Phase
1. **Ghost API Setup**: Admin creates custom integration in Ghost to get API credentials
2. **Razorpay Webhook Configuration**: Admin configures Razorpay to send payment notifications to Zapier
3. **Zapier Zap Creation**: Admin creates automated workflow to catch Razorpay webhooks and interact with Ghost API
4. **Testing**: Admin tests the integration with sample payments

### Runtime Automation Phase
1. **Payment Trigger**: Customer makes payment through Razorpay
2. **Webhook Delivery**: Razorpay sends payment notification to Zapier
3. **Member Lookup**: Zapier searches Ghost for existing member using payment email
4. **Member Management**: Zapier either updates existing member or creates new member in Ghost
5. **Completion**: Automated membership management based on successful payments

## Critical Components

- **Ghost Admin API**: Provides programmatic access to member management
- **Razorpay Webhooks**: Real-time payment notifications 
- **Zapier Integration**: No-code automation platform connecting Razorpay and Ghost
- **Authentication**: Secure API key-based authentication for Ghost Admin API
- **Error Handling**: Built-in retry mechanisms and error logging in Zapier

## Benefits

- **Automated Membership**: No manual intervention needed for member creation/updates
- **Real-time Processing**: Members are added immediately upon successful payment
- **Scalable Solution**: Handles high volume of payments automatically
- **Audit Trail**: Complete log of all automation activities in Zapier

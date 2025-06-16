
1. **Auth Service**
    
    - Handles user login, registration, social login, JWT issuance, token refresh.
        
2. **Access Control Service**
    
    - Evaluates user permissions/roles using Casbin (or similar) for authorization decisions.
        
3. **User Service**
    
    - Manages user profiles, preferences, booking history, and related user data.
        
4. **Movie Service**
    
    - Handles CRUD operations for movie metadata (title, genre, cast, etc.).
        
5. **Schedule Service**
    
    - Manages show timings, recurrence rules, show-to-theater mapping.
        
6. **Theater Service**
    
    - Manages theaters, screens, seating layouts, and location info.
        
7. **Seat Management Service**
    
    - Handles seat maps per show, locking/unlocking seats in real-time (via Redis).
        
8. **Booking Service**
    
    - Controls the booking lifecycle (lock → payment → confirm), enforces cancellation policy.
        
9. **Payment Service**
    
    - Integrates with payment gateways like Stripe/Razorpay, emits booking events on success/failure.
        
10. **Notification Service**

- Sends booking confirmations, reminders via email/SMS using SendGrid, Twilio, etc.

11. **Discovery/Search Service**

- Enables users to search/filter movies by title, genre, actor, location, date, etc.


#### GraphQL (Federated Services):

- User Service
- Movie Service
- Schedule Service
- Theater Service
- Booking Service
- Discovery/Search Service

#### REST Services:

- Auth Service
- Access Control Service
- Seat Management Service
- Payment Service
- Notification Service
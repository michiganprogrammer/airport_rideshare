# AirportVan — Business Rules Document
**Version:** 1.0 (MVP)  
**Status:** Draft  
**Owner:** Founder  

---

## 1. Mission
Transport airport passengers to their destination, shared with up to 9 others, in a professional, safe, and efficient manner. One direction only — airport to destination. Never destination to airport.

---

## 2. Scope

**In scope (MVP):**
- Passenger booking and ride management
- Driver assignment and trip execution
- Payment processing
- Live van tracking
- Post-ride receipt and rating

**Out of scope (MVP):**
- Destination to airport pickups
- Scheduling rides in advance beyond same-day
- Multiple airports
- Surge pricing
- Driver-to-driver transfers

---

## 3. Passenger Rules

### Account
- Passenger must provide: name, phone number, email, home address, payment method
- Payment method must be verified before first booking is allowed
- One account per person — no duplicate accounts

### Booking
- Passenger sets destination and departure time
- System shows price and estimated arrival time before passenger commits
- Passenger must agree to ride policy before confirming (no food, no drink, no pets, no smoking)
- Booking for a guest is allowed — guest name required at booking
- Baby seat warning must be shown at booking — passenger is responsible for bringing their own

### Payment
- Payment hold placed immediately upon booking confirmation
- Hold amount = full ride fare
- Actual charge captured when last passenger is dropped off
- If payment hold fails → booking is automatically cancelled, passenger notified

### Cancellation
- Passenger cancels before van departs → no charge
- Passenger does not show within 1 minute of van arrival → van departs, cancellation fee charged
- Cancellation fee = fixed amount (TBD by founder)

### Identification
- Passenger receives unique QR code upon booking confirmation
- QR code must be scanned by driver at boarding
- QR code is single-use — cannot be shared or reused
- If QR code does not match → passenger is denied boarding

### Extra Passengers
- Only booked passengers may board
- If passenger arrives with unbooked guests → deny extra guests
- Passenger may cancel and rebook with correct passenger count

---

## 4. Driver Rules

### Account & Approval
- Driver must provide: name, address, phone number, van details, license plate
- Driver account requires admin approval before activation
- Admin must verify: driver's license, insurance, van inspection
- Driver cannot receive rides until approved

### Availability
- Driver sets availability for next 2 weeks
- Rides are automatically assigned based on availability — no manual acceptance required
- Driver is notified of assignment with: passenger names, destinations, total estimated trip time

### Safety
- Driver may flag any destination as a safety concern at any time
- Flagged destinations are reviewed by admin — no penalty to driver regardless of outcome
- Driver is never penalized for declining a destination on safety grounds

### Trip Execution
- Driver goes to designated airport pickup zone
- Driver scans each passenger QR code at boarding
- Driver confirms each dropoff individually in app as passengers are delivered
- Driver returns to airport after last dropoff

### Van
- Driver inputs maximum seat count for their van at signup
- System never books more passengers than driver's stated max seats
- If van breaks down mid-ride → driver taps problem button in app
- System response to breakdown: attempt to dispatch another van, otherwise arrange Uber for remaining passengers

---

## 5. Ride Rules

### Grouping
- Maximum 9 passengers per van (plus driver)
- Ride departs airport when van is full OR after maximum wait time (TBD)
- Destinations are optimized by system for most efficient route
- Passengers are grouped by geographic proximity of destinations

### In-Ride Policies
- No food or drink
- No pets
- No smoking
- Violations reported by driver → damage/cleaning fee charged to passenger via saved payment method
- Damage fee = $500 or actual cost, whichever is greater

### Gated Communities
- System flags gated community destinations at booking
- Driver is notified in advance
- Passenger is responsible for providing gate access code

### Conduct
- Rude or abusive passenger is reported by driver after ride
- Admin reviews report
- Confirmed violations → passenger added to do-not-rebook list

---

## 6. Post-Ride Rules

- Payment captured automatically when last passenger is dropped off
- Passenger receives email receipt immediately after dropoff
- Passenger is prompted to rate driver (1-5 stars)
- Driver's trip is logged with: pay amount, mileage, passenger count, destinations, duration

---

## 7. Edge Cases — MVP Required

| Scenario | Response |
|---|---|
| Van breaks down | Driver hits problem button, system dispatches replacement or Uber |
| Payment hold fails | Booking cancelled, passenger notified immediately |
| Passenger no-show at 1 minute | Van departs, cancellation fee charged |
| Extra unbooked passengers | Deny boarding, offer rebooking |
| QR code mismatch | Deny boarding |
| Gated community | Flag at booking, notify driver |
| Van damage | Charge damage fee via saved payment |

---

## 8. Edge Cases — Post MVP

| Scenario | Planned Response |
|---|---|
| Unsafe destination area | Driver flags, admin reviews, no driver penalty |
| Rude passenger | Driver reports post-ride, admin reviews, do-not-rebook if confirmed |
| Driver wants alternate route | Notify passengers in app |
| Van seat count varies | Driver sets max seats at signup |
| Guest booking | Allowed, guest name required |
| Baby seat needed | Warning shown at booking |
| Duplicate rider prevention | QR code single-use enforced |

---

## 9. Open Questions (Founder Decisions Needed)

- [ ] What is the maximum wait time before van departs with empty seats?
- [ ] What is the cancellation fee amount?
- [ ] What is the minimum number of passengers to run a ride? (or does 1 passenger pay full van?)
- [ ] How is pricing calculated? (flat rate per zone? per mile? per passenger?)
- [ ] What airports do we launch at first?
- [ ] Do we offer scheduled advance bookings or walk-up only?

---

*Next document: State Machine*

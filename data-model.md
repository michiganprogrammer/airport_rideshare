Rider
─────────────────
id
firstName
lastName
phoneNumber
email

Rider Booking (active)
─────────────────
tripId
riderId
driverId
status              (booked | inVan | completed)
starting_location
current_locationRider
current_locationDriver
ending_location
price
dateStarted
dateEnded
tripTime

Rider Trip (history)
─────────────────
tripId
riderId
driverId
starting_location
ending_location
price
dateStarted
dateEnded
tripTime
goodDriver          (thumbs up | thumbs down)


Driver
─────────────────
id
firstName
lastName
phoneNumber
email
carNumber
driverActive  (ready to accept rides)

Driver Active Booking
─────────────────
tripId
driverId
riderId             (multiple)
starting_location   (one per rider)
ending_location     (one per rider)
price               (one per rider)
riderStatus         (arrived | waiting)

Driver Trip (history)
─────────────────
tripId
driverId
dateStarted                     (one — when trip began)

riderId             (multiple)
starting_location   (per rider)
ending_location     (per rider)
dateEnded           (per rider)
tripTime            (per rider)
price               (per rider)
goodRider           (thumbs up | thumbs down, per rider)

totalPrice          (all riders combined)
totalTripTime       (dateStarted → last dateEnded)

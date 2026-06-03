
Rider

core
As a rider I want to book a ride from the airport so that I can get to my destination
As a rider I want to see the price before I commit a booking so that I know the price
As a rider I want to see the time it will take to reach a destination so that I arrive on time

non-core
As a rider I want to see a trip i took before so that I can have a receipt
as a rider I want to see myself and the driver as he is coming so that I can be ready
as a rider I want to give a rating after the service so that the app knows if the service was good or bad
as a rider I want to see how many stops (riders) before my stop so i feel like I know what will happen (possibly not good idea)
as a rider I want to close the app and it continues to where I was so I'm not in some unknown state
as a rider I want an alert when driver is 1 minute away so that I'm prepared
as a rider I want to be able to cancel incase rider is stuck in traffic so that maybe I find someone closer
As a rider I want to be able to make claim of dangerous drive (distracted etc) so that accidents are prevented 



advanced (will not be implemented in first phase)
as a rider I want to check in by scanning QR code so that the driver knows I'm legit
as a rider I want to be able to auto save payment information
as a rider i want to login with either ID or using phone number


Edge cases
as a rider I want to be able to notify rider if I have a pet so that they can plan accordingly
as a rider I want to be able to submit a video or picture if I make a claim against driver





Driver

core
As a driver I want to enable or disable my online status so that I can take a break
As a driver I want the ability to reject a rider before they come in the car so further problems don't occur
As a driver I want set pay per mile or pay per hour so that I can be profitable

non-core
As a driver I want to see two modes one for directions and one for overall map so that I get an idea of where I'm going (maybe traffic etc)
As a driver I would like to say i'm pumping gas so that it can track how much I paid for gas
As a driver I want to see my total compensation for the day so that I know my pay for that day (or week or month or year )

Edge case
as a driver I want to be able to report car breakdown so that riders can book another driver
as a driver I could get sick therefore status can change
as s driver halfway through drive client could be unacceptable (no necessairly programming problem but need to think of how ot handle it)
as a driver client could be late, need determined time before cancelling trip
as s driver the person who is booked needs to verify their ID
as a driver you can't have three people coming when only one booked
as a driver you can't have customer bring large items
as a driver you need to be able to reject someone for no baby seat
as a driver you can't let someoen dirty come in the car
as a driver you have to consider someone could smoke (need action plan)
as a driver you have to consider once someone is at their destination they might not leave car(maybe their not happy)
as a driver someone could vomit or other medical episodes
as a driver someone could threaten another rider


Rider
- Signup / login flow
- Viewing driver profile before booking
  (name, rating, car, plate number)
- How does rider actually get matched to a driver?
- Setting pickup location
- Payment method setup
- What happens if no drivers are available?

Driver
- Driver signup / onboarding
  (license upload, vehicle details, insurance)
- Rating riders (not just riders rating drivers)
- Viewing their own rating
- Setting service area / coverage zone
- Earnings history (you have daily, but what about history)
- What happens if rider doesn't show up?


You as the platform owner need to:
- Approve / reject driver applications
- Handle claims submitted by riders
- Suspend accounts
- See platform analytics
- Handle disputes between rider and driver

Rider
└── books seat
└── sees their booking
└── gets picked up
└── gets dropped off

Driver
└── sees all booked riders
└── sees all destinations
└── marks each rider as dropped off
└── ends trip


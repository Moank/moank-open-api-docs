# Parners API
> This is a draft, all names can be update later 

---
## Signup

### /bank/get-link
- **req**
  - ssn
  - ?bank
  - ?state
- **steps**
  - validate inputs
  - build tink link based on partner app and inputs
- **res**
  - link

### /bank/fetch-report
- **req**
  - code
  - ssn
- **steps**
  - validate inputs
  - fetch tink token with code
  - get tink preliminary report
- **res**
  - {}

### /application
- **req**
  - ...required application fields
- **steps**
  - sanitase data
  - validate application data
  - create application
  - process application
- **res**
  - ...interesting application fields for partners

---
## Managing

### /application/:id
- **steps**
  - validate id
  - check if application belong to partner
- **res**
  - ...interesting application fields for partners

### /application/:id/event
- **req**
  - event
  - ...event details
- **steps**
  - validate id
  - check if application belong to partner
  - check event value (one of enum)
  - apply event needed logic
- **res**
  - ...interesting application fields for partners

---
## Auth
> Every endpoint is protected with a basic authentication `partnerAuthMiddleware()`

### Auth Middleware
- **steps**
  - decode auth token to fetch username and key
  - fetch partner with username to compare keys
  - if authenticated set `req.partner` with fetched partner

---
## Errors
- **res**
  - code
  - reason
  - errors

---
## Note
> We need to create a tink application for each partner to send them the right redirect url

> We need to go deeper in which field we want to require to take a valid preliminary decision and which ones could be optionals

> We need to define which fields return about the application filtering the ones they don't care and maybe mapping some existing ones

> We need to define the allowed events and what we want to do in that cases

> Righ know adding a partners require:
> - Create tink app
> - Add partner in collection
> - Add new event logic if needed

### Possible application payload
- amount *
- repayment time *
- is borrowing for own use ?
- reason for loan ?
- can be profilled ?
- applicants:
  - ssn ^
  - email ^
  - phone ^
  - is not pep *
  - economic details:
    - kids *
    - monthly income *
    - monthly housing *
    - monthly loans *
    - occupation *
    - mortgage amount ?
    - home type ?
  - bank details:
    - account number ?
    - clearing number ?
    - bank ?

> *: required from decision engine
>
> ^: required to create an user
>
> ?: conditional field

### Possible interesting fields for partners
- ...possible application payload
- status [pending, granted, deny, rejected, payout] based on application step and decision or other things
- decision: [preliminary_granted, granted, deny] based on decision
- offer: interesting data about application offer (amount, interest, etc...)
- agreement url: if application granted

### Possible event payload
- event (es: car-sold, rejected)
- ...data
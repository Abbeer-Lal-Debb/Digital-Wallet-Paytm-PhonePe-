# Digital Wallet (Paytm/PhonePe)

A Digital Wallet like Paytm/PhonePe Wallet, is created with different Microservices performing the different parts of the transaction involved in the wallet.


## Documentation


### Modules:
1. User Service.
2. Wallet Service.
3. Transaction Service.
4. Notification Service.

#### Note: Each of the services have their own servers & should have their own individual databases.

### Flow:
#### Sign-Up Flow:
1. User signs up
2. Create wallet request is initiated in the background

#### Transaction Flow:
1. User logs in
2. Calls the transaction API
3. Initiates the transaction and sends message to Kafka to Process
4. Kafka Listener (Wallet Service): We check the transaction details, and then update the balances and send back the transaction with updated info.
5. Kafka Listener (Transaction Service): Updates the transaction with the relevant status and calls the Kafka Listener again to send the notification for either Success or failure.
6. Kafka Listener (Notification Service): Sends email to the user.

### Database Schema:
#### User:
a. Id, b. Name, c. Email, d. Password, e. Contact Number, f. Authority, g. Time Stamp.

#### Wallet:
a. WalletId, b. Balance, c. Contact, d. Time Stamp

#### Transaction:
a. Id, b. Sender, c. Receiver, d. Amount, e. Status, f. Transaction Time

#### Notification:
a. Id, b. Sender, c. Receiver, d. Message, e. Status, f. SentTime

### Controllers/API:
#### User Module:
1. Create user with authentication
2. Create User:
  a. Sent a message to the user queue to create the wallet for the user.
3. Get User
If the user is already inside the cache, then simply return it or otherwise fetch it from the DB & then return.
4. Load user details on the basis of the username.

#### Wallet Module:
1. Kafka Listener to listen to the User Create and create a wallet, with promotional balance.
2. API for wallet transaction on Sender & Receiver

#### Transaction Module:
#### Transact:
a. Check if the Wallet Receiver Exists from the cache.
b. Check for the User's balance and if it is more than the transaction amount, then process the transaction.
c. Send the notification to the wallet to deduct the amount.

#### Notification Module:
1. Kafka Listener to notify user by sending an Email



## Tech Stack

**Client:**  HTML, CSS, JavaScript

**Programming Language:** Java

**Backend Framework:** Spring Boot

**API:** Rest API

**Database:** MySQL

**Spring Modules:** Hibernate, Spring JPA, Spring Security. 

**Message Queues:** Kafka

**Cache:** Redis

**Microservices**

**Testing tools:** Mockito, Junit, Postman

**Design Pattern:** Builder design Pattern to create the objects.



## Demo

- [Digital Wallet (Paytm/PhonePe)](https://github.com/Abbeer-Lal-Debb/Digital-Wallet-Paytm-PhonePe-)

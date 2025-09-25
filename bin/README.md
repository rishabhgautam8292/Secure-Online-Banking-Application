# Secure-Online-Banking-Application

## Project Goal

The goal of this project is to develop a Bank Security Application that allows users to access various banking services such as account creation, card selection, and investments. The application will include an admin panel for users with admin roles to view user and account information.

## Features

The application provides the following features for users:

- User registration
- Account creation
- Nominee addition for accounts
- Retrieval of card details
- Money investment through bank accounts

## Technology Stack

- **Spring Boot**: Version 2.7.16
- **Database**: MySql (for development/testing)
- **Security**: Spring Security with JWT
- **Tools**: Postman (for API testing)

## Application Structure

### Entities

1. **Account**
   - Fields: `id`, `accountType`, `status`, `balance`, `interestRate`, `branch`, `proof`, `openingDate`, `accountNumber`, `nominee`, `card`, `user`
   - Relationships: One-to-One with Nominee and Card, Many-to-One with User

2. **Card**
   - Fields: `id`, `cardNumber`, `cardHolderName`, `cardType`, `dailyLimit`, `cvv`, `allocationDate`, `expiryDate`, `pin`, `status`

3. **Investment**
   - Fields: `id`, `investmentType`, `risk`, `amount`, `returns`, `duration`, `companyName`, `user`
   - Relationships: Many-to-One with User

4. **Nominee**
   - Fields: `id`, `relation`, `name`, `accountNumber`, `gender`, `age`

5. **Role**
   - Fields: `id`, `roleName`

6. **User**
   - Fields: `id`, `name`, `username`, `password`, `address`, `number`, `identityProof`, `roles`, `accountList`, `investmentList`
   - Relationships: Many-to-One with Role, One-to-Many with Account and Investment

### Enums

- `AccountType`
- `BranchType`
- `CardType`
- `InvestmentType`

### DTOs

1. **AccountDto**
   - Fields: `accountType`, `balance`, `proof`, `nominee`

2. **AdminDto**
   - Fields: `name`, `username`, `password`, `address`, `number`, `identityProof`

3. **CardDto**
   - Fields: `cardHolderName`, `cardType`, `pin`

4. **InvestmentDto**
   - Fields: `investmentType`, `amount`, `duration`

5. **KycDto**
   - Fields: `name`, `address`, `number`, `identityProof`

6. **NomineeDto**
   - Fields: `relation`, `name`, `accountNumber`, `gender`, `age`

7. **UserDto**
   - Fields: `name`, `username`, `password`, `address`, `number`, `identityProof`, `accountList`, `investmentList`

### Repository Interfaces

- **AccountRepository**
  - Methods: `findByAccountNumber`, `findAllActiveAccounts`, `findAllInActiveAccounts`, `findAllByAccountType`, `findAllByBranch`

- **CardRepository**
  - Methods: `findByCardNumber`

- **InvestmentRepository**
- **NomineeRepository**
- **UserRepository**
  - Methods: `findByUsername`

### Security Implementation

- **JWT Authentication**
  - DTOs: `JwtRequest`, `JwtResponse`
  - Classes: `JwtAuthenticationFilter`, `JwtAuthenticationHelper`
  - Service: `CustomUserDetailService` implementing `UserDetailsService`

- **Security Configuration**
  - Beans: `authenticationManager`, `passwordEncoder`
  - Endpoint Exposure: Only `/user/register` and `/user/login` are publicly accessible

### Service Layer

- Each entity will have a corresponding service interface and implementation to manage business logic.
- CRUD operations will be implemented for managing users, accounts, cards, and investments.

### Testing

- Use Postman to test endpoints and ensure data is correctly saved and retrieved from the database.

## Endpoints

### UserController

- **POST** `/user/login`: Logs in the user and assigns a JWT token.
- **POST** `/user/register`: Registers a new user and assigns the role `ROLE_CUSTOMER`.

### UserAccountController

- **POST** `/account/create/{userId}`: Creates an account for the user with a unique account number.
- **GET** `/account/all/{userId}`: Fetches all accounts associated with the user.
- **GET** `/account/balance`: Retrieves the account balance.
- **GET** `/account/nominee`: Fetches the nominee details.
- **PUT** `/account/updateNominee/{accountId}`: Updates the nominee for the account.
- **GET** `/account/getKycDetails`: Fetches KYC details for the user.
- **PUT** `/account/updateKyc/{accountId}`: Updates KYC details.
- **GET** `/account/getAccount/summary`: Fetches account summary.

### UserCardController

- **GET** `/card/block`: Blocks the card associated with the given account number.
- **POST** `/card/apply/new`: Applies for a new card if no card is linked to the account.

## How to Run

1. Clone the repository.
2. Configure the database settings in `application.properties`.
3. Run the Spring Boot application.
4. Use Postman or a similar tool to test the API endpoints.

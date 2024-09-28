# Updated API Structure Outline

## 1. Developer Authentication

### POST /api/v1/developers/login

- Description: Initiate developer login process
- Request body:
  - email: string
- Response:
  - message: string (e.g., "OTP code sent to your email")

### POST /api/v1/developers/verify

- Description: Verify OTP and complete login
- Request body:
  - otp_code: string
- Response:
  - access_token: string
  - wallet_address: string

### GET /api/v1/developers/wallet

- Description: Get the developer's wallet address (for subsequent requests)
- Headers:
  - Authorization: Bearer {access_token}
- Response:
  - wallet_address: string

## 2. Application Management

### POST /api/v1/applications

- Description: Create a new application
- Headers:
  - Authorization: Bearer {access_token}
- Request body:
  - name: string
- Response:
  - application_id: string

### GET /api/v1/applications

- Description: List all applications for the developer
- Headers:
  - Authorization: Bearer {access_token}
- Response:
  - applications: Array<{id: string, name: string}>

## 3. Ticket Management

### POST /api/v1/applications/{application_id}/tickets/deploy

- Description: Deploy a new ticket type for the application
- Headers:
  - Authorization: Bearer {access_token}
- Request body:
  - name: string
  - initial_supply: number
- Response:
  - ticket_id: string
  - interaction_endpoints: Array<{endpoint: string, description: string}>

### PUT /api/v1/applications/{application_id}/tickets/{ticket_id}/supply

- Description: Update the supply of a ticket type
- Headers:
  - Authorization: Bearer {access_token}
- Request body:
  - new_supply: number

### GET /api/v1/applications/{application_id}/tickets

- Description: List all ticket types for an application
- Headers:
  - Authorization: Bearer {access_token}
- Response:
  - tickets: Array<{id: string, name: string, supply: number}>

## 4. End User Management

### POST /api/v1/applications/{application_id}/users

- Description: Onboard end users for an application
- Headers:
  - Authorization: Bearer {access_token}
- Request body:
  - users: Array<{email: string}>
- Response:
  - users: Array<{email: string, wallet_address: string}>

## 5. Ticket Distribution

### POST /api/v1/applications/{application_id}/tickets/{ticket_id}/distribute

- Description: Distribute tickets to end users
- Headers:
  - Authorization: Bearer {access_token}
- Request body:
  - distributions: Array<{email: string, amount: number}>
- Response:
  - distributions: Array<{email: string, amount: number, transaction_id: string}>

## 6. Utility Endpoints

### GET /api/v1/applications/{application_id}/details

- Description: Get the details for an application
- Headers:
  - Authorization: Bearer {access_token}
- Response:
  - id: string
  - name: string
  - ticket_types: Array<{id: string, name: string, supply: number}>

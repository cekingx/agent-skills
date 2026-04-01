# Example Task: Backend Layer

**User Story**: As a token holder, I want to stake my tokens so that I can earn rewards

---

## Task: Create Staking Backend API

**Layer**: Backend

---

### User Story Context

**As a** token holder
**I want to** stake my tokens
**So that** I can earn rewards

---

### Technical Scope

#### Endpoints (New APIs)

**POST** `/api/v1/staking/stake` — Initiates a staking transaction on behalf of the user
- **Authentication**: Required (JWT)
- **Request**:
  ```json
  {
    "amount": "string (token amount in wei)",
    "userAddress": "string (Ethereum address)"
  }
  ```
- **Response (200)**:
  ```json
  {
    "status": "success",
    "transactionHash": "string",
    "amount": "string",
    "timestamp": "number"
  }
  ```
- **Error Codes**:
  - `400` — Invalid amount or below minimum stake
  - `401` — Missing or invalid auth token
  - `422` — Insufficient token balance
  - `500` — Blockchain submission failure

**GET** `/api/v1/staking/balance/:address` — Returns staking balance and rewards estimate for a user
- **Authentication**: None
- **Request**: path param `address` (Ethereum address)
- **Response (200)**:
  ```json
  {
    "address": "string",
    "stakedBalance": "string (wei)",
    "stakingStartTime": "number (unix timestamp)",
    "estimatedRewards": "string (wei)"
  }
  ```
- **Error Codes**:
  - `400` — Invalid Ethereum address format
  - `500` — Failed to fetch from blockchain

**GET** `/api/v1/staking/config` — Returns staking configuration (minimum stake, reward rate, contract address)
- **Authentication**: None
- **Request**: none
- **Response (200)**:
  ```json
  {
    "minimumStake": "string (wei)",
    "contractAddress": "string",
    "rewardRate": "string (percentage)",
    "totalStaked": "string (wei)"
  }
  ```
- **Error Codes**:
  - `500` — Failed to fetch config from contract

#### Database Changes

- New table `staking_transactions`: store user address, transaction hash, amount, status (pending/confirmed/failed), block number, timestamps
- Index on `user_address` and `transaction_hash`

#### Event Listeners

- Listen to `Staked` contract event — update transaction status to confirmed, record block number
- Listen to `Unstaked` contract event — update records accordingly

---

### Notes

- Use `ethers.js` or `web3.js` for blockchain interaction
- Cache `/config` response — changes rarely, 60s TTL is fine
- Add rate limiting on `/stake` endpoint (per user)
- Implement transaction retry logic for failed submissions
- Log all blockchain errors with full context for debugging
- Depends on smart contract task being deployed to testnet first

---

### Implementation Tasks

- [ ] Set up route handlers for all 3 endpoints
- [ ] Implement staking transaction submission logic
- [ ] Implement balance and rewards query logic
- [ ] Implement config fetch from contract
- [ ] Create database migration for `staking_transactions`
- [ ] Set up `Staked` and `Unstaked` event listeners
- [ ] Add transaction status tracking
- [ ] Add input validation and authentication middleware
- [ ] Add error handling and structured logging
- [ ] Write API integration tests against testnet
- [ ] Update API documentation

---

### Definition of Done

- [ ] Code implemented according to specifications
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Merged to main branch
- [ ] Deployed to staging
- [ ] QA verified

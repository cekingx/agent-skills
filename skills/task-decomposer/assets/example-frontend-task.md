# Example Task: Frontend Layer

**User Story**: As a token holder, I want to stake my tokens so that I can earn rewards

---

## Task: Build Staking Frontend UI

**Layer**: Frontend

---

### User Story Context

**As a** token holder
**I want to** stake my tokens
**So that** I can earn rewards

---

### Technical Scope

#### Components

- `StakingForm` — Main form for entering stake amount. Includes input validation, "Max" button, estimated rewards display, and submit trigger.
- `StakingConfirmationModal` — Confirmation dialog shown before submitting. Displays stake amount, estimated gas, and confirm/cancel actions.
- `StakingBalance` — Displays user's current staked balance, staking duration, and estimated rewards. Refreshes periodically.
- `StakingStatus` — Shows transaction status (pending, confirmed, failed) with a link to the block explorer.

#### State to Manage

- Current staked balance and available token balance
- Stake amount input and validation state
- Staking loading/submitting state
- Transaction hash and status
- Error messages

#### API Calls

- `GET /api/v1/staking/config` — on page load, to show minimum stake and reward rate
- `GET /api/v1/staking/balance/:address` — on page load and after successful stake
- `POST /api/v1/staking/stake` — on user confirmation

#### UX Considerations

- Disable submit if amount is below minimum or above available balance
- Show loading spinner while transaction is pending
- Show inline validation errors on the amount input
- Handle wallet not connected state
- Mobile responsive layout

---

### Notes

- Use `ethers.js` for wallet connection (MetaMask / WalletConnect)
- Format all token amounts from wei to human-readable before display
- Use `BigNumber` for all token math to avoid precision issues
- Check token approval status before staking — prompt approval transaction first if needed
- Optimistic UI: update balance display immediately, rollback on error
- Depends on backend API being available — use mock data during development

---

### Implementation Tasks

- [ ] Set up component structure and routing
- [ ] Implement `StakingForm` with input validation
- [ ] Implement `StakingConfirmationModal`
- [ ] Implement `StakingBalance` with auto-refresh
- [ ] Implement `StakingStatus` with block explorer link
- [ ] Connect state management to API calls
- [ ] Integrate wallet connection and token approval flow
- [ ] Add error handling and user-facing error messages
- [ ] Apply responsive styles
- [ ] Write component unit tests
- [ ] Integration test against testnet backend
- [ ] Cross-browser testing

---

### Definition of Done

- [ ] Code implemented according to specifications
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Merged to main branch
- [ ] Deployed to staging
- [ ] QA verified

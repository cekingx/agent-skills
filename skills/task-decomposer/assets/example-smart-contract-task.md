# Example Task: Smart Contract Layer

**User Story**: As a token holder, I want to stake my tokens so that I can earn rewards

---

## Task: Implement Staking Smart Contract

**Layer**: Smart Contract

---

### User Story Context

**As a** token holder
**I want to** stake my tokens
**So that** I can earn rewards

---

### Technical Scope

#### Functions

- `stake(uint256 amount)` — Transfers tokens from user to contract, records staked balance and start time. Public. Emits `Staked`.
- `unstake(uint256 amount)` — Returns staked tokens to user, updates staked balance. Public. Emits `Unstaked`.
- `getStakedBalance(address user)` — Returns current staked balance for a user. Public view.
- `getMinimumStake()` — Returns the minimum allowed stake amount. Public view.

#### Events

- `Staked(address indexed user, uint256 amount, uint256 timestamp)` — emitted on successful stake
- `Unstaked(address indexed user, uint256 amount, uint256 timestamp)` — emitted on successful unstake

#### State Variables

- `stakedBalance` — mapping of user address to staked amount
- `stakingStartTime` — mapping of user address to stake timestamp
- `totalStaked` — total tokens staked across all users
- `minimumStake` — configurable minimum stake amount

---

### Notes

- Reentrancy protection required on `stake` and `unstake`
- Use `SafeERC20` for token transfers (not raw `transfer`)
- Validate `amount > 0` and `amount >= minimumStake` before state changes
- Use Solidity 0.8+ overflow checks or SafeMath
- User must approve the contract to spend their tokens before calling `stake`
- Consider adding a pause mechanism for emergencies

---

### Implementation Tasks

- [ ] Design contract architecture and state variables
- [ ] Implement `stake` function with validation and reentrancy guard
- [ ] Implement `unstake` function
- [ ] Implement view functions (`getStakedBalance`, `getMinimumStake`)
- [ ] Add events and access control modifiers
- [ ] Write unit tests for happy path and edge cases (zero amount, below minimum, insufficient balance)
- [ ] Run gas optimization analysis
- [ ] Deploy to testnet and verify

---

### Definition of Done

- [ ] Code implemented according to specifications
- [ ] All tests passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Merged to main branch
- [ ] Deployed to staging
- [ ] QA verified

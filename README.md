# 🛡️ Blockchain Insurance Smart Contract

A decentralized insurance platform built on the Stacks blockchain that enables automated insurance policy management, claim submission, and payout processing.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Contract Details](#contract-details)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

The Automated Insurance Smart Contract provides a trustless, transparent insurance system where:
- Users can purchase insurance policies with fixed premiums
- Claims are submitted and processed through the blockchain
- Payouts are automated based on admin/oracle decisions
- All transactions are immutable and verifiable

## ✨ Features

### 🏪 Policy Management
- **Fixed Premium**: 1 STX per policy
- **Coverage Period**: 30 days (~52,560 blocks)
- **Maximum Payout**: 5 STX per approved claim
- **Active Policy Tracking**: Real-time policy status validation

### 📝 Claim Processing
- **Claim Submission**: Policyholders can submit claims during active periods
- **Status Tracking**: Pending → Approved/Rejected workflow
- **Automated Payouts**: Instant STX transfers for approved claims
- **Duplicate Prevention**: One claim per policy period

### 🔐 Administrative Controls
- **Admin Management**: Transferable administrative rights
- **Contract Funding**: Admin can fund contract for claim payouts
- **Claim Processing**: Manual approval/rejection system
- **Balance Monitoring**: Real-time contract balance tracking

### 📊 Analytics & Reporting
- Total policies issued
- Total claims submitted
- Approved vs rejected claims ratio
- Contract funding history
- Block height tracking

## 🔧 Contract Details

### Constants
```clarity
POLICY_DURATION_BLOCKS: 52,560 blocks (~1 month)
INSURANCE_PREMIUM: 1,000,000 microstacks (1 STX)
INSURANCE_PAYOUT: 5,000,000 microstacks (5 STX)
```

### Data Structures
- **Policies Map**: `{start-block: uint, active: bool}`
- **Claims Map**: `{block: uint, status: string-ascii}`
- **State Variables**: Balance, counters, and block tracking

### Error Codes
- `ERR_UNAUTHORIZED (100)`: Access denied
- `ERR_POLICY_NOT_FOUND (101)`: Policy doesn't exist
- `ERR_POLICY_ACTIVE (102)`: Policy already active
- `ERR_POLICY_INACTIVE (103)`: Policy not active
- `ERR_CLAIM_NOT_FOUND (104)`: Claim doesn't exist
- `ERR_CLAIM_ALREADY_SUBMITTED (105)`: Duplicate claim
- `ERR_INSUFFICIENT_FUNDS (106)`: Contract underfunded
- `ERR_INVALID_AMOUNT (107)`: Incorrect payment amount
- `ERR_ALREADY_INSURED (108)`: User already has active policy
- `ERR_NOT_INSURED (109)`: User doesn't have active policy
- `ERR_CLAIM_NOT_ELIGIBLE (110)`: Claim not in pending status

## 🚀 Getting Started

### Prerequisites
- [Clarinet](https://github.com/hirosystems/clarinet) installed
- Stacks wallet for testing
- Basic understanding of Clarity smart contracts

### Installation

1. Clone the repository:
```bash
git clone https://github.com/BigUchechukwu1/blockchain-insurance.git
cd blockchain-insurance
```

2. Install dependencies:
```bash
clarinet check
```

3. Run tests:
```bash
clarinet test
```

## 💡 Usage

### For Policyholders

#### 1. Purchase Insurance Policy
```clarity
(contract-call? .Automated-insurance buy-policy u1000000)
```

#### 2. Submit a Claim
```clarity
(contract-call? .Automated-insurance submit-claim)
```

#### 3. Check Policy Status
```clarity
(contract-call? .Automated-insurance get-policy tx-sender)
```

#### 4. Check Claim Status
```clarity
(contract-call? .Automated-insurance get-claim tx-sender)
```

#### 5. Cancel Policy
```clarity
(contract-call? .Automated-insurance cancel-policy)
```

### For Administrators

#### 1. Fund Contract
```clarity
(contract-call? .Automated-insurance fund-contract u50000000)
```

#### 2. Process Claims
```clarity
;; Approve claim
(contract-call? .Automated-insurance process-claim 'SP1ABC...XYZ true)

;; Reject claim
(contract-call? .Automated-insurance process-claim 'SP1ABC...XYZ false)
```

#### 3. Transfer Admin Rights
```clarity
(contract-call? .Automated-insurance set-admin 'SP1NEW...ADMIN)
```

## 📚 API Reference

### Public Functions

#### `buy-policy(amount: uint)`
Purchase an insurance policy.
- **Parameters**: `amount` - Premium amount (must be 1,000,000 microstacks)
- **Returns**: `(ok true)` on success
- **Errors**: `ERR_ALREADY_INSURED`, `ERR_INVALID_AMOUNT`

#### `submit-claim()`
Submit an insurance claim.
- **Returns**: `(ok true)` on success
- **Errors**: `ERR_NOT_INSURED`, `ERR_CLAIM_ALREADY_SUBMITTED`

#### `process-claim(user: principal, approve: bool)`
Process a pending claim (admin only).
- **Parameters**: 
  - `user` - Claimant's principal
  - `approve` - Whether to approve the claim
- **Returns**: `(ok "approved")` or `(ok "rejected")`
- **Errors**: `ERR_UNAUTHORIZED`, `ERR_CLAIM_NOT_FOUND`, `ERR_INSUFFICIENT_FUNDS`

#### `cancel-policy()`
Cancel an active policy.
- **Returns**: `(ok true)` on success
- **Errors**: `ERR_POLICY_NOT_FOUND`, `ERR_POLICY_INACTIVE`

#### `fund-contract(amount: uint)`
Add funds to the contract (admin only).
- **Parameters**: `amount` - Amount to add to contract balance
- **Returns**: `(ok true)` on success
- **Errors**: `ERR_UNAUTHORIZED`

#### `get-policy(user: principal)`
Retrieve policy information.
- **Parameters**: `user` - User's principal
- **Returns**: `(ok (some policy-data))` or `(ok none)`

#### `get-claim(user: principal)`
Retrieve claim information.
- **Parameters**: `user` - User's principal
- **Returns**: `(ok (some claim-data))` or `(ok none)`

## 🧪 Testing

Run the test suite:
```bash
clarinet test
```

Check contract syntax:
```bash
clarinet check
```

Interactive testing:
```bash
clarinet console
```

## 🚀 Deployment

### Testnet Deployment
```bash
clarinet deployments generate --testnet
clarinet deployments apply --testnet
```

### Mainnet Deployment
```bash
clarinet deployments generate --mainnet
clarinet deployments apply --mainnet
```

## 🎯 Use Cases

- **Agricultural Insurance**: Crop protection against weather events
- **Travel Insurance**: Trip cancellation and delay coverage
- **Property Insurance**: Protection against theft or damage
- **Health Insurance**: Medical expense coverage
- **Parametric Insurance**: Automated payouts based on predefined triggers

## 🔮 Future Enhancements

- [ ] Oracle integration for automated claim verification
- [ ] Multi-tier policy options with variable premiums
- [ ] Partial claim payouts
- [ ] Risk-based premium calculation
- [ ] Decentralized governance for claim processing
- [ ] Mobile app integration
- [ ] Multi-asset support (BTC, other tokens)

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/BigUchechukwu1/blockchain-insurance/issues)
- **Discussions**: [GitHub Discussions](https://github.com/BigUchechukwu1/blockchain-insurance/discussions)
- **Documentation**: This README file

## 🙏 Acknowledgments

- [Stacks Foundation](https://stacks.org/) for the blockchain platform
- [Clarinet](https://github.com/hirosystems/clarinet) for development tools
- The Stacks community for support and feedback

---

**Built with ❤️ on Stacks blockchain**

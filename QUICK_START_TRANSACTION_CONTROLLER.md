# Quick Start - Transaction Controller

## 🚀 Quick Setup

### 1. Admin Setup (One-time)
```rust
// Initialize contract
contract.initialize(&admin, &usdc_token, &250);

// Register agent
contract.register_agent(&agent);
```

### 2. User Setup
```rust
// Approve user KYC (admin only)
let expiry = env.ledger().timestamp() + (365 * 24 * 60 * 60); // 1 year
contract.set_kyc_approved(&user, &true, &expiry)?;
```

### 3. Execute Transaction
```rust
// Execute complete transaction
let record = contract.execute_transaction(
    &user,
    &agent,
    &1000,  // amount
    &None   // expiry (optional)
)?;

// Check result
println!("Transaction ID: {:?}", record.remittance_id);
println!("State: {:?}", record.state);
```

## 📋 Common Operations

### Check Transaction Status
```rust
let status = contract.get_transaction_status(&remittance_id)?;
println!("Current state: {:?}", status.state);
```

### Retry Failed Transaction
```rust
let record = contract.retry_transaction(&remittance_id)?;
```

### Blacklist User
```rust
// Admin only
contract.set_user_blacklisted(&user, &true)?;
```

### Check KYC Status
```rust
if contract.is_kyc_approved(&user) {
    // User can transact
}
```

## ⚠️ Error Handling

```rust
match contract.execute_transaction(&user, &agent, &1000, &None) {
    Ok(record) => {
        println!("Success! TX ID: {:?}", record.remittance_id);
    }
    Err(ContractError::KycNotApproved) => {
        println!("User needs KYC approval");
    }
    Err(ContractError::UserBlacklisted) => {
        println!("User is blacklisted");
    }
    Err(e) => {
        println!("Transaction failed: {:?}", e);
    }
}
```

## 🔑 Key Features

✅ **Automatic Validation** - Blacklist and KYC checks  
✅ **Automatic Rollback** - Refunds on failure  
✅ **Retry Logic** - Up to 3 automatic retries  
✅ **Audit Trail** - Complete transaction history  
✅ **Security** - Admin controls and authorization  

## 📚 Full Documentation

- **API Reference**: `TRANSACTION_CONTROLLER.md`
- **Implementation Details**: `TRANSACTION_CONTROLLER_IMPLEMENTATION.md`
- **Tests**: `src/test.rs` (search for "Transaction Controller Tests")

## 🆘 Troubleshooting

| Error | Solution |
|-------|----------|
| `KycNotApproved` | Admin must approve user KYC |
| `UserBlacklisted` | Admin must remove from blacklist |
| `KycExpired` | Admin must renew KYC with new expiry |
| `AgentNotRegistered` | Admin must register agent |

## 🔗 Pull Request

Branch: `feature/transaction-controller`  
PR Link: https://github.com/zeekman/QuickTransmit/pull/new/feature/transaction-controller

---

**Ready to use!** 🎉

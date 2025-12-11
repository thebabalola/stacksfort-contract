# Deploying Clarity 4 Contracts to Mainnet

## Overview

Since Clarinet 3.6.1 doesn't support Clarity 4 in the config file, but Clarity 4 is live on mainnet, we need to use a workaround for deployment.

## Deployment Steps

### 1. Generate Deployment Plan

```bash
clarinet deployments generate --mainnet --low-cost
```

This creates a deployment plan in `deployments/default.mainnet-plan.yaml`.

### 2. Edit Deployment Plan for Clarity 4

Open the generated deployment plan and change:

```yaml
# Before (Clarity 3)
- contract-publish:
    contract-name: multisig
    expected-sender: YOUR_ADDRESS
    path: contracts/multisig.clar
    clarity-version: 3  # <-- Change this

# After (Clarity 4)
- contract-publish:
    contract-name: multisig
    expected-sender: YOUR_ADDRESS
    path: contracts/multisig.clar
    clarity-version: 4  # <-- Changed to 4
```

### 3. Deploy to Mainnet

```bash
clarinet deployment apply -p deployments/default.mainnet-plan.yaml
```

## Important Notes

1. **Local Testing**: You can still write Clarity 4 code, but `clarinet check` may not validate Clarity 4 features
2. **Testnet First**: Always test on testnet before mainnet
3. **Manual Validation**: Review your Clarity 4 code carefully since local tools have limited support
4. **Deployment Plan**: Always verify the `clarity-version: 4` in your deployment plan before deploying

## Testing Clarity 4 Features

Since local testing is limited:
1. Deploy to testnet first with `clarity-version: 4`
2. Test all Clarity 4 features on testnet
3. Verify everything works before mainnet deployment

## Clarity 4 Features We're Using

- `restrict-assets?` - Post-conditions for token transfers (Issue #7)
- `stacks-block-time` - Transaction expiration (Issue #15)
- `contract-hash?` - Token contract verification (Issue #7)
- `to-ascii?` - Enhanced logging (Issues #2, #6, #7)

See [CLARITY4-IMPLEMENTATION-PLAN.md](./CLARITY4-IMPLEMENTATION-PLAN.md) for details.


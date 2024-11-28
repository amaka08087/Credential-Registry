# Credential Verification Smart Contract

## Overview
This smart contract enables educational institutions to issue and verify digital credentials on the Stacks blockchain. It provides a secure, transparent, and decentralized system for managing academic credentials, professional certifications, and other educational achievements.

## Features
- Institution registration and verification
- Credential type management
- Credential issuance and verification
- Credential revocation
- Expiration management
- Role-based access control

## Contract Structure

### Data Maps
1. **authorized-educational-institutions**
   - Stores information about registered educational institutions
   - Contains institution name, website, and verification status
   - Indexed by institution's principal address

2. **credential-records**
   - Stores all issued credentials
   - Contains issuance details, expiration, credential type, and status
   - Indexed by credential UUID and recipient's address

3. **supported-credential-categories**
   - Defines valid credential types
   - Specifies description and validity period for each type
   - Indexed by category ID

## Main Functions

### Administrative Functions
- `transfer-ownership(new-owner-address)`: Transfers contract ownership
- `verify-education-provider(provider-address)`: Verifies an educational institution

### Institution Management
- `register-education-provider(name, url)`: Registers a new educational institution
- `get-education-provider-profile(address)`: Retrieves institution details

### Credential Management
- `register-credential-category(id, description, validity-period)`: Creates new credential type
- `issue-credential(uuid, recipient, type, hash, metadata)`: Issues new credential
- `revoke-credential(uuid, recipient)`: Revokes an issued credential
- `verify-credential-status(uuid, recipient)`: Checks credential validity
- `get-credential-record(uuid, recipient)`: Retrieves credential details

## Error Codes
- `ERR-NOT-AUTHORIZED (u100)`: Unauthorized access attempt
- `ERR-DUPLICATE-INSTITUTION (u101)`: Institution already exists
- `ERR-RECORD-NOT-FOUND (u102)`: Requested record not found
- `ERR-UNSUPPORTED-CREDENTIAL-TYPE (u103)`: Invalid credential type
- `ERR-CREDENTIAL-STATUS-REVOKED (u104)`: Credential has been revoked
- `ERR-CREDENTIAL-STATUS-EXPIRED (u105)`: Credential has expired

## Usage Examples

### Registering an Institution
```clarity
(contract-call? .credential-verification register-education-provider 
    "University of Blockchain" 
    "https://uniblock.edu"
)
```

### Issuing a Credential
```clarity
(contract-call? .credential-verification issue-credential
    "grad-2024-001"  ;; credential UUID
    'SPAB...'        ;; student address
    "bachelor-degree" ;; credential type
    0x1234...        ;; document hash
    "Bachelor of Computer Science, Magna Cum Laude"
)
```

### Verifying a Credential
```clarity
(contract-call? .credential-verification verify-credential-status
    "grad-2024-001"
    'SPAB...'
)
```

## Security Considerations

1. **Access Control**
   - Only verified institutions can issue credentials
   - Only the contract owner can verify institutions
   - Only the issuing institution can revoke its credentials

2. **Data Integrity**
   - Credential hashes stored on-chain
   - Immutable record of issuance and revocation
   - Transparent verification process

3. **Expiration Management**
   - Automatic expiration based on block height
   - Configurable validity periods per credential type
   - Clear distinction between expired and revoked states

## Best Practices

1. **Institution Registration**
   - Use official institution details
   - Maintain current website URL
   - Verify institution status before issuing credentials

2. **Credential Issuance**
   - Use unique, meaningful UUIDs
   - Include comprehensive metadata
   - Calculate accurate document hashes

3. **Credential Management**
   - Regularly check credential validity
   - Document revocation reasons
   - Monitor expiration dates

## Integration Guidelines

1. **Smart Contract Integration**
   ```clarity
   ;; Contract definition in Clarinet
   (impl-trait 'SP000...credential-trait.credential-issuer)
   ```

2. **Frontend Integration**
   - Use Stacks.js for contract interactions
   - Implement proper error handling
   - Cache institution and credential type data

3. **Backend Integration**
   - Maintain credential metadata database
   - Implement hash calculation service
   - Handle blockchain event notifications
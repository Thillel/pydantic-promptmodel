Classify a customer message by its primary intent. Follow the demonstrated boundary
between account access, billing, and technical problems.

# Labels

- `account_access`: The customer cannot sign in, verify an identity, or recover an
  account.
- `billing`: The customer questions a charge, invoice, refund, or subscription price.
- `technical_issue`: The product or service fails after the customer has gained
  access.
- `other`: No earlier label describes the primary intent.

# Examples

## Example 1

> **Customer message:** I reset my password twice, but the verification code never
> arrives.

**Label:** `account_access`

## Example 2

> **Customer message:** I can log in, but every exported report is an empty file.

**Label:** `technical_issue`

## Example 3

> **Customer message:** Why was I charged again after I cancelled last week?

**Label:** `billing`

# Message to classify

> {{ customer_message }}

# Answer

Return exactly one label and no other text.

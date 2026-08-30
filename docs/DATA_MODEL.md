
### `docs/DATA_MODEL.md`

```markdown
# Data Model

This document describes domain concepts.

It is not yet a final SQL schema.

## Household

Represents one household budget.

Potential fields:

- Id
- Name
- Currency
- TimeZone
- CreatedAt

The initial product may support one household per installation.

## User

Represents a person allowed to use the application.

Potential fields:

- Id
- HouseholdId
- DisplayName
- Username or Email
- PasswordHash
- Role
- Active

## Account

Represents a financial account.

Fields:

- Id
- HouseholdId
- Name
- AccountType
- Active
- Notes

Possible account types:

- Checking
- Savings
- CreditCard
- Cash
- Investment
- Loan
- Other

## IncomeSource

Represents a recurring source of income.

Fields:

- Id
- HouseholdId
- Name
- Active
- EmployerOrSource
- Recipient
- ScheduleType
- DepositAccountId
- FirstPayDate
- PayDay1
- PayDay2
- ProjectionMethod
- StartingProjection
- MinimumPay
- ProjectionWeight
- ActualWeight

## Paycheck

Represents one paycheck occurrence.

Fields:

- Id
- IncomeSourceId
- PayDate
- ProjectedAmount
- ActualAmount
- Notes
- Status

Paychecks must preserve historical actual values.

## RecurringBill

Represents the definition of a recurring obligation.

Fields:

- Id
- HouseholdId
- Name
- Active
- Provider
- Frequency
- FirstDueDate
- BudgetedAmount
- PaymentAccountId
- Notes

## BillOccurrence

Represents a specific recurring bill occurrence.

Fields:

- Id
- RecurringBillId
- BillingPeriod
- DueDate
- BudgetedAmount
- ActualAmount
- Paid
- PaidDate
- Notes

Occurrences should be generated deterministically.

## InternalTransfer

Represents a recurring planned movement of money between household accounts.

Fields:

- Id
- HouseholdId
- Name
- Active
- FromAccountId
- ToAccountId
- AmountPerPaycheck
- StartDate

## InternalTransferIncomeSource

Join table identifying which income sources trigger a transfer.

Fields:

- InternalTransferId
- IncomeSourceId

## TransferOccurrence

Represents one planned transfer.

Fields:

- Id
- InternalTransferId
- IncomeSourceId
- PayDate
- PlannedAmount
- ActualAmount
- Transferred
- TransferDate
- Notes

## RevolvingDebt

Represents debt where the balance can rise and fall.

Examples:

- credit card,
- store card,
- line of credit,
- HELOC.

Fields:

- Id
- HouseholdId
- Name
- Active
- Creditor
- AccountId
- DebtType
- CreditLimit
- Apr
- MinimumPayment
- PlannedPayment
- FirstStatementDate
- FirstDueDate
- StartingExpectedBalance

## RevolvingStatement

Represents one revolving debt cycle.

Fields:

- Id
- RevolvingDebtId
- StatementDate
- DueDate
- StatementBalance
- ExpectedBalance
- MinimumPayment
- PlannedPayment
- ActualPayment
- NewChargesOrCredits
- Notes

Business rule:

If StatementBalance exists, it is authoritative for that cycle.

Otherwise ExpectedBalance is used.

## MajorLoan

Represents installment debt.

Examples:

- Mortgage
- Auto Loan
- Student Loan
- Personal Loan
- HELOC when modeled as installment debt

Fields may include:

- Id
- HouseholdId
- Name
- Active
- Lender
- LoanType
- ScheduleType
- StartingBalance
- BasePrincipalAndInterest
- BaseEscrow
- PlannedExtraPrincipal
- InterestRate
- FirstDueDate

## LoanPayment

Represents one scheduled loan payment.

Fields:

- Id
- MajorLoanId
- DueDate
- RequiredPayment
- ActualPayment
- PrincipalPaid
- InterestPaid
- EscrowPaid
- ExtraPrincipal
- BalanceAfter
- Notes

## AuditEntry

Records significant changes.

Potential fields:

- Id
- HouseholdId
- UserId
- EntityType
- EntityId
- Action
- BeforeJson
- AfterJson
- CreatedAt

Audit history should not replace normal domain history.

## ApplicationSetting

Stores installation-level or household-level settings.

Examples:

- timezone,
- currency,
- backup preferences.
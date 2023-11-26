# binance-transaction-history-docs
Unofficial documentation for the binance.com transaction history CSV file format

## Introduction

This is about the CSV files that can be exported at `Order | Transaction History | Export Transaction Records`.

Apparently, there is no official documentation for the CSV file format. I could not find any, and only found [other users who could not find any either](https://dev.binance.vision/t/docs-for-transaction-history-csv-files/9777). All I know about it is from analysis of the files I have exported myself, communication with the Binance support and third-party sources.

## Completeness and correctness

At least in the past, the CSV files had incomplete data. I vaguely remember that margin account data was once missing, which is now present. Also, the crypto portfolio/tax service Cointracking has documented [multiple missing kinds of data](https://cointracking.freshdesk.com/en/support/solutions/articles/29000039887) missing data in the CSV. Unfortunately, the information therein is very vague.

As of November 2023, the Binance support claims that the CSV file is complete, i.e. contains all of the user's transactions. However, I have observed that this is not quite true.

### Impossibility to calculate a correct balance from the transactions

Theoretically, it should be easy to calculate the balance of any combination of account and coin for any given point in time by adding up the values of the column *Change* for all transactions which have the appropriate values for the columns *Account*, *Coin* and *UTC_Time*. However, when doing this, incorrect balances may arise - particularly, incorrectly negative balances.

Until recently, one of the cuplrits for this where incorrect transfer transaction. A coin could have a *transfer_out* transaction without any matching *transfer_in* transaction, suggesting the coin had been withdrawn from Binance, even though this was not the case. As of late November 2023, Binance seems to have fixed this issue, replacing the single *transfer_in* transaction with a pair of *Transfer Between Main Account/Futures and Margin Account* transactions.

As of late November 2023, I am currently in contact with Binance to work on the remaining inconsistencies.

### Fees for fiat purchases are missing

The fees for purchases with fiat - which are exported via API endpoint `/sapi/v1/fiat/payments` - are not represented.

### Some data is aggregated

At least staking rewards may appear aggregated into fewer transactions in the CSV file than in the API.

## Peculiarities

### No transaction ID

The transactions do not have IDs. Thus, a CSV file can contain multiple transactions that look identical.

### Different type of timestamp than in the API

Both CSV and API provide transaction timestamps. However, these can be different types of timestamps at least in some cases. According to the Binance support, in the API it is sometimes the timestamp of calculation, while in the CSV it is the timestamp of credit to the account.

## Columns

### Account

Incomplete list of possible values:

- Cross Margin
- Earn
- Isolated Margin
- Spot

### Operation

If you enable the export option *Hide transfer record*, *transfer_in* and *transfer_out* tranactions will be hidden, but not *Transfer Between Main Account/Futures and Margin Account* transactions.

It seems that in November 2023, new operation types have been added. At least for my data - for the same time interval - new operation types appeared after Binance had informed me about bug fixes.

Incomplete list of possible values:

- Airdrop Assets
- Asset Recovery
- BNB Fee Deduction
- BNB Vault Rewards
- Binance Convert
- Borrow
- Broker Withdraw
- Buy
- Collateral Transfer
- Crypto Loan - Borrow
- Crypto Loan - Collateral Added
- Crypto Loan - Collateral Removed
- Crypto Loan - Collateral Return
- Crypto Loan - Collateral Return after Liquidation
- Crypto Loan - Collateral Spent
- Crypto Loan - Repayment
- Deposit
- Distribution
- ETH 2.0 Staking
- ETH 2.0 Staking Rewards
- Fee
- Fiat Deposit
- Fiat Withdraw
- Fiat Withdrawal
- Flexible Loan - Borrow
- Flexible Loan - Collateral Added
- Flexible Loan - Collateral Removed
- Flexible Loan - Collateral Return
- Flexible Loan - Collateral Spent
- Flexible Loan - Repayment
- Isolated Margin Liquidation
- Isolated Margin Loan
- Isolated Margin Repayment
- Sell
- Simple Earn Flexible Airdrop
- Simple Earn Flexible Interest
- Simple Earn Flexible Redemption
- Simple Earn Flexible Subscription
- Simple Earn Locked Redemption
- Simple Earn Locked Rewards
- Simple Earn Locked Subscription
- Staking Purchase
- Staking Redemption
- Staking Rewards
- Token Swap - Redenomination/Rebranding
- Transaction Buy
- Transaction Fee
- Transaction Related
- Transaction Revenue
- Transaction Sold
- Transaction Spend
- Transfer Between Main Account/Futures and Margin Account
- Withdraw
- transfer_in
- transfer_out

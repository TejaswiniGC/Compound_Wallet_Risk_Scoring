Wallet Risk Scoring Model
This document explains how a risk score (0-1000) is assigned to cryptocurrency wallets that interact with the Compound lending platform.

1. How We Got the Data
I aimed to get real transaction data from the Compound protocol using The Graph, a tool that organizes blockchain information.

Challenges: I tried to get data from Compound V2 and V3 on both Ethereum and Polygon networks. However, I couldn't get any live transaction data for the provided wallets. This happened because either these specific wallets didn't have activity on Compound or the public data access points (APIs) now require special keys.

My Solution: To still complete the task and show how the system works, I created my own small, synthetic dataset of Compound transactions. This synthetic data includes different types of actions like depositing, borrowing, repaying and even some liquidations. This allowed me to build the rest of the scoring model.

2. What Feature I Used
To understand a wallet's risk, I looked at different pieces of information from its transactions. These are called "features":

total_tx_count: How many transactions the wallet made.

wallet_age_days: How long the wallet has been active on Compound.

total_supply_usd: The total value of assets the wallet deposited.

total_borrow_usd: The total value of assets the wallet borrowed.

total_repay_usd: The total value of assets the wallet repaid.

num_liquidations_as_borrower: How many times the wallet's loan positions were forcibly closed (this is a very bad sign).

repayment_ratio_usd: How much of the borrowed money was repaid (e.g, 1.0 means all repaid).

borrow_to_supply_ratio_usd: How much was borrowed compared to what was deposited (high means more risky).

num_withdraws: How many times the wallet withdrew assets.

3. How I Calculated the Risk Score
I used a simple rule-based system to give each wallet a score from 0 to 1000.

Starting Point: Every wallet begins with a neutral score of 500.

Good Behaviors (Increase Score):

Older wallets get a higher score.

Wallets that deposit more money get a higher score.

Wallets with a high repayment ratio (meaning they repay their loans well) get a much higher score.

Wallets that have repaid many times get a higher score.

Bad Behaviors (Decrease Score):

Liquidations (num_liquidations_as_borrower): This causes a very big drop in score. Each liquidation means the wallet failed to manage its loan.

High Borrow-to-Supply Ratio: Borrowing too much compared to what's deposited lowers the score.

Frequent Withdrawals: Withdrawing assets often (compared to deposits) can slightly lower the score.

Final Score: The score is always kept between 0 and 1000.

Why These Scores Matter:

These indicators directly show how financially healthy and reliable a wallet is on a lending platform:

Liquidations: The biggest sign of risk. It means the user couldn't handle their loan.

Repayment Ratio: Shows if a user is good at paying back what they borrow.

Borrow-to-Supply Ratio: Shows how much risk a user is taking by borrowing against their deposits.

Wallet Age & Activity: Longer, consistent activity usually means a more experienced and trustworthy user.

Total Supplied Value: A large amount of deposited money suggests a user who cares about their funds and the platform.

This method is easy to understand and can be used for many wallets, even with our simulated data, to demonstrate the full process of risk scoring.
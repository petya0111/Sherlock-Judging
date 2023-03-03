# Surge Judging
https://github.com/sherlock-audit/2023-02-surge-judging-petya0111

```

001-H First depositor can inflate share price and steal funds from other users
002-H A malicious depositor can systematically manipulate utilisation rate to bring the collateral ratio to the level she wants
003-H According to the documentation, the protocol states it accepts all ERC20 tokens. However it doesn't have the necessary measures to support fee-on-transfer tokens correctly.
004-H Frontrunning with allowence can cause users to loose funds.
005-H The borrower can lose funds by calling more than once the function `Pool.liquidate`, which is used to repay loan tokens debt.
006-H In certain circumstances it is possible that borrowers might not be able to pay theirs debt, and so their collateral tokens will be locked.
007-H Transfer does not check whether the msg.sender has enough tokens
008-H Low-level calls always return true if the input parameter type is zero address or EOA account, which causes no tokens to actually be transferred, and the functions calling `safeTransfer` and `safeTransferFrom` are affected uncontrollably.
009-H Potential reentrancy in Pool.withdraw
010-H Users borrowing at maximum collateral ratio can be immediately liquidated
011-H deposit() front-run steal funds
012-H Sandwich attack allows users to get more loan token
013-H Deposits can be fully sandwich attacked, leading to loss of funds for attacked users
014-H A user can manipulate Loantoken balance to manipulate shares in a grieving attack
015-H Pool.borrow calculates _newUtil rartio incorrectly and can wrongly restrict user from borrowing
016-H A liquidator can gain not only collateral, but also can reduce his own debt!
017-H Liquidations would freeze when collateral price falls faster than drop in collateral ratio, ie. longer `collateralRatio fall duration` can freeze liquidations and lead to total loss for depositors
018-H If collateral token has a blacklist (like USDC / USDT/ ...), then removeCollateral() does not work for user.
019-H Zero address can be set as a fee reciever with a non-zero fee.
020-H 






001-M The variable `Pool.allowance` should be incremented or decremented by using the ERC20 standard method `increaseAllowance()` instead of `approve()` which as shown is susceptible to front-running.
002-M Hardcoding decimals in a Solidity ERC20 smart contract can create issues with interoperability and user experience.
003-M Integer overflow on lender withdraw
004-M Centralization risk: operator have privileges: set the fee recipient, set fee and add operator
005-M No check for whether the user has enough collateral to borrow the requested amount
006-M It is possible front-run attack in `reapy()`
007-M Some ERC20 tokens deduct a fee on transfer
008-M liquidate() Inexistent Slippage Protection
009-M ERC20 transfer zero amount can be reverted
010-M Pool share tokens can be burnt unexpectedly
011-M Pools could be misconfigured when the collateral and loan tokens do not have the same decimals
012-M Inefficient check in Pool.liquidate for repay amount
013-M No gap between user's collateral ratio and the current collateral ratio
014-M State changes in a single block.timestamp are not taken into account
015-M Pool approve/transferFrom is vulnerable to race condition
016-M Pool tokens are susceptible to double use of an allowance
017-M Protocol assumes that all ERC20 tokens have 18 decimals. Accounting functions will be greatly impaired.
018-M A malicious user can create a DOS attack to deposit()
019-M DOS repay function
020-M DoS attack in deploySurgePool
021-M `safeTransfer` and `safeTransferFrom` lacks contract check
022-M Positive feeMantissa WIth Zero feeRecipient
023-M Adding collateral to zero address locks collateral forever
024-M Missing deadline check in key functions withdraw Deposit
025-M Limited support for non-rebasing ERC20 Tokens
026-M Potential DoS when a user calls liquidate with the total debt as the amount
027-M Pool fees are incorrectly calculated which may allow users to avoid fees entirely
028-M Grieving attack by failing user repay transaction
029-M Pool shares can be sent to the zero address as fees
030-M A malicious depositor can increase the interest payment of borrowers by manipulating the compounding frequency of borrowing
031-M Attacker can use reorg attack in order to steal depositors funds




```
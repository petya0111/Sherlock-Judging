# OlympusDAO Judging
https://github.com/sherlock-audit/2023-02-olympus-judging-petya0111

```
001-H In [WstethLiquidityVault.sol] we have `_withdraw()` function.  It is possible reentrancy there.
002-H In [WstethLiquidityVault.sol]() we have `_getPoolOhmShare()` and `getUserWstethShare()` functions. These functions will return an incorrect value of `bptTotalSupply` because they use `totalSupply` instead of `virtualSupply`.
003-H Users can get double rewards ```rewardDebtDiff``` is always greater than  ```userRewardDebts[msg.sender][rewardToken] resulting in double rewards for users.
004-H User's rewards will be locked for a period of time _claimInternalRewards
005-H deposit()  and withdraw() will revert when Balancer pool is paused.
006-H `_claimInternalRewards` does not update contract state after claiming, allowing for continuous internal reward token claims via `claimRewards`
007-H Potential array manipulation vulnerabilities in removeVault function
008-H OHM Token contract allows unauthorized burning, causing an overflow or underflow.
009-H Decimal error in reward debt handling accumulatedRewardsPerShare
010-H user can still get same rewards after withdraw
011-H Incorrect caching of rewards by the `withdraw` function
012-H Reverts on reward claims and withdrawals. Permanently stuck LP tokens
013-H Liquidity Vault can be drained
014-H Missing Reset of newAdmin Variable in RolesAdmin Contract.
015-H Reentrancy attack possible through burnFrom function in Burner contract of Olympus Protocol.
016-H Flashloan attack to get lots of OHM at very low cost
017-H An attacker can easily drain the contract by calling the `claimRewards()` function multiple times. 
018-H Removed reward tokens will no longer be claimable and will cause loss of funds to users who haven't claimed
019-H Last claimed timestamp for internal rewards is not updated resulting in the theft of LDO tokens
020-H Vault may prevent withdraw / deposit due to outdated oracle price
021-H Impermanent loss different from stated in docs
022-H `cachedUserRewards` and `userRewardDebts` shouldn’t be divided by 1e18 in `internalRewardsForToken()` and `externalRewardsForToken()
023-H In `_withdrawUpdateRewardState()` the `cachedUserRewards` is updated after setting `userRewardDebts` to zero. It should be updated before that. This causes the `userRewardDebts` not to be subtracted from the `cachedUserRewards` which will be larger than expected. This allows to steal rewards.




001-M No upper bound for `FEE` in SingleSidedLiquidityVault.sol allows `liquidityvault_admin` to set fee to 100% and steal users rewards
002-M liquidityvault_admin can rug pull the vault by calling `deactivate()` and then `rescueToken()`
003-M Possible DoS in `claimFees()` if any internal or external reward token revert of transfer of `0` tokens amount
004-M No check for internal and external rewards tokens being the same
005-M Solmate safeTransfer and safeTansferFrom does not check the code size of the token address
006-M In [WstethLiquidityVault.sol]() we call `auraPool.booster.deposit(auraPool.pid, lpAmountOut, true)` and do not check the returned value.
007-M In [WstethLiquidityVault.sol]() we have function `_withdraw()` and function `rescueFundsFromAura()`. These functions call `auraPool.rewardsPool.withdrawAndUnwrap` but the returned value is not checked.
008-M `getExpectedLPAmount()` have lack of slippage control
009-M Missing deadline check in `deposit()` function
010-M Unbounded array in `SingleSidedLiquidityVault.sol`
011-M Unbound loop can cause DOS in protocol _accumulateExternalRewards
012-M `pairToken` with 24 decimals is not supported.
013-M In `getMaxDeposit()`, it uses 18 instead of `pairToken` decimals while converting OHM to `pairToken`.
014-M The deposit `LIMIT` variable is updated incorrectly, allowing the deposit limit to xbe circumvented and leading to external contracts working with incorrect OHM emissions.  ohmRemoved
015-M ActiveVaultCount Variable Can Overflow and Underflow.
016-M In addInternalRewardToken, when startTimestamp_ > block.timestamp, _accumulateInternalRewards will revert due to overflow
017-M If a duplicate vault address has been added then removing vault will only remove the first instance and the second instance of vault address will still remain active `removeVault`
018-M When addInternalRewardToken/addExternalRewardToken re-add previously removed reward tokens, it will prevent users from claiming rewards
019-M  claiming external rewards via `externalRewardsForToken` fails in some cases even when users are entitled to external rewards
020-M Chainlink has no stETH/USD feed on mainnnet
021-M Any unclaimed rewards will be lost when the admin removes a reward token
022-M configureDependencies fails to remove allowance from an old MINTR
023-M addExternalRewardToken() fails to set the inititial balance to the correct value.
024-M DOS attack to getUsers()
025-M Creating an internal reward token with a start timestamp in the future will cause deposits & withdrawals to revert
026-M Absence of `emergencyWithdraw` feature
027-M As per audit instructions, deactivating a contract should prevent deposits. But OlympusTreasury::repayDebt is missing the onlyWhileActive decorator, therefore deposits (repayments) remain possible even if the contract is deactivated.
028-M `getMaxDeposit` returns less then it should; possibly resulting in fewer tokens being bought
029-M `THRESHOLD` can be set to values that would endanger protocol by allowing manipulated oracle prices
030-M The function`getUserWstethShare` is unused in the contracts, and is marked internal, thus unreachable.
031-M `deposit()` is supposed to return the amount of LP tokens received through the named return parameter [`lpAmountOut`] However, `lpAmountOut` is not set in the method body, resulting in the method always returning zero.
032-M When reward token is being removed it should send rewards to users
033-M SingleSidedLiquidityVault._accumulateInternalRewards will revert with underflow error if rewardToken.lastRewardTime is bigger than current time
034-M WstethLiquidityVault contract will be needed to redeploy as getUserWstethShare function is internal instead of external
035-M Missing deduplication can result in vaults not being correctly removed using `removeVault` in `OlympusLiquidityRegistry`
036-M Without a sanity check for a new internal reward token, it can be an external reward token or duplicated.
037-M PairTokenDeposits can be 0 while still having deposits in. And can be > 0 while having no deposits in.
038-M `removeInternalRewardToken()`  and `removeExternalRewardToken()` can lead to the waste of gas due to index shifting.
039-M If reward token is added users will loose thier claim to that token. And those funds will be distributed to undeserving users.
040-M Admin are able to remove tokens from the reward list in `removeExternalRewardToken()`. This is excpected to be done when  rewards are not being offered anymore from external sources or internal. 
041-M Admin removing rewardtoken issues no warning to users
042-M If the vault is not active ,user's assets will be frozen in the protocol. The same with claiming rewards.
043-M It appears that the constructor of a smart contract does not include an integrity check on the contract itself.
044-M SingleSidedLiquidityVault.activate can be activated multiple times
045-M Reward tokens can't be added again once they are removed because there is no way to reset the user's previous debt/cache state.
046-M While validating the ohm amount for deposit, it uses different conditions in `_canDeposit()` and `getMaxDeposit()`.
047-M SingleSidedLiquidityVault.deposit frontrun
048-M The function `setDebt` in `OlympusTreasury.sol` can be front run.
049-M Some tokens like USDT do not allow approving an amount `M > 0` when an existing amount `N > 0` is already approved.
050-M auraPool.booster.deposit boolean return value not handled in WstethLiquidityVault.sol
051-М `_hasDeposited` remains true after a user withdraws causing state changes to occur on a user who has withdrawn
052-M All claims may revert if reward is equal to fee amount for any single reward
053-M Pool price in Balancer Pool can be miscalculated
054-M The liquidity vault uses 3 oracles. Being the least reliable connected contracts, there should be an admin function to replace them in case they stop operating. Currently there is no way to do that.
055-M In the `withdraw` function of `SingleSidedLiquidityVault.sol`, there is a lack of validation for for `minTokenAmounts_ `, which could lead to possible attacks e.g. sandwich attacks.
056-M The vault uses `_isPoolSafe()` function that checks whether the pool price is `THRESHOLD` away from the oracle price and prevents withdraw / deposits if it is.
057-M Adding reward token with future timestamp may break vault
058-M If an internal token startTimestamp_ ( The timestamp at which to start distributing rewards ) is higher that block.timestamp first depositor can earn rewards before startTimestamp_ 
059-M `abstracts/SingleSidedLiquidityVault.sol` doesn't handle fee-on-transfer pairToken.
060-M In `SingleSidedLiquidityVault.sol` we have function [withdraw()] A user can only use this function when `onlyWhileActive` modifier is true. 
061-M The Aura/Balancer community DAO's behavior of adding extra rewards can break the caveats around external reward token of the SSLV
062-M If denominator > numerator ,in that case 0 will be returned ,causing miscalculation
063-M when first depositor made first deposit the updateInternalRewardState function sets lastRewardTime to block.timestamp without checking lastRewardTime has arrived 
064-M claiming rewards can be started before startTimestamp_
065-M The `deactivate()` function of `SingleSidedLiquidityVault.sol` removes the vault from the registry.
066-M SingleSidedLiquidityVault.sol contract uses the PRECISION as 1000. since the ERC tokens mentioned in the document has different decimal values, precision based calculation could affect the AURA.
067-M `pairTokenDecimals` VARIABLE IS USED AS BOTH `storage` variable and `memory` VARIABLE





```
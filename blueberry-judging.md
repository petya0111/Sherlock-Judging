# Blueberry judging
https://github.com/sherlock-audit/2023-02-blueberry-judging-petya0111 

```
001-H ERC20 transfer result in the contract MockWETH.sol is not checked
002-H ISOLATED COLLATERALS CAN BE STOLEN  IN ICHIVAULTSPELL.SOL VIA reducePosition() AS THERE IS NO ACCESS CONTROL IMPLEMENTED
003-H First depositor can manipulate share price of SoftVault
004-H Liquidation logic is incorrect when user has debt to more than one bank
005-H WIchiFarm.sol makes the incorrect assumption that IchiVaultLP doesn't reduce allowance when using the transferFrom if allowance is set to type(uint256).max.
006-H When user withdraw assets from ICHI Vault ,if the actual amount to remove in the value is less than lpTakeAmt ,then the user will lose the liquidity
007-H WIchiFarm#burn sends too few IchiV2 tokens to users
008-H The `liquidate` function should check the return value of the call to `safeTransferFrom` method to ensure that the transfer of the collateral was successful.
009-H IchiLpOracle is extemely easy to manipulate due to how IchiVault calculates underlying token balances
010-H High slippage tolerance when swapping on uniswapV3 can lead to frontrunning
011-H User never receive the interest on lending to the protocol
012-H Vault shares can be withdrawn when using `withdrawInternal` in IchiVaultSpell.sol, but when done, if `amountLpWithdraw != vault.balanceOf(address(this))`, some vault balance stays in IchiVaultSpell, and can be swept by anybody by calling `reducePosition` with `collAmount == 0` and `collToken==vault`
013-H IchiVaultSpell#openPositionFarm can cause Ichi to be harvested but doesn't send it to the user
014-H BlueBerryBank#withdrawLend will cause underlying token accounting error if soft/hard vault has withdraw fee
015-H IchiVaultSpell#closePosition will leave LP tokens in the contract if amountLpWithdraw != 0
016-H Notice that `deposit` in `HardVault` does not call cToken mint unlike in `SoftVault`, it just deposits and issues erc1155 tokens in return, isolated from the rest of the blueberry protocol.
017-H The ```BlueBerryBank``` contract does not allow users to submit a deadline for there actions. In ```closePosition()``` function call  there is a swap on Uniswap V3. This missing check enable pending transactions to be mailciously executed at a later point.
018-H The external function withdrawLend() is used to withdraw isolated collateral tokens lent to bank and is supposed to be called by only SPELL but there's no Access control implemented to make sure that only SPELL can call withdrawLend(), it is callable by anyone.
019-H A borrower might drain the vault by calling borrow() repeatedly with small borrow amount each time.
020-H No ability to provide slippage when closing position, as result user can receive dust for swapping
021-H A whale can make grieving attack to strategies that use same vault by transfer vault token to spell. In ichiVaultSpell's depositInternal function it check total vault token in spell contract. If totalVaultToken*vaultPrice exceed strategies maxPosition size than reverts.
022-H A Whale can grieve specific type of borrowing token
023-H BlueBerryBank.liquidate supposes that only 1 token can be borrowed for the position
024-H Using modifier `onlyEOAEx` to ensure calls are made only from EOA will not hold true in the event EIP 3074 goes through. For `onlyEOAEx`, `tx.origin` is used to ensure that the caller is from an EOA and not a smart contract.
025-H Out-of-bounds Access Due to failure on Input Validation. getPositionInfo getPositionDebtShareOf positionId
026-H In the `BlueBerryBank.sol` contract there is a function `addBank()` to add a new bank using a different token. which adds tokens to the Dynamic array `allBanks`. It also checks the length of the array `allBanks` which is restricted to 256. 
027-H In the `BlueBerryBank.sol` contract there is a function `repay()` to repay the borrowed amount.  before executing repay() function there is a modifier called `onlyWhitelistedToken`. which checks whether the token user is repaying is whitelisted or not. I found the way through which users can repay backlisted tokens and can withdraw collateral.
028-H Incorrect amount in `bank.totalLend` after liquidating  position
029-H `isLiquidatable` function underprices risk, potentially preventing (or delaying) liquidations of undercollateralized positions
030-H The` getPrice `function may be vulnerable to _a reentrancy attack_ if any of the `primarySources` that are called by `getPrice` allow for external contract calls.
031-H `setRef() `may lead to uncontrolled access which could result in wrong prices being reported. 




001-M Use call  - Use of transfer() may lead to failures
002-M add a require to check who call it for initialize
003-M The NFT can be frozen in the contract if `msg.sender` is an address for a contract that does not support ERC721.
004-M Contracts have a single point of control implementing timelocks  - multi signature custody
005-M ChainlinkAdapterOracle use BTC/USD chainlink oracle to price WBTC which is problematic if WBTC depegs
006-M user cannot closePosition when borrow token is removed from whitelist
007-M BlueBerryBank#doCutDepositFee is problematic for ERC20 tokens that don't support zero transfers
008-M liquidate will revert if amountCall is more than debt which can lead to DOS
009-М Collateral cannot be withdrawn if it can't be priced even if the position has no outstanding debt
010-M Possibility of a raceCondition attack identified. setTokenRemappings remappedTokens
011-M An attacker can block a bank by calling `repayOnBehalfOf` directly on cToken.
012-M Not set liquidationThreshold in coreOracle does not prevent openPosition and can lead to unjust liquidations
013-M If the bank.EXECUTOR() is added to the USDC blacklist, then his assets will be frozen in the protocol
014-M Withdrawal fee is placed on user if he withdraws early from Vault. However, due to how the withdrawVaultFeeWindowStartTime is implemented, users might pay fees when he should not and not pay fees when he should have.
015-M The function Liquidate can be called by anyone. According to the docs this is intended and not a flaw in the design of the protocol. After the msg.sender pays the debt for the owner, the function transfers both the position (ERC1155 token) shown here
016-M USDT is not supported because of approval mechanism
017-M Function ```addStrategy()``` inside contract ```IchiVaultSpell.sol``` doesn't check the existence of a previously declared strategy.
018-M no refund implemented in execute might lead to caller losing excess native tokens
019-M IchiVaultSpell.openPosition() will always revert if ICHI Vault Lp Tokens are fees-on-transfer ERC20 tokens.
020-M In contrast to other external functions, the ``reducePosition`` does not use ``existingStrategy(strategyId)`` and  ``existingCollateral(strategyId, collToken)`` to perform the sanity check of strategy and collateral. 
021-M When ``BlueBerryBank.withdrawLend()`` is called, it will withdraw ``wAmount`` of the underlying tokens from the vault that corresponds to  ``shareAmount`` of vault shares. 
022-M A borrower might receive ZERO underlying tokens after burning his shares in ``pos.underlyingVaultShare``.
023-M ``IchiVaultSpell._validateMaxLTV()`` mistakenly evaluates underylying token value as the value for the collateral!
024-M `execute` function is made `payable` but SPELL variable is not, so any call with non zero value to address SPELL will fail 
025-M ERC20.approve() call but does not check the success return value. Some tokens do not revert if the approval failed but return false instead.
026-M When users call function ```closePosition()``` or ```closePositionFarm()```, users need to repay  his debt. However ,if the token is removed from the whitelist, users are not able to repay it any more.
027-M In case if user withdrawLend in withdrawVaultFee window, his position.underlyingAmount is not updated correctly
028-M BlueBerryBank.getPositionIdsByOwner will stop working with out of gass error when a lot of positions will be created
029-M Borrowers continue accruing interests when repay is paused
030-M BasicSpell.doCutRewardsFee uses depositFee instead of withdraw fee
031-M Timestamp Dependence Manipulation in `Withdraw` Vault Fee Window.
032-M Inadequate Transaction Reversion Due to Lack of Error Handling. deposit withdraw safeTransferFrom
033-M Use of public state variables in `whitelistedTokens`, `whitelistedSpells`, and `whitelistedContracts`.
034-M Information leakage in the functions `getPositionInfo` and `getCurrentPositionInfo`.
035-M But the `lend` function doesn't check the balance of the "user" to ensure that they have sufficient funds/collateral to lend the amount of tokens. 
036-M ChainlinkAdapterOracle will return the wrong price for asset if underlying aggregator hits minAnswer
037-M BlueBerryBank.getPositionRisk shows risk 0, when underlying price is 0
038-M Unrestricted burn and harvest users can loss their funds.
039-M No storage gap for upgradable contracts might lead to storage slot collision
040-M The protocol is missing the feature to remove an user from whitelist. Once an user has been added to the whitelist, it is not possible to remove him from the whitelist.
```
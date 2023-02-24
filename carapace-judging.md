# Carapace judging
https://github.com/sherlock-audit/2023-02-carapace-judging-petya0111 


001-H When user make a withdrawal request there is no change and record for user's balance in the process. User's balance can be reused to submit a withdrawal request that breaks the logic of deposit and withdrawal.
002-H Run withdraw() after accruePremiumAndExpireProtections() can redeem more more underlying tokens
003-H In the function ```lockCapital()```, the protocol may set ```totalSTokenUnderlying``` to be 0,It is an important factor in the calculation of the exchange rate and leverage ratio which used in ```deposit()``` ,```withdraw()``` and ```buyProtection()```.
004-H When a lender buys a protection, the remaining principle amount is calculated by `GoldfinchAdapter.calculateRemainingPrincipal()` function, then  `ReferenceLendingPools.canBuyProtection()` function verifies that protection amount is less than or equal to the remaining principal.
005-H _accruePremiumAndExpireProtections() and lockCapital() could revert due to out of gas when the activeProtectionIndexes array size is large
006-H It was observed that withdraw request is executed post 2 cycles of request creation. This could be tricked by attacker to create a withdraw request for funds which does not even exist. This allows attacker to withdraw funds early than required delay as shown in POC
007-H Malicious user can create fake withdraw requests for different accounts in a specific cycle , this gives the user ability to deposit and withdraw in that same cycle with different accounts and sandwich buyProtection between deposits and withdraws in one cycle or even transaction if user be a smart contract .
008-H Lack of check of emergency shutdown of Goldfinch's lending pools in Carapace protocol
009-H Nobody can deposit anymore after ``_underlyingAmount`` becomes ZERO. This is due to a divide-by-zero error. 
010-H Malicious sellers can claim more unlocked capital than they deserve. They can do that by front-running ``lockCapital()`` (actually front-running ``assetStates()`` due to ``assessStates() -> _assessState() -> _moveFromActiveToLockedState() -> _protectionPool.lockCapital()``)  to deposit more underlyingtokens to boost their shares but withdraw after the execution of ``lockCapital`` (sandwich attack). In this way, they can claim a larger portion of the unlocked capital than their peers. 
011-H When the locked capital is unlocked, the seller can retrieve the capital via ProtectionPool.claimUnlockedCapital().
012-H _calculateClaimableAmount()  wrong calculate the claimable amount, seller lost part of the claimable funds
013-H accruePremiumAndExpireProtections() protection may never expire and premiumAmount will never be assigned
014-H `DefaultStateManager` contract has `calculateAndClaimUnlockedCapital` function to calculate and return the total claimable amount from all locked capital instances in a given protection pool for a user address and marks the unlocked capital as **claimed**.
015-H Protocol assumes that USDC token have 18 decimals. Accounting functions will be greatly impaired.
016-H `calculateAndSetPoolCycleState` first checks to see if the state is in `Open`, in this case, it updates the state to `Locked` if the time difference with `cycleStartTime` is more than the `openCycleDuration`.
017-H possible race condition between `ProtectionPool::withdraw` and `DefaultStateManager::assessState`
018-H Seller can request multiple withdrawals using same sTokens by using multiple addresses. This will allow him to deposit protection without risk two cycles in the future.
019-H withdrawlRequests and totalSTokenRequested are not updated when sTokens are transferred



001-M Duplicate access to a storage variable in the assessState() function of ReferenceLendingPools causes increased gas usage. 
002-M In ```ProtectionPool```  there are  functions ```deposit()``` and ```depositAndRequestWithdrawal()``` , which accepts USDC, SToken.  And the list may extend.But if any of these tokens will start charge fee on transfers, the logic will be broken and resulting in the loss of funds in the protocol
003-M Run deposit() after lockCapital() can get more stoken shares
004-M ProtectionPool phase can move from OpenToBuyers to Open phase with no totalptotection
005-M When a lender buys a protection, the remaining principle amount is calculated by `GoldfinchAdapter.calculateRemainingPrincipal()` .
006-M There is no verification that  if the ```_protectionPurchaseParams.protectionAmount```  is 0 in the validation process when user buy or renew protection,  user can buy a large amount of protection to fill the array with
007-M User is not allowed to renew protection if contract is paused. This becomes a problem because renewal is only allowed till grace period.
008-M It seems new cycle is not instantly started post open cycle duration has elapsed. The cycle state is first placed in Locked state instead of starting a new cycle. This has adverse impact on other contracts like User unable to create protection even with correct params
009-M Leverage ratio floor and ceiling can be set by protocol owner , but there is no check to ensure floor is lower than ceiling , if owner set this by wrong it may block pool functionalities .
010-M There are two edges cases under which ``withdraw()`` is disabled when it is supposed to be enabled: ``_cycleParams.openCycleDuration == _cycleParams.cycleDuration``, that means there is no locked state, it is always open. ``_cycleParams.openCycleDuration`` is near ``_cycleParams.cycleDuration``. That means, the open period is long, and the locked period is short. The cron job does not get a chance to be called during the locked period, but the locked period has already passed. 
011-M Payable is used in many functions for gas optimisation but if ether is sent to the protocol mistakenly, it will be locked permanently.
012-M When ``lockCapital()`` gets executed, the ``exchangeRate`` - underlying tokens/share, will dramatically decreased. A user, however, can front-run ``lockCapital()`` to withdraw the underling tokens with the higher rate before the execution of ``lockCapital()``, and thus gaining advantage over their peers who have not done so. 
013-M Some protection buyers might never  be able to renew their protections due to delayed expiration processing as a result of a bug of ``verifyAndAccruePremium()``.``getActiveProtections()`` might return some protections that are supposed to have expired, but not processed due to the above bug.
014-M When all capitals are locked due to a late payment in lending pool, the exchange rate will be 0, causing ProtectionPool.deposit() to fail. The pool will be stuck with remaining premium locked in the contract.
015-M withdraw() Missing slippage parameter may cause users to lose shares
016-M User can loose Stoken without receiving underlying token when withdrawing small Stoken shares due to the function `convertToUnderlying` might return 0 if `_sTokenShares` is small enough, and also because function `withdraw` does not check if `_underlyingAmountToTransfer` (result from `convertToUnderlying`) > 0.
017-M No storage gap for upgradeable contracts might lead to storage slot collision
018-M USDC  have a contract level admin controlled address blocklist. If an address is blocked, then transfers to and from that address are forbidden.
019-M Unable to withdraw the ETH locked in the contract
020-M sToken can be inflated because the balanceOf function does not specify the token used.
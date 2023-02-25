# OpenQ judging
https://github.com/sherlock-audit/2023-02-openq-judging-petya0111 

```
001-H Unbounded loop in BounyCore.getLockedFunds function leads to DOS in DepositManagerV1.refundDeposit function
002-H Remaining funds cannot be refunded after partial refund
003-H Issuer can front-run claimTieredFixed() and change payoutTokenAddress. This call, in turn, calls `TieredFixedBountyV1.claimTieredFixed()` and transfers the `payoutTokenAddress` token to the `_payoutAddress`.
004-H Contract initialization can be frontran.
005-H BountyCore setKycRequired, setInvoiceRequired, setSupportingDocumentsRequired function can block users from claiming
006-H Malicious bounty issuers may specify payouts that are not fulfillable
007-H Denial Of Service On `fundBountyToken()` Of Any Non-whitelisted Tokens
008-H Attacker can fund bounty with malicious ERC20 and block payouts
009-H Anyone can deposit tokens outside of the whitelist into the bounty, which may result in the winner not be able to claim the prize
010-H lack of checks for overflow/underflow and input validity to prevent potential security risks. 
011-H Attacker can deposit and refund NFT which leads to DOS in claim functionality
012-H Attacker can block user from claiming AtomicBountyV1 by depositing token that doesn't support 0 transfer and then refunding
013-H BountyCore.claimNft doesn't check that nft is not refunded
014-H Post-contest refunds block tier winners in tiered fixed bounty from claiming funds
015-H Funders can deny rewards to last claimants by calling `refundDeposit` between tiers claims
016-H Refunds can be bricked by triggering OOG (out of gas) in DepositManager
017-H Ongoing claims can be re-used due to lack of eligibility checks ClaimManagerV1 _eligibleToClaimOngoingBounty
018-H Function `closeCompetition()` in `TieredBountyPercentage` contract can be bricked, stopping claims
019-H [Stuck fund] Claimer may get less fund in percentage bounty if more than 1 tiers have the same token address


001-M Missing onlyInitializer modifier
002-M Depositor can send accidentally ether while sending erc20 as funds
003-M Centralization risk: OpenQV1.sol have a single point of control for the function transferOracle
004-M out of bound `setPayoutSchedule` and `setPayoutScheduleFixed` will revert if `_payoutSchedule` has a length lower compared to `tierWinners`
005-M Funders lose msg.value if they deposit native token and ERC20 together
006-M In BountyCore.refundDeposit the _volume param should not exceed the volume received
007-M TieredPercentageBountyV1: refunds after close make amount that is paid out bigger than expected percentage
008-M OpenQV1.mintBounty function can be front-run
009-M Bounty C reation Can Be Griefed By Frontrunning Bounty ID
010-M The expiration time for bounty refund can be gamed in DepositManager.sol
011-M When tokenAddresses set has reached TOKEN_ADDRESS_LIMIT, tokens that are contained in the tokenAddresses set cannot be used for funding
012-M Protocol cannot handle rebasing tokens
013-M Owner of Tiered bounties can frontrun the claim process by updating the payout schedule / winners
014-M DepositManagerV1.refundDeposit will revert when big number of deposits will be created in bounty
015-М OngoingBountyV1 contract should not implement receiveNft function because NFT cannot be paid out
016-M No Storage Gap for Upgradeable Contract Might Lead to Storage Slot Collision
017-M OngoingBountyV1 doesn't verify that `claimId` was unused
018-M some initialize function miss onlyProxy or onlyOwner
019-M BountyCore: funding goal cannot be set to false when it is true which can make bounties dysfunctional
020-M Attacker can front-run refund functionality such that user receives less funds than expected availableFunds
021-M Early bounty contracts can be bricked.
022-M [Prevent admin to make mistake which cause lose fund] Ongoing close functionality should only be allowed when there is no balance left
023-M Bounties can be paid with expired deposits
024-M [Logic error] Invoice or supportingDocuments can be marked as completed even if it is not required
025-M BountyCore.receiveFunds doesn't return back native payment in case when it was provided along with another token
026-M ClaimManagerV1 will not be able to claim for user if he is blocked by any reward token
027-M `TieredFixedBounty` accepts any ERC20 token as deposit, but can only pay in the pre-specified token
```
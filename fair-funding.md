# Fair funding judging
https://github.com/sherlock-audit/2023-02-fair-funding-judging-petya0111 

```
001-H return value transfer from 
002-H No NFTs can be minted after initial `max_token_id`
003-H withdraw_underlying_to_claim() allows anyone to call it resulting in  that  all shares in the protocol  can be burned , and the user profits by claiming
004-H Attacker can call `Vault.withdraw_underlying_to_claim()` with no slippage protection to cause loss of funds
005-H sandwich attacks
006-H incorrect calculation claimable amount
007-H Starting timestamp can be bypassed by calling `settle`
008-H The function `migrate` in `Vault` has access control issues and is callable by anyone. 
009-H  In the current implementation, the `claim()` function does not support the withdrawal of the claimable amount for positions that have been liquidated. 
010-H Conversion from `int256` to `uint256` can break protocol functionality



001-M check balance transfer
002-M permit add or remove operators admin
003-M Incorrect Access Control onlyOwner settle
004-М safeMint instead of mint for ERC721
005-M Check `_amount_shares` is not zero
006-M a large number of users liquidate their assets, there may not be enough share to burn
007-M In case when `start_auction` is not called yet, it will return false.
008-M. Front running attack in bid. Block timestamp
009-M whitelisted tokens Usage of malicious ERC20 contracts for DOS of various protocol functions
010-М In the loops that are used to resize the metadata arrays (`tierWinners`, `invoiceComplete`, `supportingDocumentsComplete`), iterate over `min(payoutSchedule.length, metadataArray.length)`
 011-M Theft of funds through reentrancy
013-M Auction ends can be post-poned indefinitely
012-M The value of `_is_valid_token_id` in the `register_deposit()` function is unpredictable
014-M Duplicate tokens sold when calling set_max_token_id
015-M If an AuctionHouse gets stuck, cannot start a new AuctionHouse with same vault
016-M AlchemistV2._checkMintingLimit can cause Vault.register_deposit to revert
017-M `FALLBACK_RECEIVER` is an important address and should be changeable
018-M Vault is vulnerable to a first depositor attack
019-M Any excess yield after full debt repayment will be lost.
020-M Attacker can front-run `AuctionHouse.refund_highest_bidder` to block refunding
021-M User might be forced to pay bad debt of the vault
```
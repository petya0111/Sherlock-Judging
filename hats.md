# hats Judging
https://github.com/sherlock-audit/2023-02-hats-judging-petya0111 

```
High
1 _constructURI function allows to break out of json format and to inject malicious properties / code
2 The threshold is incorrectly being updated to `validSignerCount`. It should instead be updated to `newThreshold`.
3 `createHat` allows creation of hats without intermediary, which can lead to hats getting completely overwritten in the future
4 Contract breaks if `targetThreshold` is ever reduced
5 Hat wearer can call function with limited amount of gas in order to make toggle call revert and use previous active status
6 createHat can be called by indirect admins
7 A tree root can be moved to a tree not within its current sub-tree without its explicit consent due to unfortunate ordering of actions.
8 Unlink hat checks for admin instead of root tree
9 Inconsistent isAdminOfHat function for top hats
10 If another module adds a module, the safe will be bricked
11 attacker can perform malicious transactions in the safe because reentrancy is not implemented correctly in the checkTransaction() and checkAfterExecution() function in HSG
12 If a hat is owned by address(0), phony signatures will be accepted by the safe
13 Signers can brick safe by adding unlimited additional signers while avoiding checks
14 `maxSigners` can be exceeded, causing all safe transactions to revert
15 Hat wearers who are not the safe's owners can execute safe's transaction
16 Hats.sol, function isInGoodStanding may return success for a renounced Hat also





Medium
1 Hats.uri function can be DOSed by providing large details or imageURI string or cause large gas fees
2 So the `supportsInterface` function returns `true` for ERC1155 which is wrong.
3 Contracts that integrate with `Hats` and use the `balanceOfBatch` function get wrong results and misbehave.
4 When an admin has the intention to transfer a revoked hat it's not possible because the hat is burned.Wrong if check for wearer of hat
5 `_checkAdminOrWearer()` checks whether the msg.sender is either an admin or wearer or a hat, and reverts the appropriate error if not.  
6 Hats.sol: staticcalls can revert by consuming all gas which can cause other functionality to be blocked
7 Hats.sol: linkedTreeRequests entry should be deleted when unlinking
8 Safe threshold can be set above target threshold, causing transactions to revert
9 `MultiHatsSignerGate` allows addition but not removal of signer hats
10 Function `claimsigner` can revert even in conditions where it is supposed to let the user register as a signer.
11 When during initialization, the caller accidently sends eth to the contract, the eth is stuck forever in the contract.
12 HatsSignerGateBase: signers can add / remove / swap signers which bypasses the HSG logic and can lead to multiple bad outcomes including DOS and increased control over Safe
13 There is no check in `mintHat()` about the status of a hat before it is minted.
14 Bad admins can front-run mintHat()
15 checkHatWearerStatus does not match _isEligible
16 noCircularLinkage() might fail to detect  circles in the tree
17 DOS attack to getHatLevel()
18 HatsSignerGateBase will not allow tx, that decrease owners count(some wearers are not eligible to wear hat)
19 Changing hat toggle address can lead to unexpected changes in status
20 buildHatId returns incorrect value for lowest level child
21 Can get around hats per level constraints using phantom levels
22 Direct usage of ecrecover allows signature malleability
24 changeHaxMaxSupply() function implementation is different and misleading from its documentation
25 HatsSignerGateFactory: Should revert if there are more than 5 existing modules
26 `transferHat` returns no value prevents accidental input
27 Top Hats can be overriden due to arithmetic overflow
28 Invalid token ID hat with skipped level should not be able to be created
29 False default value for badStanding can expose the hat to side effects.
30 By calling `removeOwner` or `swapOwner` on the `safe`, signers can remove other valid signers from the safe.
31 Recursive functions are used regularly and can increase gas usage quadratically or might face stack too deep
32 `_swapSigner` might cause `signerCount` to increase incorrectly
33 Transactions will be frozen if incorrect settings are used during a deployment on HatsSignerGateFactory
34 Hats._isEligible and Hats._isActive functions might access old data
35 HatsSignerGateBase: _removeSigner function may revert so it is not possible to remove a signer
36 last Invalid Signer will never be swapped!
37 topHat admin can lose its token and there is no backup to recover
38 A malicious admin can batch create hats and freeze its hat
39 Signatures can duplicate in function `countValidSignatures`
40 Hats contract functions doesn't check that all upper level hats exists and it would be possible to link a hat to non-existing hats
41 There is the WRONG calculation in the lastHatId logic.
42 The variable "HATS" which occurs in  MultiHatsSignerGate.sol  twice and in  HatsSignerGate.sol  once seems to be defined nowhere.
43 Inconsistency of expected behavior when address is ineligible and good standing
44 Can get around hats per level constraints using phantom levels
45 An admin can secure the good standing and eligibility of any hat wear under him easily.

21 "HATS" variable defined nowhere





```

## 1
##  issue
	* Delegatecall to contracts
## Severity:
	* High
## Context: 
	* https://github.com/spearbit-audits/writing-exercise/blob/de45a4c5b654710812e9fa29dde6e12526fe4786/contracts/Implementation.sol#L17
## Description of the attack.
	* There is a special variant of a message call, named delegatecall which is identical to a message call apart from the fact that the code at the target address is executed in the context of the calling contract and msg.sender and msg.value do not change their values. This allows a smart contract to dynamically load code from a different address at runtime. Storage remains of the current address and balance still refer to the calling contract, which will be proxy in this case.
Using delegatecall to call any function is very dangerous, as the code at the target address can change any storage values of the caller and has full control over the caller's balance.
## Recommendation: 
	* Use only trusted contracts which do not modify the storage slots 0,1 of their contract so that it does not modify user's wallet proxy contract's storage.
	* Remove word delegate from line https://github.com/spearbit-audits/writing-exercise/blob/de45a4c5b654710812e9fa29dde6e12526fe4786/contracts/Implementation.sol#L18
	* Line 18 should look like (bool success, bytes memory ret) =  a.call(_calldata); so that even if someone calls delegatecallContract method, it should make a normal call.

## 2
##  issue
	* No address check 
## Severity:
	*low
## Context: 
	* https://github.com/spearbit-audits/writing-exercise/blob/de45a4c5b654710812e9fa29dde6e12526fe4786/contracts/Implementation.sol#L17
## Description of the attack.
	* the implementation contract allows any user to input an address , this aadress is not validated or might be a EOA address which might not be an account that funds sent can be withdraw
## Recommendation: 
	* I strongly recommend that the addres inputed should be checked and validated that is a contract account 

## 3
##  issue
	* Reentracy
## Severity:
	* low
## Context: 
	* https://github.com/spearbit-audits/writing-exercise/blob/de45a4c5b654710812e9fa29dde6e12526fe4786/contracts/Implementation.sol#L12
## Description of the attack.
	* One of the major dangers of calling external contracts is that they can take over the control flow. In the reentrancy attack (a.k.a. recursive call attack), a malicious contract calls back into the calling contract before the first invocation of the function is finished. This may cause the different invocations of the function to interact in undesirable ways.
## Recommendation: 
	* The best practices to avoid Reentrancy weaknesses are:
Make sure all internal state changes are performed before the call is executed. This is known as the Checks-Effects-Interactions pattern
Use a reentrancy lock (ie. OpenZeppelin's ReentrancyGuard.

## 4
##  issue
	* No error message in require
## Severity:
	* informational
## Context: 
	* https://github.com/spearbit-audits/writing-exercise/blob/de45a4c5b654710812e9fa29dde6e12526fe4786/contracts/Implementation.sol#L13
	* https://github.com/spearbit-audits/writing-exercise/blob/de45a4c5b654710812e9fa29dde6e12526fe4786/contracts/Implementation.sol#L19
## Description of the attack.
	* when the function fails at the above line of code there is no way to know why it fails , which will make debugging and code readability hard
## Recommendation: 
	* I recommend that an error message is included so that it is more user friendly and easy to debug.
  

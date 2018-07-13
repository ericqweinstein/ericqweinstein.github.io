---
layout: post
title: Complexity, Completeness, and Smart Contracts
date: 2018-07-12 14:00:00
summary: Or, Why Dumb is Better
categories: programming smart contracts solidity
---

### Some Background
Over the past several months, as the team at <a href="http://auxtoken.com/" target="_blank">AUX</a> has been building out <a href="http://auxplatform.com/" target="_blank">the AUX Platform</a>, I've had the opportunity to read and write several <a href="https://en.wikipedia.org/wiki/Smart_contract" target="_blank">smart contracts</a>. Smart contracts for the Ethereum blockchain can be written in a few different languages, but the most popular one is <a href="http://solidity.readthedocs.io/" target="_blank">Solidity</a>, which looks a little like a cross between JavaScript and C++ (with a little payment-specific DSL thrown in). Here's an example from <a href="https://openzeppelin.org/" target="_blank">OpenZeppelin</a>'s <a href="https://github.com/OpenZeppelin/openzeppelin-solidity/blob/5daaf60d11ee2075260d0f3adfb22b1c536db983/contracts/token/ERC721/ERC721BasicToken.sol" target="_blank">ERC721 basic token contract</a>:

```js
function approve(address _to, uint256 _tokenId) public {
  address owner = ownerOf(_tokenId);
  require(_to != owner);
  require(msg.sender == owner || isApprovedForAll(owner, msg.sender));

  tokenApprovals[_tokenId] = _to;
  emit Approval(owner, _to, _tokenId);
}
```

This `approve` function is called in the process of transferring an <a href="http://erc721.org/" target="_blank">ERC721 asset</a> from one owner to another. Owners are identified by their addresses, which in Ethereum are forty-character hexadecimal strings; non-fungible tokens are tracked in registry contracts by their ID numbers (usually in a 256-bit address space).

First, the function looks up the owner of the token via the `ownerOf` function (not shown here). Next, it ensures that we're not attempting to transfer the token to the person who already owns it (the first `require` statement) and that the person doing the transferring is either the item's owner or someone who's approved to transfer it on their behalf (the second `require` statement). After that, it goes ahead and updates its mapping of which token is owned by which address. Finally, the contract emits an `Approval` <a href="http://solidity.readthedocs.io/en/v0.4.24/contracts.html#events" target="_blank">event</a> to signal that the approval has taken place.

While I find Solidity contracts relatively readable, I've come to believe that Solidity is not an ideal (or even a very good) language for smart contract development. I have three fundamental problems with it:

* <a href="http://solidity.readthedocs.io/en/v0.4.24/contracts.html#fallback-function" target="_blank">Fallback functions</a> scare the hell out of me;
* I don't think the type system does enough to protect programmers from silly errors;
* I fundamentally don't think <a href="https://en.wikipedia.org/wiki/Turing_completeness" target="_blank">Turing-completeness</a> is necessary or desirable.

### Fallback Functions
Fallback functions are anonymous functions that can accept no arguments and can have no return value. They're called when a contract receives a function call for which there is no matching identifier (that is, someone called a function that the contract does not implement). An example might look like this:

```js
function() payable { }
```

In this case, the contract is designed to accept ether sent to it, even if no data are included or the intended function signature is incorrect. (Without this fallback function, the contract would instead throw an error and return the ether.) While contract writers might prefer this behavior, I think it's surprising and confusing to users interacting with it (whether honestly maliciously), and this sort of behavior is ripe for abuse/misuse, as can be seen in the "King of the Ether Throne" examples found <a href="https://applicature.com/blog/history-of-ethereum-security-vulnerabilities-hacks-and-their-fixes" target="_blank">here</a>. Generally speaking, I agree with <a href="https://www.python.org/dev/peps/pep-0020/" target="_blank">PEP 20</a> that explicit is better than implicit, and these kinds of implicit behaviors are especially thorny when contracts and payments are involved.

### The Type System
I think the only thing I like about the Solidity type system is that it's static. String modifications almost always require casting to `bytes` (including string concatenation: yes, this has to be done manually). While Solidity supports enums, <a href="http://solidity.readthedocs.io/en/develop/frequently-asked-questions.html#if-i-return-an-enum-i-only-get-integer-values-in-web3-js-how-to-get-the-named-values" target="_blank">the ABI does not</a>. As of this writing, Solidity <a href="http://solidity.readthedocs.io/en/v0.4.24/types.html#fixed-point-numbers" target="_blank">still does not support fixed-point numbers</a>, in which it is _much_ easier to reason about arithmetic operations like addition and subtraction. This seems like it would be a good feature for a language designed to handle financial transactions. Even if you argue that money shouldn't be modelled this way, there are many non-currency values for which this kind of arithmetic would be really useful (risk modelling, logistics planning, and so on).

While some of these problems will probably eventually be fixed by improvements to the EVM and/or Solidity language, I think they're always going to feel like exactly that: fixes rather than core design decisions. To my mind, a language built to enforce smart contracts should prioritize correctness and security, and a more powerful type system would help further that goal. I think a dependently-typed language like <a href="https://www.idris-lang.org/" target="_blank">Idris</a> could be ideal. (NB: I really like Idris and have made small contributions to the language.) A type system that can enforce invariants like "non-negative integer" or "list of string where each member is at least three characters long" means more correctness can be confirmed by the compiler, leaving less to ever-incomplete testing and well-meaning but error-prone wetware. (Idris also has excellent tooling for formal verification and theorem proving; more on this in the next section.)

### Turing Completeness
Solidity is Turing-complete, meaning that a Solidity smart contract can simulate any Turing machine. While this makes a rich universe of contracts available to Solidity programmers, this necessarily includes programs that are valid but undesirable, such as those that loop forever (at least until they run out of gas), only throw exceptions, or don't do anything at all. It's hard enough to write correct, performant programs as it is, and I think that by excluding valid but useless programs, especially when coupled with a more powerful type system, we get a simpler language in which it is harder to make mistakes and lose people's money.

While it <a href="https://hackernoon.com/smart-contracts-turing-completeness-reality-3eb897996621" target="_blank">has been argued</a> that eliminating Turing completeness is not necessary to achieve provable correctness, it certainly makes things simpler, and since I've yet to see any compelling argument for why it's necessary, I tend to think it's better to exclude it until proven a requirement. Theorem proving and formal verification can be more easily accomplished in a less expressive language, and if there is no demonstrable, practical downside to that tradeoff, I think it should be made without hesitation.

### In Conclusion
While I find Solidity readable because I'm used to ALGOL-family languages like C++ or Java, I don't think it's a particularly good choice for developing smart contracts. So what is?

There are a few newer languages that I think are promising. As mentioned, I think Idris or an Idris-like language with a powerful, dependent type system could be ideal. It's also possible that such a language could be useful for proving the correctness of contracts written in existing languages like <a href="https://github.com/ethereum/vyper" target="_blank">Vyper</a> or <a href="http://kadena.io/" target="_blank">Kadena</a>'s <a href="http://pact-language.readthedocs.io/en/latest/" target="_blank">Pact</a>, both of which aim to improve on existing smart contract language designs (and the latter of which is intentionally Turing-incomplete). Although I do worry that Pact's LISP-inspired syntax raises the barrier to entry, as someone who picked up Clojure after learning JavaScript and Ruby, I think that barrier is very surmountable.

Ultimately, I think smart contracts are a fascinating area of software development, and the languages used to construct them, while imperfect, will have to improve as adoption increases. I'm really excited to see that evolution take place, and I'm lucky to witness it firsthand while building AUX.

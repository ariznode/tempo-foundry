<br>
<br>

<p align="center">
  <a href="https://tempo.xyz">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/tempoxyz/.github/refs/heads/main/assets/combomark-dark.svg">
      <img alt="tempo combomark" src="https://raw.githubusercontent.com/tempoxyz/.github/refs/heads/main/assets/combomark-bright.svg" width="auto" height="120">
    </picture>
  </a>
</p>

<br>
<br>

# Tempo Foundry

[Tempo](https://docs.tempo.xyz/) is a blockchain designed specifically for stablecoin payments. Its architecture focuses on high throughput, low cost, and features that financial institutions, payment service providers, and fintech platforms expect from modern payment infrastructure.

`Tempo Foundry` is a custom fork of [Foundry](https://github.com/foundry-rs/foundry) that integrates Tempo's payment-native protocol features directly into the familiar Foundry developer workflow.

This is a temporary required drop-in replacement for upstream Foundry while Tempo-specific features are being integrated into upstream Foundry, after which this fork will be deprecated.

Get started [here](https://docs.tempo.xyz/sdk/foundry) to use Tempo's features in Foundry.

## Fund your wallet

- Head into : https://docs.tempo.xyz/quickstart/faucet
- Connect evm wallet
- Claim Faucet

## Instalation

```sh
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup
```

```sh
forge --version
```

```sh
git clone https://github.com/tempoxyz/tempo-foundry.git
cd tempo-foundry
```

```
echo 'export TEMPO_RPC_URL=http://0.0.0.0:8545' >> ~/.bashrc
```

```sh
forge install
```

```sh
cast chain-id --rpc-url $TEMPO_RPC_URL
```
Make sure to see : 

42429

```sh
export PRIVATE_KEY=0x<PRIVATE_KEY_TESTNET>
```

```
mkdir src
```

### Deploy Smart Contract

```sh
nano src/Counter.sol
```

Copy all of this :

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Counter {
    uint256 public number;

    function increment() external {
        number += 1;
    }

    function setNumber(uint256 newNumber) external {
        number = newNumber;
    }
}
```

```sh
forge build
```

Deploy Smart Contract
```
forge create src/Counter.sol:Counter \
  --rpc-url $TEMPO_RPC_URL \
  --private-key $PRIVATE_KEY \
  --broadcast
```

### Deploy ERC20

```sh
nano src/MyToken.sol
```

Copy this and change token name, and symbol. 

Change Ariz Node and Ariz to whatever did you want

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MyToken {
    string public constant name = "Ariz Node";
    string public constant symbol = "MTT";
    uint8  public constant decimals = 18;

    uint256 public constant totalSupply = 1_000_000 * 10 ** 18;

    mapping(address => uint256) public balanceOf;

    event Transfer(address indexed from, address indexed to, uint256 value);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address to, uint256 amount) external returns (bool) {
        require(balanceOf[msg.sender] >= amount, "balance too low");
        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
}
```
Deploy ERC 20

```sh
forge create src/MyToken.sol:MyToken \
  --rpc-url http://127.0.0.1:8545 \
  --chain 42429 \
  --private-key "$PRIVATE_KEY" \
  --broadcast \
  --force
```




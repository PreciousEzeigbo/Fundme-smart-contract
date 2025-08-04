# FundMe Smart Contract

Welcome to the FundMe smart contract! This was a fun learning exercise to dive into the world of smart contract development. It's a simple, yet powerful, contract that lets people fund a project with a minimum amount of ETH and then lets the lucky project owner (me!) withdraw it all. Think of it as a piggy bank that only the owner can smash open.

## Features

- **Fund Functionality:** Users can send ETH to the contract. The contract checks if the sent amount is equivalent to or greater than the defined minimum USD value.
- **Withdraw Functionality:** The contract owner can be a little greedy and withdraw all the funds. A custom modifier makes sure no one else gets to play the bank robber.
- **Minimum Funding Value:** The `MINIMUM_USD` constant is set to `5e18` (5 USD), which is dynamically checked against the current ETH price using Chainlink's data.
- **Owner-only Modifier:** A custom modifier `onlyOwner` ensures that the `withdraw` function can only be executed by the contract's deployer.
- **Chainlink Integration:** The `PriceConverter` library uses Chainlink's `AggregatorV3Interface` to fetch the latest ETH/USD price, enabling the contract to handle price volatility.

---

## Remix IDE Quick Start

This project is configured to work out-of-the-box with **Remix IDE**. You can follow these steps to quickly compile and deploy the contracts.

1.  **Open Remix IDE:** Navigate to the Remix web application.
2.  **Create the Files:** In the file explorer on the left, create two new files:
    * `FundMe.sol`
    * `PriceConverter.sol`
3.  **Copy the Code:** Copy and paste the respective code snippets from this project into the new files you just created.
4.  **Compile:**
    * Switch to the **Solidity Compiler** tab.
    * Select a compiler version that is compatible with `^0.8.18`, such as `0.8.20`.
    * Ensure `FundMe.sol` is selected and click the "Compile" button.
5.  **Deploy:**
    * Switch to the **Deploy & Run Transactions** tab.
    * In the "Environment" dropdown, select `Injected Provider - Metamask` to deploy to a real test network (like **Sepolia**) or `Remix VM` for a simulated environment.
    * Click the **"Deploy"** button for the `FundMe` contract.
    * Confirm the transaction in your MetaMask wallet if you are using an injected provider.

Once deployed, you can interact with the contract's functions and view its state variables directly from the Remix interface.

---

## Contract Details

### `FundMe.sol`
This is the main contract that handles the core logic of funding and withdrawing.

- `fund()`: A payable function that allows anyone to send ETH. It uses the `PriceConverter` library to validate the sent amount against `MINIMUM_USD`.
- `withdraw()`: A function restricted to the contract owner via the `onlyOwner` modifier. It iterates through all funders, resets their funded amounts to zero, clears the `funders` array, and sends the entire contract balance to the owner.
- `onlyOwner` Modifier: A custom modifier that reverts the transaction with a `NotOwner` error if the caller is not the contract owner.
- `receive()` and `fallback()`: These special functions redirect any direct ETH transfers to the `fund()` function, allowing the contract to receive funds even without a specific function call.

### `PriceConverter.sol`
This is a utility library that handles all interactions with the Chainlink Price Feed.

- `getPrice()`: A function that retrieves the latest ETH/USD price from the Chainlink data feed. The address `0x694AA1769357215DE4FAC081bf1f309aDC325306` is for the Sepolia testnet.
- `getConversionRate()`: Calculates the USD value of a given amount of ETH. It uses `getPrice()` to get the current rate and performs the necessary conversion.
- `getVersion()`: A simple function to retrieve the version of the Chainlink Aggregator contract.

---

## Technologies Used

- **Solidity:** The programming language for writing the smart contract.
- **Remix IDE:** The development environment used for writing, compiling, and deploying the contract.
- **Chainlink Price Feeds:** Oracles that provide reliable, tamper-proof price data.

## License

This project is licensed under the MIT License. This means you're free to use, modify, and distribute this code as you see fit. You can find the full text of the license in the LICENSE file.

# CrowdCoin — Decentralized Crowdfunding dApp

A trustless crowdfunding platform built on Ethereum. Think "Kickstarter on the blockchain": backers contribute Ether to a campaign, but the campaign manager **cannot** spend the funds freely. Instead, every payment must be submitted as a **spending request** and approved by a majority of the contributors before any Ether leaves the contract.

This removes the need to trust the campaign creator — the backers collectively control where the money goes.

## How it works

1. **Anyone** can create a campaign by setting a minimum contribution amount.
2. **Backers** contribute Ether (above the minimum) to become approvers of that campaign.
3. The **manager** creates spending requests, each specifying a description, an amount, and a recipient (e.g. a vendor).
4. **Approvers** vote on each request.
5. Once **more than 50%** of approvers have approved a request, the manager can finalize it, which releases the funds to the recipient.

## Tech stack

| Layer | Technology |
| --- | --- |
| Smart contracts | Solidity `^0.8.9` |
| Compilation | solc |
| Local blockchain / testing | Ganache + Mocha |
| Web3 client | web3.js v4 |
| Frontend | Next.js 12 + React 18 |
| UI components | Semantic UI React |
| Deployment provider | Infura + `@truffle/hdwallet-provider` |

## Smart contracts

Two contracts live in [`ethereum/contracts/Campaign.sol`](ethereum/contracts/Campaign.sol):

- **`CampaignFactory`** — deploys and keeps a registry of every `Campaign` so the frontend can list them.
- **`Campaign`** — holds the funds and the approval logic (contributions, requests, voting, finalization).

## Project structure

```
.
├── ethereum/
│   ├── contracts/Campaign.sol   # CampaignFactory + Campaign contracts
│   ├── compile.js               # Compiles contracts into ethereum/build/*.json
│   ├── deploy.js                # Deploys CampaignFactory via Infura
│   ├── web3.js                  # web3 instance (MetaMask in browser, Infura on server)
│   ├── factory.js               # Deployed CampaignFactory instance
│   └── campaign.js              # Helper to instantiate a Campaign by address
├── pages/                       # Next.js routes
│   ├── index.js                 # Lists open campaigns
│   └── campaigns/               # Create / show / requests pages
├── components/                  # Shared UI (Layout, Header, ContributeForm, RequestRow)
├── routes.js                    # next-routes route definitions
├── server.js                    # Custom Next.js server
└── test/Campaign.test.js        # Mocha contract tests
```

## Getting started

### Prerequisites

- Node.js (16+ recommended)
- A MetaMask wallet for interacting with the dApp in the browser

### Install

```bash
npm install
```

### Compile the contracts

```bash
node ethereum/compile.js
```

This writes `Campaign.json` and `CampaignFactory.json` into `ethereum/build/`.

### Run the tests

```bash
npm test
```

The tests spin up an in-memory Ganache chain and exercise the factory and campaign logic.

### Deploy the contracts

1. Open [`ethereum/deploy.js`](ethereum/deploy.js) and replace `YOUR_MNEMONIC` and `YOUR_INFURA_URL` with your own wallet mnemonic and Infura endpoint.
2. Run:

```bash
node ethereum/deploy.js
```

3. Copy the deployed `CampaignFactory` address into [`ethereum/factory.js`](ethereum/factory.js).

> ⚠️ **Never commit a real mnemonic or private key.** Use environment variables for production.

### Run the app

```bash
npm run dev
```

The app starts on [http://localhost:3000](http://localhost:3000).

## Available scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the Next.js dev server on port 3000 |
| `npm test` | Run the Mocha contract test suite |
| `node ethereum/compile.js` | Compile the Solidity contracts |
| `node ethereum/deploy.js` | Deploy the factory to a network |

## License

ISC

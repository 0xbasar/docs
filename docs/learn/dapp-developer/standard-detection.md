---
sidebar_label: '👮 Standard Detection'
sidebar_position: 6
---

# Standard Detection

If you want to ensure that LSP standards are implemented and working correctly before letting your application interact with smart contracts, you can check their supported ERC725 storage keys and interfaces.

:::tip

You can also use the [ERC725 Inspect Tool](https://erc725-inspect.lukso.tech/) to fetch and check standards of smart contract addresses within the browser.

:::

## Metadata Detection

> How to detect if a contract contains a specific set of ERC725Y in its storage.

We can verify if a contract contains a specific set of ERC725Y keys (= **metadata**) by checking the value stored under the ERC725Y storage key `SupportedStandards:{StandardName}` using the [ERC725.js](https://www.npmjs.com/package/@erc725/erc725.js) library.

:::note Example

**[LSP7DigitalAsset](../../standards/nft-2.0/LSP7-Digital-Asset.md)** is a contract that contains ERC725Y Data keys defined in **[LSP4 - Digital Asset Metadata](https://github.com/lukso-network/LIPs/blob/main/LSPs/LSP-4-DigitalAsset-Metadata.md)**. Therefore, the contract **SHOULD** have the following ERC725Y Data keys set by default: `LSP4TokenName`, `LSP4TokenSymbol`, `LSP4Metadata`, `LSP4CreatorsMap:<address>` and `LSP4Creators[]`.

:::

Similar to the [Read Profile Data Guide](./read-profile-data.md), you can use the `getData()` function to check if the contract has a specific metadata standard like [LSP3 Profile](../../standards/universal-profile/lsp3-profile-metadata), [LSP4 Digital Asset](../../standards/nft-2.0/LSP4-Digital-Asset-Metadata), or a [LSP9 Vault](../../standards/universal-profile/lsp9-vault).

:::tip

You can check all `SupportedStandards:{StandardName}` data keys within the **[erc725.js](https://github.com/ERC725Alliance/erc725.js/blob/develop/src/schemas/index.ts)** GitHub repository.

:::

:::info
⌨️ The full code of this example can be found in the 👾 [lukso-playground](https://github.com/lukso-network/lukso-playground/tree/main/metadata-detection) repository and ⚡️ [StackBlitz](https://stackblitz.com/github/lukso-network/lukso-playground?file=metadata-detection%2Fdigital-asset-check.js).
:::

```
import { ERC725 } from '@erc725/erc725.js';
import lsp3ProfileSchema from '@erc725/erc725.js/schemas/LSP3ProfileMetadata.json' assert { type: 'json' };

// Initatiate erc725.js
const erc725js = new ERC725(
  lsp3ProfileSchema,
  '<myProfileAddress>',
  'https://rpc.testnet.lukso.gateway.fm',
  {
    ipfsGateway: 'https://api.universalprofile.cloud/ipfs',
  },
);

// Fetch the supported storage standard of LSP3
let isLSP3 = await erc725js.getData('SupportedStandards:LSP3Profile');

// Verify if the standard is supported (value !== null)
console.log(isLSP3);

```

<details>
    <summary>Show Vault Storage Check</summary>

```js
import { ERC725 } from '@erc725/erc725.js';
import lsp9VaultSchema from '@erc725/erc725.js/schemas/LSP9Vault.json' assert { type: 'json' };

// Initatiate erc725.js
const erc725js = new ERC725(
  lsp9VaultSchema,
  '0x9139def55c73c12bcda9c44f12326686e3948634',
  'https://rpc.testnet.lukso.gateway.fm',
  {
    ipfsGateway: 'https://api.universalprofile.cloud/ipfs',
  },
);

// Fetch the supported storage standard of LSP9
let isLSP9 = await erc725js.getData('SupportedStandards:LSP9Vault');

// Verify if the standard is supported (value !== null)
console.log(isLSP9);
```

</details>

<details>
    <summary>Show Asset Storage Check</summary>

```js
import { ERC725 } from '@erc725/erc725.js';
import lsp3ProfileSchema from '@erc725/erc725.js/schemas/LSP4DigitalAsset.json' assert { type: 'json' };

// Initatiate erc725.js
const erc725js = new ERC725(
  lsp3ProfileSchema,
  '0x6395b330F063F96579aA8F7b59f2584fb9b6c3a5',
  'https://rpc.testnet.lukso.gateway.fm',
  {
    ipfsGateway: 'https://api.universalprofile.cloud/ipfs',
  },
);

// Fetch the supported storage standard of LSP4
let isLSP4 = await erc725js.getData('SupportedStandards:LSP4DigitalAsset');

// Verify if the standard is supported (value !== null)
console.log(isLSP4);
```

</details>

:::note

You can also check custom data on smart contract storage by loading your own JSON schemas.

:::

## Interface Identification

> How to detect if a contract implements a specific interface.

Every LSP standard has their own interface ID using [ERC165](https://eips.ethereum.org/EIPS/eip-165). To verify their specific set of functions (= an **interface**) we can call the standardized `supportsInterface(interfaceId)` function, passing the bytes4 `interfaceId` as a parameter.

Calling this function will return **TRUE** if the contract implements this specific interfaceId.

:::note Example

A **[Universal Profile](../../standards/universal-profile/lsp3-profile-metadata.md)** is a contract based on [ERC725Account](../../standards/universal-profile/lsp0-erc725account.md)(LSP0). Therefore, the contract MUST implement the functions defined in the [ERC725Account interface](https://github.com/lukso-network/LIPs/blob/main/LSPs/LSP-0-ERC725Account.md#interface-cheat-sheet).

:::

:::info
⌨️ The full code of this example can be found in the 👾 [lukso-playground](https://github.com/lukso-network/lukso-playground/blob/main/interface-detection/erc165-interface-check.js) repository and ⚡️ [StackBlitz](https://stackblitz.com/github/lukso-network/lukso-playground?file=interface-detection%2Ferc165-interface-check.js).
:::

```javascript
import UniversalProfile from '@lukso/lsp-smart-contracts/artifacts/UniversalProfile.json' assert { type: 'json' };
import { INTERFACE_IDS } from '@lukso/lsp-smart-contracts/dist/constants.cjs.js';
import Web3 from 'web3';

// Connect to the LUKSO L14 network
const web3 = new Web3('https://rpc.testnet.lukso.network');

// Create an instance of the Universal Profile
const myUPContract = new web3.eth.Contract(
  UniversalProfile.abi,
  '<myContractAddress>',
);

const LSP0_INTERFACE_ID = INTERFACE_IDS.LSP0ERC725Account;
console.log(
  // true or false
  await myUPContract.methods.supportsInterface(LSP0_INTERFACE_ID).call(),
);
```

<details>
    <summary>
    
Instead of using the interface ID from `LSP0ERC725Account`, you can use any of the supported IDs within the `lsp-smart-contracts` library to check all standardizations used by the LSP ecosystem:

</summary>

```js
ERC165                        ERC20
ERC223                        ERC721
ERC721Metadata                ERC725X
ERC725Y                       ERC777
ERC1155

LSP0ERC725Account             LSP1UniversalReceiver
LSP6KeyManager                LSP7DigitalAsset
LSP8IdentifiableDigitalAsset  LSP9Vault
LSP11BasicSocialRecovery      LSP14Ownable2Step
LSP17Extendable               LSP17Extension
LSP20CallVerification         LSP20CallVerifier
LSP25ExecuteRelayCall

```

</details>

:::info Further Information

See [ERC165 - Standard Interface Detection](https://eips.ethereum.org/EIPS/eip-165) for more details.

:::

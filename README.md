## ethereum-abi-types-generator

THIS IS NOT DEPLOYED YET ITS STILL WORK IN PROGRESS!!!!!

Dev typings for all your ethereum ABI contract methods and events with 1 liner integrations with `web3` and `ethers` 👏👏👏👏. Never have to have a runtime error again and bring them into compile time errors in 2 minutes.

<img src="./images/gif-demo.gif" />

A CLI tool which allows you to convert an ABI json file into fully loaded interfaces types.

## Features 🚀

🚀 Compile time errors, never make easy dynamic mistakes again.
<br/>
🚀 Easy 1 line solution to get working
<br/>
🚀 Use the same interfaces as the provider your use to, no changes in how you develop
<br/>
🚀 Works with just a simple ABI json file
<br/>
🚀 Supports `ethers` and `web3` out the box
<br/>
🚀 0 bundle increase, its all dev dependencies so get all the benefit with no negative impact on your build size
<br/>
🚀 Nice easy CLI to allow you to generate these however you like, build scripts, watch file change events its up to you
<br/>
🚀 Supported multidimensional return types aka `bytes32[4] >`[string, string, string, string, string]`
<br/>
🚀 Automatic documenation generated on each method outputting all the details about it which is on the ABI - no jumping back and forth

## Motivation

Blockchain development in JavaScript is already super hard. You have all these tools like `truffle,` `ethers`, `web3` (the list goes on) which you have to get use to and the learning curve is already quite high. On top of this, you have loads of other tools to get things to work as you need. TypeScript allows you to bring runtime errors in the compiler but on contract calls most developers have to either build their own types meaning maintaining them and easily getting out of sync or have no compile type errors using the dreaded `any` hoping and praying you don't break anything. The idea was to not have to make the developer wrap any kind of `web3` or `ethers` instance or use a new tool to get this working but with a simple 1 line change you can use all the same libraries interfaces as what the developer is use to but with `types` `auto-generated` for you to bring back compile-time errors on any contract calls with super ease.

The ABI file is the source of truth for all contract calls so by building types from this file we can be assured our types correct. This is perfect for teams that are working across both contracts and the client.

## Installation

### npm:

```js
$ npm install ethereum-abi-types-generator --save-dev
```

### yarn:

```js
$ yarn add ethereum-abi-types-generator --dev
```

You can install this globally as well but you **must** make sure wherever the `--output` location is which generates the typings file has `ethereum-abi-types-generator` installed in that project, as it uses imports from this package to map the `ContractContext` to make your life easier handling the generic type build up automatically.

## CLI usage

```ts
$ abi-types-generator <abiFileLocation>
$ abi-types-generator <abiFileLocation> --name=ABI_NAME
$ abi-types-generator <abiFileLocation> --watch
$ abi-types-generator <abiFileLocation> --name=ABI_NAME --watch
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --watch
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --name=ABI_NAME
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --name=ABI_NAME --watch
$ abi-types-generator <abiFileLocation> --provider=web3|ethers
$ abi-types-generator <abiFileLocation> --provider=web3|ethers --watch
$ abi-types-generator <abiFileLocation> --name=ABI_NAME --provider=web3|ethers
$ abi-types-generator <abiFileLocation> --name=ABI_NAME --provider=web3|ethers --watch
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --provider=web3|ethers
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --provider=web3|ethers --watch
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --name=ABI_NAME --provider=web3|ethers
$ abi-types-generator <abiFileLocation> --output=PATH_DIRECTORY --name=ABI_NAME --provider=web3|ethers --watch
```

### Arguments

#### `<abiFileLocation>` \* required

The ABI json file path location. Must be a JSON file path.

### `--name=ABI_NAME`

The ABI ts file you want it to be named as, this will also name interface and enums as this. It will remove any `-` and `.` for the `ts` interfaces and enum names.

If not supplied it will take the file name and use that as the abi name throughout.

Example:

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json
```

generates: `./examples/abi-examples/uniswap-factory-abi.ts`

Example:

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json --name=foo-abi
```

generates: `./examples/abi-examples/foo-abi.ts`

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json --name=foo.abi
```

generates: `./examples/abi-examples/foo.abi.ts`

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json --name=foo
```

generates: `./examples/abi-examples/foo.ts`

### `--output=PATH_DIRECTORY`

Where you want the `.ts` to be generated and saved to.

If not supplied it will use the directory of the `<abiFileLocation>` and generate it there.

Example:

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json
```

generates in: `./examples/abi-examples/uniswap-factory-abi.ts`

Example:

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json --output=./examples/ethers/generated-typings
```

generates in: `./examples/ethers/generated-typings/uniswap-factory-abi.ts`

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json --output=./examples/ethers/generated-typings --name=foo
```

generates: `./examples/ethers/generated-typings/foo.ts`

### `--provider=web3|ethers`

What `contract` support you want to generate. This is the library you are using on your dapp and the lib you use to call any contract calls.

```ts
export enum Provider {
  web3 = 'web3',
  ethers = 'ethers',
}
```

If not supplied it will fallback to `web3`

Example:

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json
```

generates the web3 contract typings based on the web3 lib.

Example:

```ts
$ abi-types-generator ./examples/abi-examples/uniswap-factory-abi.json --provider=ethers
```

generates the ethers contract typings based on the web3 lib.

### `--watch`

This will watch the `<abiFileLocation>` for changes and if anything does change it will regenerate the typings and resave them in the `--output` defined or use the default one if one is not defined.

### Formatting

We use `prettier` to format all files, to make sure it matches your coding style just make sure you have a `.prettierrc` defined anywhere in the project and it will use that. If it can not find any it will take these default prettier:

```ts
{
    parser: 'typescript',
    trailingComma: 'es5',
    singleQuote: true,
    bracketSpacing: true,
    printWidth: 80,
}
```

### Using with web3 and ethers

#### Web3 - https://www.npmjs.com/package/web3

The cli tool will generate all your typings for you and expose them in the generated file. Its super easy to start using strongly typed interfaces for all your ABI calls.

Lets say i run the cli command:

```ts
$ abi-types-generator ./abi-examples/fake-contract-abi.json  --output=./web3/generated-typings --name=fake-contract
```

This will generate an `ts` file of `./examples/web3/fake-contract.ts` which has all your strongly typed methods and events.

All you meed to do is cast your `new web3.eth.Contract` code to an `ContractContext` which is exposed in where you defined the `--output` path to. In this example it is `'./examples/web3/fake-contract.ts'`.

#### Example:

```ts
import Web3 from 'web3';
import { AbiExamples } from '../abi-examples';
import { ContractContext } from './generated-typings/fake-contract';

const web3 = new Web3(
  'https://mainnet.infura.io/v3/280bb5b627394709938a7cc0b71a4a58'
);

// Has to cast to unknown as we have made some typings changes to the
// contract interfaces which conflicts with `web3` typings.
// This all work great but the compiler gets confused.
// Casting to unknown first then the `ContractContext` solves this.
const contract = (new web3.eth.Contract(
  AbiExamples.YOUR_ABI as any,
  AbiExamples.YOUR_CONTRACT_ADDRESS
) as unknown) as ContractContext;
```

Easy as that 🔥🔥

#### Full example:

```ts
import Web3 from 'web3';
import { AbiExamples } from '../abi-examples';
import {
  ContractContext,
  TupleInputOnlyRequest,
  TupleNoInputNamesResponse,
} from './generated-typings/fake-contract';

const example = async () => {
  const mockEthereumAddress = '0x419D0d8BdD9aF5e606Ae2232ed285Aff190E711b';

  const web3 = new Web3(
    'https://mainnet.infura.io/v3/280bb5b627394709938a7cc0b71a4a58'
  );

  // Has to cast to unknown as we have made some typings changes to the
  // contract interfaces which conflicts with `web3` typings.
  // This all work great but the compiler gets confused.
  // Casting to unknown first then the `ContractContext` solves this.
  const contract = (new web3.eth.Contract(
    AbiExamples.YOUR_ABI as any,
    AbiExamples.YOUR_CONTRACT_ADDRESS
  ) as unknown) as ContractContext;

  // you now have full typings on `contract.methods` which has generated docs
  const simpleCall = await contract.methods
    .easyExample(true, mockEthereumAddress, new Date().getTime())
    .call();

  console.log(simpleCall);

  // build up a proper typed request object with the interface importable
  // from the typings file generated
  const tupleExampleRequest: TupleInputOnlyRequest = {
    address: mockEthereumAddress,
    timestamps: [
      new Date().getTime(),
      new Date().getTime(),
      new Date().getTime(),
    ],
  };

  // encode abi method all exposed
  const data = contract.methods.tupleInputOnly(tupleExampleRequest).encodeABI();
  console.log(data);

  // any none constant methods will have the correct interface on them as well
  // aka you cant call `.call()` here and the compile will show you this.
  // will also expose the event emitters for your typings to still work with web3
  contract.methods
    .tupleInputOnly(tupleExampleRequest)
    .send({ from: mockEthereumAddress })
    .on('transactionHash', (hash) => {
      console.log(hash);
    });

  const result: TupleNoInputNamesResponse = await contract.methods
    .tupleNoInputNames(mockEthereumAddress, mockEthereumAddress)
    .call();

  console.log(result);

  // full typings on your events with even the filter indexs which will
  // not compile if supply it incorrectly and only expose the correct ones for you
  contract.events
    .Event1({ filter: { token: '0x00' } })
    .on('changed', (event) => {
      console.log(event);
    });

  // can any past events only allowing you to query events which actually exist
  const event = await contract.getPastEvents('Event1', {
    filter: { token: '0x00' },
  });

  console.log(event);
};

example();
```

If the ABI changes and I run the CLI command again or have a --watch on the file, when you try to compile it will flag any errors with your typings for you.

### Ethers - https://www.npmjs.com/package/ethers

The cli tool will generate all your typings for you and expose them in the generated file. Its super easy to start using strongly typed interfaces for all your ABI calls.

Lets say i run the cli command:

```ts
$ abi-types-generator ./abi-examples/fake-contract-abi.json  --output=./ethers/generated-typings --name=fake-contract --provider=ethers
```

This will generate an `ts` file of `./examples/ethers/fake-contract.ts` which has all your strongly typed methods and events.

All you meed to do is cast your `new ethers.Contract` code to an `ContractContext` which is exposed in where you defined the `--output` path to. In this example it is `'./examples/ethers/fake-contract.ts'`.

#### Example:

```ts
import { ethers } from 'ethers';
import { AbiExamples } from '../abi-examples';
import { ContractContext } from './generated-typings/fake-contract';

// Connect to the network
const customHttpProvider = new ethers.providers.JsonRpcProvider(
  'https://mainnet.infura.io/v3/280bb5b627394709938a7cc0b71a4a58'
);

// Has to cast to unknown as we have made some typings changes to the
// contract interfaces which conflicts with `ethers` typings.
// This all work great but the compiler gets confused.
// Casting to unknown first then the `ContractContext` solves this.
const contract = (new ethers.Contract(
  AbiExamples.factoryAddress,
  AbiExamples.factoryAbi,
  customHttpProvider
) as unknown) as ContractContext;
```

Easy as that 🔥🔥

#### Full example:

```ts
import { ethers } from 'ethers';
import { AbiExamples } from '../abi-examples';
import {
  ContractContext,
  TupleInputOnlyRequest,
  TupleNoInputNamesResponse,
} from './generated-typings/fake-contract';

const example = async () => {
  const mockEthereumAddress = '0x419D0d8BdD9aF5e606Ae2232ed285Aff190E711b';

  // Connect to the network
  const customHttpProvider = new ethers.providers.JsonRpcProvider(
    'https://mainnet.infura.io/v3/280bb5b627394709938a7cc0b71a4a58'
  );

  // Has to cast to unknown as we have made some typings changes to the
  // contract interfaces which conflicts with `ethers` typings.
  // This all work great but the compiler gets confused.
  // Casting to unknown first then the `ContractContext` solves this.
  const contract = (new ethers.Contract(
    AbiExamples.factoryAddress,
    AbiExamples.factoryAbi,
    customHttpProvider
  ) as unknown) as ContractContext;

  // you now have full typings on `contract.x` which has generated docs
  const simpleCall = await contract.easyExample(
    true,
    mockEthereumAddress,
    new Date().getTime()
  );

  console.log(simpleCall);

  // you can use the same etherjs flows to send and sign transactions
  // `contract.connect` will return a `ContractContext` so will still have
  // all the typings exposed for you
  const privateKey =
    '0x0123456789012345678901234567890123456789012345678901234567890123';
  const wallet = new ethers.Wallet(privateKey, customHttpProvider);

  // Create a new instance of the Contract with a Signer, which allows
  // update methods
  const contractWithSigner = contract.connect(wallet);

  // build up a proper typed request object with the interface importable
  // from the typings file generated
  const tupleExampleRequest: TupleInputOnlyRequest = {
    address: mockEthereumAddress,
    timestamps: [
      new Date().getTime(),
      new Date().getTime(),
      new Date().getTime(),
    ],
  };

  const tx = await contractWithSigner.tupleInputOnly(tupleExampleRequest);
  console.log(tx.hash);
  // "0xaf0068dcf728afa5accd02172867627da4e6f946dfb8174a7be31f01b11d5364"

  // The operation is NOT complete yet; we must wait until it is mined
  await tx.wait();

  const result: TupleNoInputNamesResponse = await contract.tupleNoInputNames(
    mockEthereumAddress,
    mockEthereumAddress
  );

  console.log(result);

  // full typings on your events
  contract.on(
    'Event1',
    (author: string, oldValue: string, newValue: string, event: any) => {
      // Called when anyone changes the value

      console.log(author);
      // "0x14791697260E4c9A71f18484C9f997B308e59325"

      console.log(oldValue);
      // "Hello World"

      console.log(newValue);
      // "I like turtles."

      console.log(event.blockNumber);
      // 4115004
    }
  );

  // filter that matches my signer as the author
  const filter = contract.filters.Event1(wallet.address);

  // full typings on filter interfaces as well
  contract.filters.Event1(
    filter,
    (author: string, oldValue: string, newValue: string, event: any) => {
      // Called ONLY when your account changes the value

      console.log(author);
      // "0x14791697260E4c9A71f18484C9f997B308e59325"

      console.log(oldValue);
      // "Hello World"

      console.log(newValue);
      // "I like turtles."

      console.log(event.blockNumber);
      // 4115004
    }
  );
};

example();
```

If the ABI changes and I run the CLI command again or have a --watch on the file, when you try to compile it will flag any errors with your typings for you.

## Issues

Please raise any issues in the below link.

https://github.com/joshstevens19/ethereum-abi-types-generator/issues

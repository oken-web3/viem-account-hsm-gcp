# @oken/viem-account-hsm-gcp

[![GitHub License](https://img.shields.io/github/license/oken-web3/viem-account-hsm-gcp?color=blue)](https://github.com/oken-web3/viem-account-hsm-gcp/blob/main/LICENSE)
[![npm version](https://img.shields.io/npm/v/@oken/viem-account-hsm-gcp.svg)](https://www.npmjs.com/package/@oken/viem-account-hsm-gcp)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/oken-web3/viem-account-hsm-gcp/.github/workflows/workflow.yaml?branch=main)](https://github.com/oken-web3/viem-account-hsm-gcp/actions/workflows/workflow.yaml?query=branch%3Amain)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/oken-web3/viem-account-hsm-gcp#contributing)

Use a Google Cloud HSM key to sign transactions with [viem](https://viem.sh/).

## Installing the library

```
yarn add @oken/viem-account-hsm-gcp
```

Note: viem is a peer dependency, so make sure it's installed in your project.

## Using the library

### Simple usage

```typescript
import { createWalletClient, http } from 'viem'
import { mainnet } from 'viem/chains'
import { gcpHsmToAccount } from '@oken/viem-account-hsm-gcp'

const account = await gcpHsmToAccount({
  hsmKeyVersion:
    'projects/your-gcp-project/locations/global/keyRings/your-keyring/cryptoKeys/your-hsm/cryptoKeyVersions/1',
})

const client = createWalletClient({
  account,
  chain: mainnet,
  transport: http(),
})

// Test send a transaction to the HSM key (the HSM key needs to be funded)
const hash = await client.sendTransaction({
  to: account.address,
  value: parseEther('0.001'),
})
```

> [!IMPORTANT]  
> Make sure the key in [Google Cloud KMS](https://console.cloud.google.com/security/kms/keyrings) is a secp256k1 key.

<img src="./docs/images/gcp-key-creation.png" alt="gcp-key-creation" width="500"/>

> [!TIP]
> You don't have to use an HSM key, you can use a software key as well. See the [Google Cloud KMS documentation](https://cloud.google.com/kms/docs/algorithms#protection_levels) for more information.

## Resources

- [viem](https://viem.sh/)
- [@google-cloud/kms](https://www.npmjs.com/package/@google-cloud/kms)

## Acknowledgements

This independent project is based on the original
[@valora/viem-account-hsm-gcp](https://github.com/valora-xyz/viem-account-hsm-gcp), which is a port of
[@celo/wallet-hsm-gcp](https://github.com/celo-org/developer-tooling/tree/0c61e7e02c741fe10ecd1d733a33692d324cdc82/packages/sdk/wallets/wallet-hsm-gcp)
to viem.

Thanks to the Valora and Celo teams for the original implementations.

## End-to-end tests

End-to-end tests require Application Default Credentials with permission to use a compatible Google Cloud KMS key.

```shell
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json
export GCP_HSM_KEY_VERSION=projects/your-project/locations/global/keyRings/your-keyring/cryptoKeys/your-key/cryptoKeyVersions/1
yarn test:e2e
```

## Publishing

Releases use npm trusted publishing and semantic-release. Before enabling them, configure `@oken/viem-account-hsm-gcp`
as a trusted package for this GitHub repository, create the `npm-publish` GitHub environment, and set the repository
variable `NPM_PUBLISH_ENABLED` to `true`.

## Contributing

- [Reporting issues](https://github.com/oken-web3/viem-account-hsm-gcp/issues)
- [Submitting a pull request](https://github.com/oken-web3/viem-account-hsm-gcp/pulls)
- Publishing updates is done automatically via [semantic-release](https://github.com/semantic-release/semantic-release).
  Remember to use [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) or your PR will be rejected (since
  merging it would mess up the changelog and version numbers).

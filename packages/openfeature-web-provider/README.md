# OpenFeature Web SDK JavaScript Confidence Provider

![](https://img.shields.io/badge/lifecycle-beta-a0c3d2.svg)

JavaScript implementation of the Confidence OpenFeature web provider, to be used in conjunction wth the OpenFeature Web SDK.
This implements the static paradigm of OpenFeature.

# Usage

## Installation

To use the OpenFeature Web Provider, install the following packages:

```ts
import { createConfidenceWebProvider } from '@spotify-confidence/openfeature-web-provider';
import { OpenFeature, OpenFeatureAPI } from '@openfeature/web-sdk';

const provider = createConfidenceWebProvider({
  // Configuration options
});

// Use the provider to access flags and evaluate them
// ...
import { OpenFeature, OpenFeatureAPI } from '@openfeature/web-sdk';

const provider = createConfidenceWebProvider({
  // Configuration options
});

// Use the provider to access flags and evaluate them
// ...
```
```

To add the packages to your dependencies run:

```sh
yarn add @openfeature/web-sdk @spotify-confidence/openfeature-web-provider
```

## Usage

To enable the provider, set the evaluation context, and resolve flags, use the following code:

`setProvider` makes the Provider launch a network request to initialize the flags. In cases of success the
`ProviderEvents.Ready` event will be emitted. In cases of failure of the network request, the `ProviderEvent.Error`
event will be emitted. The ProviderEvents events will be emitted only when we are done with the network request, either
a successful or a failed network response. If the network response failed, default values will be returned on flag
evaluation, if the network request is successful we update the flags and then emit `ProviderEvents.Ready`.

```ts
import { createConfidenceWebProvider } from '@spotify-confidence/openfeature-web-provider';
import { OpenFeature, OpenFeatureAPI } from '@openfeature/web-sdk';

const provider = createConfidenceWebProvider({
import { createConfidenceWebProvider } from '@spotify-confidence/openfeature-web-provider';
import { OpenFeature, OpenFeatureAPI } from '@openfeature/web-sdk';

const provider = createConfidenceWebProvider({
  // Configuration options


});

// Use the provider to access flags and evaluate them
// ...

await OpenFeature.setContext({
  targetingKey: 'myTargetingKey',
});
OpenFeature.setProvider(provider);

const client = OpenFeature.getClient();
const result = client.getBooleanValue('flag.my-boolean', false);
```

Notes:

- It's advised not to perform `setContext` while `setProvider` is running, you can await setting the context first, or listen to the `ProviderEvent.Ready` via a handler on `OpenFeaure`.
- It's advised not to perform resolves while `setProvider` and `setContext` are running: resolves might return the default value with reason `STALE` during such operations.

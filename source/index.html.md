---
title: Javascript Library Documentation

language_tabs:
  - javascript: JavaScript

toc_footers:
  - <a href='#'>IOTA Sandbox</a>
  - <a href='#'>IOTA Core Documentation</a>
  - <a href='#'>Other Libraries</a>

includes:
  - api
  - multisig
  - utils
  - validate

search: true
---

# Introduction

This is the official Javascript library for the IOTA Core. It implements both the [official API](https://iota.readme.io/), API wrapper functions as well as newly proposed functionality (such as signing, bundles, utilities and conversion).

<aside class="notice">
    It should be noted that the Javascript Library as it stands right now is an **early beta release**. As such, there might be some unexpected results. Please join the community (see links below) and post [issues on here](https://github.com/iotaledger/iota.lib.js/issues), to ensure that the developers of the library can improve it.
</aside>

> **Join the Discussion**

> If you want to get involved in the community, need help with getting setup, have any issues related with the library or just want to discuss Blockchain, Distributed Ledgers and IoT with other people, feel free to join our Slack. [Slack](http://slack.iotatoken.com/) You can also ask questions on our dedicated forum at: [IOTA Forum](https://forum.iotatoken.com/).

---

# Installation

```
npm install iota.lib.js
```
<br>
<br>
<br>
<br>

```
bower install iota.lib.js
```

The Javascript library can be easily installed either via `npm` or via `bower`. You can also directly use the browserify'ed version, which can be found in the `dist` [folder](https://github.com/iotaledger/iota.lib.js/tree/master/dist) or you can compile it yourself by cloning the repo and running `gulp`.You can either use `iota.js` or the minified version `iota.min.js` in the browser.



---


# Getting Started

```
// Create IOTA instance with host and port as provider
var iota = new IOTA({
    'host': 'http://localhost',
    'port': 14265
});

// Create IOTA instance directly with provider
var iota = new IOTA({
    'provider': 'http://localhost:14265'
});

// now you can start using all of the functions
iota.api.getNodeInfo();
```

After you've successfully installed the library, it is fairly easy to get started by simply launching a new instance of the IOTA object with an optional settings object. When instantiating the object you have the option to decide the API provider that is used to send the requests to and you can also connect directly to the Sandbox environment.

The optional settings object can have the following values:

1. **`host`**: `String` Host you want to connect to. Can be DNS, IPv4 or IPv6. Defaults to `localhost `
2. **`port`**: `Int` port of the host you want to connect to. Defaults to 14265.
3. **`provider`**: `String` If you don't provide host and port, you can supply the full provider value to connect to
4. **`sandbox`**: `Bool` Optional value to determine if your provider is the IOTA Sandbox or not.

You can either supply the remote node directly via the `provider` option, or individually with `host` and `port`, as can be seen in the example on the right.

Overall, there are currently four subclasses that are accessible from the IOTA object:

- **`api`**: Core API functionality for interacting with the IOTA core.
- **`utils`**: Utility related functions for conversions, validation and so on  
- **`multisig`**: Functions for creating and signing multi-signature addresses and transactions.
- **`validate`**: Validator functions that can help with determining whether the inputs or results that you get are valid.

In the future new IOTA Core modules (such as Flash, MAM) and all IXI related functionality will be available.

## How to use the Library

```
iota.api.getNodeInfo(function(error, success) {
    if (error) {
        console.error(error);
    } else {
        console.log(success);
    }
})
```

It should be noted that most API calls are done asynchronously. What this means is that you have to utilize callbacks in order to catch the response successfully. We will add support for sync API calls, as well as event listeners in future versions.

On the right is a simple example of how to access the `getNodeInfo` function.

---

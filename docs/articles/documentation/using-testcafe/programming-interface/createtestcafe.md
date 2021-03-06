---
layout: docs
title: createTestCafe
permalink: /documentation/using-testcafe/programming-interface/createtestcafe.html
checked: true
---
# createTestCafe Factory

Creates a [TestCafe](testcafe.md) server instance.

```text
async createTestCafe([hostname], [port1], [port2], [sslOptions], developmentMode) → Promise<TestCafe>
```

Parameter                     | Type   | Description                                                                                                                                                                                                  | Default
----------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------
`hostname`&#160;*(optional)*       | String | The hostname or IP you will use to address the TestCafe server. Must resolve to the current machine. To test on external devices, use the hostname that is visible in the network shared with these devices. | Hostname of the OS. If the hostname does not resolve to the current machine - its network IP address.
`port1`, `port2`&#160;*(optional)* | Number | Ports that will be used to serve tested webpages.                                                                                                                                                            | Free ports selected automatically.
`sslOptions`&#160;*(optional)*     | Object | Options that allow you to establish HTTPS connection between the TestCafe server and the client browser. This object should contain options required to initialize [a Node.js HTTPS server](https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener). The most commonly used SSL options are described in the [TLS topic](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options) in Node.js documentation. See [Connect to the TestCafe Server over HTTPS](../common-concepts/connect-to-the-testcafe-server-over-https.md) for more information.
`developmentMode`&#160;*(optional)* | Boolean | Enables/disables mechanisms to log and diagnose errors. You should enable this option if you are going to contact TestCafe Support to report an issue. | `false`

**Example**

Create a `TestCafe` instance with the `createTestCafe` function.

```js
const createTestCafe = require('testcafe');

createTestCafe('localhost', 1337, 1338)
    .then(testcafe => {
        runner = testcafe.createRunner();
        /* ... */
    });
```

Establish HTTPS connection with the TestCafe server. The [openssl-self-signed-certificate](https://www.npmjs.com/package/openssl-self-signed-certificate) module is used to generate a self-signed certificate for development use.

```js
'use strict';

const createTestCafe        = require('testcafe');
const selfSignedSertificate = require('openssl-self-signed-certificate');
let runner                  = null;

const sslOptions = {
    key:  selfSignedSertificate.key,
    cert: selfSignedSertificate.cert
};

createTestCafe('localhost', 1337, 1338, sslOptions)
    .then(testcafe => {
        runner = testcafe.createRunner();
    })
    .then(() => {
        return runner
            .src('test.js')

            // Browsers restrict self-signed certificate usage unless you
            // explicitly set a flag specific to each browser.
            // For Chrome, this is '--allow-insecure-localhost'.
            .browsers('chrome --allow-insecure-localhost')
            .run();
    });
```

## See Also

* [TestCafe Class](testcafe.md)
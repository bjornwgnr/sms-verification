# [Parity](https://ethcore.io/parity.html) SMS verification

[![Join the chat at https://gitter.im/ethcore/parity][gitter-image]][gitter-url] [![GPLv3][license-image]][license-url]

[gitter-image]: https://badges.gitter.im/Join%20Chat.svg
[gitter-url]: https://gitter.im/ethcore/parity
[license-image]: https://img.shields.io/badge/license-GPL%20v3-green.svg
[license-url]: https://www.gnu.org/licenses/gpl-3.0.en.html

The following process **verifies a number**:

```
confirm(sha(number),token)
         +-------------------> +--------+
         |                     |contract|   puzzle(sha(token),sha(number))
         |       +-----------> +--------+ <-----------+
         |       |                                    |
         |       | request(sha(number))               |
         |       |                                    |
         |       |                                    |
         |   +------+          POST /:number      +------+
         +-- |client| +-------------------------> |server| code=rand()
             +------+                             +------+ token=sha(code)
token=sha(code)  ^             SMS with code          |
                 +------------------------------------+
```

1. client requests verification (`request(sha(number))`)
2. client calls verification server (`POST /:number`)
3. server generates `code` and computes `token`
4. server posts challenge (`puzzle(sha(token), sha(number))`)
5. server sends SMS to client (with `code`)
6. client computes `token`
7. client posts response (`confirm(token)`)

Now, anyone can easily **check if a number is verified by calling `certified(sha3(number))`** on the contract.

## Installation

```shell
git clone https://github.com/ethcore/sms-verification.git
cd sms-verification
npm install --production
```

## Usage

Up to now, **the account calling `puzzle` has to be the owner of the contract.**

1. Set up an account and put its password in a file.
2. Run parity with `--jsonrpc-apis eth,personal --unlock <account-address> --password <account-password-file>`.
3. Create a config file `config/<env>.json`, which partially overrides `config/default.json`.
4. `export NODE_ENV=<env>; node index.js`

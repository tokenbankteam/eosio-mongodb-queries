# EOSIO MongoDB Queries

[![Build Status](https://travis-ci.org/EOS-BP-Developers/eosio-mongodb-queries.svg?branch=master)](https://travis-ci.org/EOS-BP-Developers/eosio-mongodb-queries)
[![npm version](https://badge.fury.io/js/eosio-mongodb-queries.svg)](https://badge.fury.io/js/eosio-mongodb-queries)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/EOS-BP-Developers/eosio-mongodb-queries/master/LICENSE)

Quickly and easily create complex MongoDB Queries for the EOSIO blockchain.

## Install

**npm**

```bash
$ yarn add eosio-mongodb-queries
```

**web**

```html
<script src="https://wzrd.in/standalone/eosio-mongodb-queries@latest"></script>
```

## Quickstart

```javascript
import { MongoClient } from "mongodb";
import { getAccount } from "eosio-mongodb-queries";

(async () => {
    const client = await MongoClient.connect("mongodb://localhost:27017", { useNewUrlParser: true });

    // Optional Parameters
    const options = {
        gte_block_num: 0,
        lte_block_num: Infinity,
    };
    const result = await getAccount(client, "eosnationftw", options);
    // {
    //   name: 'eosnationftw',
    //   block_num: 6101090,
    //   stake_quantity: 2.8,
    //   stake_net_quantity: 0.4,
    //   stake_cpu_quantity: 2.4
    // }
})();
```

## EOSIO Full Node

You must first enable the MongoDB plugin to your EOSIO full node by including the following to your configuration.

**config.in**

```ini
# Override default maximum ABI serialization time allowed in ms (eosio::chain_plugin)
abi-serializer-max-time-ms = 5000

# Plugin(s) to enable, may be specified multiple times
plugin = eosio::mongo_db_plugin

# MongoDB URI connection string, see: https://docs.mongodb.com/master/reference/connection-string/. If not specified then plugin is disabled. Default database 'EOS' is used if not specified in URI. Example: mongodb://127.0.0.1:27017/EOS (eosio::mongo_db_plugin)
mongodb-uri = mongodb://localhost:27017
```

**Replay Blocks**

To allow actions to be decoded from the ABI, you must replay all blocks from the genesis.

    $ nodeos --replay-blockchain --hard-replay-blockchain --mongodb-wipe

> [More Information on EOSIO GitHub](https://github.com/EOSIO/eos/pull/4304)

## Query Ideas

-   [ ] Vote Tally for `eosio.forum` (submitted by [Denis](https://t.me/deniscarrier) from [EOS Nation](https://eosnation.io))
-   [ ] Block Producer votes & positions (submitted by [Nathan](https://t.me/nsrempel) from [GenerEOS](https://www.genereos.io))
-   [ ] Name auction for all-time bids or current bid (submitted by [Syed](https://t.me/syed_jafri) from [EOS Cafe](https://www.eos.cafe))

## References

**MongoDB Pipeline**

-   <https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/>
-   <https://docs.mongodb.com/manual/reference/operator/aggregation/graphLookup/>

## Contributors

This is made with ♥ by:

-   [EOS Nation](https://eosnation.io) (`eosnationftw`)

> Voting on the EOSIO mainnet helps build more awesome tools for the EOS community.

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

-   [getAccount](#getaccount)
    -   [Parameters](#parameters)
    -   [Examples](#examples)
-   [getActions](#getactions)
    -   [Parameters](#parameters-1)
    -   [Examples](#examples-1)

### getAccount

Get Account Details

#### Parameters

-   `client` **MongoClient** MongoDB Client
-   `name` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Account Name
-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Optional Parameters (optional, default `{}`)
    -   `options.block_num` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Filter by Less-than or equal (&lt;=) the Reference Block Number

#### Examples

```javascript
const name = "eosnationftw";
const options = {
  block_num: 6000000,
};
const result = await getAccount(client, name, options);
// {
//   name: 'eosnationftw',
//   block_num: 2092984,
//   stake_quantity: 1.8,
//   stake_net_quantity: 0.9,
//   stake_cpu_quantity: 0.9
// }
```

Returns **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Account Details

### getActions

Get Account Actions

#### Parameters

-   `client` **MongoClient** MongoDB Client
-   `options` **[Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Optional Parameters (optional, default `{}`)
    -   `options.accounts` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>?** Filter by account contracts (eg: ["eosio","eosio.token"])
    -   `options.names` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>?** Filter by action names (eg: ["undelegatebw", "delegatebw"])
    -   `options.match` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)>?** Match by entries (eg: [{"data.from": "eosio"}])
    -   `options.trx_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Filter by exact Transaction Id
    -   `options.block_num` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Filter by exact Reference Block Number
    -   `options.block_id` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Filter by exact Reference Block ID
    -   `options.lte_block_num` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Filter by Less-than or equal (&lt;=) the Reference Block Number
    -   `options.gte_block_num` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Filter by Greater-than or equal (>=) the Reference Block Number
    -   `options.skip` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Skips number of documents
    -   `options.limit` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Limit the maximum amount of of actions returned
    -   `options.sort` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Sort by ascending order (1) or descending order (-1). (optional, default `-1`)

#### Examples

```javascript
const options = {
    accounts: ["eosio"],
    names: ["delegatebw", "undelegatebw"],
    match: [{"data.from": "eosnationftw"}, {"data.receiver": "eosnationftw"}],
};
const results = await getActions(client, options);
console.log(await results.toArray());
```

Returns **AggregationCursor** MongoDB Aggregation Cursor

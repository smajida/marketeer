Marketeer
=========

Augur market monitor/cache.

Installation
------------

    $ npm install marketeer

Usage
-----
```javascript
var marketeer = require("marketeer");
```
`marketeer.scan` fetches up-to-date data for all Augur markets from the Ethereum blockchain, and stores it in levelDB:
```javascript
var config = {
    http: "rpc_server url",          //specify at least 1 of http, ws, or ipc
    ws: "websocket server url",
    ipc: "path to geth.ipc",
    db: "./path_to_db",              //will be created automatically if doesn't exist already
    limit: null,                     //How many markets to fetch (null for all)
    filtering: true,                 //Listen for new markets, price changes, etc? (only used with watch)
    interval: null,                  //Rescan peridoically? (in ms)
    scan: true,                      //Scan blockchain for markets on startup?
}
marketeer.scan(config, function (err, numUpdates) {
    if (err) throw err;
    console.log("Oh happy day!", numUpdates, "markets have been updated!");
    // fun times here
});
```

`marketeer.watch` creates a persistent blockchain listener.

```javascript
marketeer.watch(config, function (err, numUpdates, data) {
    if (err) throw err;
    console.log("Oh happy day!", numUpdates, "markets have been updated!");
    // fun times here
});
```

`marketeer.unwatch` stops watching the blockchain, removes the filters, and closes the database connection.

APIs
-----
```javascript
//returns marketInfo for a single market
marketeer.getMarketInfo(id, function (err, market){ ... });
```
```javascript
//returns basic market info for all markets in a branch
marketeer.getMarketsInfo(branch, function (err, markets){ ... });
```
```javascript
//retuns full market data for an array of market ids
marketeer.batchGetMarketInfo(ids, function (err, markets) { ... });
```
```javascript
var options = {
    toBlock: blockNumber
    fromBlock: blockNumber
}
//returns price history for a single market
//options is an optional param. w/o it, returns price history for all blocks
marketeer.getMarketPriceHistory(id, options, function (err, history) { ... });
```
```javascript
var options = {
    toBlock: blockNumber
    fromBlock: blockNumber
}
//returns trade history for a single user
//options is an optional param. w/o it, returns trade history for all blocks
marketeer.getAccountTrades(id, options, function (err, trades) { ... });
```

Tests
-----
These tests require two geth testnet accounts funded with cash and testnet ether, as they simulate trading back and forth to test marketeer's listeners.


    $ npm test

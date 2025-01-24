# XRPL Websocket Stream
Subscribe and get notified on various events happening on the XRPL, such as trades.

## WebSocket API Documentation

### Overview
This WebSocket API allows clients to subscribe to trade and ledger updates from the XRP Ledger. Clients can subscribe to specific trading pairs or to all trades and ledgers.

### WebSocket Server
- **URL**: `wss://stream.xrpldata.com`

### Authentication
Clients must provide an API key in the request headers to establish a WebSocket connection.

**Header**:
- `x-api-key`: Your API key.

### Commands
Clients can send the following commands to the WebSocket server:

#### Subscribe
- **Command**: `subscribe`
- **Stream**: `trades` or `ledgers`
- **Parameters for trades**:
  - `base`: Base currency (e.g., `all`, `*`, `XRP`, `issuer+token`)
  - `quote`: Quote currency (e.g., `all`, `*`, `XRP`, `issuer+token`)
  - `type`: `all` or `latest`
  - `exchange`: `all`, `dex`, or `amm`
- **Example**:
  ```json
  {
    "command": "subscribe",
    "stream": "trades",
    "base": "XRP",
    "quote": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq+USD",
    "type": "all",
    "exchange": "amm"
  }
  ```

#### Subscribe Response
The server will respond with a JSON message containing a status code and a message.

**Example**:
```json
{
  "code": 200,
  "message": "Successfully subscribed to: trades_XRP|rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq+USD_all_amm"
}
```

#### Exchange Response
Whenever a trade on one of your subscriptions happens, you receive a message in the following format:

**Example**:
```json
{
  "pair": "XRP|rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq+USD",
  "rate": 2.8306259103514932,
  "volume_base": 345.834205,
  "volume_quote": 978.92726135881,
  "buyer": "rs9ineLqrCzeAGS1bxsrW8x2n3bRJYAh3Q",
  "seller": "rU2ctdgT5jcCMuMP5C3zM57aTt7tUe71cM",
  "taker": "rU2ctdgT5jcCMuMP5C3zM57aTt7tUe71cM",
  "provider": "rs9ineLqrCzeAGS1bxsrW8x2n3bRJYAh3Q",
  "isAMM": true,
  "autobridged": null,
  "tx_hash": "1DFC33471C2044AAB1080AD2CEE1CFA27DDE5512B2F86DE1A210B5885EE84BD9",
  "tx_type": "OfferCreate",
  "offer_sequence": null,
  "ledger_index": 93674332,
  "tx_index": 22,
  "ledger_close_time_utc": "2025-01-24T12:33:52.000Z"
}
```

#### Unsubscribe
- **Command**: `unsubscribe`
- **Stream**: `trades` or `ledgers`
- **Parameters for trades**:
  - `base`: Base currency (e.g., `all`, `*`, `XRP`, `issuer+token`)
  - `quote`: Quote currency (e.g., `all`, `*`, `XRP`, `issuer+token`)
- **Example**:
  ```json
  {
    "command": "unsubscribe",
    "stream": "trades",
    "base": "XRP",
    "quote": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq+USD",
    "type": "all",
    "exchange": "amm"
  }
  ```

#### List Subscriptions
- **Command**: `list_subscriptions`
- **Example**:
  ```json
  {
    "command": "list_subscriptions"
  }
  ```

#### Unsubscribe All
- **Command**: `unsubscribe_all`
- **Example**:
  ```json
  {
    "command": "unsubscribe_all"
  }
  ```

### Error Handling
If an error occurs, the server will respond with a JSON message containing a status code and an error message.

**Example**:
```json
{
  "code": 400,
  "message": "Invalid request: Invalid trading pair"
}
```

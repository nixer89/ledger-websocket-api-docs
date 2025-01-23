# ledger-feed
Publishes information about the ledger via WebSocket.

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
  - `trades`: `all` or `latest`
  - `types`: `all`, `dex`, or `amm`
- **Example**:
  ```json
  {
    "command": "subscribe",
    "stream": "trades",
    "base": "XRP",
    "quote": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq+USD",
    "trades": "all",
    "types": "amm"
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
    "types": "amm"
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

### Responses
The server will respond to commands with a JSON message containing a status code and a message.

**Example**:
```json
{
  "code": 200,
  "message": "Successfully subscribed to: trades_XRP|rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq+USD_amm"
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

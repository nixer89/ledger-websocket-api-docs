# ledger-feed
publishes information about the ledger via websocket

# WebSocket API Documentation

## Overview
This WebSocket API allows clients to subscribe to trade and ledger updates from the XRP Ledger. Clients can subscribe to specific trading pairs or to all trades and ledgers.

## WebSocket Server
- **URL**: `ws:s//stream.xrpldata.com`

## Authentication
Clients must provide an API key in the request headers to establish a WebSocket connection.

**Header**:
- `x-api-key`: Your API key.

## Commands
Clients can send the following commands to the WebSocket server:

### Subscribe
- **Command**: `subscribe`
- **Stream**: `trades` or `ledgers`
- **Parameters for trades**:
  - `base`: Base currency (e.g., `all`, `*`, `XRP`, `issuer+token`)
  - `quote`: Quote currency (e.g., `all`, `*`, `USD`, `issuer+token`)
  - `trades`: `all` or `latest`
- **Example**:
  ```json
  {
    "command": "subscribe",
    "stream": "trades",
    "base": "XRP",
    "quote": "USD",
    "trades": "all"
  }
  ```

### Unsubscribe
- **Command**: `unsubscribe`
- **Stream**: `trades` or `ledgers`
- **Parameters for trades**:
  - `base`: Base currency (e.g., `all`, `*`, `XRP`, `issuer+token`)
  - `quote`: Quote currency (e.g., `all`, `*`, `USD`, `issuer+token`)
- **Example**:
  ```json
  {
    "command": "unsubscribe",
    "stream": "trades",
    "base": "XRP",
    "quote": "USD"
  }
  ```

### List Subscriptions
- **Command**: `list_subscriptions`
- **Example**:
  ```json
  {
    "command": "list_subscriptions"
  }
  ```

### Unsubscribe All
- **Command**: `unsubscribe_all`
- **Example**:
  ```json
  {
    "command": "unsubscribe_all"
  }
  ```

## Responses
The server will respond to commands with a JSON message containing a status code and a message.

**Example**:
```json
{
  "code": 200,
  "message": "Successfully subscribed to: trades_XRP|USD"
}
```

## Error Handling
If an error occurs, the server will respond with a JSON message containing a status code and an error message.

**Example**:
```json
{
  "code": 400,
  "message": "Invalid request: Invalid trading pair"
}
```

## WebSocket Events
- **open**: Triggered when a WebSocket connection is opened.
- **message**: Triggered when a message is received from the client.
- **close**: Triggered when a WebSocket connection is closed.

## Redis Integration
The server subscribes to Redis channels to receive updates and publish them to WebSocket clients.

**Channels**:
- `trades`
- `ledgers`
- `ledger_and_exchanges`

## Scheduled Jobs
The server periodically reloads the WebSocket API keys from a key file.

**Schedule**:
- Every 5 minutes.

## Helper Functions
- `sendMessage(ws, code, message)`: Sends a message to the WebSocket client.
- `checkInputDataBasic(data)`: Checks the basic validity of the input data.
- `checkInputDataSingleTrades(data)`: Checks the validity of the input data for single trades.
- `createChannelId(data)`: Creates a channel ID based on the subscription data.
- `publishMessageOnChannelTrades(server, channel, message)`: Publishes a message on a trades channel.
- `publishMessageOnChannel(server, channel, message)`: Publishes a message on a channel.
- `loadWebsocketKeys()`: Loads the WebSocket API keys from the key file.
- `isValidAddress(address)`: Checks if an address is valid.

This documentation provides an overview of the WebSocket API and its usage. For more details, refer to the source code.

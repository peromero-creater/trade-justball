# trade-justball

# Crypto Trading Terminal — Integration Overview

**Status:** internal/student prototype (non-public)  
**Goal:** integrate TradingView **Trading Platform (Terminal)** with our **own market-data and trading backend**.

## Data Feed we provide (for TradingView)

We do **not** use TradingView data. We aggregate exchange data via our connector and expose:

### UDF (HTTP) for charts


- `GET /config` → datafeed capabilities (resolutions)
- `GET /symbols?symbol=BTCUSDT` → symbol metadata
- `GET /history?symbol=BTCUSDT&resolution=1&from=<sec>&to=<sec>` → bars
  - Response example:
    ```json
    {"s":"ok","t":[1700000100,1700000160],"o":[64000.1,64020.0],
     "h":[64030.0,64040.0],"l":[63990.0,64010.0],"c":[64020.0,64025.0],
     "v":[12.34,10.01]}
    ```
  - When empty: `{"s":"no_data","nextTime":1700000000}`
  - **Times are UNIX seconds.**
- `GET /time` → current UNIX seconds (e.g., `1700000123`)

### Realtime (WebSocket)
- **Bars:** `wss://API.BASE.URL/ws/bars?symbol=BTCUSDT&res=1`  
  Payload: `{ "time":1700000160,"open":64020,"high":64040,"low":64010,"close":64025,"volume":10.01 }`
- **Depth (L2):** `wss://API.BASE.URL/ws/depth?symbol=BTCUSDT`  
  Payload: `{ "bids":[[64020,0.5],...], "asks":[[64025,0.4],...], "ts":1700000188 }`

### Trading (broker) endpoints (minimum)
`POST /orders`, `DELETE /orders/{id}`, `GET /orders`, `GET /positions`, `GET /balances`, and WS `/ws/orders` for updates.

**Initial symbol for demo:** `BTCUSDT` (others will be added).  


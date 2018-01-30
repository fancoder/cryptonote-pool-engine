# Redis schema

## Keys

### `{coin}:blocks:candidates` (sorted set)

- Score: `{block.height}`
- Value: `{hashHex}:{timestamp}:{block.difficulty}:{block.shares}`
- Member removed every time a block is orphaned (lib/blockUnlocker.js)
- Member removed every time a block matures (lib/blockUnlocker.js)
- Member added every time a block is found (lib/pool.js)

### `{coin}:blocks:matured` (sorted set)

- Score: `{block.height}`
- Value: `{block.hash}:{block.time}:{block.difficulty}:{block.shares}:{block.orphaned}:{block.reward}`
- Member added every time a block is orphaned (lib/blockUnlocker.js)
- Member added every time a block matures (lib/blockUnlocker.js)

### `{coin}:charts:hashrate` (string)

- Value: `[[timestamp, value, count], ...]`
- Updated every `{config.charts.pool.hashrate.updateInterval}` seconds (lib/charts.js)

### `{coin}:charts:hashrate:{wallet}` (string)

- Value: `[[timestamp, value, count], ...]`
- Updated every `{config.charts.user.hashrate.updateInterval}` seconds (lib/charts.js)

### `{coin}:charts:workers` (string)

- Value: `[[timestamp, value, count], ...]`
- Updated every `{config.charts.pool.workers.updateInterval}` seconds (lib/charts.js)

### `{coin}:charts:difficulty` (string)

- Value: `[[timestamp, value, count], ...]`
- Updated every `{config.charts.pool.difficulty.updateInterval}` seconds (lib/charts.js)

### `{coin}:charts:price` (string)

- Value: `[[timestamp, value, count], ...]`
- Updated every `config.charts.pool.price.updateInterval` seconds (lib/charts.js)

### `{coin}:charts:profit` (string)

- Value: `[[timestamp, value, count], ...]`
- Updated every `config.charts.pool.profit.updateInterval` seconds (lib/charts.js)

### `{coin}:hashrate` (sorted set)

- Score: `{timestamp}`
- Value: `{jobDifficulty}:{wallet}:{timestamp}`
- Set truncated to last {config.api.hashrateWindow} seconds every {config.api.updateInterval} seconds (lib/api.js)
- Member added every time a share is recorded (lib/pool.js)

### `{coin}:payments:all` (sorted set)

- Score: `{timestamp}`
- Value: `{txHash}:{amount}:{fee}:{mixin}`
- Member added every time a transaction is sent (lib/paymentProcessor.js)

### `{coin}:payments:{wallet}` (sorted set)

- Score: `{timestamp}`
- Value: `{txHash}:{amount}:{fee}:{mixin}`
- Member added every time a transaction is sent (lib/paymentProcessor.js)

### `{coin}:shares:round{height}` (hash)

- Field: `{wallet}`
  - Value: weighted sum of job difficulty of validated and trusted shares
- Renamed from `{coin}:shares:roundCurrent` every time a block is found (lib/pool.js)

### `{coin}:shares:roundCurrent` (hash)

- Field: `{wallet}`
  - Value: weighted sum of job difficulty of validated and trusted shares
  - Incremented every time a share is recorded (lib/pool.js)

### `{coin}:stats` (hash)

- Field: `lastBlockFound`
  - Value: unix timestamp in milliseconds of last block found by pool
  - Updated every time a block is found (lib/pool.js)

### `{coin}:workers:{wallet}` (hash)

- Field: `balance`
  - Value: balance owed in currency unit
  - Decremented every time a wallet is paid (lib/paymentProcessor.js)
- Field: `hashes`
  - Value: sum of job difficulty of validated or trusted shares
  - Incremented every time a share is recorded (lib/pool.js)
- Field: `lastShare`
  - Value: unix timestamp in seconds of last validated share
  - Updated every time a share is recorded (lib/pool.js)
- Field: `paid`
  - Value: total paid in currency unit
  - Incremented every time a wallet is paid (lib/paymentProcessor.js)

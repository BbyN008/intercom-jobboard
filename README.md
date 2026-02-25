# IntercomJobBoard 💼

A **P2P Job Board** forked from [Trac Intercom](https://github.com/Trac-Systems/intercom).

Post jobs and apply for them directly peer-to-peer — no central server, no middleman. Built on the full Trac/Intercom stack using Hyperswarm sidechannels, SC-Bridge, and the MSB settlement layer.

## Trac Address

```
trac1xgva9nn43ehtk4dekcmqrd74msjpjphm53jkkweu5pw5mkathkgs53cpxx
```

## What It Does

- 📡 Peers discover each other via the global `0000intercom` entry channel
- 💼 Three job channels out of the box: `jobs-general`, `jobs-remote`, `jobs-local`
- 📢 Post jobs or applications directly over P2P sidechannels
- 🤖 Agent-ready via SC-Bridge WebSocket interface
- 🔒 PoW spam protection, rate limiting, and welcome enforcement built in
- 💰 Optional TNK-settled transactions via MSB layer

## How to Run

### Prerequisites
```bash
# Node.js 22.x required
node -v

# Install Pear runtime
npm install -g pear
pear -v
```

### Install & Run (Admin / First Peer)
```bash
git clone https://github.com/YOUR_USERNAME/intercom-jobboard
cd intercom-jobboard
npm install

pear run . --peer-store-name admin --msb-store-name admin-msb --subnet-channel intercom-jobboard-v1
```

Copy the **Peer Writer key** from the startup banner.

### Join as Another Peer
```bash
pear run . --peer-store-name peer1 --msb-store-name peer1-msb \
  --subnet-channel intercom-jobboard-v1 \
  --subnet-bootstrap <admin-writer-key-hex>
```

### Run with SC-Bridge (for agents/tools)
```bash
pear run . --peer-store-name agent --msb-store-name agent-msb \
  --subnet-channel intercom-jobboard-v1 \
  --subnet-bootstrap <admin-writer-key-hex> \
  --sc-bridge 1 --sc-bridge-token <your-secret-token>
```

## Posting Jobs (TTY Commands)

```bash
# Post a job on general channel
/sc_send --channel jobs-general --message "HIRE: Senior Dev | Remote OK | 3yr exp | DM trac1..."

# Post a remote job
/sc_send --channel jobs-remote --message "HIRE: UI Designer | Full remote | $80k | Apply: trac1..."

# Post a local job
/sc_send --channel jobs-local --message "HIRE: Barista | Lagos Nigeria | Part time | Call 080..."

# Apply for a job
/sc_send --channel jobs-general --message "APPLY: Looking for dev roles | JS/Python | DM me: trac1..."

# See channel stats
/sc_stats

# Get your peer info
/stats
```

## SC-Bridge Usage (Agents / Bots)

```javascript
// Connect: ws://127.0.0.1:49222
// 1. Authenticate
{ "type": "auth", "token": "YOUR_TOKEN" }

// 2. Post a job
{ "type": "send", "channel": "jobs-general", "message": "HIRE: Python Dev | Remote | trac1..." }

// 3. Listen for jobs (incoming sidechannel_message events)
// { "type": "sidechannel_message", "channel": "jobs-remote", "from": "...", "message": "..." }
```

## Job Channels

| Channel | Purpose |
|---|---|
| `0000intercom` | Global entry / peer discovery |
| `jobs-general` | All job types |
| `jobs-remote` | Remote-only positions |
| `jobs-local` | Location-specific jobs |

## Built On

- [Trac Intercom](https://github.com/Trac-Systems/intercom) — reference P2P stack (forked)
- [trac-peer](https://github.com/Trac-Systems/trac-peer) @ commit d108f52
- [main_settlement_bus](https://github.com/Trac-Systems/main_settlement_bus) @ commit 5088921
- [trac-wallet](https://www.npmjs.com/package/trac-wallet) npm 1.0.1
- [Pear Runtime](https://pears.com) — mandatory runtime

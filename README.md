# Step 1: Install Timechain Node

## Option 1: Binary Installation

```
wget https://example.com/timechain-node -O timechain-node
chmod +x timechain-node
./timechain-node --chain analog --name "Saletaisst"
```

## Option 2: Docker Container

```
docker pull timechain/timechain-node
docker run -d --name timechain-node -p 30333:30333 -p 9944:9944 timechain/timechain-node --chain analog --name "Saletaisst"
```

## Option 3: Build from Source

```
git clone https://github.com/timechain/timechain-node.git
cd timechain-node
cargo build --release
./target/release/timechain-node --chain analog --name "Saletaisst"
```

# Step 2: Generate and Register Session Keys

Generate Session Keys

```
echo '{"id":1,"jsonrpc":"2.0","method":"author_rotateKeys","params":[]}' | websocat -n1 -B 99999999 ws://127.0.0.1:9944
Save the hex output.
Register Session Keys
```

```
# Open Polkadot.Js > Network > Staking > Accounts
# Click "Set Session Key"
# Paste the hex output from the previous step
# Submit the extrinsic
```

# Step 3: Monitoring with Prometheus

Expose Prometheus Metrics

```
./timechain-node --chain analog --name "Saletaisst" --prometheus-external
```
Metrics available at: http://<node-ip>:9090/metrics

Change Prometheus Port

```
./timechain-node --prometheus-port 9091
```

Prometheus Configuration

Add the following job to prometheus.yml:

```
scrape_configs:
  - job_name: 'timechain'
    static_configs:
      - targets: ['<node-ip>:9090']
```

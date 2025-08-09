## Guideline - Bismuth Ledger Mainnet Snapshot

Snapshot Block Height: 4,405,000
Date: August 8, 2025

This **guide** provides step-by-step instructions to download and apply a snapshot of the Bismuth mainnet database at block height 4,405,000.

## Prerequisites
- Access to your Bismuth node server
- Basic command-line knowledge
- Ability to edit cron jobs (if applicable)

## Steps
### 1. Disable Bismuth Node Cron Job (If Applicable)
If you have a cron job that automatically restarts the Bismuth node, disable it to prevent unintended restarts during this process.

- Open the crontab editor:
```crontab -e```
- Comment out the line that starts the Bismuth node by adding a `#` at the beginning.

### 2. Gracefully Shut Down the Bismuth Mainnet Node
Navigate to your Bismuth main folder and run:
```python3 commands.py stop```

### 3. Download the Mainnet Database Snapshot
Change to the `Bismuth/static` directory:
```cd Bismuth/static``` 

Download the snapshot:
```wget https://bismuth.world/snapshot/ledger-4405000.tar.gz```

### 4. Remove Old Database Files
Before extracting the snapshot, delete any old database files to prevent conflicts:
```rm ledger.db hyper.db index.db```

### 5. Extract the Snapshot
Extract the snapshot within the `Bismuth/static/` folder:
```tar -xvzf ledger-4405000.tar.gz```

### 6. Update peers.txt File (Optional)
For optimal network connectivity, ensure you have the latest `peers.txt` file:

- Download it from the [official repository](https://github.com/bismuthfoundation/Bismuth/blob/master/peers.txt).


### 7. Start the Bismuth Mainnet Node
From the main folder, start the node:
```python3 node.py```

### 8. Re-enable the Bismuth Node Cron Job (If Applicable)
If you disabled a cron job in step 1, re-enable it:

- Open the crontab editor:
```crontab -e```
- Uncomment the line by removing the `#` at the beginning.

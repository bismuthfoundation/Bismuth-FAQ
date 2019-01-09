At the webpage https://hypernodes.bismuth.live/?page_id=205 a number of recent Bismuth ledger mainnet snapshots are made available. These snapshots are updated several times per day by members of the foundation. In order to download and validate these snapshots, a script named snapshot_download.py has been created.

On Linux the script can be downloaded with the following command:   
```wget https://raw.githubusercontent.com/bismuthfoundation/util/master/snapshot_download.py```

Run the script inside the main Bismuth folder ~/Bismuth   
node.py must be stopped when running the script   
node.py must have been run at least once before running snapshot_download.py, because the script makes use of the Junction Noise file from the mining algorithm when validating the ledger.

Run the download script with:   
```python3 snapshot_download.py```

The script will then ask the following question:   
```Select snapshot number (1-3) and press ENTER:```

The snapshots are ordered by the most recent first, so snapshot number 1 in the list would normally be chosen.   
After the snapshot has been downloaded the script validates the ledger (file hash, block hashes and difficulties). If the ledger is correctly validated, the following output is generated:   
```
---> Checking file hash (sha256)
---> Correct file hash
---> Extracting tar file
---> Verifying block hashes
Correct db hash
All blocks in the local ledger are valid
---> Verifying mining difficulties
---> Verification of diffs started...
All diffs in the local ledger are valid
```

After this the script ends, node.py can be started, for example with:   
```screen -mS node python3 node.py```

If the downloaded snapshot ledger is accepted, the output from node.py should be:   
```
Status: Chain coherence test complete for static/ledger.db
Status: Chain coherence test complete for static/hyper.db
Status: Indexing tokens from ledger static/ledger.db
Status: Indexing aliases
Status: Recompressing hyperblocks (keeping full ledger)
Status: Moving database to RAM
Status: Moved database to RAM
```

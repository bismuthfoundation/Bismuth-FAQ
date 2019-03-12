# Bismuth for Wallets devs

WIP, thanks to @bizzzy

## Summary in English

For key gen:
- Use RSA to generate private/public keys (be careful - Many pitfalls).
- sha224(public_key) to generate bis address

For transaction signing:
- create transaction tuple: (timestamp, address, recipient, amount, operation, openfield)
- hash the transaction tuple using SHA1
- sign the hashed transaction using PKCS1_v1_5 & private key
- base64 encode the signature & public key
- verify it has been signed correctly
- send tuple to mpinsert with added base64 encoded signature & public key fields : (timestamp, address, recipient, amount, signature_base64encoded, public_key_base64encoded, input, openfield)

The node that you send the transaction to base64 decodes the public key & signature and uses them to verify that the transaction has been signed by the matching private key, then puts it into the mempool to awaiting mining/inclusion on the blockchain.

You will need to test it thoroughly and ensure data sent is in the exact format expected by nodes (timestamp to exactly 2 decimal places, amount to exactly 8 d.p, etc.) so that the signature matches the data sent.

> Test vectors will be provided

## API Reference

There's a load of Bismuth APIs in different languages at https://github.com/EggPool/BismuthAPI with examples

Javascript current reference for signing transactions: https://github.com/gabidi/bismuth-js-crypto/blob/master/src/sign.js  
Do **not** use https://github.com/gabidi/bismuth-js-crypto/blob/master/src/generate.js for generation

## Minimal working code

```
# ------- KEY GEN ---------
key = RSA.generate(4096)
private_key_readable = key.exportKey().decode("utf-8")
public_key_readable = key.publickey().exportKey().decode("utf-8")
address = hashlib.sha224(public_key_readable.encode("utf-8")).hexdigest()  # hashed public key

# ------- SIGN TRANSACTION ---------
timestamp = '%.2f' % time.time()
transaction = (str(timestamp), str(address), str(recipient_input), '%.8f' % float(amount_input), str(operation_input), str(openfield_input))  # this is signed

h = SHA.new(str(transaction).encode("utf-8"))
signer = PKCS1_v1_5.new(key)
signature = signer.sign(h)
signature_enc = base64.b64encode(signature)

# misnomer? should it be public_key_enc?
public_key_hashed = base64.b64encode(public_key_readable.encode('utf-8')) 

verifier = PKCS1_v1_5.new(key) #perhaps verifier should use public key? verifier = RSA.importKey(base64.b64decode(db_public_key_hashed))
if verifier.verify(h, signature):
    tx_submit = (str (timestamp), str (address), str (recipient_input), '%.8f' % float (amount_input), str (signature_enc.decode ("utf-8")), str (public_key_hashed.decode("utf-8")), str (operation_input), str (openfield_input))
    
# --------- CONNECT TO NODE -------------
s = socks.socksocket()
s.settimeout(10)
#s.connect(("bismuth.live", 5658))
s.connect(("127.0.0.1", 5658))

# ------------ SEND TRANSACTION -----------
connections.send (s, "mpinsert", 10)
connections.send (s, tx_submit, 10)
reply = connections.receive (s, 10)
print ("Client: {}".format (reply))

# --------- GET TRANSACTION LIST INVOLVING ADDRESS ----------
connections.send(s, "addlist", 10)  # also worth investigating "addlistlim" & "addlistlimjson"
connections.send(s, keyring.myaddress, 10)

tx_list = connections.receive(s, 10)
print(tx_list)
for transaction in tx_list:
    address = transaction[2]
    recipient = transaction[3]
    amount = transaction[4]
...    
```

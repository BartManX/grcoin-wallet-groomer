# ghostridercoin-wallet-groomer
Collects small transactions in a QT wallet into larger ones. This tool has been updated for Ghostridercoin, but should work on other coins using a BTC-forked QT wallet.

## Requirements:

This tool only works with Python 3. You will need to install the `bitcoinrpc` Python package. This can be done using `pip` by:

Linux:
`pip3 install -r requirements.txt`
or 
Windows:
`python -m pip install -r requirements.txt`

You will also need to set up your QT wallet to start a RPC server, since this is how the tool interacts with the wallet. You can do so by editing your `grcoin.conf` file. Feel free to edit the user/pass/port.

```
rpcuser=user
rpcpassword=password
rpcallowip=127.0.0.1
rpcport=15420
server=1
```

## Usage:

First, you will need to unlock your wallet. You can do this in the Debug Console by typing:

`walletpassphrase password 300`

Replace `password` with your wallet password. `300` is the number of seconds the wallet should stay unlocked for; you will need to finish running the tool within that time (feel free to change it if you'd like).

You should be able to simply run:

`python3 groomer.py "http://rpcuser:rpcpassword@127.0.0.1:15420" --fee .0001

(replace the user, password, and port if you've changed it from the `grcoin.conf` example above)

## Optional Arguments

Normally, there should be no need to use any of the optional arguments. If you're having trouble with the tool, you can run `python groomer.py -h` to get the help page:

```
usage: groomer.py [-h] [-i MAX_AMT_INPUT] [-n MAX_NUM_TX]
                  [-o MAX_AMT_PER_OUTPUT] [-f FEE]
                  [-a ADDRESS] [--reuse] [--auto]
                  rpc_server

This script generates transaction(s) to cleanup your wallet. It looks for the
single addresses which have the most small confirmed payments made to them and
merges all those payments, along with those for any addresses which are all
tiny payments, to a single txout. It must connect to your wallet to inspect your
wallet and to get fresh addresses to pay your coin to.

positional arguments:
  rpc_server            Wallet RPC server info. Example:
                        http://user:password@127.0.0.1:15420

optional arguments:
  -h, --help            show this help message and exit
  -i MAX_AMT_INPUT, --max_amt_input MAX_AMT_INPUT
                        The maximum input amount of a single transaction to
                        consolidate (default: 25 GRR)
  -n MAX_NUM_TX, --max_num_tx MAX_NUM_TX
                        The maximum number of transactions to consolidate at
                        once. Lower this if you are getting a tx-size error
                        (default: 500)
  -o MAX_AMT_PER_OUTPUT, --max_amt_per_output MAX_AMT_PER_OUTPUT
                        The maximum amount (in GRR) to send to a single output
                        address (default: 10000 GRR)
  -f FEE, --fee FEE     The amount of fees (in GRR) to use for the transaction
  --reuse               Reuse the same address for the consolidated funds (default: False)
  -a ADDRESS, --address ADDRESS
                        The address to send the consolidated funds to
  --auto                Automatically answer "yes" to all questions
```

# Useful SecretCLI commands

Some useful `secretcli` commands to query useful debug information and contract interactions

## Setup the `secretcli`

Here you have to make choices, are you going to use the SN testnet or your own tesnet?

Check the actual `secretcli` config status by running 
```sh
secretcli config
```

You should receive the actual config in a JSON response:
```json
{
        "chain-id": "secretdev-1",
        "keyring-backend": "os",
        "output": "text",
        "node": "http://[SERVER]:26657",
        "broadcast-mode": "sync"
}
```
This output is from a secrectli that is already setup on a private testnet.

To change and use your own config run
```sh
secretcli config node http://server:26657
secretcli config chain-id secretdev-1
```

If everything is ok and ready, check the node status by running
```sh
secretcli status
```
The output must be something like this, if everything is correct.
```json
{"NodeInfo":{"protocol_version":{"p2p":"8","block":"11","app":"0"},"id":"115aa0a629f5d70dd1d464bc7e42799e00f4edae","listen_addr":"tcp://0.0.0.0:26656","network":"secretdev-1","version":"0.34.19","channels":"40202122233038606100","moniker":"banana","other":{"tx_index":"on","rpc_address":"tcp://0.0.0.0:26657"}},"SyncInfo":{"latest_block_hash":"89BB084E348C2F893D6DBB26F5451A710CF2D380BFD8EA39972D5DA07BC5AF46","latest_app_hash":"09A71CDBD9632076807105FEAD215D363D2A14F75572943F01CCCB5C2C04FED1","latest_block_height":"556","latest_block_time":"2022-06-04T00:08:04.653715742Z","earliest_block_hash":"519E62F75570A9735ABABEA1AB96D6CD5055C7089A6394FD8E5989CB61219ACF","earliest_app_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855","earliest_block_height":"1","earliest_block_time":"2022-06-03T23:21:18.267451675Z","catching_up":false},"ValidatorInfo":{"Address":"7CBF52A1940609237CCF87B614D9F61F1D9B129B","PubKey":{"type":"tendermint/PubKeyEd25519","value":"KZkY1r9AZKRLnqQpa1UDv0ZpR8qstfN5TNRvEVEfPLI="},"VotingPower":"1"}}
```
<hr>

## Useful Commands For Advanced Use
`Query commands are used to retrieve information about transactions, contracts, wallets... from the blockchain.`

`Querying does not modify nor interacts with contracts. `
`Pure query commands usually are defined by`
```sh
secretcli q [command] [params]

#Querying a transaction hash.

#If the tx hash is the result of a contract upload, you'll receive the contract's code ID.

#If is the result of a contract instantiation, you'll receive the contract address as output.
secretcli q tx [hash]
```
* Query Commands    
  * Query a single transaction
    ```sh
    secretcli q tx [hash]
    ```
  * Query Transactions
    ```sh
    secretcli q txs --events='message.sender=secret1...'
    ```
  * Query All Contract's IDs On The Chain
    ```sh
    secretcli query compute list-code
    #If you're on the public testnet it'll list ALL the contract's IDs.
    #This command is more useful in a private chain.
    ```
    - The output in a private chain should be:

        ```json
        [
            {
                "id": 1,
                "creator": "secret1dkt3232q2pdu80xk9gp8t53wm9jf0wt3l39atm",
                "data_hash": "CAB57FE04F3605A69C53D45115C8DCA23EFB45FDA4D46A0AC2A16FB0B7B707DE"
            },
            {
                "id": 2,
                "creator": "secret1hdfxvpfzg245kj9qvzu4ps9v3q5qmd5evscpz4",
                "data_hash": "87BE1FB7D093DAFF0F2B754E8CACEE682B5654C1D75B92667D5FC21B97F43D0A"
            }
        ]    
        ```
  * Query All Instantiated Contract's By It's ID
    ```sh
    secretcli query compute list-contract-by-code [id] 
    ```
    - The output in a private chain should be:
        ```json
        [
            {
                "address": "secret10pyejy66429refv3g35g2t7am0was7ya6hvrzf",
                "code_id": 2,
                "creator": "secret1hdfxvpfzg245kj9qvzu4ps9v3q5qmd5evscpz4",
                "label": "My contract6121"
            }
        ]
        ```


--

* Compute Commands


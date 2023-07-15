
# Aleo prover guide

## INSTALATION
   
Please use this scrpt to start node

```wget -q -O aleo_snarkos3.sh https://api.nodes.guru/aleo_snarkos3.sh && chmod +x aleo_snarkos3.sh && sudo /bin/bash aleo_snarkos3.sh```

## Useful commands:
 
Check your acoount and save keys: 
```cat $HOME/aleo/account_new.txt```

Check your private key
```grep "prover" /etc/systemd/system/aleo-prover.service | awk '{print $5}'```

Check prover logs
```journalctl -u aleo-prover -f -o cat```

Check aleo client logs
```journalctl -u aleo-client -f -o cat```

Stop and restart prover
```systemctl stop aleo-prover``` 
```systemctl restart aleo-client```

Delete Aleo and SnarkOS
```wget -q -O aleo_remove_snarkos.sh https://api.nodes.guru/aleo_remove_snarkos2.sh && chmod +x aleo_remove_snarkos.sh && sudo /bin/bash aleo_remove_snarkos.sh```


System requirments:

CPU 16 cores (32 preferred)
RAM 16GB (32 preferred)
SSD 128GB
OSUbuntu 20.04 recommended

# Aleo Smart contract deployment guide

Download required packages and create a tmux session
```bash

sudo apt update && \
sudo apt install make clang pkg-config libssl-dev build-essential gcc xz-utils git curl vim tmux ntp jq llvm ufw -y && \
tmux new -s deploy

Add your wallet and private key as a variable. 

```bash
echo Enter your Private Key: && read PK && \
echo Enter your View Key: && read VK && \
echo Enter your Address: && read ADDRESS
```
Compleate next steps
```bash
echo Private Key: $PK && \
echo View Key: $VK && \
echo Address: $ADDRESS
```
___
Generate a tweet with your wallet to get tokens
``` bash
echo "https://twitter.com/intent/tweet?text=@AleoFaucet%20send%2010%20credits%20to%20$ADDRESS"
```
You need to past the output of the command above into your browser, publish a tweet and wait for the response form a bot. 


Install Aleo

```bash
cd $HOME
git clone https://github.com/AleoHQ/snarkOS.git --depth 1
cd snarkOS
bash ./build_ubuntu.sh
source $HOME/.bashrc
source $HOME/.cargo/env
```

```bash
cd $HOME
git clone https://github.com/AleoHQ/leo.git
cd leo
cargo install --path .
```
___
Deploy contract
```bash
echo Enter the Name of your contract "(any)": && read NAME
```
```bash
cd $HOME && mkdir leo_deploy && cd leo_deploy
leo new $NAME
```
In the command below past the link which you got from a bot on Twitter 

```bash
echo Paste the link: && read QUOTE_LINK && \
CIPHERTEXT=$(curl -s $QUOTE_LINK | jq -r '.execution.transitions[0].outputs[0].value')
```

```bash
RECORD=$(snarkos developer decrypt --ciphertext $CIPHERTEXT --view-key $VK)
```
```bash
snarkos developer deploy "$NAME.aleo" \
--private-key "$PK" \
--query "https://vm.aleo.org/api" \
--path "$HOME/leo_deploy/$NAME/build/" \
--broadcast "https://vm.aleo.org/api/testnet3/transaction/broadcast" \
--fee 600000 \
--record "$RECORD"
```

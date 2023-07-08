
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




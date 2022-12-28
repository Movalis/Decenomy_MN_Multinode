![Logo](https://cdn.discordapp.com/attachments/1057646100225462303/1057680243198001303/Decenomy_multinode_guide.png)

# FLS MN Multinode Setup Guide ( GNU/Linux 64bit system )
***
***
## Required
1) **FLS collateral value at current block** ([consult the collateral table](https://github.com/decenomy/FLS#rewards-breakdown))
2) **Local Wallet** ([download wallet](https://github.com/decenomy/FLS/releases/latest))
3) **VPS with modern GNU/Linux 64bit system ( example UBUNTU 20.04 )**
4) **Putty https://www.putty.org/**
5) **Text editor on your local pc to save data for copy/paste**
***
***

### ***On your Local Wallet***
* Create an address with the label MN1 and send exactly the collateral amount valid at the current block to it ([consult the collateral table](https://github.com/decenomy/FLS#rewards-breakdown)). Wait to complete 6 confirmations on “ Payment to yourself “ created.

* Open the Debug Console ( Settings – Debug - Console ) and type ***createmasternodekey***.
You will then receive your masternode private key, and save it in a *.txt to use later.
  ```
  Example:
          createmasternodekey
          w8723KqiiqtiLH6y2ktjfwzuSrNucGAbagpmTmCn1KnNEeQTJKf
* Still at Debug Console type ***getmasternodeoutputs*** and save txhash and outputidx on a txt to use it later.
  ```
  Example:
          "txhash" : "12fce79c1a5623aa5b5830abff1a9feb6a682b75ee9fe22c647725a3gef42saa",
		         "outputidx" : 0

* Repeat this process according to the number of masternodes you want to deploy, just label them differently, MN2, MN3, and so on.

* Do not close your wallet window at this point.

***

### ***On Putty***

* Once logged in to your VPS, *copy/paste* each line one by one with *Enter*

	`wget -q https://raw.githubusercontent.com/Movalis/Decenomy_MN_Multinode/main/multinode.sh`

	`bash multinode.sh`


	Let the script run until you see a similar screen like this

  ![Logo](https://cdn.discordapp.com/attachments/1057646100225462303/1057666832967946361/DSW_Multinode_script.png)

    Remember to do `flits-cli getinfo` to check if VPS synced with the chain

* To add masternodes to the multinode feature we need to add the alias and masternodeprivkey to the new activemasternode.conf file generated by this script

	To do that we can easily use this template (bold needs to be changed) command for each masternode we want to add:

	echo **alias** **activemasternodeprivkey** >> .flits/activemasternode.conf
	
		Guide:
		ALIAS             = The label you create for the masternode address on your Local Wallet
		masternodeprivkey = value you got after preformed the command createmasternodekey, previously done on your Local Wallet
   
 	Final look at how the command should look ( as an example )
   
		echo MN1 w8723KqiiqtiLH6y2ktjfwzuSrNucGAbagpmTmCn1KnNEeQTJKf >> .flits/activemasternode.conf

	To review what we already introduce in activemasternode.conf we can do 

		cat .flits/activemasternode.conf

	![Logo](https://cdn.discordapp.com/attachments/1057646100225462303/1057666866681757696/cat_activemasternode.png)
		
	In case we need to edit the file activemasternode.conf we can do

		nano .flits/activemasternode.conf

* After we introduce all the masternodeprivkey we need to restart the service, for that, we need to do

	`systemctl restart flits.service`

   
	Do not close your terminal/ command prompt window at this point.

***

### ***Go back to your Local Wallet***

* Open the Masternode Configuration file by clicking in the correspondent icon ( top right wallet icons ) 

  ![Logo](https://cdn.discordapp.com/attachments/1057646100225462303/1057649522936922242/DSW_masternode_conf.png)

  and add a new line in it (without #) using this template (bold needs to be changed) in the final save it and close the editor

  **ALIAS VPS_IP**:32972 **masternodeprivkey TXhash Output**

	  Guide:
	  ALIAS             = The label you create for the masternode address
	  VPS_IP            = You can catch this value from the script screen above
	  masternodeprivkey = value you got after preformed the command createmasternodekey, previously done
	  TXhash            = value you got after preformed the command getmasternodeoutputs, previously done on the line "txhash" :
	  Output            = value you got after preformed the command getmasternodeoutputs, previously done on the line "outputidx" :


     Final look at how mastenode.conf file should look ( as an example )
   
	  # Masternode config file
	  # Format: alias IP:port masternodeprivkey collateral_output_txid collateral_output_index
	  # Example: mn1 127.0.0.2:32972 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0#
	  MN1 125.67.32.10:32972 w8723KqiiqtiLH6y2ktjfwzuSrNucGAbagpmTmCn1KnNEeQTJKf 12fce79c1a5623aa5b5830abff1a9feb6a682b75ee9fe22c647725a3gef42saa 0
	
     Repeat this process according to the others masternodes you want to deploy ( as an example )

	  # Masternode config file
	  # Format: alias IP:port masternodeprivkey collateral_output_txid collateral_output_index
	  # Example: mn1 127.0.0.2:32972 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0#
	  MN1 125.67.32.10:32972 w8723KqiiqtiLH6y2ktjfwzuSrNucGAbagpmTmCn1KnNEeQTJKf 12fce79c1a5623aa5b5830abff1a9feb6a682b75ee9fe22c647725a3gef42saa 0
	  MN2 125.67.32.10:32972 wehrndhfyiqtiLH6y2ktjfwzuSrNucGAbagpmTmCn1KnNEeedsfg 43ere77c1a5623aa5b5830abff1a9feb6a682b75ee9fe22c647725a3gef42sww 1

* Close and Re-open Local Wallet, let it sync, and at Masternodes Tab you will find your MN with the status MISSING

	
***(For the next steps you need to have already 16 confirmations on “Payment to yourself “ created in the first step)***


* Go to Debug Console and type the following:

    `startmasternode all false`
***

### ***Go back to Putty***

   `flits-cli getmasternodestatus`

You need to get **"status" : 4**

## Congratulations your Flits multinode service is running now

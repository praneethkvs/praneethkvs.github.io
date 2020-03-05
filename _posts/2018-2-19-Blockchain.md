---
layout: post
title:  Setting up a Blockchain using MultiChain
---

### Overview
This is a tutorial that walks you through the process of spinning up a blockchain using the MultiChain software
which people can download and run to design, deploy, and operate distributed ledgers (blockchains) that track
ownership of digital tokens or assets.

### Initial Setup
#### Step:1  
The First thing we need to do is download the MultiChain software.
You can head on over to <https://www.multichain.com> and click "Download MultiChain" or 
click on the following link <https://www.multichain.com/download-install/>
which should directly take you to the download page as seen in the image below.  
  

![MultiChain Download Page]({{ site.baseurl }}/images/Download.PNG "MultiChain Download Page")

Select the correct version based on your operating system.   
**Note: There is no supported Mac version for MultiChain as of now.**

#### Step:2  
Extract the downloaded zip file into a directory. You should see the following files.
  
![MultiChain Folder]({{ site.baseurl }}/images/folder.PNG "MultiChain Folder")

#### Step:3
Open the extracted multichain folder then type in cmd into the address bar to open a command window. Note,
you should open the command window in the directory where MultiChain is stored or it won't work.

![MultiChain cmd]({{ site.baseurl }}/images/cmd.png "MultiChain cmd")
  
  
### Creating the Blockchain
Now that you have your command window open, we can start coding.  

#### Step:4
To create a blockchain, type the following command into the command window.  

```cmd
multichain-util create name
```  
Replace name with the preferred name for your blockchain then press enter.
  
![MultiChain cmd]({{ site.baseurl }}/images/create.png "Create Blockchain")  

This does not actually create the blockchain but as you can see from the output the blockchain parameters
have been setup. You can view and change the parameters by editing the **params.dat** file.  

Now we run the following command to create the blockchain.

```cmd
multichaind name -daemon
```
![MultiChain]({{ site.baseurl }}/images/daemon.png "Create Daemon")  
  
This command starts the blockchain by creating what is called a **genesis block** which has the blockchain parameters and runs a daemon which other nodes can connect to.
It also gives you an ip address using which other nodes can connect to the blockchain.

### Connecting to the Blockchain
#### Step:5

Now we can connect to this blockchain from elsewhere. So we open up a command window in the MultiChain folder from another server and use the following command to connect to the blockchain.

```cmd
multichaind name@10.226.37.163:7723
```
![MultiChain]({{ site.baseurl }}/images/request.PNG "Request Connection")  

The blockchain will successfully intialize, but you must be granted permission to connect. Copy and send the wallet address provided to you to the blockchain originator/blockchain admin. The hex string starting with 1... in the image above is the wallet address

#### Step:6
The user with authority to grant connection i.e. the blockchain creator must grant requests to each of the connecting nodes by running the following command.

```cmd
multichain-cli name grant 1... connect,send,receive
```
where "1..." is the adress of the server node trying to connect.  

![MultiChain]({{ site.baseurl }}/images/access_granted.png "Access Granted")
  
![MultiChain]({{ site.baseurl }}/images/multinodesaccess.png "Multiple Nodes Access Granted")
  
#### Step:7
After permission is granted, the new user must again type in the node address as done in Step 5 (Note, "multichaind name -daemon" could also be used here.) Successful connection to the blockchain is confirmed by "Node ready".  

Now we have our two daemon nodes running, one that created the blockchain and one that is now connected to the blockchain. Any number of nodes can connect to the blockchain. **To carry out other commands and functions we need to open up new command windows in the multichain folder.**  So open up a new command window in the multichain folder like we did in **Step:3**. 

**Caveat:** The daemon nodes should always keep running in the background as long as the blockchain is alive.  

**Caveat:** The commands used henceforth must be typed into a new command window and not the daemon windows.

### Creating Assets
#### Step:8
To create an asset, we need to get get the address that has the permission to create assets, so on the first server that is the node that created the blockchain we run the following command in a new command window:

```cmd
multichain-cli name listpermissions issue
```

![MultiChain]({{ site.baseurl }}/images/listpermissions.png "List Permission")
  
Now that we have the address, we create an asset on this node using the following command:  
```cmd
multichain-cli name issue 1... assetname 1000 
```   
![MultiChain]({{ site.baseurl }}/images/createasset.png "Create Asset")
  
where 1... is the address we obtained earlier, assetname is the name of the asset and 1000 is the number of units of the asset we want to create. If we want to create an asset which can be further sub-divided into parts, we can pass another parameter to the command, for example in the example below .01 implies that each asset can further be subdivided into 100 parts.   

```cmd
multichain-cli name issue 1... assetname 1000 0.01
```
  
To check if the asset was created successfully or not, we can use the following command to get a list of all the available assets in the blockchain.  
```cmd
multichain-cli name listassets
```
![MultiChain]({{ site.baseurl }}/images/listassets.png "List Assets")  
As you can see in the image above, an asset was created with an issue quantity of 1000 units.  



### Sending Assets
#### Step:9
 We can send assets to the other nodes that are connected to the blockchain using the following command  
 
 ```cmd
 multichain-cli name sendasset 1... asset1 100
```
where 1... is the address of the node to which the asset is being sent.   
    
Instead of just sending assets, we can also send transaction metadata along with assets. But before doing this we need to create some data that can be sent.

#### Step:10
We create the following JSON Object that represents a Car Title consisting of Make, Model and VIN.

```
{"make":"Ford","model":"Mustang","VIN":"1HGCM82633A004352"}

```

#### Step:11
Metadata that is sent in a blockchain must be in a hexadecimal format. So we need to convert this JSON object into hex. For this we can use several websites available online. The site used in the image below <http://www.online-toolz.com/tools/text-hex-convertor.php>

![MultiChain]({{ site.baseurl }}/images/jsontohex.png "JSON to Hex")

The Hexadecimal data that we get is as shown below.
```hex
7b226d616b65223a22466f7264222c226d6f64656c223a224d75737461  
6e67222c2256494e223a22314847434d383236333341303034333532227d
```
#### Step:12
Now we send an asset along with the hexadecimal metadata to another node using the following command

```cmd
multichain-cli name sendwithdata 1... "{\"assetname\":1}" 7b226d6...
```
Where 1... is the address of the node the asset is being sent to, the next parameter is a json object of the asset name which is "assetname" in the above code and the quantity of assets to be sent which is "1", followed by the hexadecimal metadata.  

![MultiChain]({{ site.baseurl }}/images/sendwithdata.png "Send With Data")
  
The long hex string in the image above "842865..." is the transaction id. Every transaction returns a transaction id which is used to identify the transaction.  
    
You can see four different transactions in the image below where one asset each is being sent to four different nodes.  

![MultiChain]({{ site.baseurl }}/images/multisend.png "Multi Send")  

If we want to look at the quantity of assets remaining with each node after performing transactions, each node can run the following command to get a list of assets available with that node.  
```cmd
multichain-cli name gettotalbalances
```

Since we created 1000 units of an asset in **Step:8** and then sent 1 asset each to 4 other nodes in **step:12**, the first node that issued the assets must be left with a quantity of 996 units, which you can see in the image below:  

![MultiChain]({{ site.baseurl }}/images/gettotalbalances.png "Total Balances Creator Node")  

If we run the same command in one of the other nodes that is connected to the blockchain we should see a quantity of 1 asset, which is seen in the image below:  

![MultiChain]({{ site.baseurl }}/images/gettotalbalances2.png "Total Balances Other Nodes")  



### Verifying Transactions
#### Step:13

The receiving node can verify that the transaction is successfull by using the following command:

```cmd
multichain-cli name getwallettransaction a1b2...
```
Where a1b2... is the transaction id.

![MultiChain]({{ site.baseurl }}/images/getwaltrans.png "Verify Transaction")  

Also remember we sent some data along with the transaction in **Step:12**, this metadata is also displayed in the image above in the data field, we can verify that it is the same hex code that we obtained from **Step:11**.


### Other Useful Commands
Below is a list of some other useful commands to query the blockchain.  
```cmd
multichain-cli name getblockchainparams //Get the parameters of this blockchain
multichain-cli name getpeerinfo         //For each node, get a list of connected peers
multichain-cli name help                //See a list of all available commands
multichain-cli name getinfo             //To get general information
```

The output of getpeerinfo is as shown in the images below.  

![MultiChain]({{ site.baseurl }}/images/getpinfo1.PNG "Get Peer Info")  
![MultiChain]({{ site.baseurl }}/images/getpinfo2.PNG "Get Peer Info") 
![MultiChain]({{ site.baseurl }}/images/getpinfo3.PNG "Get Peer Info") 
  
### References:
For a more thorough tutorial, you can check out the MultiChain Getting Started page here <https://www.multichain.com/getting-started/>  
For a comprehensive list of commands, check out the link below <https://www.multichain.com/developers/json-rpc-api/>








  



  








  











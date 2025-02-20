# How to Connect to MetaMask?

## Introduction

After starting the Indexing Service on your preferred hosting enviornment(Local, AWS, or Linux), the next step is to connect to MetaMask.

MetaMask is the browser-based crypto wallet that allows you to interact with the SubQuery Web3 application. If you do not have MetaMask, please download it from the appropriate “store” and install it. 


## Steps to Connect to MetaMask

### 1. Initial Preparation:

Navigate to your Indexer administration page. The URL will depend on where you have installed your Indexing service. 

For: 

- localhost, it will most likely be [http://localhost:8000/](http://localhost:8000/)
- AWS, it will likely be [http://you-ec2-public-path:8000/](http://localhost:8000/) for example ([ec2-14-273-116-26.ap-southeast-2.compute.amazonaws.com:8000](http://ec2-54-253-236-26.ap-southeast-2.compute.amazonaws.com/))

Then, you will see the following message: <br />

![Connect MetaMask](/assets/img/connect_metamask.png)

### 2. Add a network

Once MetaMask is installed, select the network dropdown and click “Add Network”. <br />

![Add a Network](/assets/img/add_network_metamask.png)

Now, fill in the network details with the following:

1. Network Name: Acala
2. New RPC URL: https://tc7-eth.aca-dev.network
3. Chain ID: 595
4. Currency Symbol: ACA
5. Block Explorer URL: https://blockscout.mandala.acala.network/ (Optional)

<br />

![Network Details](/assets/img/network_details_metamask.png)

<br />

The MetaMask will display as follows: <br />

![Network Added](/assets/img/network_added_metamask.png)

### 3. Connect with MetaMask

Now, go back and refresh the Indexer administration page. MetaMask will pop up asking you to connect with MetaMask. <br />

![Select Account_Connect MetaMask](/assets/img/connectmetamask_selectaccount.png)

Follow the steps. When MetaMask is connected, you will see the status as connected. <br />

![Metask Connected Screen](/assets/img/metask_connected_status.png)

After connecting to MetaMask, the admin app will display the registry page. <br />

![Stake SQT_Registry Page](/assets/img/stakeSQT_index_project.png)

---
## Next Steps:

Once you have connected with MetaMask, you need to fund your account with some DEV and Testnet tokens. 

Please visit [How to Request for Testnet Tokens](../metamask/request-token.md) to learn more.
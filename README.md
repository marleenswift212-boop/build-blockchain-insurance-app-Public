# Build Blockchain Insurance Application 
This project showcases the use of blockchain in insurance domain for claim processing. In this application, we have four participants, namely insurance, police, repair shop and the shop. Furthermore, each participant will own its own peer node. The insurance peer is the insurance company providing the insurance for the products and it is responsible for processing the claims. Police peer is responsible for verifying the theft claims. Repair shop peer is responsible for repairs of the product while shop peer sells the products to consumer. The value of running this network on the IBM Blockchain Platform is that you can easily customize the network infrastructure as needed, whether that is the location of the nodes, the CPU and RAM of the hardware, the endorsement policy needed to reach consensus, or adding new organizations and members to the network.

Note: This code pattern can either be run locally, or connected to the IBM Blockchain Platform. If you only care about running this pattern locally, please find the local instructions here.

Audience level : Intermediate Developers

When the reader has completed this code pattern, they will understand how to:

Create a Kubernetes Cluster using the IBM Kubernetes Service
Create an IBM Blockchain service, and launch the service onto the Kubernetes cluster
Create a network, including all relevant components, such as Certificate Authority, MSP (Membership Service Providers), peers, orderers, and channels.
Deploy a packaged smart contract onto the IBM Blockchain Platform by installing and instantiating it on the peers.
Use the connection profile from IBM Blockchain Platform to create application admins, and submit transactions from our client application.
View transaction details on our channel from IBM Blockchain Platform.
Application Workflow Diagram
<img width="1530" height="1189" alt="image" src="https://github.com/user-attachments/assets/a5adbc19-c90d-44e6-8191-1774a74d5559" />


The blockchain operator creates a IBM Kubernetes Service cluster (32CPU, 32RAM, 3 workers recommended) and an IBM Blockchain Platform 2.0 service.
The IBM Blockchain Platform 2.0 creates a Hyperledger Fabric network on an IBM Kubernetes Service, and the operator installs and instantiates the smart contract on the network.
The Node.js application server uses the Fabric SDK to interact with the deployed network on IBM Blockchain Platform 2.0.
The React UI uses the Node.js application API to interact and submit transactions to the network.
The user interacts with the insurance application web interface to update and query the blockchain ledger and state.
# Included Components
IBM Blockchain Platform V2 Beta gives you total control of your blockchain network with a user interface that can simplify and accelerate your journey to deploy and manage blockchain components on the IBM Cloud Kubernetes Service.
IBM Cloud Kubernetes Service creates a cluster of compute hosts and deploys highly available containers. A Kubernetes cluster lets you securely manage the resources that you need to quickly deploy, update, and scale applications.
# Featured technologies
Hyperledger Fabric v1.4 is a platform for distributed ledger solutions, underpinned by a modular architecture that delivers high degrees of confidentiality, resiliency, flexibility, and scalability.
Node.js is an open source, cross-platform JavaScript run-time environment that executes server-side JavaScript code.
React A declarative, efficient, and flexible JavaScript library for building user interfaces.
Docker Docker is a computer program that performs operating-system-level virtualization. It was first released in 2013 and is developed by Docker, Inc.
# Watch the Video - Multiple Organization and Multiple Peer App Demo #1 - Intro
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/0a607f2e-0a8d-450d-8816-516478ff61d3" />


# Watch the Video - IBM Blockchain Tutorial: Multiple Organization and Multiple Peer App Demo #2 - Build Nodes
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/220a87d0-62b7-48f8-8049-7464245096c7" />


# Watch the Video - Multiple Organization and Multiple Peer App Demo #1 - Intro
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/6be53e77-36fd-4554-821b-78adb3a076e8" />


# Prerequisites
We find that Blockchain can be finicky when it comes to installing Node. We want to share this StackOverflow response - because many times the errors you see with Compose are derived in having installed either the wrong Node version or took an approach that is not supported by Compose:

IBM Cloud account
Docker - latest
Docker Compose - latest
NPM - latest
nvm - latest
Node.js - Node v8.9.x
Git client - latest
Python - 2.7.x
React - 15.6.1
# Steps
# Steps (Local Network)
To run a local network, you can find steps here

# Steps (Cloud Network)
Create IBM Cloud services
Build a network - Certificate Authority
Build a network - Create MSP Definitions
Build a network - Create Peers
Build a network - Create Orderer
Build a network - Create and Join Channel
Deploy Insurance Smart Contract on the network
Connect application to the network
Enroll App Admin Identities
Run the application
Important Note: This pattern is more advanced because it uses four organizations. For this reason, you will likely have to get a paid kubernetes cluster to run this pattern on the cloud, since a free cluster will not have the CPU/storage necessary to deploy all of the pods that we need to run this pattern. There are other patterns that leverage a free Kubernetes cluster (and only two organizations), so if you want to try that one out first, go here.

#Step 1. Create IBM Cloud services
Create the IBM Cloud Kubernetes Service. You can find the service in the Catalog. Note that for this code pattern, we need to use the 32CPU, 32GB RAM cluster.

Once you reach the create a new cluster page you will need to do the following:

Choose standard cluster type
Fill out cluster name
choose Geography: North America
Choose Location and availability: Multizone
Choose Metro: Dallas
Choose Worker nodes: Dallas 10 only
Choose Master service endpoint: Both private & public endpoints
Choose Default worker pool: 1.12.7 (Stable, Default)
Choose Master service endpoint: Both private & public endpoints
Choose Flavor 32 Cores 32GB RAM, Ubuntu 18
Choose Encrypt local disk Yes
Choose Worker nodes 3
Click on create cluster. The cluster takes around 15-20 minutes to provision, so please be patient!

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/25694ee8-bd13-4535-b45d-c9d4d6e0291e" />



After your kubernetes cluster is up and running, you can deploy your IBM Blockchain Platform V2 Beta on the cluster. The service walks through few steps and finds your cluster on the IBM Cloud to deploy the service on.

<img width="1420" height="798" alt="image" src="https://github.com/user-attachments/assets/9a7f9df6-ce2a-4bc3-8a2d-23c930f78b0b" />



Once the Blockchain Platform is deployed on the Kubernetes cluster, you can launch the console to start operating on your blockchain network.

<img width="1536" height="860" alt="image" src="https://github.com/user-attachments/assets/7d53d4ef-a2aa-4275-93d7-eda5c7a41fcc" />



# Step 2. Build a network - Certificate Authority
We will build a network as provided by the IBM Blockchain Platform documentation. This will include creating a channel with a single peer organization with its own MSP and CA (Certificate Authority), and an orderer organization with its own MSP and CA. We will create the respective identities to deploy peers and operate nodes.

Create your insurance organization CA
Click Add Certificate Authority.
Click IBM Cloud under Create Certificate Authority and Next.
Give it a Display name of Insurance CA.
Specify an Admin ID of admin and Admin Secret of adminpw.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/7d2e9f14-3967-4b72-a36b-ec942dfeadf1" />



Create your shop organization CA (process is same as shown in gif above)
Click Add Certificate Authority.
Click IBM Cloud under Create Certificate Authority and Next.
Give it a Display name of Shop CA.
Specify an Admin ID of admin and Admin Secret of adminpw.
Create your repair shop organization CA (process is same as shown in gif above)
Click Add Certificate Authority.
Click IBM Cloud under Create Certificate Authority and Next.
Give it a Display name of Repair Shop CA.
Specify an Admin ID of admin and Admin Secret of adminpw.
Create your police organization CA (process is same as shown in gif above)
Click Add Certificate Authority.
Click IBM Cloud under Create Certificate Authority and Next.
Give it a Display name of Police CA.
Specify an Admin ID of admin and Admin Secret of adminpw.
Use your CA to register insurance identities
Select the Insurance CA Certificate Authority that we created.
First, we will register an admin for our Insurance Organization. Click on the Register User button. Give an Enroll ID of insuranceAdmin, and Enroll Secret of insuranceAdminpw. Click Next. Set the Type for this identity as client and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
We will repeat the process to create an identity of the peer. Click on the Register User button. Give an Enroll ID of insurancePeer, and Enroll Secret of insurancePeerpw. Click Next. Set the Type for this identity as peer and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/bf123a7c-8ec5-4259-a4c5-0f7f24a410ef" />




Use your CA to register shop identities (process is same as shown in gif above)
Select the Shop CA Certificate Authority that we created.
First, we will register an admin for our Insurance Organization. Click on the Register User button. Give an Enroll ID of shopAdmin, and Enroll Secret of shopAdminpw. Click Next. Set the Type for this identity as client and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
We will repeat the process to create an identity of the peer. Click on the Register User button. Give an Enroll ID of shopPeer, and Enroll Secret of shopPeerpw. Click Next. Set the Type for this identity as peer and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
Use your CA to register repair shop identities (process is same as shown in gif above)
Select the Repair Shop CA Certificate Authority that we created.
First, we will register an admin for our Insurance Organization. Click on the Register User button. Give an Enroll ID of repairShopAdmin, and Enroll Secret of repairShopAdminpw. Click Next. Set the Type for this identity as client and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
We will repeat the process to create an identity of the peer. Click on the Register User button. Give an Enroll ID of repairShopPeer, and Enroll Secret of repairShopPeerpw. Click Next. Set the Type for this identity as peer and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
Use your CA to register police shop identities (process is same as shown in gif above)
Select the Police CA Certificate Authority that we created.
First, we will register an admin for our Insurance Organization. Click on the Register User button. Give an Enroll ID of policeAdmin, and Enroll Secret of policeAdminpw. Click Next. Set the Type for this identity as client and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
We will repeat the process to create an identity of the peer. Click on the Register User button. Give an Enroll ID of policePeer, and Enroll Secret of policePeerpw. Click Next. Set the Type for this identity as peer and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
# Step 3. Build a network - Create MSP Definitions
Create the insurance MSP definition
Navigate to the Organizations tab in the left navigation and click Create MSP definition.
Enter the MSP Display name as Insurance MSP and an MSP ID of insurancemsp.
Under Root Certificate Authority details, specify the peer CA that we created Insurance CA as the root CA for the organization.
Give the Enroll ID and Enroll secret for your organization admin, insuranceAdmin and insuranceAdminpw. Then, give the Identity name, Insurance Admin.
Click the Generate button to enroll this identity as the admin of your organization and export the identity to the wallet. Click Export to export the admin certificates to your file system. Finally click Create MSP definition.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/55312821-b705-42c5-ac1b-8503538fd7f0" />




Create the shop MSP definition (same process as shown in gif above)
Navigate to the Organizations tab in the left navigation and click Create MSP definition.
Enter the MSP Display name as Shop MSP and an MSP ID of shopmsp.
Under Root Certificate Authority details, specify the peer CA that we created Shop CA as the root CA for the organization.
Give the Enroll ID and Enroll secret for your organization admin, shopAdmin and shopAdminpw. Then, give the Identity name, Shop Admin.
Click the Generate button to enroll this identity as the admin of your organization and export the identity to the wallet. Click Export to export the admin certificates to your file system. Finally click Create MSP definition.
Create the repair shop MSP definition (same process as shown in gif above)
Navigate to the Organizations tab in the left navigation and click Create MSP definition.
Enter the MSP Display name as Repair Shop MSP and an MSP ID of repairshopmsp.
Under Root Certificate Authority details, specify the peer CA that we created Repair Shop CA as the root CA for the organization.
Give the Enroll ID and Enroll secret for your organization admin, repairShopAdmin and repairShopAdminpw. Then, give the Identity name, Repair Shop Admin.
Click the Generate button to enroll this identity as the admin of your organization and export the identity to the wallet. Click Export to export the admin certificates to your file system. Finally click Create MSP definition.
Create the police MSP definition (same process as shown in gif above)
Navigate to the Organizations tab in the left navigation and click Create MSP definition.
Enter the MSP Display name as Police MSP and an MSP ID of policemsp.
Under Root Certificate Authority details, specify the peer CA that we created Police CA as the root CA for the organization.
Give the Enroll ID and Enroll secret for your organization admin, policeAdmin and policeAdminpw. Then, give the Identity name, Police Admin.
Click the Generate button to enroll this identity as the admin of your organization and export the identity to the wallet. Click Export to export the admin certificates to your file system. Finally click Create MSP definition.
# Step 4. Build a network - Create Peers
Create an insurance peer
On the Nodes page, click Add peer.
Click IBM Cloud under Create a new peer and Next.
Give your peer a Display name of Insurance Peer.
On the next screen, select Insurance CA as your Certificate Authority. Then, give the Enroll ID and Enroll secret for the peer identity that you created for your peer, insurancePeer, and insurancePeerpw. Then, select the Administrator Certificate (from MSP), Insurance MSP, from the drop-down list and click Next.
Give the TLS Enroll ID, admin, and TLS Enroll secret, adminpw, the same values are the Enroll ID and Enroll secret that you gave when creating the CA. Leave the TLS CSR hostname blank.
The last side panel will ask you to Associate an identity and make it the admin of your peer. Select your peer admin identity Insurance Admin.
Review the summary and click Submit.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/ba1ae322-7d2a-43d4-9e55-2238ce634a52" />




Create a shop peer (same process as shown in gif above)

On the Nodes page, click Add peer.
Click IBM Cloud under Create a new peer and Next.
Give your peer a Display name of Shop Peer.
On the next screen, select Shop CA as your Certificate Authority. Then, give the Enroll ID and Enroll secret for the peer identity that you created for your peer, shopPeer, and shopPeerpw. Then, select the Administrator Certificate (from MSP), Shop MSP, from the drop-down list and click Next.
Give the TLS Enroll ID, admin, and TLS Enroll secret, adminpw, the same values are the Enroll ID and Enroll secret that you gave when creating the CA. Leave the TLS CSR hostname blank.
The last side panel will ask you to Associate an identity and make it the admin of your peer. Select your peer admin identity Shop Admin.
Review the summary and click Submit.
Create a repair shop peer (same process as shown in gif above)

On the Nodes page, click Add peer.
Click IBM Cloud under Create a new peer and Next.
Give your peer a Display name of Repair Shop Peer.
On the next screen, select Repair Shop CA as your Certificate Authority. Then, give the Enroll ID and Enroll secret for the peer identity that you created for your peer, repairShopPeer, and repairShopPeerpw. Then, select the Administrator Certificate (from MSP), Repair Shop MSP, from the drop-down list and click Next.
Give the TLS Enroll ID, admin, and TLS Enroll secret, adminpw, the same values are the Enroll ID and Enroll secret that you gave when creating the CA. Leave the TLS CSR hostname blank.
The last side panel will ask you to Associate an identity and make it the admin of your peer. Select your peer admin identity Repair Shop Admin.
Review the summary and click Submit.
Create a police peer (same process as shown in gif above)

On the Nodes page, click Add peer.
Click IBM Cloud under Create a new peer and Next.
Give your peer a Display name of Police Peer.
On the next screen, select Police CA as your Certificate Authority. Then, give the Enroll ID and Enroll secret for the peer identity that you created for your peer, policePeer, and policePeerpw. Then, select the Administrator Certificate (from MSP), Police MSP, from the drop-down list and click Next.
Give the TLS Enroll ID, admin, and TLS Enroll secret, adminpw, the same values are the Enroll ID and Enroll secret that you gave when creating the CA. Leave the TLS CSR hostname blank.
The last side panel will ask you to Associate an identity and make it the admin of your peer. Select your peer admin identity Police Admin.
Review the summary and click Submit.
# Step 5. Build a network - Create Orderer
Create your orderer organization CA
Click Add Certificate Authority.
Click IBM Cloud under Create Certificate Authority and Next.
Give it a unique Display name of Orderer CA.
Specify an Admin ID of admin and Admin Secret of adminpw.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/6a20db44-a6a7-467e-bc4c-438f08b6f121" />




Use your CA to register orderer and orderer admin identities (shown in gif above)
In the Nodes tab, select the Orderer CA Certificate Authority that we created.
First, we will register an admin for our organization. Click on the Register User button. Give an Enroll ID of ordereradmin, and Enroll Secret of ordereradminpw. Click Next. Set the Type for this identity as client and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
We will repeat the process to create an identity of the orderer. Click on the Register User button. Give an Enroll ID of orderer1, and Enroll Secret of orderer1pw. Click Next. Set the Type for this identity as peer and select org1 from the affiliated organizations drop-down list. We will leave the Maximum enrollments and Add Attributes fields blank.
Create the orderer organization MSP definition
Navigate to the Organizations tab in the left navigation and click Create MSP definition.
Enter the MSP Display name as Orderer MSP and an MSP ID of orderermsp.
Under Root Certificate Authority details, specify the peer CA that we created Orderer CA as the root CA for the organization.
Give the Enroll ID and Enroll secret for your organization admin, ordereradmin and ordereradminpw. Then, give the Identity name, Orderer Admin.
Click the Generate button to enroll this identity as the admin of your organization and export the identity to the wallet. Click Export to export the admin certificates to your file system. Finally click Create MSP definition.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/c047153f-dd4a-4fb0-a3f8-5d7c6e736132" />



Create an orderer
On the Nodes page, click Add orderer.
Click IBM Cloud and proceed with Next.
Give your peer a Display name of Orderer.
On the next screen, select Orderer CA as your Certificate Authority. Then, give the Enroll ID and Enroll secret for the peer identity that you created for your orderer, orderer1, and orderer1pw. Then, select the Administrator Certificate (from MSP), Orderer MSP, from the drop-down list and click Next.
Give the TLS Enroll ID, admin, and TLS Enroll secret, adminpw, the same values are the Enroll ID and Enroll secret that you gave when creating the CA. Leave the TLS CSR hostname blank.
The last side panel will ask to Associate an identity and make it the admin of your peer. Select your peer admin identity Orderer Admin.
Review the summary and click Submit.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/682c36d0-d407-4dff-807f-669cf0b00a2c" />



Add organizations as Consortium Member on the orderer to transact
Navigate to the Nodes tab, and click on the Orderer that we created.
Under Consortium Members, click Add organization.
From the drop-down list, select Insurance MSP.
Click Submit.
Repeat the same steps, but add Shop MSP, Repair Shop MSP, and Police MSP as well.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/50e4a740-57a1-4600-b0ec-1e853d8a81b3" />



# Step 6. Build a network - Create and Join Channel
Create the channel
Navigate to the Channels tab in the left navigation.
Click Create channel.
Give the channel a name, mychannel.
Select the orderer you created, Orderer from the orderers drop-down list.
Select the MSP identifying the organization of the channel creator from the drop-down list. This should be Insurance MSP (insurancemsp).
Associate available identity as Insurance Admin.
Click Add next to the insurance organization. Make the insurance organization an Operator.
Click Add next to the shop organization. Make the shop organization an Operator.
Click Add next to the repair shop organization. Make the repair shop organization an Operator.
Click Add next to the police organization. Make the insurance organpoliceization an Operator.
Click Create.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/6d3df36a-627b-4a67-ac9a-2b75ca70084e" />



Join your peer to the channel
Click Join channel to launch the side panels.
Select your Orderer and click Next.
Enter the name of the channel you just created. mychannel and click Next.
Select which peers you want to join the channel, click Insurance Peer, Shop Peer, Repair Shop Peer, and Police Peer.
Click Submit.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/075bcdef-cba6-42b7-8367-e06147c1f79b" />



Add anchor peers to the channel
In order to communicate between organizations, we need to enroll anchor peers.
From the channels tab, click on the channel you have created, mychannel.
From the channel overview page, click on channel details. Scroll all the way down until you see Anchor peers.
Click Add anchor peer and add the Insurance, Police, Repair Shop, and Shop peers.
Select which peers you want to join the channel, click Insurance Peer, Shop Peer, Repair Shop Peer, and Police Peer.
Click Add anchor peer.
If all went well, your channel Anchor peers should look like below:

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/778ecec8-d637-48a0-a810-783882668dff" />



# Step 7. Deploy Insurance Smart Contract on the network
Install a smart contract
Clone the repository:
git clone https://github.com/IBM/build-blockchain-insurance-app
Click the Smart contracts tab to install the smart contract.
Click Install smart contract to upload the insurance smart contract package file.
Click on Add file and find your packaged smart contract. It is the file in the build-blockchain-insurance-app/chaincodePackage directory.
Select all peers - we need to install the contract on each peer.
Once the contract is uploaded, click Install.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/376fc09d-e6c3-4801-81ab-ec8922f3a404" />



Instantiate smart contract
On the smart contracts tab, find the smart contract from the list installed on your peers and click Instantiate from the overflow menu on the right side of the row.
On the side panel that opens, select the channel, mychannel to instantiate the smart contract on. Click Next.
Select the organization members to be included in the policy, insurancemsp, shopmsp, repairshopmsp, policemsp. Click Next.
Give Function name of Init and leave Arguments blank.
Click Instantiate.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf1b2d05-4192-4c0d-b012-e5b7a5caf9b2" />



# Step 8. Connect application to the network
Connect with sdk through connection profile
Under the Instantiated Smart Contract, click on Connect with SDK from the overflow menu on the right side of the row.
Choose from the dropdown for MSP for connection, insurancemsp.
Choose from Certificate Authority dropdown, Insurance CA.
Download the connection profile by scrolling down and clicking Download Connection Profile. This will download the connection json which we will use soon to establish connection.
You can click Close once the download completes.

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/6617aedf-f5c5-44fe-b3ed-b7ea7d3cea64" />



Create insurance application admin
Go to the Nodes tab on the left bar, and under Certificate Authorities, choose your Insurance CA.
Click on Register user.
Give an Enroll ID and Enroll Secret to administer your application users, insuranceApp-admin and insuranceApp-adminpw.
Choose client as Type.
You can leave the Use root affiliation box checked.
You can leave the Maximum enrollments blank.
Under Attributes, click on Add attribute. Give attribute as hf.Registrar.Roles = *. This will allow this identity to act as registrar and issues identities for our app. Click Add-attribute.
Click Register.


<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/adb561f7-ab99-4646-818a-13e12582b971" />


Create shop application admin (same process as shown above in the gif)
Go to the Nodes tab on the left bar, and under Certificate Authorities, choose your Shop CA.
Click on Register user.
Give an Enroll ID and Enroll Secret to administer your application users, shopApp-admin and shopApp-adminpw.
Choose client as Type.
You can leave the Use root affiliation box checked.
You can leave the Maximum enrollments blank.
Under Attributes, click on Add attribute. Give attribute as hf.Registrar.Roles = *. This will allow this identity to act as registrar and issues identities for our app. Click Add-attribute.
Click Register.
Create repair shop application admin (same process as shown above in the gif)
Go to the Nodes tab on the left bar, and under Certificate Authorities, choose your Repair Shop CA.
Click on Register user.
Give an Enroll ID and Enroll Secret to administer your application users, repairShopApp-admin and repairShopApp-adminpw.
Choose client as Type.
You can leave the Use root affiliation box checked.
You can leave the Maximum enrollments blank.
Under Attributes, click on Add attribute. Give attribute as hf.Registrar.Roles = *. This will allow this identity to act as registrar and issues identities for our app. Click Add-attribute.
Click Register.
Create police application admin (same process as shown above in the gif)
Go to the Nodes tab on the left bar, and under Certificate Authorities, choose your Police CA.
Click on Register user.
Give an Enroll ID and Enroll Secret to administer your application users, policeApp-admin and policeApp-adminpw.
Choose client as Type.
You can leave the Use root affiliation box checked.
You can leave the Maximum enrollments blank.
Under Attributes, click on Add attribute. Give attribute as hf.Registrar.Roles = *. This will allow this identity to act as registrar and issues identities for our app. Click Add-attribute.
Click Register.
Update application connection
Copy the connection profile you downloaded into the web/www/blockchain directory.
Copy and paste everything in the connection profile, and overwrite the ibpConnection.json.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8ddcecc-7f7f-4270-bd25-6a9c38cd72b7" />


# Step 9. Enroll App Admin Identities
Enroll insurnaceApp-admin
First, navigate to the web/www/blockchain directory.

cd web/www/blockchain/
Open the config.json file, and update the caName with the URL of the insurance certificate authority from your ibpConnection.json file. Save the file.

Run the enrollAdmin.js script

node enrollAdmin.js
You should see the following in the terminal:


msg: Successfully enrolled admin user insuranceApp-admin and imported it into the wallet
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/ef912dbd-a4f3-482f-a17d-5682f8222880" />


Enroll shopApp-admin
First, change the appAdmin, appAdminSecret, and caName properties in your config.json file, so that it looks something like this (your caName should be different than mine):

{
    "connection_file": "ibpConnection.json",
    "appAdmin": "shopApp-admin",
    "appAdminSecret": "shopApp-adminpw",
    "orgMSPID": "shopmsp",
    "caName": "https://fa707c454921423c80ec3c3c38d7545c-caf2e287.horeainsurancetest.us-south.containers.appdomain.cloud:7054",
    "userName": "shopUser",
    "gatewayDiscovery": { "enabled": true, "asLocalhost": false }
}
To find the other CA urls, you will need to click on the Nodes tab in IBM Blockchain Platform, then on the Shop CA, and on the settings cog icon at the top of the page. That will take you to the certificate authority settings, as shown in the picture below, and you can copy that endpoint URL into your config.json caName field.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/fba078c1-3b71-4902-9799-d40bebbcdf9b" />




Run the enrollAdmin.js script

node enrollAdmin.js
You should see the following in the terminal:

msg: Successfully enrolled admin user shopApp-admin and imported it into the wallet
Enroll repairShopApp-admin (same process as shown in gif above)
First, change the appAdmin, appAdminSecret, and caName properties in your config.json file, so that it looks something like this (your caName should be different than mine):

{
    "connection_file": "ibpConnection.json",
    "appAdmin": "repairShopApp-admin",
    "appAdminSecret": "repairShopApp-adminpw",
    "orgMSPID": "repairshopmsp",
    "caName": "https://fa707c454921423c80ec3c3c38d7545c-caf2e287.horeainsurancetest.us-south.containers.appdomain.cloud:7054",
    "userName": "repairUser",
    "gatewayDiscovery": { "enabled": true, "asLocalhost": false }
}
Run the enrollAdmin.js script

node enrollAdmin.js
You should see the following in the terminal:

msg: Successfully enrolled admin user repairShopApp-admin and imported it into the wallet
Enroll policeApp-admin (same process as shown in gif above)
First, change the appAdmin, appAdminSecret, and caName properties in your config.json file, so that it looks something like this (your caName should be different than mine):

{
    "connection_file": "ibpConnection.json",
    "appAdmin": "policeApp-admin",
    "appAdminSecret": "policeApp-adminpw",
    "orgMSPID": "policemsp",
    "caName": "https://fa707c454921423c80ec3c3c38d7545c-caf2e287.horeainsurancetest.us-south.containers.appdomain.cloud:7054",
    "userName": "policeUser",
    "gatewayDiscovery": { "enabled": true, "asLocalhost": false }
}
Run the enrollAdmin.js script

node enrollAdmin.js
You should see the following in the terminal:

msg: Successfully enrolled admin user policeApp-admin and imported it into the wallet
# Step 10. Run the application
Navigate to the directory blockchain directory which contains the config.js file:

cd build-blockchain-insurance-app/web/www/blockchain/
In the editor of choice, change line 8 of the config.js file to isCloud: true as shown in the image below:
<img width="1522" height="750" alt="image" src="https://github.com/user-attachments/assets/ec9bfde6-ab96-4e80-ae54-7286ecc2eadb" />

Is Cloud

If you are using Mac, save the changes. Otherwise, if you are using an Ubuntu system, change line 9 of config.js file to isUbuntu: true as shown in the image below:
<img width="1384" height="770" alt="image" src="https://github.com/user-attachments/assets/79cd8d58-e851-4425-be27-4917197536f7" />

Is Ubuntu

Next, from the blockchain directory navigate to the root project directory:

blockchain$ cd ../../../
build-blockchain-insurance-app$   
Login using your docker hub credentials.

docker login
Run the build script to download and create docker images for the orderer, insurance-peer, police-peer, shop-peer, repairshop-peer, web application and certificate authorities for each peer. This will run for a few minutes.

For Mac user:

cd build-blockchain-insurance-app
./build_mac.sh
For Ubuntu user Make sure isUbuntu:true is saved in the line 9 of config.js:

cd build-blockchain-insurance-app
./build_ubuntu.sh
You should see the following output on console:

Creating repairshop-ca ...
Creating insurance-ca ...
Creating shop-ca ...
Creating police-ca ...
Creating orderer0 ...
Creating repairshop-ca
Creating insurance-ca
Creating police-ca
Creating shop-ca
Creating orderer0 ... done
Creating insurance-peer ...
Creating insurance-peer ... done
Creating shop-peer ...
Creating shop-peer ... done
Creating repairshop-peer ...
Creating repairshop-peer ... done
Creating web ...
Creating police-peer ...
Creating web
Creating police-peer ... done

<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/d5b6d96d-5d12-43be-80d9-73e0f6951081" />


Wait for few minutes for application to install and instantiate the chaincode on network

Check the status of installation using command:

docker logs web
On completion, you should see the following output on console:

> blockchain-for-insurance@2.1.0 serve /app
> cross-env NODE_ENV=production&&node ./bin/server

/app/app/static/js
Server running on port: 3000
Default channel not found, attempting creation...
Successfully created a new default channel.
Joining peers to the default channel.
Chaincode is not installed, attempting installation...
Base container image present.
info: [packager/Golang.js]: packaging GOLANG from bcins
info: [packager/Golang.js]: packaging GOLANG from bcins
info: [packager/Golang.js]: packaging GOLANG from bcins
info: [packager/Golang.js]: packaging GOLANG from bcins
Successfully installed chaincode on the default channel.
Successfully instantiated chaincode on all peers.
Use the link http://localhost:3000 to load the web application in browser.

The home page shows the participants (Peers) in the network. You can see that there is an Insurance, Repair Shop, Police and Shop Peer implemented. They are the participants of the network.
<img width="944" height="566" alt="image" src="https://github.com/user-attachments/assets/f85ee7da-4300-4875-8801-2fa541dbe55f" />

Blockchain Insurance

Imagine being a consumer (hereinafter called “Biker”) that wants to buy a phone, bike or Ski. By clicking on the “Go to the shop” section, you will be redirected to the shop (shop peer) that offers you the following products.
<img width="944" height="566" alt="image" src="https://github.com/user-attachments/assets/0146235d-8ec8-4291-a4b4-1c38d3d67731" />

Customer Shopping

You can see the three products offered by the shop(s) now. In addition, you have insurance contracts available for them. In our scenario, you are an outdoor sport enthusiast who wants to buy a new Bike. Therefore, you’ll click on the Bike Shop section.
<img width="944" height="604" alt="image" src="https://github.com/user-attachments/assets/e0687e52-74a7-4361-aacc-cc4b42de30f8" />

Shopping

In this section, you are viewing the different bikes available in the store. You can select within four different Bikes. By clicking on next you’ll be forwarded to the next page which will ask for the customer’s personal data.
<img width="944" height="566" alt="image" src="https://github.com/user-attachments/assets/585fcceb-e14b-4a07-a955-05e6848244fc" />

Bike Shop

You have the choice between different insurance contracts that feature different coverage as well as terms and conditions. You are required to type-in your personal data and select a start and end date of the contract. Since there is a trend of short-term or event-driven contracts in the insurance industry you have the chance to select the duration of the contract on a daily basis. The daily price of the insurance contract is being calculated by a formula that had been defined in the chaincode. By clicking on next you will be forwarded to a screen that summarizes your purchase and shows you the total sum.
<img width="944" height="566" alt="image" src="https://github.com/user-attachments/assets/2a92ae0b-70ed-46e7-8b5c-e0599ee5e628" />

Bike Insurance

The application will show you the total sum of your purchase. By clicking on “order” you agree to the terms and conditions and close the deal (signing of the contract). In addition, you’ll receive a unique username and password. The login credentials will be used once you file a claim. A block is being written to the Blockchain.

note You can see the block by clicking on the black arrow on the bottom-right.

At this point, you should be able to go into your IBM Blockchain Platform console, click on the channels, and then be able to see the contract_create block being added.
<img width="1728" height="1080" alt="image" src="https://github.com/user-attachments/assets/64b5ac33-6a6c-4b59-afe6-cd560705d96d" />




For additional steps on how to file more claims, and use the rest of the application, please go here.

Congratulations! You've successfully connection your React app to the IBM Blockchain Platform! Now each time you submit transactions with the UI, they will be logged by the blockchain service.

# Additional resources
Following is a list of additional blockchain resources:

Fundamentals of IBM Blockchain
Hyperledger Fabric Documentation
Hyperledger Fabric code on GitHub
# Troubleshooting
Run clean.sh to remove the docker images and containers for the insurance network.
`./clean.sh`
License
This code pattern is licensed under the Apache Software License, Version 2. Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the Developer Certificate of Origin, Version 1.1 (DCO) and the Apache Software License, Version 2.

Apache Software License (ASL) FAQ

# Chaincode implementation for managing the drafting of roaming agreements

The following is **Part-4** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement's** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

**Part 3** of the series analyzed the design of an NLP-based engine to determine whether the articles and sub-articles of a **Roaming Agreement** constitute a *standard clause*, a *variation* or a *customized text* regarding the GSMA standard templates. In addition, if the sub-article is tagged as a *variation*, the NLP engine collects the existing *variables*. Although **Part 2** is key to the development of the *project*, the main element of the *project* is constituted by the smart contract, i.e. the *chaincode* for *Hyperledger Fabric Blockchain*, which will manage on-chain all the interactions representing the businesses processes related to the drafting and negotiation of the **Roaming Agreement**.

For a better understanding, the analysis of the chaincode will be divided into two parts. This part (**Part-3**) focuses on the details of the chaincode design, while **Part-4** of this series will focus on the *implementation*, *deployment* and *testing* of the chaincode.

From a design point of view, the project chaincode is defined by actions and statuses, where interactions through actions allow the transition between each of the statuses. The statuses represnt the different phases and transition starting with the enabling of a **Mobile Network Operator (MNO)** until it reaches the final signed **Roaming Agreement** with another counter party, **MNO**. Therefore, it is necessary to put the focus on the statuses that govern the chaincode. Thus, the chaincode for **Roaming Agreement** negotiation consists of three stages: (1) **Statuses for Roaming Agreement Negotiation**, (2) **Statuses for the Articles Negotiation** and (3) **Statuses for the Article Drafting**.

The **Statuses for Roaming Agreement Negotiation** allows the negotiation to be conducted at the highest level, to determine when the final version **Roaming Agreement** is reached. Without being too rigorous with each of the transitions that take place, the following figure shows each of the states that make up this stage. Thus, in the initial status, the MNOs must be enrolled, which enables either of the two MNOs participating in the negotiation to propose to initiate the **Roaming Agreement** drafting process. Once the initiation of the agreement is confirmed, the negotiation of the drafting of articles and sub-articles proceeds, where the other stages are involved, ending with a proposal and confirmation of acceptance of the **Roaming Agreement**.

<img src="https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/documentation/images/Roaming_Agreement_State_v03.drawio.png">

The negotiation at the articles level is an intermediate stage to determine whether all the articles belonging to the **Roaming Agreement** have reached the status of being accepted by both parties. Thus, as shown in the figure below, belonging to the **Statuses for the Article Drafting** stage, once an article is added, it goes through a transition of proposed changes until finally the article is accepted by both parties.

<img src="https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/documentation/images/Article_Drafting_State_v03.drawio.png">

However, the fact that all articles are in accepted status by both MNOs at a given time, does not necessarily imply that the **Roaming Agreement** is close to being reached, but both entities may intend to add a new article despite that all currently added articles are in an accepted status. For this purpose, the **Status for the Articles Negotiation** exits. As shown in the following figure, the function of this stage is to control the status of all articles added. For this, it establishes a transient status that determines that all added articles are accepted. On the one hand, this stage allows to continue the article drafting process if a new article is proposed. On the other hand, this stage allows to move towards the achievement of the final version of the **Roaming Agreement**.

<img src="https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/documentation/images/Article_Negotiation_State_v03.drawio.png">

As mentioned, the transition between states is due to interaction between the two parties through the identified actions. From a programming point of view, the actions are performed through chaincode methods, which are invoked or queried using the frontend application through a middleware/backend service. The following Table defines the set of design methods that are part of the chaincode.

## Getting Started to deploy the chaincode
1. Download Golang Version: `wget https://golang.org/dl/go1.16.7.linux-amd64.tar.gz`.
2. To verify the tarball checksum it can be used the sha256sum command: `sha256sum go1.16.7.linux-amd64.tar.gz`.
3. Copy Golang bynary into executable folder: `sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.16.7.linux-amd64.tar.gz`.

## Golang's challenges

1. Working with pointers on nested structures [example](https://play.golang.org/p/UoeBH_2EZdb).
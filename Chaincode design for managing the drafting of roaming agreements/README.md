# Chaincode design for managing the drafting of roaming agreements

The following is **Part-3** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

**Part 2** of the series partially analyzed the design of an NLP-based engine to determine whether the articles and sub-articles of a **Roaming Agreement** constitute a *standard clause*, a *variation* or a *customized text* regarding the GSMA standard templates. In addition, if the sub-article is tagged as a *variation*, the NLP engine collects the existing *variables*. Although **Part 2** is key to the development of the *project*, the main element of the *project* is constituted by the smart contract, i.e. the *chaincode* for *Hyperledger Fabric Blockchain*, which will manage on-chain all the requests coming from the off-chain side as part of the process of drafting the **Roaming Agreement**.

For a better understanding, the analysis of the chaincode will be divided into two parts. On the one hand, this part (**Part-3**) focuses on the details of the chaincode design. On the other hand, **Part-4** of this series will focus on the *implementation*, *deployment* and *testing* of the chaincode.

From a design point of view, the project chaincode is defined by actions and statuses, where interactions through actions allow the transition between each of the statuses. The statuses allow establishing a transition from the enabling of a **Mobile Network Operator (MNO)** until it reaches the **Roaming Agreement** with another enabled **MNO**. Therefore, it is necessary to put the focus on the statuses that govern the chaincode. Thus, the chaincode for **Roaming Agreement** Negotiation consists of three stages: (1) **Statuses for Roaming Agreement Negotiation**, (2) **Statuses for the Articles Negotiation** and (3) **Statuses for the Article Drafting**.

The **Statuses for Roaming Agreement Negotiation** allows the negotiation to be conducted at the highest level, to determine when the **Roaming Agreement** will be reached. Without being too rigorous with each of the transitions that take place, the following figure shows each of the states that make up this stage. Thus, in the initial status, the MNOs must be enrolled, which enables either of the two MNOs participating in the negotiation to propose to initiate the **Roaming Agreement** drafting process. Once the initiation of the negotiation is confirmed, the negotiation of the drafting of articles and sub-articles proceeds, where the other stages are involved, ending with a proposal and confirmation of acceptance of the **Roaming Agreement**.

<img src="https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/documentation/images/Roaming_Agreement_State_v03.drawio.png">

The negotiation at the articles level is an intermediate stage to determine whether all the articles belonging to the **Roaming Agreement** have reached the status of being accepted by both parties. Thus, as shown in the figure below, belonging to the **Status for the Article Drafting** stage, once an article is added, it goes through a transition of proposed changes until finally the article drafting is accepted.

<img src="https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/documentation/images/Article_Drafting_State_v03.drawio.png">


However, the fact that all articles are in accepted status by both MNOs at a given time, does not necessarily imply that the **Roaming Agreement** is close to being reached, but both entities may intend to add a new article despite that all currently added articles are in an accepted status. For this purpose, the **Status for the Articles Negotiation** exits. As shown in the following figure, the function of this stage is to control the status of all articles added. For this, it establishes a transient status that determines that all added articles are accepted. On the one hand, this stage allows to continuing the article drafting process if a new article is proposed and on the other hand, this stage allows to move towards the achievement of the **Roaming Agreement**.

<img src="https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/documentation/images/Article_Negotiation_State_v03.drawio.png">

As mentioned, the transition between states is due to interaction through actions. From a programming point of view, the actions are performed through chaincode methods, which are invoked or queried from the off-chain side. The following Table defines the set of design methods that are part of the chaincode.

|Method                     |Description           |
|:-------------------------:|----------------------|
|addOrg                     |This method is part of the initial status and allows you to register any MNO that is part of the Hyperledger Fabric Blockchain network before drafting the **Roaming Agreement** with another MNO.|
|proposeAgreementInitiation |The proposal to initiate the drafting of the **Roaming Agreement** is carried out by one of the two participating MNOs through this method. |
|acceptAgreementInitiation  |As mentioned, the proposed wording of the iteration agreement must be confirmed by the MNO that did not propose the **Roaming Agreement** initiation, for which the following method is used. |
|proposeAddArticle          |The drafting of the **Roaming Agreement** involves the proposed addition of the articles. |
|proposeUpdateArticle       |The proposal for added articles can be modified using this method. |
|acceptProposedChanges      |The changes made to an article added or a proposed modification must be confirmed before the article being considered as accepted. |
|proposeReachAgreement      |Once all drafted articles have been accepted any of the participating MNOs can apply to reach the **Roaming Agreement** using this method. |
|acceptReachAgreement       |To reach the **Roaming Agreement** the proposed agreement must be accepted from this method.|
|queryMNO |This method allows retrieving the information associated with an MNO. |
|queryRAID |This method allows retrieving information of the **Roaming Agreement**. |
|querySingleArticle         |This method allows querying a single article. |
|queryAllArticles           |This method allows querying all articles added to the negotiation process. |

To ensure traceability of all interactions that happen from the off-chain side with the chaincode, most methods emit events that provide traceability-relevant information such as the organization(s) involved, the timestamp at which it was emitted, and the name of the event. Next, the following Table relates Methods and the Events emitted by them. It should be noted that only the methods that invoke transactions are those that emit events.

|Method                     |Event name              |
|:-------------------------:|:----------------------:|
|addOrg                     |created_org             |
|proposeAgreementInitiation |started_ra              |
|acceptAgreementInitiation  |confirmation_ra_started |
|proposeAddArticle          |proposed_add_article    |
|proposeUpdateArticle       |proposed_update_article |
|acceptProposedChanges      |accept_proposed_changes |
|proposeReachAgreement      |proposal_accepted_ra    |
|acceptReachAgreement       |confirmation_accepted_ra|

To complete the main element of the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), **part 4** of this series of 6 will analyze the *implementation*, *deployment* and *testing* approaches of the Chaincode. However, it is clear that there is relevant information located off-chain, hence **part 5** of the series details the implementation of the *backend* to manage the off-chain part and finally **part 6**  of the series details the *frontend* implementation criteria to perform the interactions from a user-friendly environment.
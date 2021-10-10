# Chaincode design for managing the drafting of roaming agreements

The following is **Part-3** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

**Part 2** of the series partially analyzed the design of an NLP-based engine to determine whether the articles and sub-articles of a **Roaming Agreement** constitute a *standard clause*, a *variation* or a *customized text*. If it is a *variation*, the engine allows to analyze the *variables* that are part of that *variation* regarding the GSMA standard templates. Although **Part 2** is key to the development of the *project*, the main element of the *project* is constituted by the smart contract, i.e. the *chaincode* for *Hyperledger Fabric Blockchain*, which will manage on-chain all the requests coming from the off-chain side as part of the process of drafting the **Roaming Agreement**.

For a better understanding, the analysis of the chaincode will be divided into two parts. On the one hand, this part (**Part-3**) focuses on the details of the chaincode design. On the other hand, **Part-4** of this series will focus on the *implementation* and *deployment* of the chaincode.

From a design point of view the project chaincode is defined by methods and statuses, where the interactions on the methods allow the transition between each of the statuses. The chaincode for Roaming Agreement Negotiation consists of three states: (1) **Status for Roaming Agreement Negotiation**, (2) **Status for the Articles Negotiation** and (3) **Status for the Article Drafting**.

The **Status for Roaming Agreement Negotiation** allows the negotiation to be conducted at the highest level, to determine when the **Roaming Agreement** will be reached. The negotiation at the articles level is an intermediate stage to determine whether all the articles belonging to the **Roaming Agreement** have reached the status of accepted by both parties. However, the fact that all articles at a given time are in the accepted status by both Mobile Network Operators (MNOs) does not necessarily imply that the agreement is close to being reached, but it is possible that both entities intend to add a new article. For this purpose, the **Status for the Articles Negotiation** exits. Finally, at a lower level is the **Status for the Article Drafting**, whose purpose is to manage the stages through which the *drafting* of the article may pass until it is finally accepted by both parties.

As mentioned, the transition between status is due to interaction using methods. The following Table defines the set of designed methods that are part of the chaincode.

|Method                     |Description           |
|:-------------------------:|----------------------|
|addOrg                     |This mechanism allows any MNO that is part of the Hyperledger Fabric Blockchain network to be registered prior to negotiation for the drafting of a Roaming Agreement with another MNO.|
|proposeAgreementInitiation |A registered organization is enabled to draft a Roaming Agreement. |
|acceptAgreementInitiation  |For the roaming agreement drafting to be valid,it must be confirmed by both MNOs. |
|proposeAddArticle          |The drafting of the Roaming Agreement involves adding article by article. This method proposes adding a new article. |
|proposeUpdateArticle       |The drafting of the Roaming Agreement involves updating existing articles. This method allow to update a previously proposed article. |
|proposeDeleteArticle       |The drafting of the Roaming Agreement involves deletion existing articles.  This method allow to delete a previously proposed article. |
|acceptProposedChanges      |This method is used to accept a proposed change, which may be to add, update or delete a previously proposed article. |
|proposeReachAgreement      |Once all drafted articles have been accepted any of the participating MNOs can apply to reach agreement using this method. |
|acceptReachAgreement       |The changes proposed in Proposal of Agreement Achieved can be accepted through this method.|
|querySingleArticle         |This merhod allow to query a single article. |
|queryAllArticles           |This merhod allow to query all articles added to the negotiation process. |

Although all methods return at least one error as part of error handling, to ensure traceability of all interactions that happen from the off-chain side with the smart contract, most methods emit events that provide traceability-relevant information such as the organization(s) involved, the timestamp at which it was emitted, and the name of the event. Next, the following Table relates Methods and Events to emit.

|Method                     |Event                   |
|:-------------------------:|:----------------------:|
|addOrg                     |created_org             |
|proposeAgreementInitiation |started_ra              |
|acceptAgreementInitiation  |confirmation_ra_started |
|proposeAddArticle          |proposed_add_article    |
|proposeUpdateArticle       |proposed_update_article |
|proposeDeleteArticle       |proposed_delete_article |
|acceptProposedChanges      |accept_proposed_changes |
|proposeReachAgreement      |proposal_accepted_ra    |
|acceptReachAgreement       |confirmation_accepted_ra|
|querySingleArticle         |-                       |
|queryAllArticles           |-                       |

**Part 4** of this series of 6 will analyze the *Chaincode implementation and deployment criteria*, to complete the main element of the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements) and focus on the last two complementary parts, i.e. the *backend* to manage the off-chain part and the *frontend* to carry out interactions from a user friendly environment.
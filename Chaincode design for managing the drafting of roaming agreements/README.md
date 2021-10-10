# Chaincode design for managing the drafting of roaming agreements

The following is **Part-3** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

**Part 2** of the series partially analyzed the design of an NLP-based engine to determine whether the articles and sub-articles of a **Roaming Agreement** constitute a *standard clause*, a *variation* or a *customized text*. If it is a *variation*, the engine allows to analyze the *variables* that are part of that *variation* regarding the GSMA standard templates. Although **Part 2** is key to the development of the *project*, the main element of the *project* is constituted by the smart contract, i.e. the *chaincode* for *Hyperledger Fabric Blockchain*, which will manage on-chain all the requests coming from the off-chain side as part of the process of drafting the **Roaming Agreement**.

For a better understanding, the analysis of the chaincode will be divided into two parts. On the one hand, this part (**Part-3**) focuses on the details of the chaincode design. On the other hand, **Part-4** of this series will focus on the *implementation* and *deployment* of the chaincode.

From a design point of view the project chaincode is defined by methods and statuses, where the interactions on the methods allow the transition between each of the statuses. The chaincode for Roaming Agreement Negotiation consists of three states: (1) **Status for Roaming Agreement Negotiation**, (2) **Status for the Articles Negotiation** and (3) **Status for the Article Drafting**.

The **Status for Roaming Agreement Negotiation** allows the negotiation to be conducted at the highest level, to determine when the roaming agreement will be reached. The **Status for the Articles Negotiation** . The **Status for the Article Drafting** .


La integración entre los estados ...

La siguiente Tabla establece el conjunto de métodos diseñados que forman parte del chaincode. Además de la breve descripción de cada uno, la columna de la derecha enlaza con el diagrama de secuencias y diagrama de clases de cada uno de estos métodos.

|Method                     |Description           |
|:-------------------------:|----------------------|
|addOrg                     |This mechanism allows any MNO that is part of the Hyperledger Fabric Blockchain network to be registered prior to negotiation for the drafting of a Roaming Agreement with another MNO.|
|proposeAgreementInitiation |A registered organization is enabled to draft a Roaming Agreement. |
|acceptAgreementInitiation  |For the roaming agreement drafting to be valid, the other MNO must confirm it. |
|proposeAddArticle          |The drafting of the Roaming Agreement involves to add article by article. |
|proposeUpdateArticle       |The drafting of the Roaming Agreement involves to update articles. |
|proposeDeleteArticle       |The drafting of the Roaming Agreement involves to delete articles. |
|acceptProposedChanges      |The changes proposed in Proposal for add article, Proposal for update article and Proposal for delete article must be accepted or refused. |
|proposeReachAgreement      |The changes proposed in Proposal for add article, Proposal for update article and Proposal for delete article must be accepted or refused. |
|acceptReachAgreement       |The changes proposed in Proposal of Agreement Achieved must be accepted or refused.|
|querySingleArticle         |Query a single article. |
|queryAllArticles           |Query all articles added to the negotiation process. |

A continuación la forma en que cada uno de los métodos actúa sobre los estados del acuerdo de itinerancia.

Aunque todos los métodos retornan al menos un error como parte de la gestión de errores, para garantizar la trazabilidad de todas las interacciones que suceden desde la parte off-chain con el contrato inteligente, la mayoría de los métodos emiten eventos que proporcionan información relavante para la trazabilidad como pueden ser la o las organizaciones que intervienen, la estampa de tiempo en el que fue emitido y el nombre del evento.

Finally, the following table relates Methods and Events to emit.

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

La parte número 4 de esta serie de 6 analizará los criterios de implementación y despliegue del Chaincode, para de esta forma completar el elemento principal del proyecto y enfocarnos las dos últimas partes complementarias, es decir el backend para gestionar la parte off-chain y el frontend para llevar a cabo interacciones desde un entornos user friendly.
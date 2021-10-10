# Chaincode design for managing the drafting of roaming agreements

The following is **Part-3** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

En la parte 2 de la serie se analizó parcialmente el diseño de un motor basado en NLP para determinar si los artículos y sub-artículos de un acuerdo de itinerancia constituyen una cláusula estándar, una variación o un texto personalizado. En caso de ser una variación, el motor permite analizar las variables que forman parte de esa variación. Aunque esta parte es clave para el desarrollo del proyecto, el elmento principal de este está constituido por el contrato inteligente, es decir, el chaincode para Hyperledger Fabric Blockchain, el cual gestionará on-chain todas las peticiones que vienen del lado off-chain como parte del proceso de redacción del acuerdo de itinerancia.

Para un mejor entendimiento el análisis relativo al chaincode será dividido en dos partes. Por un lado, esta parte se enfoca en los detalles relativos al diseño del mismo. Por otro lado, la parte número 4 de esta serie se enfoca en la implementación y el despliegue del mismo.

Desde un punto de vista de diseño el chaincode del proyecto está definido por métodos y estados, donde las interacciones sobre los métodos permiten la transición entre cada uno de los estados. El chaincode para la redacción de acuerdos de itinerancia está constituido por tres estados: (1) Status for Roaming Agreement Negotiation, (2) Status for the Articles Negotiation y (3) Status for the Article Drafting.

El Status for Roaming Agreement Negotiation permite llevar a cabo la negociación al más alto nivel, para determinar cuando se alcanzará el acuerdo de itinerancia.

La integración entre los estados ...

La siguiente Tabla establece el conjunto de métodos diseñados que forman parte del chaincode. Además de la breve descripción de cada uno, la columna de la derecha enlaza con el diagrama de secuencias y diagrama de clases de cada uno de estos métodos.

|Method                     |Description           |Method Diagrams             |
|:-------------------------:|----------------------|:--------------------------:|
|addOrg                     |This mechanism allows any MNO that is part of the Hyperledger Fabric Blockchain network to be registered prior to negotiation for the drafting of a Roaming Agreement with another MNO.| [Sequence](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/documentation/readme/chaincode-design.md#part-of-chaincode-sequence-diagram), [Class](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/documentation/readme/chaincode-design.md#part-of-chaincode-class-diagram) |
|proposeAgreementInitiation |A registered organization is enabled to draft a Roaming Agreement. | |
|acceptAgreementInitiation  |For the roaming agreement drafting to be valid, the other MNO must confirm it. | |
|proposeAddArticle          |The drafting of the Roaming Agreement involves to add article by article.    | |
|proposeUpdateArticle       |The drafting of the Roaming Agreement involves to update articles. | |
|proposeDeleteArticle       |The drafting of the Roaming Agreement involves to delete articles. | |
|acceptProposedChanges      |The changes proposed in Proposal for add article, Proposal for update article and Proposal for delete article must be accepted or refused. | |
|proposeReachAgreement      |The changes proposed in Proposal for add article, Proposal for update article and Proposal for delete article must be accepted or refused. | |
|acceptReachAgreement       |The changes proposed in Proposal of Agreement Achieved must be accepted or refused.| |
|querySingleArticle         |Query a single article. | |
|queryAllArticles           |Query all articles added to the negotiation process. | |

A continuación la forma en que cada uno de los métodos actúa sobre los estados del acuerdo de itinerancia.

Aunque todos los métodos retornan al menos un error como parte de la gestión de errores, para garantizar la trazabilidad de todas las interacciones que suceden desde la parte off-chain con el contrato inteligente, la mayoría de los métodos emiten eventos que proporcionan información relavante para la trazabilidad como pueden ser la o las organizaciones que intervienen, la estampa de tiempo en el que fue emitido y el nombre del evento.

Finally, the following table relates Methods, Events to emit and the three types of states: Status for Roaming Agreement Negotiation, Status for the Articles Negotiation and Status for the Article Drafting

|Method                     |Event                   |Status for Roaming Agreement|Status for Articles Negotiation|Status for Article Drafting   |
|:-------------------------:|:----------------------:|:--------------------------:|:-----------------------------:|:----------------------------:|
|addOrg                     |created_org             |-                           |-                              |-                             |
|proposeAgreementInitiation |started_ra              |started_ra                  |Init                           |-                             |
|acceptAgreementInitiation  |confirmation_ra_started |started_ra_confirmation     |Init                           |-                             |
|proposeAddArticle          |proposed_add_article    |ra_negotiating              |articles_drating               |added_article                 |
|proposeUpdateArticle       |proposed_update_article |ra_negotiating              |articles_drating               |proposed_changes              |
|proposeDeleteArticle       |proposed_delete_article |ra_negotiating              |articles_drating               |proposed_changes              |
|acceptProposedChanges      |accept_proposed_changes |ra_negotiating              |transient_confirmation         |accepted_changes              |
|proposeReachAgreement      |proposal_accepted_ra    |accepted_ra                 |end                            |-                             |
|acceptReachAgreement       |confirmation_accepted_ra|acepted_ra_confirmation     |end                            |-                             |
|querySingleArticle         |-                       |-                           |-                              |-                             |
|queryAllArticles           |-                       |-                           |-                              |-                             |

La parte número 4 de esta serie de 6 analizará los criterios de implementación y despliegue del Chaincode, para de esta forma completar el elemento principal del proyecto y enfocarnos las dos últimas partes complementarias, es decir el backend para gestionar la parte off-chain y el frontend para llevar a cabo interacciones desde un entornos user friendly.
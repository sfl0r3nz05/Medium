# Chaincode implementation for managing the drafting of roaming agreements
The following is **Part-4** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement's** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. Other authors who contributed to this series of articles are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

[**Part-3**](https://medium.com/@sfl0r3nz05/chaincode-design-for-managing-the-drafting-of-roaming-agreements-73d3ed1b3645) of the series analyzed the design of the Hyperledger Fabric Blockchain (HFB) chaincode as the main element of the project to manage on-chain all the interactions to represent the businesses processes related to the drafting and negotiation of the **Roaming Agreement**. In the proposed design, the interactions through actions led to the transition between different states providing all the necessary traceability from the time a Mobile Network Operator (MNO) registers on the HFB platform until an agreement is reached between two MNOs. This **Part-4** continues with the chaincode analysis, however, we focus in this article on the implementation side of the project.

## The chaincode modules
The chaincode implementation consists of 6 modules which are described below:

1. [Proxy](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/proxy.go): This module receives the interactions from the off-chain side and routes them to the different points within the chaincode.
2. [Organization](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/organization.go): This module contains all the interactions related to organizations, allowing to create a new organization, querying existing organizations, etc.
3. [Agreement](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/agreement.go): This module contains all interactions related to the roaming agreement, allowing to add and update articles by specifying the articles variables values, variations selection and any proposed custom text. Also, this module handles article state transactions (e.g., proposed-to-accepted) and the possible proposed changes.
4. [Identity](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/proxy.go): This module is inserted inside the proxy and allows identity verification using the [Client Identity Chaincode Library (cid)](https://pkg.go.dev/github.com/hyperledger/fabric-chaincode-go/pkg/cid?utm_source=godoc).
5. [Util](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/util.go): This module contains common functionalities for the rest of the modules, e.g., UUID generation.
6. [Models](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/models.go): This module contains the definitions of variables, structures and data types supported by the chaincode. In addition, different error types are defined for proper error handling.

Other relevant features defined in the chaincode implementation are:
- [Logrus library](https://github.com/sirupsen/logrus) used for log generation. Relevant information such as the channel, the method, the error definition as well as the error message obtained from the chaincode itself is included in each of the logs generated. **Figure 1** is an example of a log defined for the *verifyOrg* method:

    ```
    log.Errorf("[%s][%s][verifyOrg] Error recovering: %v", CHANNEL_ENV, ERRORRecoveringOrg, err.Error())
    ```
- Definition of error handling through a set of variables included in the model. **Figure 2** shows an example of this definition:
    ```
    ERRORWrongNumberArgs                = `Wrong number of arguments. Expecting a JSON with token information.`
    ERRORParsingData                    = `Error parsing data `
    ERRORPutState                       = `Failed to store data into the ledger.  `
    ```

## Modules integration
The integration between the different modules takes place in each of the methods defined for the HFB chaincode. Considering that in [**Part-3**](https://medium.com/@sfl0r3nz05/chaincode-design-for-managing-the-drafting-of-roaming-agreements-73d3ed1b3645) each of the chaincode methods were defined, we will now focus on a single method to analyze how the integration between modules takes place. The selected method is `proposeAgreementInitiation`, which has been defined in [**Part-3**](https://medium.com/@sfl0r3nz05/chaincode-design-for-managing-the-drafting-of-roaming-agreements-73d3ed1b3645) as **the proposal to initiate the Roaming Agreement** drafting by one of the two participating MNOs, causing the transition from the initial state to the `started_ra` as shown in **Figure 3**:

<img src="https://github.com/sfl0r3nz05/Medium/blob/main/Chaincode%20implementation%20for%20managing%20the%20drafting%20of%20roaming%20agreements/images/Roaming_Agreement_State_v03.drawio.png">

**Figure 4** below shows the sequence diagram illustrating the relationship between the modules defined above. 

<img src="https://github.com/sfl0r3nz05/Medium/blob/main/Chaincode%20implementation%20for%20managing%20the%20drafting%20of%20roaming%20agreements/images/diagram_sequence_chaincode_v17.drawio.png">

In this way, a registered MNO enables the drafting of a **Roaming Agreement**. Thus, the *Proxy Module* enables the interactions with other modules. Firstly, the *Identity Module* allows to verify the MNO Identity. The *Organization Module* verifies whether the MNO exists, i.e. has been previously registered. Considering that the names of the two participating organizations and the Roaming Agreement name constitute the input arguments, the *Agreements Module* performs the following functionalities:

1. Generation of the unique identifier for the list of Articles: `articlesId`
2. Generation of the unique identifier for the roaming agreement: `RAID`.
3. The `started_ra` event is emitted.
4. The Status for the Roaming Agreement Negotiation is set as `started_ra` and contained as part of the emitted event.
5. The Status for the Articles Negotiation is set as `init`.

The main conclusion we can reach is that each method involves the verification, update or generation of states, which are stored as parts of the data structures established for the chaincodes and traced thanks to the events emitted from each of the methods.

Table 1 summarizes the details of the implementation of the `proposeAgreementInitiation` method:
|           Method           |   Event    | Status for Roaming Agreement | Status for Articles Negotiation | Status for Article Drafting |
| :------------------------: | :--------: | :--------------------------: | :-----------------------------: | :-------------------------: |
| proposeAgreementInitiation | started_ra |          started_ra          |              Init               |              -              |

## How methods drive state change?
Considering that the execution of each of these methods allows to verify, update or generate states, Table 2 summarizes the types of states defined in Part-3, associated to the application level. Thus, the next figure allows relating methods and states according to the different application levels.

|                  Status                  |    Application level    |
| :--------------------------------------: | :---------------------: |
| Status for Roaming Agreement Negotiation | Roaming Agreement Level |
|   Status for the Articles Negotiation    |     Articles Level      |
|     Status for the Article Drafting      |      Article Level      |

<img src="https://github.com/sfl0r3nz05/Medium/blob/main/Chaincode%20implementation%20for%20managing%20the%20drafting%20of%20roaming%20agreements/images/Status_Integration.png">

## Getting Started to deploy the chaincode
Having explained the main implementation aspects, and to offer an easy guide for getting started we thought it is necessary to provide a step-by-step guide on how to deploy the chaincode and test it. Those steps are listed hereafter:

1. Download Golang Version: 
    ```
    wget https://golang.org/dl/go1.16.7.linux-amd64.tar.gz`
    ```
2. To verify the tarball checksum it can be used the sha256sum command: 
    ```
    sha256sum go1.16.7.linux-amd64.tar.gz`
    ```
3. Copy Golang bynary into executable folder: 
    ```
    sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.16.7.linux-amd64.tar.gz`
    ```
4. Define the GOPATH environmental variable:
    ```
    export GOPATH=$HOME/go
    export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
    ```
5. Edit the profile file: Edit the `profile` file
    ```
    sudo nano $HOME/.profile
    ```
6. Add next line into `profile` file
    ```
    export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
    ```
8. Enabling changes in the `profile` file
    ```
    source ~/.profile
    ```
9. Verify Golang version
    ```
    go version
    ```
10. By default the workspace directory is set to $HOME/go
    ```
    mkdir ~/go
    ```
11. Inside the workspace create a new directory
    ```
    mkdir -p $GOPATH/src/github.com/nlp-dlt/network/chaincode/testcc
    ```
    ```
12. Enable go mod
    ```
    go mod init $GOPATH/src/github.com/nlp-dlt/network/chaincode/testcc
    ```
13. Install dependencies
    ```
    go get github.com/google/uuid
    go get github.com/sirupsen/logrus
    go get github.com/hyperledger/fabric-protos-go/peer
    go get github.com/hyperledger/fabric-chaincode-go/shim
    go get github.com/hyperledger/fabric-chaincode-go/pkg/cid
    ```
14. Build the changes
    ```
    go build
    ```
15. Building a vendor is necessary to import all the external dependencies needed for the basic functionality of the chaincode into a local vendor directory
    If the chaincode does not run because of the vendor, it can be built from scratch:
    ```
    cd   $GOPATH/src/github.com/nlp-dlt/network/chaincode/testcc
    dep  init
    ```
16. Also if it already exists, the missing packages can be imported using the update option:
    ```
    cd   $GOPATH/src/github.com/nlp-dlt/network/chaincode/testcc
    dep  ensure -v
    ```
    
## Challenges encountered during Implementation: 
Finally, in this topic we consider ceratins points of interest throughout the chaincode implementation process that represented challenges to us as designers and developers:

1. Working with pointers on nested structures: Our chaincode defines numerous nested structures, which must be initialized and updated constantly throughout the chaincode's life cycle. To avoid receiving a copy of a value receiver that does not update the structure, a pointer to the memory that contains the structure must be used. For a better understanding of how to work with nested structures using pointers we have included the following [example](https://play.golang.org/p/UoeBH_2EZdb) on a golang test runtime.
2. Generating unique identifiers: The chaincode lifecycle contains two main unique identifiers: Roaming Agreement Idenfier (RAID) and Articles Identifier (articlesID). To ensure that the identifiers are unique and avoid collisions, the resource used has been to generate a uuid and then compute the *SHA-256* from it, which will constitute the identifier. The functionalities used for this purpose are implemented within the Util Module.

## Interest links:
- The designs can be modified using the [App Diagrams Tool](https://app.diagrams.net/). 
- Drawio files can be found at:
    - [Chaincode Sequence Diagram](https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/chaincode/design/diagram_sequence_chaincode_v17.drawio)
    - [Chaincode Class Diagram](https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/chaincode/design/class_diagram_chaincode_v17.drawio)
    - [Status Diagram for Roaming Agreement Negotiation](https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/chaincode/design/Roaming_Agreement_State_v03.drawio)
    - [Status Diagram for Article Negotiation](https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/chaincode/design/Article_Negotiation_State_v03.drawio)
    - [Status Diagram for Article Drafting](https://github.com/sfl0r3nz05/nlp-dlt/blob/sentencelvl/chaincode/design/Article_Drafting_State_v03.drawio)

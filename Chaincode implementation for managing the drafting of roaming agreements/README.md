# Chaincode implementation for managing the drafting of roaming agreements

The following is **Part-4** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement's** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif.

**Part 3** of the series analyzed the design of the Hyperledger Fabric Blockchain (HFB) chaincode as the main element of the project to manage on-chain all the interactions to represent the businesses processes related to the drafting and negotiation of the **Roaming Agreement**. In this way, the interactions through actions led to the transition between different states providing all the necessary traceability from the time an Mobile Network Operator (MNO) registers on the HFB platform until an agreement is reached between two MNOs. This **Part-4** continues with the chaincode analysis, however, now from an implementation perspective.

## The chaincode modules

The chaincode implementation consists of 6 modules which are described below:

1. [Proxy](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/proxy.go): This module receives the interactions from the off-chain side and routes them to the different points within the chaincode.
2. [Organization](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/organization.go): This module contains all the interactions related to organizations, allowing to create a new organization, querying existing organizations, etc.
3. [Agreement](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/agreement.go): This module contains all interactions related to the roaming agreement, allowing to add and update articles, change states, etc.
4. [Identity](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/proxy.go): This module is inserted inside the proxy and allows identity verification using the cid library.
5. [Util](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/util.go): This module contains common functionalities for the rest of the modules. E.g., UUID generation.
6. [Models](https://github.com/sfl0r3nz05/NLP-DLT/blob/sentencelvl/chaincode/implementation/models.go): This module contains the definitions of variables, structures and data types supported by the chaincode. In addition, different error types are defined for proper error handling.

Other relevant features defined chaincode implementation are:
- [Logrus library](https://github.com/sirupsen/logrus) used for log generation. Relevant information such as the channel, the method, the error definition as well as the error message obtained from the chaincode itself is included in each of the logs emitted. The following is an example of a log defined for the *verifyOrg* method:

    ```
    log.Errorf("[%s][%s][verifyOrg] Error recovering: %v", CHANNEL_ENV, ERRORRecoveringOrg, err.Error())
    ```
- Definition of error handling through a set of variables included in the model. For instance:
    ```
    ERRORWrongNumberArgs                = `Wrong number of arguments. Expecting a JSON with token information.`
    ERRORParsingData                    = `Error parsing data `
    ERRORPutState                       = `Failed to store data into the ledger.  `
    ```
## Getting Started to deploy the chaincode
1. Download Golang Version: `wget https://golang.org/dl/go1.16.7.linux-amd64.tar.gz`.
2. To verify the tarball checksum it can be used the sha256sum command: `sha256sum go1.16.7.linux-amd64.tar.gz`.
3. Copy Golang bynary into executable folder: `sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.16.7.linux-amd64.tar.gz`.

## Implementation's challenges

1. Working with pointers on nested structures [example](https://play.golang.org/p/UoeBH_2EZdb).
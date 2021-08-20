# Blockchain-based digitization of the roaming agreement drafting process.

The following is **Part 1** of a **6-Part series** associated to the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), which has as its main objective to transform **Telecom Roaming Agreement** documents and the drafting and negotiation process into a digitalized version based on the transparency promoted by blockchain technology. 

Although future posts will go into technical details of each of the modules that have been generated, this first post wants to address the initial vision of the project and why there is a need for it, therefore the following is intended to put the focus on **Roaming Agreements** between mobile operators, highlighting the value associated with blockchain technology.

### Overview of Roaming and Roaming Agreement
**International mobile roaming** is a service that allows mobile users to continue to use their mobile phone or other mobile device to make and receive voice calls and text messages, browse the internet, and send and receive emails, while visiting another country. **Roaming** extends the coverage of the home operator’s retail voice and SMS services, allowing the mobile user to continue using their home operator phone number and data services within another country. The seamless extension of coverage is enabled by a wholesale **Roaming Agreement** between a mobile user’s home operator and the visited mobile operator network.

<p align="center">
  <img src=https://github.com/sfl0r3nz05/Medium/blob/main/Blockchain-based%20digitization%20of%20the%20roaming%20agreement%20drafting%20process/images/roaming_agreement.png>
</p>
 
The **Roaming Agreement** addresses the technical and commercial components required to enable the service. During the drafting stage of the **Roaming Agreement** the parties, i.e., Mobile Network Operators (MNOs), go through a negotiation process and exactly in this process the need for digitalization has been found using the transparency promoted by blockchain technology as the basis of the interactions between the MNOs.

### Roaming Agreement drafting based on GSMA templates
In order to standardize the legal commercial aspects of roaming negotiated between roaming partners for the billing of the services obtained, the GSM Association broadly outlines the content of such **Roaming Agreement** in standardized form for its members [1].

The next Table summarizes the standard templates for International **Roaming Agreement**, as well as the standard templates commonly used. The order commonly followed when creating the **Roaming Agreement** is the one established in the Table, where it is common to start with a base framework (AA.12), from which it is added the set of common annexes (AA.13). From this point, individual annexes and addendums are included depending on the operating characteristics of the agreement. It should be noted that although 99% of mobile network operators (MNOs) manage their **Roaming Agreement** based on the above-mentioned template sequence, there is no limit to the number of documents to be included.

|Standard Templates for International Roaming Agreement                              |Common use|
|:----------------------------------------------------------------------------------:|:--------:|
|AA.12: International Roaming Agreement (Base Legal Framework)	                     |X         |
|AA.13: International Roaming Agreement- Common Annexes	                             |X         |
|AA.14: Individual Annexes Roaming Agreement Exchange Operational Data	             |X         |
|AA.14: Individual Annexes Roaming Agreement Exchange Inter-Operator Tariff	         |X         |
|AA.19: Addendum to the International Roaming Agreement SMS Internetworking Agreement|	        |
|AA.40: Addendum to the International Roaming Agreement MMS Internetworking Agreement|	        |
|AA.43: Addendum to the International Roaming Agreement WLAN Roaming Agreement       |          |
|AA.60: Addendum to the International Roaming Agreement Internetworking Template     |          |
|AA.70: Addendum to the International Roaming Agreement MMS Hubbing Agreement        |	        |
|AA.71: Addendum to the International Roaming Agreement SMS Hubbing Agreement        |          |

- **AA.12** constitutes a standard GSMA or Permanent Reference Document. 
- **AA.13** contains the common annexes with operational information (e.g., information on tap file, billing data, settlement procedure, customer care, fraud, etc.). 
- **AA.14** involves the individual annexes containing information about the operator (e.g., contact details of the roaming team, fraud team, IREG team, TADIG team, etc.). 
- **AA.19** constitutes an addendum to the international **Roaming Agreement** in order to determine specific properties such as charging, billing, and accounting for a specific scenario like the SMS interworking.	

Therefore, a key part of the negotiation process involves the drafting of a **Roaming Agreement** based on the establishment of standard clauses, variations and customized texts with respect to the aforementioned set of templates defined by GSMA. Thus, in the process of drafting the **Roaming Agreement** operators may:

1. Leave an article/sub-article as found in the template thereby establishing a **standard clause**.
2. Introduce certain **variations** in the articles/sub-articles, by changing variables, e.g., MNO, dates, penalties, currencies and so on with respect to the original text, i.e., the GSMA templates.
3. Introduce completely new articles/sub-articles that respond to particular interests by constituting **customized texts**.

### Overview of the Roaming Agreement negotiation process
The lifecycle of the **Roaming Agreement** negotiation process is presented as the compilation of a set of best practices integrated in 7 phases. These phases do not represent standards or rules that must be followed in a mandatory way.
<p align="center">
<img width="572" height="457" src="https://github.com/sfl0r3nz05/Medium/blob/main/Blockchain-based%20digitization%20of%20the%20roaming%20agreement%20drafting%20process/images/negotiation_process.png">
</p>

- **Phase** 1 allows receiving the information package from the future roaming partners, which contains as one of the main points to be analyzed by the operators is the deviations document, which contains all the requested changes with respect to the standard templates (e.g., AA.12 and AA.13) thus becoming an essential part of the negotiation. This information is exchanged between operators usually via information systems such as e-mail.

- **Phase 2** allows to confirm that the **Roaming Agreement** is going ahead. This step depends largely on the circumstances of the deviation document.
- **Phase 3** encourages one of the parties to complete the **Roaming Agreement** contract based on the deviations. In order to create the **Roaming Agreement**, either party can complete the templates.
- **Phase 4** implies that one party sends a soft copy of the contract via email and the other confirm of received. At this point, it is common that some sections be changed, being usual that AA.12 does not change normally and AA.13 changes occasionally, although in the case of the individual annexes of rights they usually suffer changes.
- **Phase 5** enables one of the parties to print hard copies and sign them. Once it is negotiated which party prints the hard copy (at least two copies), it must sign AA.12 in the correct place, although there are cases of operators agreeing to sign all pages of AA.12. Finally, AA.13 is signed and dated for every page.
- **Phase 6** enables the party who did not sign the contract to countersign it. This new signature must be included on both hard copies.
- **Phase 7** states that the party that has countersigned the document shall return one of the signed and countersigned hard copies to the other party. It is recommended that a soft copy be stored for easy access by the entire team.

The **conclusion** reached at this point lies in the fact that the process of drafting the **Roaming Agreement** is mostly very 'analog' with successive exchanges of information between the parties using traditional means such as e-mail or postal mail. This opens the door to a process of digitalization of **Roaming Agreements** where blockchain technology oriented to business environments such as Hyperledger Fabric Blockchain to provide transparency in the interaction between the parties, carrying out a digitalization process on a single platform.

 ## References

 1. GSMA, “Direct Wholesale Roaming Access Agreement Version 2.7 08 December 2017,” London, 2019.
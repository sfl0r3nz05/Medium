# Blockchain-based digitization of the roaming agreement drafting process.

The following is **Part-1** of a **6-Part** series associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by *blockchain* technology. The other authors of this story are Ahmad Sghaier, Noureddin Sadawi, and Mohamed Elshrif. 

Although future parts will go into the technical details of each of the project components, the purpose of this first part is to address the [project's](https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements) initial vision by outlining the problem it solves. Next, we focus our discussion on the main components of **Roaming Agreements** and highlighting the value that **blockchain** technology brings to enable digitization and digitalization of these processes.

### Overview of Roaming and Roaming Agreement
The roaming procedure maintains the persistent connectivity of the subscribers across different networks and geographical regions. Roaming refers to the capability for a subscriber to access the mobile services offered by the Visited Public Mobile Network (VPMN) via the Home Public Mobile Network (HPMN), when moving out of the coverage range of HPMN [1]. The back office part of this seamless extension of coverage is enabled by the process of wholesale **Roaming Agreement**, which are technical, commercial, and legal documents that govern the relationship and billing and accounting between the user's home operator and the visited mobile operator network.

<p align="center">
  <img src=https://github.com/sfl0r3nz05/Medium/blob/main/Blockchain-based%20digitization%20of%20the%20roaming%20agreement%20drafting%20process/images/roaming_agreement.png>
</p>

**Figure 1**: Roaming and Roaming Agreement [2].
 
The **Roaming Agreement** addresses the technical and commercial components necessary to enable the service to a Roaming Customer. During the drafting phase of the **Roaming Agreement** the parties, i.e. the Mobile Network Operators (MNOs), go through a **negotiation process** that currently still uses asynchronous flows such as email or even regular mail for information exchange. *This manual, slow and untrustworthy process has been the reason for the project to promote a transparent negotiation process that ensures the roaming agreement drafting using blockchain technology to record the interactions between MNOs, ensuring reliable traceability*. To elaborate more, next we describe further the two processes: drafting and negotiation of the **Roaming Agreement**.

### Roaming Agreement drafting based on GSMA templates
To standardize the legal commercial aspects of the **Roaming Agreement**, the GSM Association broadly outlines the content of such **Roaming Agreement** in standardized form for its members [3]. Thus, during the drafting process of the agreement, the parties should analyze the articles/sub-articles contained in the standard templates to determine whether:

1. Leave an article/sub-article as found in the template thereby establishing a *standard clause*.
2. Introduce certain *variations* in the articles/sub-articles, by changing *variables*, e.g., MNO, dates, penalties, currencies, and so on concerning the original text, i.e., the GSMA templates.
3. Introduce completely new articles/sub-articles that respond to particular interests by constituting *customized texts*.
4. Specify the value of certain *variables* that are found in a certain text, such as dates, names of entities, amounts, and others.

To clarify the *standardization* presented by GSMA, the next Table summarizes the standard templates for **International Roaming Agreement**, as well as the *standard templates* commonly used. The order commonly followed when creating the **Roaming Agreement** is the one established in the Table, where it is common to start with a base framework (AA.12), from which it is added the set of common annexes (AA.13). From this point, individual annexes and addendums are included depending on the operating characteristics of the agreement. It should be noted that although 99% of mobile network operators (MNOs) manage their **Roaming Agreement** based on the above-mentioned template sequence, there is no limit to the number of documents to be included. The following is a brief description of each common standard template commonly used:

|Standard Templates for International Roaming Agreement                              |Common use|
|:----------------------------------------------------------------------------------:|:--------:|
|AA.12: International Roaming Agreement (Base Legal Framework)	                     |X         |
|AA.13: International Roaming Agreement- Common Annexes	                             |X         |
|AA.14: Individual Annexes Roaming Agreement Exchange Operational Data	             |X         |
|AA.14: Individual Annexes Roaming Agreement Exchange Inter-Operator Tariff	         |X         |
|AA.19: Addendum to the International Roaming Agreement SMS Internetworking Agreement|X	        |
|AA.40: Addendum to the International Roaming Agreement MMS Internetworking Agreement|	        |
|AA.43: Addendum to the International Roaming Agreement WLAN Roaming Agreement       |          |
|AA.60: Addendum to the International Roaming Agreement Internetworking Template     |          |
|AA.70: Addendum to the International Roaming Agreement MMS Hubbing Agreement        |	        |
|AA.71: Addendum to the International Roaming Agreement SMS Hubbing Agreement        |          |

**Table 1**: GSMA standard templates commonly used in Roaming Agreements.

- **AA.12** constitutes a standard GSMA or Permanent Reference Document.
- **AA.13** contains the common annexes with operational information (e.g., information on tap file, billing data, settlement procedure, customer care, fraud, etc.).
- **AA.14** involves the individual annexes containing information about the operator (e.g., contact details of the roaming team, fraud team, IREG team, TADIG team, etc.).
- **AA.19** constitutes an addendum to the international Roaming Agreement to determine specific properties such as charging, billing, and accounting for a specific scenario like the SMS interworking.	

Having described the **Roaming Agreement** drafting process, it is time to analyze the *negotiation process* between the parties, i.e., MNOs.

### Overview of the Roaming Agreement negotiation process
The lifecycle of the **Roaming Agreement** negotiation process is presented as the compilation of a set of best practices integrated in 7 phases. These phases do not represent standards or rules that must be followed in a mandatory way.
<p align="center">
<img width="572" height="457" src="https://github.com/sfl0r3nz05/Medium/blob/main/Blockchain-based%20digitization%20of%20the%20roaming%20agreement%20drafting%20process/images/negotiation_process.png">
</p>

**Figure 2**: Roaming Agreement negotiation phases.

- **Phase** 1 allows receiving the information package from the future roaming partners, which contains, as one of the main points to be analyzed by the operators, the deviations document. This docuemnt contains all the requested changes with respect to the standard templates (e.g., AA.12 and AA.13) thus becoming an essential part of the negotiation. This information is exchanged between operators usually via information systems such as email.
- **Phase 2** allows to confirm that the **Roaming Agreement** is going ahead. This step depends largely on the circumstances of the deviation document, and the changes that might be introduced by the counterparty.
- **Phase 3** encourages one of the parties to complete the **Roaming Agreement** contract based on the deviations. In order to create the **Roaming Agreement**, either party can approve the submitted templates.
- **Phase 4** implies that one party sends a soft copy of the contract via email and the other confirm of received. At this point, it is common that some sections might have changed, considering that usually the AA.12 framework does not change and AA.13 changes occasionally, although in the case of the individual annexes of rights they usually include most of the changes.
- **Phase 5** enables one of the parties to print hard copies and sign them. Once it is negotiated which party prints the hard copy (at least two copies), it must sign AA.12 in the correct place, although there are cases of operators agreeing to sign all pages of AA.12. Finally, AA.13 is signed and dated for every page.
- **Phase 6** enables the party who did not sign the contract to countersign it. This new signature must be included on both hard copies.
- **Phase 7** states that the party that has countersigned the document shall return one of the signed and countersigned hard copies to the other party. It is recommended that a soft copy be stored for easy access by the entire team.

The **conclusion** reached at this point lies in the fact that the process of drafting the **Roaming Agreement** is mostly very 'analog' with successive exchanges of information between the parties using traditional means such as email or regular mail. While other digital platforms also offers a digitized version of this process, yet these platforms lack the transparency and auditbility that **blockchain** solutions can provide. The digitalization of the drafting and negotiation process of  of **Roaming Agreements** using **blockchain** technology such as **Hyperledger Fabric Blockchain** can provide the transparency and auditability in capturing all the interactions between the parties. 

In our next part ([Part-2](https://medium.com/@sfl0r3nz05/nlp-engine-to-detect-variables-standard-clauses-variations-and-customized-texts-893ff9f903e5)), we will start the discussion of our proposed use of *Natural Language Processing (NLP)* as an engine to digitize the legal text in the **Roaming Agreements** drafts and how to build a library that can map the different *variations*, *variables*, *custom texts*, and *standard clauses* in the **Roaming Agreement** articles.

 ## References

1. I. Tanaka, "Volte roaming and interconnection standard technology", NTT Docomo Technical Journal, vol. 15, no. 2, pp. 37–41, 2013.
2. GSMA, "International Roaming explained", online available: https://www.gsma.com/latinamerica/wp-content/uploads/2012/08/GSMA-Mobile-roaming-web-English.pdf, United Kingdom, 2012.
3. GSMA, "Direct Wholesale Roaming Access Agreement Version 2.7 08 December 2017," London, 2019.
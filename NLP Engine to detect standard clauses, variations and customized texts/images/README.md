# NLP Engine to detect standard clauses, variations and customized texts

The following is the second approach of a six posts series associated to the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), which has as its main objective to transform **Telecom Roaming Agreement** documents and the drafting and negotiation process into a digitized version based on the transparency promoted by blockchain technology.
In the [first post of the series]( https://medium.com/@sfl0r3nz05) was mentioned as key part of the negotiation process involves the drafting of a **Roaming Agreement** based on the establishment of standard clauses, variations and customized texts with respect to a set of templates defined by GSMA. In this way, in the process of drafting the roaming agreement operators may:
1. Leave an article/sub-article as found in the template thereby establishing a **standard clause**.
2. Introduce certain **variations** in the articles/sub-articles, by changing variables, e.g., MNO, dates, penalties, currencies and so on with respect to the original text, i.e., the GSMA templates.
3. Introduce completely new articles/sub-articles that respond to particular interests by constituting **customized texts**.

In order to determine as accurately as possible the presence of these three characteristics as part of each of the articles that make up the roaming agreement in this [project]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), a tool for text processing and analysis based on natural language processing (NLP) has been designed, which is hereinafter referred to as NLP engine.

The following figure shows the architecture of the **NLP Engine** that integrates over a docker infrastructure, establishing as inputs the **Roaming Agreement**, as well as the **GSMA templates**; as processing layer the logic associated to the **NLP Engine** and as output the classification of articles/sub-articles in of standard clauses, variations and customized texts.

<img src="https://github.com/sfl0r3nz05/Medium/blob/main/NLP%20Engine%20to%20detect%20standard%20clauses%2C%20variations%20and%20customized%20texts/images/images/NLP_Engine.png">

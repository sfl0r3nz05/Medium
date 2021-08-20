# NLP Engine to detect standard clauses, variations and customized texts

The following is the second approach of a six posts series associated to the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), which has as its main objective to transform **Telecom Roaming Agreement** documents and the drafting and negotiation process into a digitized version based on the transparency promoted by blockchain technology.
In the [first post of the series]( https://medium.com/@sfl0r3nz05) was mentioned as key part of the negotiation process involves the drafting of a **Roaming Agreement** based on the establishment of standard clauses, variations and customized texts with respect to a set of templates defined by GSMA. In this way, in the process of drafting the roaming agreement operators may:
1. Leave an article/sub-article as found in the template thereby establishing a **standard clause**.
2. Introduce certain **variations** in the articles/sub-articles, by changing variables, e.g., MNO, dates, penalties, currencies and so on with respect to the original text, i.e., the GSMA templates.
3. Introduce completely new articles/sub-articles that respond to particular interests by constituting **customized texts**.

In order to determine as accurately as possible the presence of these three characteristics as part of each of the articles that make up the roaming agreement in this [project]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), a tool for text processing and analysis based on natural language processing (NLP) has been designed, which is hereinafter referred to as NLP engine.

The following figure shows the architecture of the **NLP Engine** that integrates over a docker infrastructure, establishing as inputs the **Roaming Agreement**, as well as the **GSMA templates**; as processing layer the logic associated to the **NLP Engine** and as output the **classification of articles/sub-articles** in of standard clauses, variations and customized texts.

<img src="https://github.com/sfl0r3nz05/Medium/blob/main/NLP%20Engine%20to%20detect%20standard%20clauses%2C%20variations%20and%20customized%20texts/images/images/NLP_Engine.png">

The logic designed for the **NLP Engine** has two approaches: *detection* and *comparison*. 
- *Detection* represents the location of variables in the Roaming Agreement. 
- *Comparison* is performed between each sub-article present in the Roaming Agreement with respect to the sub-articles present in the GSMA template.

While a near-total coincidence between texts at the sub-article level represents a **standard clause**, a near-zero coincidence between texts (or simply the non-existence of the sub-article) represents a **customized text**. The intermediate case is represented by the **variation** where there is a high coincidence and the differences are given by the presence of **variables** such as MNO, date, currencies, etc.

## Variables Detection

### Analysis based on Amazon comprehend
The first NLP approach allows to detect entities, key phrases and syntax using the functionalities proposed by the Amazon Comprehend tool. For the text sent to the Amazon Comprehend tool via REST API, it returns a list of objects as shown in expression (1), when entities are been detected. This information is combined with processing and validations based on expression (2) and expression (3), allowing to determine variables. The expression (2) constitutes an object of the list of objects returned by Amazon Comprehend tool when key phrases are detected. The expression (3) also constitutes an object of the list of objects returned by Amazon Comprehend tool when syntaxis are detected.
 ````
 {'BeginOffset':0,'EndOffset':8,'Score':0.43067169189453125,'Text':'Proximus','Type':'ORGANIZATION'}    				(1)
 {'BeginOffset':0,'EndOffset':24, 'Score':0.956875205039978,'Text':'Proximus reference offer'}    				(2)
 {'BeginOffset': 0,'EndOffset':8,'PartOfSpeech':{'Score':0.9324524402618408,'Tag':'PROPN'}    				(3)
 ````

## Establish comparisons


### Analysis of Similarities

The similarity analysis is based on Jaccard's similarity analysis, which has been selected for its simplicity of implementation for this stage of the project, however, the way the code has been developed allows to state that it is pluggable to use, for example, cosine similarity. The similarity analysis consists in comparing the sub-articles of the Roaming Agreement with the sub-articles used as reference. The expression (4) also constitutes an object of the list of objects returned by Jaccard similarity when the sub-article 1.1 is compared for one Roaming Agreement with the reference.
 
 ````
    {'id': '1.1', 'similarity': 0.7380952380952381}    							                                            (4)
 ````
# NLP Engine to detect variables, standard clauses, variations, and customized texts

The following is **Part-2** of a **6-Part series** associated with the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of *transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by **blockchain** technology*. [Ahmad Sghaier](https://medium.com/@asghaier76), [Noureddin Sadawi](https://medium.com/@noureddin.sadawi), and [Mohamed Elshrif](https://medium.com/@melshrif) are also authors of this part.

In [Part-1 of the series](https://github.com/sfl0r3nz05/Medium/tree/main/Blockchain-based%20digitization%20of%20the%20roaming%20agreement%20drafting%20process) it was mentioned that during the *negotiation process of **Roaming Agreement** drafting*, the parties, i.e., MNOs, should analyze the articles/sub-articles contained in the standard templates defined by GSMA to determine whether:

1. Leave an article/sub-article as found in the template thereby establishing a **standard clause**.
2. Introduce certain **variations** in the articles/sub-articles, by changing variables, e.g., MNO, dates, penalties, currencies, and so on concerning the original text, i.e., the **GSMA templates**.
3. Introduce completely new articles/sub-articles that respond to particular interests by constituting **customized texts**.
4. Specify the value of certain **variables** that are found in a certain text, such as dates, names of entities, amounts, and others.

To determine as accurately as possible the presence of these four characteristics as part of each of the articles that make up the roaming agreement in this [project]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), a tool for text processing and analysis based on *Natural Language Processing (NLP)* has been designed, which is hereinafter referred to as the *NLP engine*.

The following figure shows the *overall architecture* of the **NLP Engine** that integrates over a docker infrastructure, establishing as inputs the **Roaming Agreement**, as well as the **GSMA templates**; as processing layer the logic associated to the **NLP Engine**, and as output the **classification of articles/sub-articles** in of *standard clauses*, *variations*, *customized texts*, and *variables*.
<p align="center">
   <img width="239" height="322" src="https://github.com/sfl0r3nz05/Medium/blob/main/NLP%20Engine%20to%20detect%20variables%2C%20standard%20clauses%2C%20variations%20and%20customized%20texts/images/NLP_Engine.png">
<p>

## The logic behind NLP Engine

The logic designed for the **NLP Engine** has two approaches: *detection*, and *comparison*. 
- *Detection* represents the location of **variables** in the Roaming Agreement. 
- *Comparison* is performed between each sub-article present in the Roaming Agreement concerning the *sub-articles* present in the **GSMA template**.

While a near-total coincidence between texts at the sub-article level represents a **standard clause**, a near-zero coincidence between texts (or simply the non-existence of the sub-article) represents a **customized text**. The intermediate case is represented by the **variation** where there is a high coincidence and the differences are given by the presence of **variables** such as MNO, date, currencies, etc.

### Detection of Variables
Variable detection goes through the following steps:
1. A parsing of the **Roaming Agreement** document converted into text is carried out, removing undesired characters. 
2. The parsed text is divided into groups of *100 words*, from which *entities*, *key phrases*, and *syntax* are detected using the **Amazon Comprehend tool** [1].
3. Post-processing mechanisms are applied based on the combination of the detected elements, i.e., *entities*, *key phrases*, and *syntax*.

#### Analysis based on Amazon Comprehend
*Entities*, *key phrases*, and *syntax* are detected using **Amazon Comprehend Tool**. Each piece of text (*100 words*) sent to the **Amazon Comprehend tool** via REST API returns a list of objects as shown in expression (1) when entities have been detected. This information is combined with processing and validations based on expression (2), and expression (3), allowing to determine **variables**. Expression (2) constitutes an object of the list of objects returned by Amazon Comprehend when key phrases are detected. Expression (3) also constitutes an object of the list of objects returned by **Amazon Comprehend** when syntax analysis is performed.

 ````
    {'BeginOffset':0,'EndOffset':8,'Score':0.43067169189453125,'Text':'Proximus','Type':'ORGANIZATION'}				(1)
    {'BeginOffset':0,'EndOffset':24, 'Score':0.956875205039978,'Text':'Proximus reference offer'}    				(2)
    {'BeginOffset': 0,'EndOffset':8,'PartOfSpeech':{'Score':0.9324524402618408,'Tag':'PROPN'}    			        (3)
 ````

### Comparisons of sub-articles
To establish comparisons between text elements a pre-processing step is necessary:
1. A parsing of the **Roaming Agreement** document converted into text is carried out, removing undesired characters.
2. The parsed text is divided into articles, which in turn are divided into *sub-articles*.
3. The *sub-articles* are used to establish the degree of *similarity* concerning the reference represented by the **GSMA template**.
4. In the case that from the *similarity* analysis it is determined that the sub-article is a variation, then we proceed to look for the respective **variables** within the variations.

#### Analysis of Similarities
Similarity analysis, in this project, consists of comparing the *sub-articles* of the **Roaming Agreement** with the *sub-articles* used as reference. The *similarity* analysis is based on *Jaccard's similarity* [2], which has been selected because of its simplicity of implementation for this stage of the project. However, the way the code of this project has been developed allows using other similarity types such as *cosine similarity*. Expression (4) also constitutes an object of the list of objects returned by *Jaccard similarity* when the *sub-article* 1.1 is compared with one **Roaming Agreement** concerning the **sub-article* present in the **GSMA template**.
 
 ````
    {'id': '1.1', 'similarity': 0.7380952380952381}    				                                                (4)
 ````
 
 ### Populate the file with the classification of articles/sub-articles
 Once the determination of the **variables** is performed, the subsequent similarity analysis to classify `sub-articles` into **standard clauses**, **variations**, and **customized texts** and the existing **variables** within the **variations** have been identified, further processing is carried out to populate the file with the classification of articles/sub-articles.

## Accuracy determination
Since pdf files consist of unstructured text and e.g., undesired characters may remain despite parsing of the text, it is mandatory to determine the **accuracy** of the results obtained once the file with the **classification of articles/sub-articles** has been populated. For this purpose, a verification based on the human eye inspection mechanism has been performed. The following experimental tests were performed on the **Roaming Agreement** sample of the MNO *Proximus*. 

### Accuracy determination based on human eye inspection
This **accuracy analysis** involves randomly selecting 5 articles from the sample **Roaming Agreement** and performing a visual inspection (human-eye inspection) to determine the number of *variations*, *standard clauses*, and *custom texts* that exist. The results obtained are then compared with the values populated in the  **article and sub-article classification file** for the same articles. The confusion matrix below sets the hit rate for *standard clause* detection at 80%, for *variation* detection at 81%, and for *custom text* detection at 100%.

###### Proximus Roaming Agreement
|n = 33           |stdClause|variation|customText| 
|:---------------:|:-------:|:-------:|:--------:| 
|stdClause        |9        |1        |0         |
|variation        |1        |17       |3         | 
|customText       |0        |0        |3         |

The detailed results obtained for this experiment have been published in the scientific paper "A Natural Language Processing Approach for the
Digitalization of Roaming Agreement" in the conference [ILCICT 2021](https://ilcict.lit.ly/en/). This manuscript also contains the results for another experiment based on an analysis conducted at the symbol level using the text comparison tool [Countwordsfree](https://countwordsfree.com/comparetexts).

In our next part (Part-3), we will start the discussion of our proposed design of the *Hyperledger Fabric Blockchain chaincode* to manage the digitalization of the drafting and negotiation process of **Roaming Agreements** providing a transparent and auditable mechanism to capture all the interactions between the parties.

## References
1. AWS, “Amazon Comprehend Developer Guide,” 2021. [Online]. Available: https://docs.aws.amazon.com/comprehend/latest/dg/comprehend-dg.pdf. [Accessed: 24-Jul-2021].
2. S. Gupta, “Overview of Text Similarity Metrics in Python,” May 15, 2018, 2018. [Online]. Available: https://towardsdatascience.com/overview-of-text-similarity-metrics-3397c4601f50. [Accessed: 24-Jul-2021].

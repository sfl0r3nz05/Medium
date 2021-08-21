# NLP Engine to detect variables, standard clauses, variations and customized texts

The following is **Part-2** of a **6-Part series** associated to the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), with the main objective of *transforming the **Telecom Roaming Agreement** drafting and negotiation process into a digitalized version based on the transparency promoted by **blockchain** technology*.

In the [Part-1 of the series](https://github.com/sfl0r3nz05/Medium/tree/main/Blockchain-based%20digitization%20of%20the%20roaming%20agreement%20drafting%20process) was mentioned that during *negotiation process of **Roaming Agreement** drafting*, the parties, i.e., MNOs, should analyze the articles/sub-articles contained in the standard templates defined by GSMA to determine whether:

1. Leave an article/sub-article as found in the template thereby establishing a **standard clause**.
2. Introduce certain **variations** in the articles/sub-articles, by changing variables, e.g., MNO, dates, penalties, currencies and so on with respect to the original text, i.e., the GSMA templates.
3. Introduce completely new articles/sub-articles that respond to particular interests by constituting **customized texts**.
4. Specify the value of certain **variables** that are found in a certain text, such as dates, names of entities, amounts and others.

In order to determine as accurately as possible the presence of these four characteristics as part of each of the articles that make up the roaming agreement in this [project]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), a tool for text processing and analysis based on *Natural Language Processing (NLP)* has been designed, which is hereinafter referred to as NLP engine.

The following figure shows the *overall architecture* of the **NLP Engine** that integrates over a docker infrastructure, establishing as inputs the **Roaming Agreement**, as well as the **GSMA templates**; as processing layer the logic associated to the **NLP Engine** and as output the **classification of articles/sub-articles** in of *standard clauses*, *variations*, *customized texts* and *variables*.
<p align="center">
   <img width="239" height="322" src="https://github.com/sfl0r3nz05/Medium/blob/main/NLP%20Engine%20to%20detect%20variables%2C%20standard%20clauses%2C%20variations%20and%20customized%20texts/images/images/NLP_Engine.png">
<p>

## Logic behind NLP Engine

The logic designed for the **NLP Engine** has two approaches: *detection* and *comparison*. 
- *Detection* represents the location of **variables** in the Roaming Agreement. 
- *Comparison* is performed between each sub-article present in the Roaming Agreement with respect to the *sub-articles* present in the GSMA template.

While a near-total coincidence between texts at the sub-article level represents a **standard clause**, a near-zero coincidence between texts (or simply the non-existence of the sub-article) represents a **customized text**. The intermediate case is represented by the **variation** where there is a high coincidence and the differences are given by the presence of **variables** such as MNO, date, currencies, etc.

### Detection of Variables
Variable detection goes through the following steps:
1. A parsing of the **Roaming Agreement** document converted into text is carried out, removing undesired characters. 
2. The parsed text is divided into groups of *100 words*, from which *entities*, *key phrases* and *syntax* are detected from **Amazon Comprehend tool** [1].
3. Post-processing mechanisms are applied based on the combination of the detected elements, i.e., *entities*, *key phrases* and *syntax*.

#### Analysis based on Amazon Comprehend
From **Amazon Comprehend Tool** *entities*, *key phrases* and *syntax* are detected. The piece of texts (*100 words*) sent to the **Amazon Comprehend tool via** REST API return a list of objects as shown in expression (1), when entities are been detected. This information is combined with processing and validations based on expression (2) and expression (3), allowing to determine **variables**. The expression (2) constitutes an object of the list of objects returned by Amazon Comprehend tool when key phrases are detected. The expression (3) also constitutes an object of the list of objects returned by **Amazon Comprehend tool** when syntaxis are detected.

 ````
    {'BeginOffset':0,'EndOffset':8,'Score':0.43067169189453125,'Text':'Proximus','Type':'ORGANIZATION'}				(1)
    {'BeginOffset':0,'EndOffset':24, 'Score':0.956875205039978,'Text':'Proximus reference offer'}    				(2)
    {'BeginOffset': 0,'EndOffset':8,'PartOfSpeech':{'Score':0.9324524402618408,'Tag':'PROPN'}    			        (3)
 ````

### Comparisons of sub-articles
In order to establish comparisons between text elements a pre-processing is necessary:
1. A parsing of the **Roaming Agreement** document converted into text is carried out, removing undesired characters.
2. The parsed text is divided into articles, which in turn are divided into *sub-articles*.
3. The *sub-articles* are used to establish the degree of *similarity* with respect to the reference represented by **GSMA template**.
4. In the case that from the *similarity* analysis it is determined that the sub-article is a variation, then we proceed to looking for the respective **variables** within the variations.

#### Analysis of Similarities
The *similarity* analysis is based on *Jaccard's similarity analysis* [2], which has been selected for its simplicity of implementation for this stage of the project, however, the way the code has been developed allows to state that it is pluggable to use, for example, *cosine similarity*. The similarity analysis consists in comparing the *sub-articles* of the **Roaming Agreement** with the *sub-articles* used as reference. The expression (4) also constitutes an object of the list of objects returned by *Jaccard similarity* when the *sub-article* 1.1 is compared for one **Roaming Agreement** with respect to the **sub-article* present in the GSMA template.
 
 ````
    {'id': '1.1', 'similarity': 0.7380952380952381}    				                                                (4)
 ````
 
 ### Populate file with the classification of articles/sub-articles
 Once the determination of the **variables**, the subsequent similarity analysis to classify `sub-articles` into: **standard clauses**, **variations** and **customized texts** and the existing **variables** within the **variations** have been identified, further processing is performed to populate file with the classification of articles/sub-articles.

## Accuracy determination
Since pdf files consist of unstructured text and e.g., undesired characters may remain despite parsing of the text, it is mandatory determine the **accuracy** of the results obtained once the file with the **classification of articles/sub-articles** has been populated. For this purpose, two types of mechanisms have been used to determine the **accuracy of the results**. On the one hand, a verification based on human eye inspection and on the other hand, a verification based on symbol comparison. The tests will be performed on two **Roaming Agreements** sample of the MNOs *Proximus* and *Orange*.

### Accuracy determination based on human eye inspection
The first **accuracy analysis** involves randomly selecting 5 articles from each of the sample **Roaming Agreements** and performing a visual inspection (human-eye inspection) to determine the number of *variables*, *variations*, *standard clauses* and *custom texts* that exist. The results obtained are then compared with the values populated in the **article and sub-article classification file** for the same articles. Considering that the results obtained from the visual inspection constitute the *observations* and the values collected from the **article and sub-article classification file** constitute the *predicted values*, the confusion matrices obtained are as follows:

###### Proximus Roaming Agreement
|n = 33           |Predicted: YES   |Predicted: No  |
|:---------------:|:---------------:|:-------------:|
|Observation Yes  |TP = 19          |FN = 7         |
|Observation No   |FP = 5           |TN = 2         |

###### Orange Roaming Agreement
|n = 30           |Predicted: YES   |Predicted: No  |
|:---------------:|:---------------:|:-------------:|
|Observation Yes  |TP = 13          |FN = 11         |
|Observation No   |FP = 4           |TN = 2         |

## Accuracy determination based on symbol comparison
The second **accuracy analysis** involves establishing a comparison between the sub-articles populatedin the output file with respect to the sub-articles existing in the input file containing the roamingagreements. For that purpose, the text comparison tool [Countwordsfree](https://countwordsfree.comcomparetexts) has been used manually copying sub-article by sub-article. For each sub-article is determined:
 
1.	Common percentage of words between compared sub-articles.
2.	Difference percentage of words between compared sub-articles.
3.	Common words between compared sub-articles.
4.	Difference words between compared sub-articles.

Considering that for each sub-article the characters that determine the differences have been analyzed, determining that they are mostly end-of-line characters, quotation marks, parentheses and brackets, the threshold of the common percentage of words is determined as the worst case, i.e., where a greater combination of these characters appears. From the threshold, the rest of the cases found with lower common percentage of words will be considered failures.

|Roaming Agreement (PDF File)|Threshold |Sub-article above the threshold|Sub-article below the threshold|
|:--------------------------:|:--------:|:-----------------------------:|:-----------------------------:|
|Proximus RA                 |78.43     |51                             |21                             |
|Orange RA                   |78.74     |70                             |14                             |

Although the results should be improved in future updates of the project, they can be considered acceptable.

In our next part (Part-3), we will start the discussion of our proposed design of the *Hyperledger Fabric Blockchain chaincode* to manage digitalization of the drafting and negotiation process of  of **Roaming Agreements** providing a transparent and auditable mechanism to capture all the interactions between the parties.

## References
1. AWS, “Amazon Comprehend Developer Guide,” 2021. [Online]. Available: https://docs.aws.amazon.com/comprehend/latest/dg/comprehend-dg.pdf. [Accessed: 24-Jul-2021].
2. S. Gupta, “Overview of Text Similarity Metrics in Python,” May 15, 2018, 2018. [Online]. Available: https://towardsdatascience.com/overview-of-text-similarity-metrics-3397c4601f50. [Accessed: 24-Jul-2021].
# NLP Engine to detect standard clauses, variations and customized texts

The following is the second approach of a six posts series associated to the project [The Use of NLP and DLT to Enable the Digitalization of Telecom Roaming Agreements]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), which has as its main objective to transform **Telecom Roaming Agreement** documents and the drafting and negotiation process into a digitized version based on the transparency promoted by blockchain technology.
In the [first post of the series]( https://medium.com/@sfl0r3nz05) was mentioned as key part of the negotiation process involves the drafting of a **Roaming Agreement** based on the establishment of standard clauses, variations and customized texts with respect to a set of templates defined by GSMA. In this way, in the process of drafting the roaming agreement operators may:
1. Leave an article/sub-article as found in the template thereby establishing a **standard clause**.
2. Introduce certain **variations** in the *articles/sub-articles*, by changing **variables**, e.g., MNO, dates, penalties, currencies and so on with respect to the original text, i.e., the GSMA templates.
3. Introduce completely new *articles/sub-articles* that respond to particular interests by constituting **customized texts**.

In order to determine as accurately as possible the presence of these three characteristics as part of each of the articles that make up the roaming agreement in this [project]( https://wiki.hyperledger.org/display/INTERN/Project+Plan%3A+The+Use+of+NLP+and+DLT+to+Enable+the+Digitalization+of+Telecom+Roaming+Agreements), a tool for text processing and analysis based on natural language processing (NLP) has been designed, which is hereinafter referred to as NLP engine.

The following figure shows the architecture of the **NLP Engine** that integrates over a docker infrastructure, establishing as inputs the **Roaming Agreement**, as well as the **GSMA templates**; as processing layer the logic associated to the **NLP Engine** and as output the **classification of articles/sub-articles** in of standard clauses, variations and customized texts.

<img src="https://github.com/sfl0r3nz05/Medium/blob/main/NLP%20Engine%20to%20detect%20standard%20clauses%2C%20variations%20and%20customized%20texts/images/images/NLP_Engine.png">

## Logic behind NLP Engine

The logic designed for the **NLP Engine** has two approaches: *detection* and *comparison*. 
- *Detection* represents the location of **variables** in the Roaming Agreement. 
- *Comparison* is performed between each sub-article present in the Roaming Agreement with respect to the *sub-articles* present in the GSMA template.

While a near-total coincidence between texts at the sub-article level represents a **standard clause**, a near-zero coincidence between texts (or simply the non-existence of the sub-article) represents a **customized text**. The intermediate case is represented by the **variation** where there is a high coincidence and the differences are given by the presence of **variables** such as MNO, date, currencies, etc.

### Variables Detection
Variable detection goes through the following steps:
1. A parsing of the **Roaming Agreement** document converted into text is carried out, removing undesired characters. 
2. The parsed text is divided into groups of `100 words`, from which `entities`, `key phrases` and `syntax` are detected from `Amazon Comprehend tool` [1].
3. Post-processing mechanisms are applied based on the combination of the detected elements, i.e., `entities`, `key phrases` and `syntax`.

#### Analysis based on Amazon Comprehend
From *Amazon Comprehend Tool* entities, key phrases and syntax are detected. The piece of texts (`100 words`) sent to the Amazon Comprehend tool via `REST API` return a list of objects as shown in expression (1), when entities are been detected. This information is combined with processing and validations based on expression (2) and expression (3), allowing to determine **variables**. The expression (2) constitutes an object of the list of objects returned by Amazon Comprehend tool when key phrases are detected. The expression (3) also constitutes an object of the list of objects returned by `Amazon Comprehend tool` when syntaxis are detected.
 ````
    {'BeginOffset':0,'EndOffset':8,'Score':0.43067169189453125,'Text':'Proximus','Type':'ORGANIZATION'}				(1)
    {'BeginOffset':0,'EndOffset':24, 'Score':0.956875205039978,'Text':'Proximus reference offer'}    				(2)
    {'BeginOffset': 0,'EndOffset':8,'PartOfSpeech':{'Score':0.9324524402618408,'Tag':'PROPN'}    			        (3)
 ````

### Establish comparisons
In order to establish comparisons between text elements a pre-processing is necessary:
1. A parsing of the **Roaming Agreement** document converted into text is carried out, removing undesired characters.
2. The parsed text is divided into articles, which in turn are divided into *sub-articles*.
3. The *sub-articles* are used to establish the degree of `similarity` with respect to the reference represented by **GSMA template**.
4. In the case that from the similarity analysis it is determined that the sub-article is a variation, then we proceed to looking for the respective **variables** within the variations.

#### Analysis of Similarities
The similarity analysis is based on `Jaccard's similarity analysis` [2], which has been selected for its simplicity of implementation for this stage of the project, however, the way the code has been developed allows to state that it is pluggable to use, for example, `cosine similarity`. The similarity analysis consists in comparing the *sub-articles* of the **Roaming Agreement** with the *sub-articles* used as reference. The expression (4) also constitutes an object of the list of objects returned by Jaccard similarity when the *sub-article* 1.1 is compared for one **Roaming Agreement** with the reference.
 
 ````
    {'id': '1.1', 'similarity': 0.7380952380952381}    				                                                (4)
 ````
 
 ### Populate file with the classification of articles/sub-articles
 Once the determination of the **variables**, the subsequent similarity analysis to classify `sub-articles` into: **standard clauses**, **variations** and **customized texts** and the existing **variables** within the **variations** have been identified, further processing is performed to populate file with the classification of articles/sub-articles.

 ## Accuracy determination
 Since pdf files consist of unstructured text and e.g., undesired characters may remain despite parsing of the text, it is mandatory determine the **accuracy** of the results obtained once the file with the **classification of articles/sub-articles** has been populated. This **accuracy analysis** involves establishing a comparison between the sub-articles populated in the output file with respect to the sub-articles existing in the input file containing the roaming agreements. For that purpose, the text comparison tool [Countwordsfree](https://countwordsfree.com/comparetexts) has been used manually copying sub-article by sub-article. For each sub-article is determined:
1.	Common percentage of words between compared sub-articles
2.	Difference percentage of words between compared sub-articles
3.	Common words between compared sub-articles
4.	Difference words between compared sub-articles

Considering that for each sub-article the characters that determine the differences have been analyzed, determining that they are mostly end-of-line characters, quotation marks, parentheses and brackets, the threshold of the common percentage of words is determined as the worst case, i.e., where a greater combination of these characters appears. From the threshold, the rest of the cases found with lower common percentage of words will be considered failures.

|Roaming Agreement (PDF File)|Threshold |Sub-article above the threshold|Sub-article below the threshold|
|:--------------------------:|:--------:|:-----------------------------:|:-----------------------------:|
|Proximus RA                 |78.43     |51                             |21                             |
|Orange RA                   |78.74     |70                             |14                             |

Although the results need to be improved, being the improvements, an important part of the project's "To Do", the results can be considered acceptable, and therefore the way is clear to move the project to production.

## References
1. AWS, “Amazon Comprehend Developer Guide,” 2021. [Online]. Available: https://docs.aws.amazon.com/comprehend/latest/dg/comprehend-dg.pdf. [Accessed: 24-Jul-2021].
2. S. Gupta, “Overview of Text Similarity Metrics in Python,” May 15, 2018, 2018. [Online]. Available: https://towardsdatascience.com/overview-of-text-similarity-metrics-3397c4601f50. [Accessed: 24-Jul-2021].
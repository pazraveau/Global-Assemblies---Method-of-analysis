# Global-Assemblies---Method-of-analysis
In this tutorial, we explain the analysis method for the Global Assemblies project. In the first stage, we propose a methodological design for analyzing citizen dialogues, which we test using dummy data generated for this purpose. 
The processed data contains the following columns. The first ones characterize the dialogue itself:

+ *id_dialogue*: dialogue identifier  (eg., 43) <br>
+ *city*: city ​​where the dialogue took place (eg., Potters Village) <br>
+ *country*: country ​​where the dialogue took place (eg., Antigua and Barbuda)<br>
+ *continent*: continent ​​where the dialogue took place (eg., America)<br>
+ *created_at*: date of the dialogue (eg., 2025-09-07)<br>
+ *org_type*: type of organization that hosted the dialogue (eg., Activist Network)<br>

The following columns are for each of the "actions" mentioned in the dialogue:

+ *id*: action idetifier  (eg., 43_3, woul be the third action associated with dialogue 43)<br>
+ *text*: text describing the action<br>

Example: "Establish a community-run farmers' market to showcase and sell locally-grown produce, promoting food security and supporting small-scale farmers in Potters Village."

+ *actor_name*: name of actors mentioned in the action text (eg., Potters Village Local Growers' Cooperative)<br>
+ *actor_type*: type of actors mentioned in the action text (eg., Community Based Organization)<br>
+ *task*: community tasks, in relation to the proposed action<br>

Example: "The community assembly participants could provide support, resources, and promotion to help the Potters Village Local Growers' Cooperative successfully launch and sustain a regular farmers' market featuring locally-grown produce."

+ *updates*: updates regarding the action, following the dialogue<br>

Example: "Two months on, the Potters Village farmers' market is thriving. Held every Saturday, it features 20 local farmers selling fresh produce, baked goods, and artisanal crafts. Community volunteers help with setup and promotion. Attendance has doubled since opening day, bolstering the local economy and food security."

MONITO

## 1. Topic  Modeling

### 1.1 Preprocessing

Data preprocessing depends on the technique used. In this case, we use Latent Dirichlet Allocation (LDA), therefore, the preprocessing is like in any Bag of Words (BoW) technique: 

- Removal of punctuation and numbers
- Removal of stopwords
- Lemmatization

Implementing LDA requires a BoW matrix, typically a Document Term Matrix (DTM) or a Term Frequency-Inverse Document Frequency Matrix (TF-IDF). The results shown below use a TF-IDF matrix, but this should be evaluated on a case-by-case basis. To improve the model's interpretability, we generated the TF-IDF matrix using words and bigrams.

### 1.2 Topic diagnosis

Topic diagnosis was originally performed by calculating the “perplexity” of the model, i.e., how good is the model to predict unseen data. According to Blei (2012), however, "there is no technical reason to suppose that held-out accuracy corresponds to better organization or easier interpretation.”

Thus, a new score was subsequently developed to remedy the problem that topic models give no guarantee on the interpretability of their output (Röder, Both and Hinneburg, 2015). This score is called “coherence” or “semantic coherence”, and measures if the most probable terms in a topic co-occur frequently.

Hence, here we calculate only the semantic coherence to perform the topic diagnosis. The higher the coherence, the better the model.  A semantic coherence function is included in gensim implementation.

This is done by running the LDA model for different numbers of topics and for each case, calculating the semantic coherence.


<img width="452" height="299" alt="image" src="https://github.com/user-attachments/assets/eb2c7e87-a999-4791-ba6f-b5f18093c0af" />


### 1.3 LDA model

The output for an LDA model consists of two matrices: 

(a) Theta: dataframe with the distribution of documents by topics <br>
(b) Beta: dataframe with the distribution of topics by words  <br>

To evaluate the LDA model, we will look at (a) 10 most probable words (or bigrams) of the topic, the 10 words with the highest FREX scores, and the 10 most representative documents of the topic (the ones with the highest score in Theta).


| Topic | Highest Prob. Words | Frex Words |
| :----------- | :------------ | :------------ |
| 0  | crop, share, energy, preserve, traditional, local, seed_bank, bank, knowledge, establish  |  collaborate, irrigation, energy, diversity, pacific_island, finance, sustainably, pacific, harvesting, traditional     |
| 1  | crop, bank, seed, seed_bank, crop_variety, variety, preserve, resilient, local, establish  | particularly, affect, california, displace, heirloom, crop_variety, safe, nutrition, variety, bank      |
| 2  | develop, food, sustainable, establish, community, climate, program, local, variety, healthy  | land, youth, rich, diet, skill, cost, farm, healthy, uncertain, educational  |
| 3  |  support, climate, global, climate_change, change, advocate, practice, agreement, food, agriculture | agreement, regenerative, scale, small_scale, lobby, adopt, ethiopia, goma, adoption, africa  |
| 4  | agriculture, initiative, global, produce, provide, promote, income, production, low, food  | neighborhood, carbon, carbon_tax, tax, urban_agriculture, foster, integrate, dense, aquaponics, self  |
| 5  | water, food, access, ensure, program, sustainable, face, climate, agricultural, change  | exchange_knowledge, access_affordable, especially, health, scarcity, run, sovereignty, empower, training, water  |


Topic 0: Sharing knowledge (also about energy) <br>
Topic 1: Seed banks and crop varieties <br>
Topic 2: ¿? <br>
Topic 3: Food and agricultural practices in climate change <br>
Topic 4: Food production <br>
Topic 5: Food and water security in the face of climate change <br>

Notes: 

+ The results may vary greatly when changing the seed, specially for small corpus. Try chaging the seed to improve results.  
+ When the entries look very similar, the topics are best differentiated using an LDA- TfIDF. However, this can distort the actual weight of the topics in the corpus. 
+ If LDA partition looks odd, evaluate using a qualitative labeling and subsequent classification algorithm. In which case, the codes for the other tasks would still be useful, and the LDA-based codes will need adjustment. 

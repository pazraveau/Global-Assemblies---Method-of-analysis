# Global Assemblies- Method of analysis

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

*Figure 1: Number of dialogues per country in the artificial dataset used for this tutrial*
<img width="871" height="342" alt="image" src="https://github.com/user-attachments/assets/f3d219e2-9bb7-4810-b050-e4c411879c06" />

## 1. Topic  Modeling

The first step in the analysis is a description of the content, for which we will use Topic Modeling. The model takes the "actions," that is, the texts that describe the conclusions of each dialogue.

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

*Figure 2: Topic diagnosis.*
<img width="452" height="299" alt="image" src="https://github.com/user-attachments/assets/eb2c7e87-a999-4791-ba6f-b5f18093c0af" />


### 1.3 LDA model

According to Figure 2, we choose 6 topics for an LDA model with Tf-IDF matrix.

The output for an LDA model consists of two matrices: 

(a) Theta: dataframe with the distribution of documents by topics <br>
(b) Beta: dataframe with the distribution of topics by words  <br>

To evaluate the LDA model, we will look at 10 most probable words (or bigrams) of the topic, and the 10 words with the highest FREX scores.


| Topic | Highest Prob. Words | Frex Words |
| :----------- | :------------ | :------------ |
| 0  | crop, share, energy, preserve, traditional, local, seed_bank, bank, knowledge, establish  |  collaborate, irrigation, energy, diversity, pacific_island, finance, sustainably, pacific, harvesting, traditional     |
| 1  | crop, bank, seed, seed_bank, crop_variety, variety, preserve, resilient, local, establish  | particularly, affect, california, displace, heirloom, crop_variety, safe, nutrition, variety, bank      |
| 2  | develop, food, sustainable, establish, community, climate, program, local, variety, healthy  | land, youth, rich, diet, skill, cost, farm, healthy, uncertain, educational  |
| 3  |  support, climate, global, climate_change, change, advocate, practice, agreement, food, agriculture | agreement, regenerative, scale, small_scale, lobby, adopt, ethiopia, goma, adoption, africa  |
| 4  | agriculture, initiative, global, produce, provide, promote, income, production, low, food  | neighborhood, carbon, carbon_tax, tax, urban_agriculture, foster, integrate, dense, aquaponics, self  |
| 5  | water, food, access, ensure, program, sustainable, face, climate, agricultural, change  | exchange_knowledge, access_affordable, especially, health, scarcity, run, sovereignty, empower, training, water  |

The topic interpretation will rely on these lists of words, as well as the most representative documents of the topic (the ones with the highest score in Theta). A good model will be one whose topics can be interpreted. 

Based on our results, the following topic labeling is proposed: 

Topic 0: Sharing knowledge (also about energy) <br>
Topic 1: Seed banks and crop varieties <br>
Topic 2: (unable to label) <br>
Topic 3: Food and agricultural practices in climate change <br>
Topic 4: Food production <br>
Topic 5: Food and water security in the face of climate change <br>

Notes: 

+ The results may vary greatly when changing the seed, specially for small corpus. Try chaging the seed to improve results.  
+ When the entries look very similar, the topics are best differentiated using an LDA- TfIDF. However, this can distort the actual weight of the topics in the corpus. 
+ Beyond looking at the most probable words and documents per topic, the Beta and Theta matrices allow us to examine the specific weights, in order to test whether specific words carry too much weight in the topic and also in the topic proportion.
+ If LDA partition looks odd, evaluate using a qualitative labeling and subsequent classification algorithm. In which case, the codes for the other tasks would still be useful, and the LDA-based codes will need adjustment.

### 1.4 Topic proportion

Next, we may want to know how important each topic is. This can be done with Theta matrix, which returns the topic distribution per document, in two ways: 

(a)  calculating the topic proportion on the whole corpus by summing Theta column-wise. This will return a decimal number between 0 and 1 for each topic (the topic “weight”). <br>


| Topic | Propotion | 
| :----------- | :------------ | 
|0	|0.1894433|
|1	|0.13656229|
|2	|0.11648715|
|3	|0.1371431|
|4	|0.18208739|
|5	|0.23827669|

(b) counting the number of documents in which a given topic is more probable. 

|Topic | Count|
| :----------- | :------------ | 
|0|	86|
|1|	56|
|2|	45|
|3|	56|
|4|	82|
|5|	114|


Any of these methods can be used to observe the topic importance, for example, in time.

*Figure 3: Monthly variation of topic importance*
<img width="552" height="394" alt="image" src="https://github.com/user-attachments/assets/81308de6-c675-45f3-9600-ddff6f6e0f97" />

### 1.5 What is behind topic proportion?

We fit an OLS model on each column on theta columns (e.g., one OLS model per topic), aiming to explain the topic proportion on documents. As independent variables, we will use the type of assembly organizer (org_type), fixed effect by continent (continent), and vulnerability index (vulnerability) of the country in which the assembly was held (from https://gain.nd.edu/our-work/country-index/) .

The variables “continent” and “org_type” are categorical, and the reference categories were set to “Europe” and “Local Government”, respectively. 
Results: (the constant term of the regression was omitted)

|Topic 0||
|:--|:--|
|org_type: Community Based Organization|	0.145770|
|org_type: Social Movement| 	 0.205705|

|Topic 1||
|:--|:--|
|continent: America|	 0.114585|
   

|Topic 4||
|:--|:--|
|continent: America| 	-0.148936|

Note: OLS models for Topic 3 and Topic 5 show no significant results. As Topic 2 was not labeled, we omit it from this analysis. 

The model indicates that, for example, Topic 1 (Seed banks and crop varieties) is more prevalent in dialogues held in the Americas (compared to Europe), even after controlling for other variables such as country vulnerability. Topic 0, on the other hand, is more prevalent when the dialogue is organized by civil society (community organizations and social movements) compared to government institutions.

## 2. Location identification

In addition to the general identification of topics and the study of their prevalence, we can answer other questions. One of these concerns the places that are mentioned in the dialogues.

To do this, we will use Name Entity Recognition (NER), a tool provided by the Stanford Core NLP group through the Python package, Stanza (https://stanfordnlp.github.io/stanza/). NER allows us to identify, among other things, the places mentioned in the dialogue texts (specifically, we will use the "action" entry, where the main conclusions of the dialogue are described). The places mentioned can be specific cities or countries, as well as geographical areas, such as the Amazon or the Arctic.

The geopy package (https://pypi.org/project/geopy/) provides us with the coordinates of the places found and allows us to calculate the distances between them.

For example, Figure 4 shows the cities in which the Amazon is mentioned. The map also shows the Amazon as a green dot (although it is a large region, the geopy package provides a single coordinate).

*Figure 4: Places in which the Amazon is mentioned (red dots).*
<img width="777" height="303" alt="image" src="https://github.com/user-attachments/assets/f11be38e-ce66-4460-8f2c-c33466c907df" />


## 3. Identifying vulnerable groups

Identifying references to vulnerable groups begins by establishing categories and a set of terms associated with each one.

|Label	 |Words in seed-dictionary|
|:-- | :-- |
|African|	african, black, afro, kushite, nubian, pardo, mulatto, creole, garifuna|
|Indigenous|	indigenous, aboriginal, native, tribe, māori, inuit, sami, mapuche, aymara, quechua, guarani, ashaninka, yanomami, tikuna, huichol, zapotec, mixtec, nahua, otomí, maya, mayan, purépecha, wixarika, hopi, cherokee, lakota, navajo, apache, zuni, iroquois, autochthonous, tlaxcaltec, tsotsil, tzeltal, tzotzil, mazahua, mazatec, tzotzil, omagua, chickasaw, choctaw, cree, anishinaabe, wampanoag, nanticoke, lenape, algonquin, kwakwaka, wakw, mi'kmaq, nuu-chah-nulth, tongva, yurok, kumeyaay, huave, yaqui, seri, tarahumara, taroko, aeta, ifugao, dayak, batak, pemon, embu, gikuyu, zarma, mbundu, bamileke, herero|
|Roma|	romani, gypsy, gipsy, sinti, gitano, calé|
|Minorities|	jewish, jews, muslim, muslims, islamic, islamists, assyrian, buddhist, hindus, sikhs, yazidi, kurds, kurdish, ahmadiyya, zoroastrian, bahai, shia, sunni, druze, copti, uighur, uigurs, hazaras, tatars, circassian, baloch, rohingya, hazaragi, igbo, yoruba, zulu, xhosa, tigray, tutsi, hutu, berber, amazigh, tuareg, basque, catalan, galician, breton, sami, hmong, tibetan, karen, shan, acehnese, chaldean, aramaean, assyrian, syriac, comorian, javanese, sundane|
|Migrants|	migrant, migrants, expat, foreign, noncitizen, migration, deportation'|
|Displaced|	refugee, displaced, asylum, exile, exiles, asylee, returnee|
|Poverty|	poverty, destitute, impoverished, poor, homeless, unhoused, houseless, roofless, squatters, favelado, starving, malnourished, hungry, underclass|
|Women|	woman, girl, female, feminine, femme, mother, feminist, feminism|
|LQBTQ|	lgbt, lgbtq, lgbtqi, lgbtq+, lesbian, gay, bisexual, transgender, trans, queer, asexual, pansexual, demisexual, genderqueer, agender, bigender, transwoman, transman, sapphic, polyamorous, homosexual, cisgender|

We tested two approaches, the first one consisted on searching the words of each category in the actions. Table XX shows the number of words present in the dataset, as well as some of the actual words. 

|Label | count | words|
|:-- | --: | :-- |
|African 	|0||
|Indigenous |	19| 	indigenous, native, indigenous, indigenous…|
|Roma |	0||
|Minorities| 	1| ethnic minorities|
|Migrants 	|2| migrant, migrants|
|Displaced 	|4| refugees, refugee, refugees, refugees|
|Poverty 	|68| low-income, malnourished, impoverished…|
|Women 	|24|  mothers, widows, pregnant women, women…|
|Lgbtqi 	|0||

The second option involves combining the set of words per category (the dictionary) with an embedding model. The embedding model, trained on a large, general-purpose corpus, should capture words similar, even if they are not included in the dictionary. The implementation presented here used a Word2Vec model (GoogleNews-vectors-negative300.bin) and calculates the cosine similarity between the embedding vector of each word in the action and the embedding vector of each word in the category. The resulting score corresponds to the maximum cosine similarity obtained (a possible variation would involve comparing the average embeddings of the words in the action and in the category).

Example : 

“Establish knowledge-sharing partnerships with communities transitioning from fossil fuels, like coal towns in Appalachia or oil workers in Nigeria, to exchange ideas and strategies for building resilient post-extractive economies.”		

The following table shows the result for this “action”. The third and fourth row show the word in the dictionary and the “action”, respectively, for which the cosine similarity was the highest. Note that many proper nouns or location names are not included in the model's vocabulary, for example, Appalachia and Nigeria. Also, compound words are also not included in the model's vocabulary; they would need to be preprocessed if you wanted to separate them, although this would change the meaning.	

|AFRICAN|	INDIGENOUS|	ROMA|	MINORITIES|	MIGRANTS|	DISPLACED|	POVERTY|	WOMEN|	LGBTQI|
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:---|
|0.5687768|	0.36864054	|0.19463305	|0.60938865	|0.39933679	|0.28370056	|0.3515791	|0.24430211	|0.2622174|
|Africa|	Cherokee|	Gipsy|	Igbo|	migrants|	displaced|	impoverished|	women|	lgbt|
|Nigeria	|Appalachia	|Appalachia	|Nigeria	|workers	|workers	|Appalachia	|workers	|communities|

Only “AFRICAN” and “MINORITIES” categories score above 0.5, and it seems to make sense (although Cherokee with Appalachia are a good match too, maybe a lower threshold for the first four categories). 

Now, let us take the category "Poverty". The words in category “Poverty” are: 'poverty', 'destitute', 'impoverished', 'poor', 'homeless', 'unhoused', 'houseless', 'roofless', 'squatters', 'starving', 'malnourished', 'hungry', 'underclass'

If we search for the highest score actions, all cases where the action contains a word that is included in the dictionary, will have a score equal to 1. In these cases, then, the results are equivalent to the previous approach: all the actions scoring 1, will include a word of the category, as in the sentence: 'Establish a mobile health clinic program to provide accessible nutrition education, health screenings, and mental health support for **homeless** individuals and families in temporary accommodations.'

However, the following example show the benefits of the embedding approoach: 

'Implement a subsidized, nutrient-dense school meal program sourced from local farmers to combat malnutrition and support vulnerable children from low-income families.'

In this case, the action contains no dictionary word, but the similarity is still high (0.685364). Thus, this approach allow us to evaluate the match between the action and each category, even when no word of the category is contained in the action. 

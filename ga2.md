# Review of citizen participation - Global Assemblies 2025

## 1. Data description

+ People Powered: (102 cases) <br>
https://www.peoplepowered.org/

+ Participedia:  (2327 cases) <br>
https://participedia.net/

+ Participatory budgeting (USA): (698 cases) <br>
https://www.participatorybudgeting.org/team/ 

+ OIDP, The International Observatory on Participatory Democracy: (359 cases) <br>
https://www.oidp.net/en/

+ OECD Deliberative Democracy Database (2023): (733 cases) <br>
https://airtable.com/appYFGuoyUMU4SaHK/tblrttW98WGpdnX3Y/viwX5ZutDDGdDMEep?blocks=hide

+ OPSI, Observatory of Public Sector Innovation: (380 cases) <br>
https://oecd-opsi.org/

+ LATINNO, Innovaciones para la Democracia en América Latina: (3746 cases) <br>
https://latinno.net/es/

+ KNOCA: The Knowledge Network on Climate Assemblies: (187 cases) <br>
https://www.knoca.eu/

+ Community Engagement Hub: (121 cases) <br>
https://communityengagementhub.org/

## Total : 8653 cases

|Proyect| Location | Date| Description |
|:--  |:-- |:-- |:--
People Powered | country and city | No | Only description|
OIDP  | only country | Year | Description and labels|
LATINNO |  country and city (missin data on cities) | Year | Description and labels | 
KNOCA  | country and city | Year** | Only description |
Community Engagement Hub  | country** | Year | Description and labels | 
Participedia  | country | Yes |Description and labels|
OCDE 2023  | country and city |Year | Description and labels |
OECD-OPSI  | country  | Year | Description and labels |
Participatory Budgeting USA  | USA | Year** | No| 

** imputed from other columns

## 2. Text analysis

### 2.1 Classifing dialogues about environment

First, we want to establish a subset of dialogues that focus on the environment. To do this, we have some initiatives with thematic tags, and we're using those to train a classifier.

Of the nine databases, six were used to train the classifier. These are: Participedia, Community Engagement Hub, KNOCA, LATINNO, OECD 2023, and OIDP. Except for KNOCA, all of these have categories, which were pre-selected to be assigned the label "environment=1". In the case of KNOCA, "environment=1" was assigned to all entries, as that is the purpose of the database. These are the categories that were assigned to the label "environment=1".

+ Participedia: 'agricultural', 'air', 'animal', 'carbon', 'climate', 'coal','disaster', 'ecohousing', 'energy', 'energy_efficiency', 'energy_siting', 'environmental', 'fisheries', 'food', 'food_inspection','geotechnology', 'maritime', 'natural', 'natural_resource', 'nuclear', 'recycling','rural', 'sustainable','waste', 'water','weather', 'wilderness'
+ Community Engagement Hub =  'CEA & Climate', 'CEA and Food Insecurity', 'Climate change', 'Community-led Climate Resilience',  'Disaster', 'Disaster reduction','Drought', 'Extreme weather','Flooding','Hurricanes','Tsunami','Typhoon'
+ LATINNO: 'Environment','Rural Development', 'Social Rights and Goods: Food Sovereignty'
+ OCDE 2023: 'Environment',  'Energy'
+ OIDP: 'environment and climate action','resilience and disaster management', 'SDG 2', 'SDG 6', 'SDG 7', 'SDG 13', 'SDG 14', 'SDG 15' 

From these six databases, we have a total of 7085 entries, of which 1363 are "environment=1". Therefore, to achieve a good balance, a random sample of 1363 entries with "environment=0" was taken. With the resulting 2726 entries, a seq-classification task was fine-tuned using a standard bert-base-uncased English model. An accuracy of 88% was achieved. The trained model was applied to 2 of the 3 remaining databases, OPSI and People Powered, with a total of 474 observations (excluding null values), to generate the binary variable "environment". In the case of Participatory Budget (698 observations), there are no descriptive texts that can be used for classification; therefore, the variable "environment" remains as NaN (null).


## 2.2 Extracting bigrams

Extracting content is particularly challenging when the texts lack a consistent format or length, as is the case here. Furthermore, the presence of tags eliminates the need for clustering to organize the information. Another challenge with this type of data is that summarization algorithms don't perform as well when applied to a set of short, independent texts (unlike a long paragraph).

A relatively quick option for extracting content is bigram search. Unlike individual words (tokens), bigrams are generally more informative, as they consider words along with their modifiers.


+ First, we looked for bigrams with a pattern of (i) adjective-noun or (ii) noun-noun. We kept those with 20 or more occurrences.
+ We divided the bigrams into 5 categories: (1) “delete”, for bigrams that lack grammatical logic, but also if they are too general, (2) “participation”, for everything related to how citizens participate, (3) “actor”, which includes different stakeholders and levels/scopes, (4) “targets”, intended for vulnerable groups, (5) “topic”, which are not necessarily the subject of the dialogue, but are something that could be discussed.
+ Note: one can also look for “tokens” (individual words) that were left out and that don't belong to any bigram. Interesting topics emerge, such as "corruption". However, it is complicated to mix them in visualizations with bigrams, because bigrams usually have lower frequencies, so the "tokens" eat up the visualization.

The following is an example of "topic" bigrams, taken from dialogues in South America:

<img width="349" height="512" alt="image" src="https://github.com/user-attachments/assets/0b8aaf76-5869-450f-971b-a5cf521d4acd" />


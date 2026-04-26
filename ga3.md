

Based on the recommendations generated at an Assembly, the purpose of this section is to identify those issues in our participation database.

# 1. Identifying dialogues

To do this, we first developed a list of keywords for each of the recommendations.

+ Theme 1: Food Systems and Agriculture <br>
keywords: food waste, farmers, land, seeds, food label, agroecological, pesticides, compost, biogas, consumer awareness

+ Theme 2: Environment, Climate and Nature<br>
keywords: forest, deforestation, livestock, agroforestry, habitat, conservation, food habit, consumption, food culture, beef, palm oil, cocoa, soy, consumer

+ Theme 3: Urban Life and the Built Environment<br>
keywords: transport*, zoning, rooftops, mobility*, urban*, green spaces, community garden, nature spaces, food market, fresh food

+ Theme 4: Society, Culture and Wellbeing*<br>
keywords: right, traditions, education, responsibility, access, legal

keywords marked with asteriks (including all keywords in Theme 4) are valid only when accompanied by "green" words, such as: planet, green, food, climate, energy, sustainable, sustainability, agriculture, environment, biodiversity, ecosystem, ecology

Now we search for keywords in the lemmatized texts, identifying all instances of participation whose description includes one of the keywords. The map shows the countries where at least 5 dialogues related to Theme 1 (Food Systems and Agriculture) took place.

<img width="853" height="347" alt="image" src="https://github.com/user-attachments/assets/4bd7339f-82e3-4209-a3dc-acc276cdf2cb" />

# 2. Analyzing the content

Let's continue with Theme 1 as an example. First, we use the descriptive texts from the dialogues that discussed Theme 1, as we saw in the previous section. Then, we extract keywords from bigrams, using either NOUN-NOUN or ADJ-NOUN patterns (using the lemmatized text).

Here we show in a WordCloud the results for the bigram extraction for Theme 1:

<img width="785" height="406" alt="image" src="https://github.com/user-attachments/assets/a5bef3be-d11e-4eaa-9677-01c90afbe4b6" />

# 3. Modifiers

Given that we have keywords, another strategy for analyzing content involves extracting modifiers and conjugates of those keywords from the texts. To do this, we use Stanza's dependency parser. See an example below:

Modifiers of "farmer" (subset, font size proportional to word frequency): 

<img width="273" height="325" alt="image" src="https://github.com/user-attachments/assets/1b0bd2ca-db9d-4937-a84a-9429d7e2f152" />

\

Modifers are nouns or adjectives that have dependedency to a given word, in this case, "farmer". So, the item "rural" in this list should be read as, for example: "rural farmer (s)" (we include the plural because the text was lemmatized).

Conjugates of "farmer" (subset):

<img width="704" height="176" alt="image" src="https://github.com/user-attachments/assets/5a12df1e-1824-43db-b5bc-d91ff3b7e8b6" />

\

Conjugated nouns are nouns that have the same hierarchy as the target word. They can be read with an "and" or a comma (",") in relation to the keyword. For example, the item "indigenous community" is contained in the sentence: "*The Council is made up of representatives of the President of the Republic and other entities and agencies of the National Government working on related issues, representatives of agricultural sector guilds and representatives of peasant organizations, black communities, indigenous communities, smallholder farmers, agronomist and veterinary societies.*" 



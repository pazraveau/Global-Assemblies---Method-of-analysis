

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

<img width="325" height="346" alt="image" src="https://github.com/user-attachments/assets/1a73fe46-5ed3-4d11-861e-82bd3fa3600e" />

Modifers are nouns or adjectives that have dependedency to a given word, in this case, "farmer". So, the item "rural" in this list should be read as, for example: "rural farmer (s)" (we include the plural because the text was lemmatized).

Conjugates of "farmer" (subset):


agronomist society
black community
business community
civil society
community leader
cooperative
employer
entrepreneur
family farmer
government agency
indigenous community
industry
information exchange
ngo
producer
public official
researcher
rural development
state institution
trade union
transporter
youth organization



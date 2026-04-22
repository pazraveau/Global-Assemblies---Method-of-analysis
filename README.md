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

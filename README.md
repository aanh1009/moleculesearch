**Molecule Semantic Search Engine**

A search engine using text embedding from NomicAI API to power the search of chemical compounds using text-based queries. The engine currently contains information of 50000 different compounds. 

Step-by-step breakdown: 

1. Early Data Analysis (EDA)

The original dataset (description_df.pkl) contains information of more than 300000 compounds in 43 columns. The purpose of the search engine is to embed the descriptions, compare the text-based query to them, and retrieve the name and basic information of relevant compounds. Therefore, in this stage, most columns that contain domain-specific information (like toxicity, space volume, etc.) will be excluded.

Other processes on the dataset include: excluding descriptions with more than 512 characters, deleting cases where the compound name is not a string, and data sampling (taking 50000 samples from the original dataset). 

Skills: pickle & pandas


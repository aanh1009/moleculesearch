**Molecule Semantic Search Engine**

A search engine using text embedding from NomicAI API to power the search of chemical compounds using text-based queries. The engine currently contains information of 50000 different compounds. 

Step-by-step breakdown: 

1. Early Data Analysis (EDA)

The original dataset (description_df.pkl) contains information of more than 300000 compounds in 43 columns. The purpose of the search engine is to embed the descriptions, compare the text-based query to them, and retrieve the name and basic information of relevant compounds. Therefore, in this stage, most columns that contain domain-specific information (like toxicity, space volume, etc.) will be excluded.

Other processes on the dataset include: excluding descriptions with more than 512 characters, deleting cases where the compound name is not a string, and data sampling (taking 50000 samples from the original dataset). All of these analyses are done using pandas and pickle

2. Nomic AI Setup & Data Upload

Next, I installed NomicAI and connected to my NomicAI API key:
```
pip install nomic
nomic login '[key]'
```

Following, I imported atlas from nomic then mapped the dataset to nomic using atlas:

```
data = atlas.map_data(df2, indexed_field='description')
```
Here, the data is 'df2', and the target for embedding and searching is 'description'. 

Finally, I created a map:
```
map = data.maps[0]
```

3. Search Function

When the setup was done, I started to write a function for searching molecules using a text-based query. The function first embeds the query, and then uses NomicAI's vector_search built-in function to search for k neighbors, i.e. k compounds having descriptions that match the query. Here, k is set to 10. 

Here, note that 'embed' only accepts list strings. Therefore the query has to be converted into a list in order not to incur an error.

```
def search(query):
    #embed the query with nomic.embed 
    query_vector = embed.text(
        texts = [query]
    ...
        )
```
4. Retrieving the search results

Finally, I created an input cell that stores outputs when a query is entered:

```
result = search(input('your description: '))
```

The result is displayed using the get_data method of AtlasDataset: 

```
dataset = AtlasDataset('irrational-avram') #name of the dataset i created to map the dataset previously
dataset.get_data(ids=result[0])
```

In the end, here's a snippet of what we get when we enter a query (I entered 'a stilbenoid' in this case):

![image](https://github.com/user-attachments/assets/37e3685d-7fb4-4a38-8f6b-4919d421c6ca)

###News Near Duplicate Detection

Overview

This repository demonstrates how to identify clusters of news articles that are highly similar. It fetches articles from a Trino database, computes character shingles for each article, builds MinHash signatures and uses locality sensitive hashing to find near duplicate pairs. The notebook loads articles within a configurable date range, optionally removes records with repeated source addresses, and writes out both pairwise similarity scores and groupings of similar articles. The goal is to help analysts detect redundant content in large news collections.


Data source and environment

The notebook expects a table called news.collected_news with columns url and text, and a column created_at containing creation dates. Connection details for Trino (host name, user name and catalog) are read from the environment. You can run the notebook locally or in a cloud notebook as long as those variables are set. The date range for analysis is controlled through the start_date and end_date variables near the top of the notebook. If either value is missing the code will use an open ended range. You can also choose whether to drop records that share the same source address.

Dependencies

The notebook uses Python with the trino, datasketch and pandas libraries.

Running the notebook

- Adjust start_date and end_date near the top of the notebook to choose a time interval of interest. Dates follow the year month day format. For example, 2025/09/01 to 2025/09/07 will limit the analysis to the first week of September twenty twenty five.
- Choose whether to remove duplicate addresses by setting remove_duplicate_urls to True or False.
- Configure the similarity threshold and shingle size in the section that creates the NearDuplicateDetector. Higher thresholds produce more conservative matches.

Outputs

After execution the notebook writes three JSON files into the working directory:
- duplicates_pairs.json contains a list of pairs of source addresses with a similarity score.
- duplicates_mapping.json maps each address to a list of previously seen addresses that are similar enough according to the threshold.
- duplicates_clusters.json groups addresses into clusters when they share a chain of similarity relations.

Customising the analysis

The NearDuplicateDetector class implements character shingling and MinHash locality sensitive hashing. You can change the size of the shingles, the number of hash permutations and the similarity threshold when instantiating the class.

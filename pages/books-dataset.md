---
layout: page
title: The books dataset
show_sidebar: false
toc: true
---

## About the dataset

The examples on this website use dataset with meta-information about 11,123 books. The dataset is based on [a dataset published on Kaggle by soumik](https://link.es24h.com/20c1) under a [CC0: Public Domain](https://link.es24h.com/dd41) license.

The dataset contains the following information for each book:
* `id`, a unique identifier
* `title`, the title
* `authors`, the author(s). If a book has been written by multiple authors, their names are separated by slashes (`/`)
* `average_rate`, the average rating
* `isbn`, the International Standard Book Number (ISBN)
* `language_code`, a code indicating the language in which the book has been written
* `num_pages`, the number of pages
* `publication_date`, the publication date
* `publisher`, the publisher

## Load the dataset

The dataset is available as a comma-separated values (CSV) file. Use the file upload feature of Kibana to load the file into Elasticsearch:

1. [Download](https://link.es24h.com/7c2a) the file and save it to disk.
1. Unzip the `books.csv.zip` file.
1. Open Kibana in a web browser and navigate to the main page.
1. Select **Upload a file**.
1. Select **Select or drag and drop a file**, and select the unzipped `books.csv` file.
1. Select **Import**.
1. Enter `books` for the index name and select **Import**.
1. Wait a few moments for Kibana to import the data.

To validate that the import was successful:

1. From the Kibana main menu, select **Discover**.
1. If not selected yet, select the `books` data view.
1. Discover should show you a list of 11,123 books.
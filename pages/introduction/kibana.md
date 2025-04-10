---
layout: page
title: "Kibana: the Elasticsearch UI"
menubar: content_menu
show_sidebar: false
toc: true
---

## What is Kibana

The [README in the Kibana GitHub repository](https://link.es24h.com/299e) describes Kibana as: *"a browser-based analytics and search dashboard for Elasticsearch."* Kibana is a user interface that enables you to query, explore, and visualize the data stored in Elasticsearch. But Kibana is more than that; it also serves as a frontend for managing the Elastic Stack: setting up security, data lifecycle policies, backups, alerting, and more.

## Accessing Kibana

Kibana provides a web interface that you can access with a web browser. By default, a locally run instance of Kibana can be accessed at <http://localhost:5601>. Fun fact: Kibana's port number, 5601, turned upside down resembles the word "logs," hinting at Kibana's origins as a log viewer.

Upon accessing <http://localhost:5601>, you are greeted by a login prompt. Log in using the `elastic` user and the `ELASTIC_PASSWORD` password you set in the Docker `.env` file.

The first time you log in, Kibana will prompt you to add any "integrations." You can skip this step by selecting **Explore on my own**, which will take you to the Kibana Home.

To navigate around Kibana, select the menu button in the top left to open the main menu. Feel free to click around to familiarize yourself with Kibana's features.

## Uploading data

Next, you'll use Kibana to load a data set.

{% include content/load-books-data.md context="guide" %}

## Discover

Discover is a user interface that lets you search and explore your data. It enables you to write queries, add filters, and inspect the resulting documents.

1. Ensure that the time filter in the top right is set to the last 150 years.
1. In the search bar at the top of the screen, type `tolstoy` and hit enter.
1. Below the search bar, Discover displays a list of all 28 matching documents.
1. You may notice that some results include the word `tolstoy` in the `title` field, while others match on the `authors` field. You can make your query more specific by searching within a specific field. Prepend your query with the field name, separated by a colon. For example: `title:tolstoy` or `authors:tolstoy`.
1. Select one of the results and click the double-headed arrow. This opens a pane with the entire document.

## Dashboards

Dashboards enable you to visualize your data. A dashboard consists of one or more panels. The most common type of panel is a data visualization, but a panel can also contain static text, an image, or an interactive control. By combining multiple panels onto one dashboard, you can get an overview of a dataset.

1. Select **Dashboard** from the main menu.
1. Select **Create a dashboard**.
1. Ensure that the time filter in the top right is set to the last 150 years.
1. Select **Create visualization**.
1. From the list of available fields, drag `language_code` onto the central workspace.
1. The workspace now displays a breakdown of the different languages, visualized as a vertical bar chart. You can change the visualization type in the top right. For example, try selecting **Pie** to create a pie chart instead.
1. Once you're satisfied with the visualization, select **Save and return** to add the visualization to the dashboard.
1. You can add another visualization to the dashboard by selecting **Create visualization**.
1. From the list of available fields, drag `publication_date` onto the central workspace.
1. Select **Save and return** to add this visualization to the dashboard.
1. You can adjust the size of each visualization and move it around on the dashboard.
1. Once you're satisfied with the dashboard, select **Save**.
1. Give the dashboard a name, for example `Books`, and select **Save**.
1. Select **Dashboard** from the main menu to return to the dashboard overview. You should see your saved dashboard and can open it by selecting it from the list.


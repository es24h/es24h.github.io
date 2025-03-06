---
layout: post
title: "Query of the month: match phrase"
date: 2025-03-06
tag: query of the month
summary: The `match` query is one of the most used queries in Elasticsearch, but its behavior with multi-word queries can be unexpected.
---

{: .highlight }
The Elasticsearch Query DSL provides over 50 different queries. To help you see the forest for the trees, every month, this blog series highlights a different query.

One of the most commonly used queries in Elasticsearch is the `match` query. However, its behavior with multi-word queries can sometimes be unexpected.

## How the match query handles multi-word searches

The `match` query doesn’t consider the order of the search terms. For example, if you're looking for a book about "poems and tales" in the [books dataset]({%link pages/books-dataset.md%}), using a `match` query, you could write this search request:

```json
GET books/_search
{
  "query": {
    "match": {
      "title": "poems and tales"
    }
  }
}
```

Out of the 1,668 hits, the top 3 results are:

1. `"Complete Tales and Poems"`
1. `"Essential Tales and Poems"`
1. `"Myths of the Norsemen: Retold from the Old Norse Poems and Tales"`

You might wonder: *Why are books with “Tales and Poems” in the title ranked higher than the exact match “Poems and Tales”*?

## Why the results are ranked this way

The reason is that the `match` query doesn’t care about the order of the search terms. It looks for documents that contain any of the individual terms—"poems," "and," and "tales"—regardless of the order in which they appear. A document's overall score is the sum of the individual scores for each term. So, the more terms that match, the higher the score, but the order of the terms doesn't matter. Whether "Poems" or "Tales" comes first in the title doesn't influence the score.

## Using the `match_phrase` query for exact phrases

If you do care about the order of the search terms, use the `match_phrase` query. This query only returns documents that contain the exact *phrase:* each of the search terms, in the exact order that they have been provided in the query. 

To use the `match_phrase` query, the earlier search request becomes:

```json
GET books/_search
{
  "query": {
    "match_phrase": {
      "title": "poems and tales"
    }
  }
}
```

This is much better. Now, `"Myths of the Norsemen: Retold from the Old Norse Poems and Tales"` is the only matching document because it contains the exact phrase "Poems and Tales" in the title. 

However, it introduces a new problem: Elasticsearch no longer returns any partially matching documents. In terms of information retrieval, *precision* has improved, but *recall* has decreased.

End users might still want to see documents that contain any of the search terms. Those documents should just be ranked lower. Ideally, a query returns the following results, ranked in this order:

1. All documents that contain the exact phrase.
1. All documents that contain all the search terms, even if they're not in the correct order.
1. All other documents that contain any (one or more) of the search terms.

You can achieve this by combining a `match_phrase` query with a `match` query. The idea is to return all documents that match one or more search terms, and boost those results that contain the exact phrase. This is something you can do with a `bool` query. The `bool` query accepts multiple other queries. First, put the `match` query in a `must` clause. This causes all documents that match one or more terms to be returned, ranked by score. Combine this with a `match_phrase` query in the `should` clause. An exact match on the phrase becomes optional, but if a document matches that clause, it scores higher. This is the resulting `bool` query:

```json
GET books/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "title": "poems and tales"
          }
        }
      ],
      "should": [
        {
          "match_phrase": {
            "title": "poems and tales"
          }
        }
      ]
    }
  }
}
```

Now, the top 3 results have the desired ranking, and recall is great since all 1,668 hits are returned:

1. `"Myths of the Norsemen: Retold from the Old Norse Poems and Tales"`
1. `"Complete Tales and Poems"`
1. `"Essential Tales and Poems"`

## Considerations when using the match phrase query

### Slop

By default, the `match_phrase` query doesn't allow other words to appear between search terms. You can change that behavior with the `slop` parameter.

```json
GET books/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "poems tales",
        "slop": 1
      }
    }
  }
}
```

This query allows for one word between "poems" and "tales."

Transposition (swapping the order of two words) requires a `slop` of 2. The following query for the phrase `"tales complete"` doesn't match `"complete tales"`:

```json
GET books/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "tales complete",
        "slop": 1
      }
    }
  }
}
```

However, if you increase the `slop` to 2, it will match:

```json
GET books/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "tales complete",
        "slop": 2
      }
    }
  }
}
```

### More control

For more control over the order and proximity of search terms, take a look at the [`intervals`](https://link.es24h.com/7ab6) and [`span`](https://link.es24h.com/c8a3) queries.

## Read more

* [Documentation for the match phrase query](https://link.es24h.com/3c37)
* [Documentation for the match query](https://link.es24h.com/05ba)
* [Documentation for the bool query](https://link.es24h.com/5776)
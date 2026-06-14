# NewsAPI GraphQL Schema

Conceptual GraphQL schema for the [NewsAPI](https://newsapi.org/) news aggregation REST API. NewsAPI provides access to breaking headlines and a searchable index of articles from thousands of news sources and blogs worldwide.

## Overview

The schema models three core capabilities of NewsAPI:

- **Top Headlines** — retrieve current breaking headlines, filtered by country, category, or source
- **Everything** — full-text search across all indexed articles with date, language, domain, and sort controls
- **Sources** — discover and filter the news sources registered in the NewsAPI index

## Schema Source

GraphQL schema file: `news-api-schema.graphql`

## Types

### Article Types

| Type | Description |
|---|---|
| `Article` | Top-level article node returned in results lists |
| `ArticleDetails` | Full detail block for a single article |
| `ArticleSource` | Embedded source attribution (id + name) within an article |
| `ArticleAuthor` | Author name and optional email |
| `ArticleContent` | Raw content string with truncation metadata |
| `ArticleUrl` | Canonical or alternate URL with rel attribute |
| `ArticleImage` | Image URL, dimensions, and alt text |
| `ArticlePublishedAt` | Publication timestamp in ISO 8601, Unix, and formatted forms |
| `ArticleHighlight` | Highlighted excerpt matching a search query |
| `ArticleSnippet` | Short text preview of article body |
| `RelatedArticles` | Articles related to a given article |

### Source Types

| Type | Description |
|---|---|
| `Source` | Top-level news source node |
| `SourceDetails` | Full detail block for a single source |
| `SourceCategory` | Category enum value with human-readable label |
| `SourceLanguage` | Language code with name |
| `SourceCountry` | Country code with name |
| `SourceUrl` | Source homepage URL with verification flag |

### Headline Types

| Type | Description |
|---|---|
| `Headline` | Single headline entry |
| `HeadlineDetails` | Rank and trending flag for a headline |
| `TopHeadlines` | Collection of top headlines for a query |

### Search / Everything Types

| Type | Description |
|---|---|
| `Everything` | Full article search results collection |
| `SearchQuery` | Structured representation of query parameters |
| `SearchFilter` | Typed filter criteria applied to a search |
| `SourceFilter` | Filter on specific source IDs or names |

### Trending / Topic Types

| Type | Description |
|---|---|
| `TrendingTopic` | A currently trending keyword or topic |
| `TopicDetails` | Detail block for a trending topic |
| `TopicCategory` | Category grouping for a topic |
| `Keyword` | A weighted keyword used in search or topic analysis |

### Filter Helper Types

| Type | Description |
|---|---|
| `DomainFilter` | Inclusion list of domains |
| `ExcludeDomains` | Exclusion list of domains |
| `DateRange` | Combined from/to date range |
| `DateStart` | Start boundary of a date range |
| `DateEnd` | End boundary of a date range |
| `Language` | Language code wrapper with name |

### Pagination and Meta Types

| Type | Description |
|---|---|
| `PageInfo` | Cursor-style pagination data (page, pageSize, totalPages, hasNext, hasPrev) |
| `ResultsMeta` | Metadata about a results set including timing |
| `NewsResults` | Generic news article result set wrapper |
| `SourceResults` | Source result set wrapper |

### API and Auth Types

| Type | Description |
|---|---|
| `APIKey` | API key credential with plan and rate-limit metadata |
| `Token` | Bearer token representation |
| `APIRequest` | Metadata about a request sent to the API |

### Response and Error Types

| Type | Description |
|---|---|
| `ResponseStatus` | Enum: `OK` or `ERROR` |
| `ResponseCode` | HTTP status code with code string and message |
| `ErrorResponse` | Structured error body returned by the API |

### Enums

| Enum | Description |
|---|---|
| `LanguageCode` | ISO 639-1 codes supported by NewsAPI (ar, de, en, es, fr, he, it, nl, no, pt, ru, sv, ud, zh) |
| `CountryCode` | ISO 3166-1 alpha-2 country codes supported by NewsAPI |
| `CategoryType` | Topic categories: BUSINESS, ENTERTAINMENT, GENERAL, HEALTH, SCIENCE, SPORTS, TECHNOLOGY |
| `SortBy` | Sort options: RELEVANCY, POPULARITY, PUBLISHED_AT |

## Queries

```graphql
type Query {
  everything(...): Everything
  topHeadlines(...): TopHeadlines
  sources(...): SourceResults
  source(id: String!): Source
  trendingTopics(...): [TrendingTopic!]
}
```

### `everything`

Maps to the NewsAPI `/v2/everything` endpoint. Accepts `q`, `searchIn`, `sources`, `domains`, `excludeDomains`, `from`, `to`, `language`, `sortBy`, `pageSize`, and `page` arguments.

### `topHeadlines`

Maps to the NewsAPI `/v2/top-headlines` endpoint. Accepts `country`, `category`, `sources`, `q`, `pageSize`, and `page` arguments.

### `sources`

Maps to the NewsAPI `/v2/top-headlines/sources` endpoint. Accepts `category`, `language`, and `country` arguments.

### `source`

Retrieves a single source by its NewsAPI source ID.

### `trendingTopics`

Conceptual query derived from clustering top-headline keywords. Accepts `country`, `category`, and `limit` arguments.

## References

- NewsAPI Documentation: https://newsapi.org/docs/
- NewsAPI Endpoints: https://newsapi.org/docs/endpoints
- OpenAPI: `openapi/news-api-openapi.yml`

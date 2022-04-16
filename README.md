# Lightship RFC

The RFC all implementations can follow to facilitate porting.

## Summary

- [1. Response](#1-response)
  - [1.1. Score](#11-score)
  - [1.2. RuleResult](#12-ruleresult)
- [2. Configuration file](#2-configuration-file)
  - [2.1. Domain](#21-domain)
  - [2.2. Route](#22-route)
  - [2.3. Query](#23-query)

## 1. Response

This is the top-level content in the response.

```ts
interface Response {
  url: string,
  durationInSeconds: number,
  scores: Score,
  seo: RuleResult[],
  security: RuleResult[]
}
```

Example:

```json
{
  "url": "https:\/\/example.com\/",
  "durationInSeconds": 0.48,
  "scores": {
    "seo": 100,
    "security": 50,
  },
  "seo": [
    {
      "name": "titlePresent",
      "passes": true
    },
    {
      "name: "langPresent",
      "passes": true,
    }
  ],
  "security": [
    {
      "name": "serverHeaderHidden",
      "passes": false
    },
    {
      "name:" "xFrameOptionHeaderPresent",
      "passes": true
    }
  ]
}
```

### 1.1 Score

Represents all the audits scores.

```ts
interface Score {
  seo: number,
  security: number
}
```

Example:

```json
{
  "seo": 100,
  "security": 75
}
```

### 1.2. RuleResult

Represents a rule and its result.

```ts
interface RuleResult {
  name: string,
  passes: boolean
}
```

## 2. Configuration file

Lightship implementations should be able to parse a configuration file to run the web crawler as an alternative to a code-driven execution.

```ts
interface Configuration {
  domains: Domain[],
  routes: Route[]
}
```

Example:

```json
{
  "domains": [
    {
      "base": "https://example.com",
      "routes": [
        {
          "path": "/"
        },
        {
          "path": "/register"
        }
      ]
    }
  ],
  "routes": [
    {
      "path": "https://news.google.com/topstories"
    }
  ]
}
```

### 2.1. Domain

Represent a base path to easily specify all the routes that share the same domain.

```ts
interface Domain {
  base: string,
  routes: Route[]
}
```

Example:

```json
{
  "base": "https://example.com",
  "routes": [
    {
      "path": "/",
    },
    {
      "path": "/register",
      "queries": [
        {
          "key": "utm-source",
          "value": "mailing"
        }
      ]
    }
  ]
}
```

### 2.2. Route

The route represents either a relative path to a domain, or an absolute path (with the base).

```ts
interface Route {
  path: string,
  queries?: Query[]
}
```

Example:

```json
{
  "path": "/account",
  "queries": [
    {
      "key": "theme",
      "value": "dark"
    }
  ]
}
```

### 2.3. Query

Represent a query string.

```ts
interface Query {
  key: string,
  value: string
}
```

Example:

```json
{
  "key": "lang",
  "value": "en"
}
```

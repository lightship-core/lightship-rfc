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
- [3. Rules](#3-rules)
  - [3.1. SEO](#31-seo)
    - [3.1.1. Title present](#311-title-present)
    - [3.1.2. Lang present](#312-lang-present)
  - [3.2. Security](#32-security)
    - [3.2.1. Server header hidden](#321-server-header-hidden)
    - [3.2.2. Strict-Transport-Security header present](#322-strict-transport-security-header-present)
    - [3.2.3. X-Frame-Options header present](#323-x-frame-options-header-present)
    - [3.2.4. X-Powered-By header hidden](#324-x-powered-by-header-hidden)

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

The default name cli-tools can seek for should be "lightship.json".

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

## 3. Rules

The list of rules classified by category.

### 3.1. Seo

#### 3.1.1. Title present

Passes if the HTML title tag is present and filled.

**Value**: 50

#### 3.1.2. Lang present

Passes if the lang attribute on the HTML html tag is present and filled.

**Value**: 50

### 3.2. Security

#### 3.2.1. Server header hidden

Passes if the "Server" header is not present or empty.

**Value**: 25

#### 3.2.2. Strict-Transport-Security header present

Passes if the "Strict-Transport-Security" header is present and have a valid value.

**Value**: 25

#### 3.2.3. X-Frame-Options header present

Passes if the "X-Frame-Options" header is present and have a valid value.

**Value**: 25

#### 3.2.4. X-Powered-By header hidden

Passes if the "X-Powered-By" header is not present or empty.

**Value**: 25

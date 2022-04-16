# Lightship RFC

The RFC all implementations can follow to facilitate porting.

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

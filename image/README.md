# CarBG Image APIs

CarBG Image APIs provide AI-powered image processing for automotive photography.

## Available APIs

### Replace Background

Replace the background of a car image using a CarBG podium/studio configuration.

**Endpoint:**

```http
POST /v1.0/car/image/replace-background
```

[View Replace Background API](replace-background/README.md)

## Authentication

All requests require:

```http
API-KEY: YOUR_API_KEY
```

## Request Format

```text
multipart/form-data
```

## Response Format

Successful image processing returns the generated image directly.

```http
Content-Type: image/jpeg
```

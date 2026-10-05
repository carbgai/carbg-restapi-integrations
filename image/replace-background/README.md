# Car Image Replace Background API

Replace the background of a car image using a CarBG podium/studio configuration.

The API processes the uploaded car image and returns the rendered studio image directly.

## Endpoint

### Recommended

```http
POST https://api.carbg.ai/v1.0/car/image/replace-background
```

### Legacy

```http
POST https://api.carbg.ai/v1/car/image/replace-background
```

## Authentication

Pass your CarBG API key using the `API-KEY` header.

```http
API-KEY: YOUR_API_KEY
```

## Request

### HTTP Method

```http
POST
```

### Content Type

```http
multipart/form-data
```

## Request Parameters

### Form Parameters

| Parameter | Type | Required | Default | Description |
|---|---|---:|---|---|
| `podium_id` | String / Integer | Yes | — | ID of the podium, studio, or background configuration. |
| `project_name` | String | No | Auto-generated | Project name. If omitted, a project name is automatically generated. |
| `team_code` | String | No | Default team code | Your team code. If omitted, the default team code is used. |
| `shadow` | String | No | `true` | Enable or disable shadow. Accepted values are `true` and `false`. |

### File Parameters

| Parameter | Type | Required | Description |
|---|---|---:|---|
| `source_image_file` | Image | Yes | Main car image to process. |
| `number_plate_file` | Image | No | Number plate image. |
| `banner_image_file` | Image | No | Banner image. |

## Example Request

```text
podium_id=115241
project_name=my-car-project
team_code=your_team_code
shadow=true
source_image_file=car.jpg
number_plate_file=number_plate.jpg
banner_image_file=banner.jpg
```

## Team Code

`team_code` is optional.

Use your team code:

```text
team_code=your_team_code
```

If `team_code` is omitted, the API uses the default team code.

## Shadow

The `shadow` parameter controls whether the rendered image should contain a shadow.

Accepted values:

```text
true
false
```

Default:

```text
true
```

## Response

### 200 — Success

When the image is successfully processed, the API returns the rendered studio image directly as a file.

### Response Media Type

```http
Content-Type: image/jpeg
```

### Response Body

The response body contains the binary JPEG image.

```text
200 OK
Content-Type: image/jpeg

<rendered JPEG image>
```

The client should save the response body as a `.jpg` file.

## Error Responses

For unsuccessful requests, the API may return an error response instead of an image.

Example:

```json
{
  "status": false,
  "message": "Missing API key"
}
```

## HTTP Status Codes

| Status | Description |
|---:|---|
| `200` | Image generated successfully |
| `400` | Invalid request |
| `401` | Missing or invalid API key |
| `404` | Resource not found |
| `422` | Invalid parameters |
| `500` | Internal server error |

## Important Client Behavior

Do not parse a successful `200` response as JSON.

Incorrect:

```python
response.json()
```

Correct:

```python
response.content
```

The successful response is binary image data.

## Code Examples

- [cURL](curl.md)
- [Python](python.md)
- [Node.js](nodejs.md)
- [Java](java.md)
- [Kotlin](kotlin.md)
- [.NET / C#](dotnet.md)
- [Swift](swift.md)

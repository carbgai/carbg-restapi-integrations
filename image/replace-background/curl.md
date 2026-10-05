# Car Image Replace Background — cURL

## Complete Request

```bash
curl --request POST \
  --url https://api.carbg.ai/v1.0/car/image/replace-background \
  --header "API-KEY: YOUR_API_KEY" \
  --form "podium_id=115241" \
  --form "project_name=my-car-project" \
  --form "team_code=your_team_code" \
  --form "shadow=true" \
  --form "source_image_file=@car.jpg" \
  --form "number_plate_file=@number_plate.jpg" \
  --form "banner_image_file=@banner.jpg" \
  --output result.jpg
```

The `--output result.jpg` option saves the successful API response directly as an image.

## Without Team Code

```bash
curl --request POST \
  --url https://api.carbg.ai/v1.0/car/image/replace-background \
  --header "API-KEY: YOUR_API_KEY" \
  --form "podium_id=115241" \
  --form "project_name=my-car-project" \
  --form "shadow=true" \
  --form "source_image_file=@car.jpg" \
  --output result.jpg
```

If `team_code` is omitted, the default team code is used.

## Only Required Parameters

```bash
curl --request POST \
  --url https://api.carbg.ai/v1.0/car/image/replace-background \
  --header "API-KEY: YOUR_API_KEY" \
  --form "podium_id=115241" \
  --form "source_image_file=@car.jpg" \
  --output result.jpg
```

## Successful Response

```http
HTTP/1.1 200 OK
Content-Type: image/jpeg
```

The generated image is saved to:

```text
result.jpg
```

## Error Response

For debugging, omit `--output` so an error body can be displayed:

```bash
curl --request POST \
  --url https://api.carbg.ai/v1.0/car/image/replace-background \
  --header "API-KEY: YOUR_API_KEY" \
  --form "podium_id=115241" \
  --form "source_image_file=@car.jpg"
```

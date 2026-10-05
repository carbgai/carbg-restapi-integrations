# Car Image Replace Background — Python

Install the required package:

```bash
pip install requests
```

## Complete Example

```python
import requests

API_URL = (
    "https://api.carbg.ai/v1.0/"
    "car/image/replace-background"
)

API_KEY = "YOUR_API_KEY"

headers = {
    "API-KEY": API_KEY
}

data = {
    "podium_id": "115241",
    "project_name": "my-car-project",
    "team_code": "your_team_code",
    "shadow": "true"
}

with open("car.jpg", "rb") as source_image,      open("number_plate.jpg", "rb") as number_plate,      open("banner.jpg", "rb") as banner:

    files = {
        "source_image_file": (
            "car.jpg",
            source_image,
            "image/jpeg"
        ),
        "number_plate_file": (
            "number_plate.jpg",
            number_plate,
            "image/jpeg"
        ),
        "banner_image_file": (
            "banner.jpg",
            banner,
            "image/jpeg"
        )
    }

    response = requests.post(
        API_URL,
        headers=headers,
        data=data,
        files=files
    )

if response.status_code == 200:
    with open("result.jpg", "wb") as output:
        output.write(response.content)

    print("Image saved as result.jpg")
else:
    print("Status:", response.status_code)
    print("Error:", response.text)
```

## Without Team Code

```python
data = {
    "podium_id": "115241",
    "project_name": "my-car-project",
    "shadow": "true"
}
```

If `team_code` is omitted, the API uses the default team code.

## Only Required Parameters

```python
import requests

url = (
    "https://api.carbg.ai/v1.0/"
    "car/image/replace-background"
)

headers = {
    "API-KEY": "YOUR_API_KEY"
}

data = {
    "podium_id": "115241"
}

with open("car.jpg", "rb") as image:
    files = {
        "source_image_file": (
            "car.jpg",
            image,
            "image/jpeg"
        )
    }

    response = requests.post(
        url,
        headers=headers,
        data=data,
        files=files
    )

if response.status_code == 200:
    with open("result.jpg", "wb") as output:
        output.write(response.content)

    print("Image saved as result.jpg")
else:
    print(response.status_code)
    print(response.text)
```

## Important

For a successful response, do not use:

```python
response.json()
```

Use:

```python
response.content
```

because the successful response is a JPEG image.

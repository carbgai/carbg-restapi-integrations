# Car Image Replace Background — Node.js

Install dependencies:

```bash
npm install axios form-data
```

## Complete Example

```javascript
const fs = require("fs");
const axios = require("axios");
const FormData = require("form-data");

const API_URL =
  "https://api.carbg.ai/v1.0/car/image/replace-background";

const API_KEY = "YOUR_API_KEY";

async function replaceBackground() {
  try {
    const form = new FormData();

    form.append("podium_id", "115241");
    form.append("project_name", "my-car-project");
    form.append("team_code", "your_team_code");
    form.append("shadow", "true");

    form.append(
      "source_image_file",
      fs.createReadStream("car.jpg")
    );

    form.append(
      "number_plate_file",
      fs.createReadStream("number_plate.jpg")
    );

    form.append(
      "banner_image_file",
      fs.createReadStream("banner.jpg")
    );

    const response = await axios.post(
      API_URL,
      form,
      {
        headers: {
          "API-KEY": API_KEY,
          ...form.getHeaders()
        },
        responseType: "arraybuffer",
        maxContentLength: Infinity,
        maxBodyLength: Infinity
      }
    );

    fs.writeFileSync("result.jpg", response.data);

    console.log("Image saved as result.jpg");

  } catch (error) {
    if (error.response) {
      console.error("Status:", error.response.status);
      console.error(
        "Response:",
        Buffer.from(error.response.data).toString("utf8")
      );
    } else {
      console.error("Error:", error.message);
    }
  }
}

replaceBackground();
```

## Without Team Code

Remove:

```javascript
form.append(
  "team_code",
  "your_team_code"
);
```

The API will use the default team code.

## Important

Use:

```javascript
responseType: "arraybuffer"
```

because the successful API response is binary JPEG data.

The generated image is saved as:

```text
result.jpg
```

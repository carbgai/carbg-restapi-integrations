# Car Image Replace Background — Swift

This example uses `URLSession` and multipart form-data.

## Complete Example

```swift
import Foundation

let url = URL(
    string:
        "https://api.carbg.ai/v1.0/" +
        "car/image/replace-background"
)!

let apiKey = "YOUR_API_KEY"
let boundary = "Boundary-\(UUID().uuidString)"

var request = URLRequest(url: url)

request.httpMethod = "POST"

request.setValue(
    apiKey,
    forHTTPHeaderField: "API-KEY"
)

request.setValue(
    "multipart/form-data; boundary=\(boundary)",
    forHTTPHeaderField: "Content-Type"
)

var body = Data()

func appendTextField(
    name: String,
    value: String
) {
    body.append(
        "--\(boundary)\r\n"
            .data(using: .utf8)!
    )

    body.append(
        "Content-Disposition: form-data; " +
        "name=\"\(name)\"\r\n\r\n"
            .data(using: .utf8)!
    )

    body.append(
        "\(value)\r\n"
            .data(using: .utf8)!
    )
}

func appendFile(
    fieldName: String,
    fileName: String,
    mimeType: String,
    data: Data
) {
    body.append(
        "--\(boundary)\r\n"
            .data(using: .utf8)!
    )

    body.append(
        "Content-Disposition: form-data; " +
        "name=\"\(fieldName)\"; " +
        "filename=\"\(fileName)\"\r\n"
            .data(using: .utf8)!
    )

    body.append(
        "Content-Type: \(mimeType)\r\n\r\n"
            .data(using: .utf8)!
    )

    body.append(data)

    body.append(
        "\r\n"
            .data(using: .utf8)!
    )
}

appendTextField(
    name: "podium_id",
    value: "115241"
)

appendTextField(
    name: "project_name",
    value: "my-car-project"
)

appendTextField(
    name: "team_code",
    value: "your_team_code"
)

appendTextField(
    name: "shadow",
    value: "true"
)

do {
    let imageData = try Data(
        contentsOf: URL(fileURLWithPath: "car.jpg")
    )

    appendFile(
        fieldName: "source_image_file",
        fileName: "car.jpg",
        mimeType: "image/jpeg",
        data: imageData
    )

    let numberPlateData = try Data(
        contentsOf: URL(
            fileURLWithPath: "number_plate.jpg"
        )
    )

    appendFile(
        fieldName: "number_plate_file",
        fileName: "number_plate.jpg",
        mimeType: "image/jpeg",
        data: numberPlateData
    )

    let bannerData = try Data(
        contentsOf: URL(
            fileURLWithPath: "banner.jpg"
        )
    )

    appendFile(
        fieldName: "banner_image_file",
        fileName: "banner.jpg",
        mimeType: "image/jpeg",
        data: bannerData
    )

    body.append(
        "--\(boundary)--\r\n"
            .data(using: .utf8)!
    )

    request.httpBody = body

    let task = URLSession.shared.dataTask(
        with: request
    ) { data, response, error in

        if let error = error {
            print("Error: \(error)")
            return
        }

        guard let httpResponse =
            response as? HTTPURLResponse else {
            return
        }

        print("Status: \(httpResponse.statusCode)")

        if httpResponse.statusCode == 200 {
            guard let imageData = data else {
                print("No image data received")
                return
            }

            let outputURL = URL(
                fileURLWithPath: "result.jpg"
            )

            do {
                try imageData.write(to: outputURL)
                print("Image saved as result.jpg")
            } catch {
                print(
                    "Unable to save image: \(error)"
                )
            }
        } else {
            if let data = data {
                print(
                    String(
                        data: data,
                        encoding: .utf8
                    ) ?? ""
                )
            }
        }
    }

    task.resume()

} catch {
    print(
        "Unable to read image: \(error)"
    )
}
```

## Without Team Code

Remove:

```swift
appendTextField(
    name: "team_code",
    value: "your_team_code"
)
```

The API will use the default team code.

## Successful Response

The API returns:

```http
200 OK
Content-Type: image/jpeg
```

The binary response is saved as:

```text
result.jpg
```

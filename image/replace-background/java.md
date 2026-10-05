# Car Image Replace Background — Java

This example uses Java 11+ `HttpClient`.

## Complete Example

```java
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.UUID;

public class ReplaceBackground {

    private static final String API_URL =
        "https://api.carbg.ai/v1.0/" +
        "car/image/replace-background";

    private static final String API_KEY =
        "YOUR_API_KEY";

    public static void main(String[] args)
            throws Exception {

        String boundary =
            "----CarBG" + UUID.randomUUID();

        byte[] requestBody =
            createMultipartBody(boundary);

        HttpRequest request =
            HttpRequest.newBuilder()
                .uri(URI.create(API_URL))
                .header("API-KEY", API_KEY)
                .header(
                    "Content-Type",
                    "multipart/form-data; boundary=" + boundary
                )
                .POST(
                    HttpRequest.BodyPublishers.ofByteArray(
                        requestBody
                    )
                )
                .build();

        HttpClient client =
            HttpClient.newHttpClient();

        HttpResponse<byte[]> response =
            client.send(
                request,
                HttpResponse.BodyHandlers.ofByteArray()
            );

        System.out.println(
            "Status: " + response.statusCode()
        );

        if (response.statusCode() == 200) {
            Files.write(
                Path.of("result.jpg"),
                response.body()
            );

            System.out.println(
                "Image saved as result.jpg"
            );
        } else {
            System.out.println("Error:");
            System.out.println(
                new String(
                    response.body(),
                    StandardCharsets.UTF_8
                )
            );
        }
    }

    private static byte[] createMultipartBody(
            String boundary
    ) throws IOException {

        ByteArrayOutputStream output =
            new ByteArrayOutputStream();

        addField(
            output,
            boundary,
            "podium_id",
            "115241"
        );

        addField(
            output,
            boundary,
            "project_name",
            "my-car-project"
        );

        addField(
            output,
            boundary,
            "team_code",
            "your_team_code"
        );

        addField(
            output,
            boundary,
            "shadow",
            "true"
        );

        addFile(
            output,
            boundary,
            "source_image_file",
            "car.jpg",
            "image/jpeg"
        );

        addFile(
            output,
            boundary,
            "number_plate_file",
            "number_plate.jpg",
            "image/jpeg"
        );

        addFile(
            output,
            boundary,
            "banner_image_file",
            "banner.jpg",
            "image/jpeg"
        );

        output.write(
            ("--" + boundary + "--\r\n")
                .getBytes(StandardCharsets.UTF_8)
        );

        return output.toByteArray();
    }

    private static void addField(
            ByteArrayOutputStream output,
            String boundary,
            String name,
            String value
    ) throws IOException {

        String part =
            "--" + boundary + "\r\n" +
            "Content-Disposition: form-data; " +
            "name=\"" + name + "\"\r\n" +
            "\r\n" +
            value +
            "\r\n";

        output.write(
            part.getBytes(StandardCharsets.UTF_8)
        );
    }

    private static void addFile(
            ByteArrayOutputStream output,
            String boundary,
            String fieldName,
            String fileName,
            String contentType
    ) throws IOException {

        String header =
            "--" + boundary + "\r\n" +
            "Content-Disposition: form-data; " +
            "name=\"" + fieldName + "\"; " +
            "filename=\"" + fileName + "\"\r\n" +
            "Content-Type: " + contentType + "\r\n" +
            "\r\n";

        output.write(
            header.getBytes(StandardCharsets.UTF_8)
        );

        byte[] fileBytes =
            Files.readAllBytes(Path.of(fileName));

        output.write(fileBytes);

        output.write(
            "\r\n".getBytes(StandardCharsets.UTF_8)
        );
    }
}
```

## Compile

```bash
javac ReplaceBackground.java
```

## Run

```bash
java ReplaceBackground
```

The successful image is saved as:

```text
result.jpg
```

## Without Team Code

Remove the `addField()` call for `team_code`.

The API will use the default team code.

## Important

The Java client uses:

```java
HttpResponse<byte[]>
```

because the successful response is binary JPEG data.

# Car Image Replace Background — .NET / C#

This example uses `HttpClient` and `MultipartFormDataContent`.

## Complete Example

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        const string apiUrl =
            "https://api.carbg.ai/v1.0/" +
            "car/image/replace-background";

        const string apiKey =
            "YOUR_API_KEY";

        using var client = new HttpClient();
        using var form = new MultipartFormDataContent();

        form.Add(
            new StringContent("115241"),
            "podium_id"
        );

        form.Add(
            new StringContent("my-car-project"),
            "project_name"
        );

        form.Add(
            new StringContent("your_team_code"),
            "team_code"
        );

        form.Add(
            new StringContent("true"),
            "shadow"
        );

        await using var imageStream =
            File.OpenRead("car.jpg");

        using var imageContent =
            new StreamContent(imageStream);

        imageContent.Headers.ContentType =
            new MediaTypeHeaderValue("image/jpeg");

        form.Add(
            imageContent,
            "source_image_file",
            "car.jpg"
        );

        await using var numberPlateStream =
            File.OpenRead("number_plate.jpg");

        using var numberPlateContent =
            new StreamContent(numberPlateStream);

        numberPlateContent.Headers.ContentType =
            new MediaTypeHeaderValue("image/jpeg");

        form.Add(
            numberPlateContent,
            "number_plate_file",
            "number_plate.jpg"
        );

        await using var bannerStream =
            File.OpenRead("banner.jpg");

        using var bannerContent =
            new StreamContent(bannerStream);

        bannerContent.Headers.ContentType =
            new MediaTypeHeaderValue("image/jpeg");

        form.Add(
            bannerContent,
            "banner_image_file",
            "banner.jpg"
        );

        using var request =
            new HttpRequestMessage(
                HttpMethod.Post,
                apiUrl
            );

        request.Headers.Add("API-KEY", apiKey);
        request.Content = form;

        using var response =
            await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            byte[] imageBytes =
                await response.Content.ReadAsByteArrayAsync();

            await File.WriteAllBytesAsync(
                "result.jpg",
                imageBytes
            );

            Console.WriteLine(
                "Image saved as result.jpg"
            );
        }
        else
        {
            string error =
                await response.Content.ReadAsStringAsync();

            Console.WriteLine(
                $"Status: {(int)response.StatusCode}"
            );

            Console.WriteLine("Error:");
            Console.WriteLine(error);
        }
    }
}
```

## Run

```bash
dotnet run
```

The successful image is saved as:

```text
result.jpg
```

## Without Team Code

Remove:

```csharp
form.Add(
    new StringContent("your_team_code"),
    "team_code"
);
```

The API will use the default team code.

## Important

The successful response is binary image data.

Use:

```csharp
ReadAsByteArrayAsync()
```

rather than parsing the successful response as JSON.

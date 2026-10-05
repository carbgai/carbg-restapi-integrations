# Car Image Replace Background — Kotlin

This example uses OkHttp.

## Gradle Dependency

```kotlin
dependencies {
    implementation("com.squareup.okhttp3:okhttp:4.12.0")
}
```

## Complete Example

```kotlin
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.MultipartBody
import okhttp3.OkHttpClient
import okhttp3.Request
import okhttp3.RequestBody.Companion.asRequestBody
import java.io.File

fun main() {
    val client = OkHttpClient()

    val imageFile = File("car.jpg")
    val numberPlateFile = File("number_plate.jpg")
    val bannerFile = File("banner.jpg")

    val imageBody = imageFile.asRequestBody(
        "image/jpeg".toMediaType()
    )

    val numberPlateBody = numberPlateFile.asRequestBody(
        "image/jpeg".toMediaType()
    )

    val bannerBody = bannerFile.asRequestBody(
        "image/jpeg".toMediaType()
    )

    val requestBody = MultipartBody.Builder()
        .setType(MultipartBody.FORM)
        .addFormDataPart("podium_id", "115241")
        .addFormDataPart("project_name", "my-car-project")
        .addFormDataPart("team_code", "your_team_code")
        .addFormDataPart("shadow", "true")
        .addFormDataPart(
            "source_image_file",
            imageFile.name,
            imageBody
        )
        .addFormDataPart(
            "number_plate_file",
            numberPlateFile.name,
            numberPlateBody
        )
        .addFormDataPart(
            "banner_image_file",
            bannerFile.name,
            bannerBody
        )
        .build()

    val request = Request.Builder()
        .url(
            "https://api.carbg.ai/v1.0/" +
            "car/image/replace-background"
        )
        .addHeader("API-KEY", "YOUR_API_KEY")
        .post(requestBody)
        .build()

    client.newCall(request).execute().use { response ->
        println("Status: ${response.code}")

        if (response.isSuccessful) {
            val imageBytes = response.body!!.bytes()

            File("result.jpg").writeBytes(imageBytes)

            println("Image saved as result.jpg")
        } else {
            println("Error:")
            println(response.body?.string())
        }
    }
}
```

## Without Team Code

Remove:

```kotlin
.addFormDataPart(
    "team_code",
    "your_team_code"
)
```

The API will use the default team code.

## Important

The successful response is binary image data, so use:

```kotlin
response.body!!.bytes()
```

and save those bytes as:

```text
result.jpg
```

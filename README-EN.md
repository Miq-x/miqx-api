# Miq-x API V4

Japanese version [here](aaaa)

## What is this?
This is an API, Miq-x, for generating "quote-like" collage images.  
There are many similar APIs, but none are designed with such attention to detail as this one.  
With a variety of fonts, diverse parameters, precise line breaks, and an overwhelming generation speed,  
it boasts top-class performance in every aspect.

> [!IMPORTANT]
> This API is provided with kindness and trust ( ˙꒳​˙ )ིྀ  
> For requests such as character encoding issues, bugs, or font additions, please refer to the [Issues page](https://github.com/Miq-x/miqx-api-doc/issues).

> [!CAUTION]
> The following actions are prohibited:
>
> - Distributing the API key to others
> - Modifying parameters (name, mid, id, meta, stamp)
> - Intentionally generating fake quotes through the above (excluding development stage testing)
> - Intentionally sending malicious requests to the API
> - Malicious attacks on the API server
> - Disclosing the API server's IP address

## APIKey

Ask Maguro to create an API key for you (if you're lucky).

## Miq-x (meiku)

"{host}/make" endpoint (POST)

| Parameter  | Description                       | Required |
|------------|-----------------------------------|----------|
| `key`      | API Key                           | Yes      |
| `param`    | Parameter (e.g., "meiku mono")    | Yes      |
| `name`     | DisplayName                       | Yes      |
| `text`     | Text                              | Yes      |
| `id`       | Message ID                        | Yes      |
| `mid`      | MID                               | Yes      |
| `img`      | Icon image png/jpg (binary)       | Yes      |
| `meta`     | For drawing LINE emojis           | No       |
| `stamp`    | For drawing LINE stamps           | No       |

> [!WARNING]
> All values are strings.

### Drawing LINE Emojis

Convert the data included in the Meta data to a string and send it in the request.

```python
emojiData    = eval(msg.contentMetadata["REPLACE"])
param["meta"] = str(emojiData["sticon"]["resources"])
```

### Drawing a Single LINE Stamp

Convert the data included in the Meta data to a string and send it in the request.

```python
stamp_id       = msg.contentMetadata["STKID"]
stamp_pkg      = msg.contentMetadata["STKPKGID"]
param["stamp"] = f"{stamp_pkg}_{stamp_id}"
```

### Drawing Combined LINE Stamps

Convert the data included in the Meta data to a string and send it in the request.

```python
param["stamp"] = msg.contentMetadata["CSSTKID"]
```

### Drawing Animated Stamps/Emojis

If the stamp or emoji supports animation, "gif" will be returned in the response data.

```python
with open("res.gif", mode="wb") as f:
    f.write(base64.b64decode(res["gif"]))
```

> [!NOTE]
> The behavior for drawing gifs is as follows:
>
> - All frame intervals are adjusted to 10ms.
> - If there are longer frames in the stamp/emoji, it will wait until those are completed.

## Join (Smash Bros. Join)

"{host}/smash" endpoint (POST)

| Parameter  | Description                       | Required |
|------------|-----------------------------------|----------|
| `key`      | API Key                           | Yes      |
| `img`      | Icon image png/jpg (binary)       | Yes      |

> [!WARNING]
> All values are strings.

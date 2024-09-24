# Miq-x API V4

English version [here](https://github.com/Miq-x/miqx-api-doc/blob/main/README-EN.md)

## なにこれ
名言風コラ画を生成するAPI、Miq-x  
似たAPIはよく見ますが、こんなAPIに細部までこだわって設計されているのは他にはありません  
豊富なフォント、多彩なパラメーター、絶妙な改行処理、そして圧倒的な生成スピード。  
すべての面でトップクラスのパフォーマンスを誇ります  


> [!IMPORTANT]
> このAPIは信頼のもとに親切心で提供されているものです( ˙꒳​˙ )ིྀ
>
> 文字化け・バグ・フォント追加等の要望は[Issue](https://github.com/Miq-x/miqx-api-doc/issues)どうぞ

> [!CAUTION]
> 以下の行為を禁止します
>
> - apikeyを別の人に配布
> - パラメーターの改変(name, mid, id, meta, stamp)
> - 上記による意図的に偽のめいくの生成(開発段階の動作テストは除く)
> - APIへの意図的な不正なリクエスト
> - API鯖への悪意のある攻撃
> - API鯖のIPアドレスの公開

## APIKey

まぐろに頼んでAPIキーを作成してもらいます(運が良ければ)

## Miq-x (めいく)

"{host}/make"エンドポイント (POST)

| パラメーター | 説明                             | 必須 |
| ------------ | -------------------------------- | ---- |
| `key`        | API Key                          | 〇   |
| `param`      | パラメーター ("めいくmono" など) | 〇   |
| `name`       | DisplayName                      | 〇   |
| `text`       | Text                             | 〇   |
| `id`         | Message ID                       | 〇   |
| `mid`        | MID                              | 〇   |
| `img`        | アイコン画像 png/jpg (バイナリ)  | 〇   |
| `meta`       | LINE 絵文字描画用                | ×    |
| `stamp`      | LINE スタンプ描画用              | ×    |

> [!WARNING]
> valueは全てstringです

### LINE絵文字の描画

Metaデータに含まれるデータをstringに変換してリクエスト

```python
emojiData    = eval(msg.contentMetadata["REPLACE"])
param["meta"] = str(emojiData["sticon"]["resources"])
```

### LINEスタンプ(単体)の描画

Metaデータに含まれるデータをstringに変換してリクエスト

```python
stamp_id       = msg.contentMetadata["STKID"]
stamp_pkg      = msg.contentMetadata["STKPKGID"]
param["stamp"] = f"{stamp_pkg}_{stamp_id}"
```

### LINEスタンプ(組み合わせ)の描画

Metaデータに含まれるデータをstringに変換してリクエスト

```python
param["stamp"] = msg.contentMetadata["CSSTKID"]
```

### アニメーションスタンプ/絵文字の描画

スタンプや絵文字がアニメーションに対応している場合はレスポンスデータに"gif"が追加で返ってきます。

```python
with open("res.gif", mode="wb") as f:
    f.write(base64.b64decode(res["gif"]))
```

> [!NOTE]
> gifの描画の挙動は以下の通り
>
> - 全てのフレーム間隔を10msに補正
> - フレームが長いスタンプ/絵文字があればそれがおわるまで待機

## 参戦 (スマブラ参戦)

"{host}/smash"エンドポイント (POST)

| パラメーター | 説明                            | 必須 |
| ------------ | ------------------------------- | ---- |
| `key`        | API Key                         | 〇   |
| `img`        | アイコン画像 png/jpg (バイナリ) | 〇   |

> [!WARNING]
> valueは全てstringです

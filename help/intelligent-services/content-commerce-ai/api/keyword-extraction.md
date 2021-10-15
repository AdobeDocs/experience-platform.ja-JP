---
keywords: Experience Platform；はじめに；コンテンツ ai；コマース ai；コンテンツとコマース ai；キーワード抽出；キーワード抽出
solution: Experience Platform, Intelligent Services
title: コンテンツおよびコマース AI API でのキーワード抽出
topic-legacy: Developer guide
description: キーワード抽出サービスは、テキストドキュメントを指定すると、ドキュメントの対象を最も記述したキーワードまたはキーボードを自動的に抽出する。 キーワードを抽出するために、名前付きエンティティ認識 (NER) と未監視キーワード抽出アルゴリズムの組み合わせを使用する。
exl-id: 56a2da96-5056-4702-9110-a1dfec56f0dc
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1082'
ht-degree: 4%

---

# キーワード抽出

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。このドキュメントは変更される場合があります。

キーワード抽出サービスは、テキストドキュメントを指定すると、ドキュメントの対象を最も記述したキーワードまたはキーボードを自動的に抽出する。 キーワードを抽出するために、名前付きエンティティ認識 (NER) と未監視キーワード抽出アルゴリズムの組み合わせを使用する。

[!DNL Content and Commerce AI] が認識する名前付きエンティティを次の表に示します。

| エンティティ名 | 説明 |
| --- | --- |
| PERSON | 架空を含む人々。 |
| NORP | 国籍、宗教、政治団体。 |
| GPE | 国、市、州。 |
| LOC | 非 GPE の場所、山脈、水の体。 |
| FAC | 建物、空港、高速道路、橋など |
| 組織 | 会社、機関、機関など |
| 製品 | 物、車両、食品等 （サービスではありません。） |
| イベント | ハリケーン、戦い、戦争、スポーツイベントなど。 |
| WORK_OF_ART | 書名、歌等 |
| 法律 | 法律に作成された名前付きドキュメント。 |
| 言語 | 任意の名前付き言語。 |

>[!NOTE]
>
>PDFを処理する場合は、このドキュメントの [PDFキーワード抽出 ](#pdf-extraction) の手順に進んでください。 また、docx、ppt、amd xml などの追加のファイルタイプのサポートは、後日リリースされるように設定されています。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて、ドキュメントからキーワードを抽出します。

入力ファイルの簡略化された JSON:

```json
{
  "application-id": "1234",
  "language": "en",
  "content-type": "inline",
  "encoding": "utf-8",
  "threshold": 0.01,
  "top-N": 10,
  "custom": {
    "min-n": 2,
    "entity-types": ["PERSON"]
  },
  "data": [
    {
      "content-id": "abc123",
      "content": "But an influential faction on the ATP player council, which is chaired by Novak Djokovic, staged a rebellion against Kermodes regime in the spring, and he will leave the post on Dec 31"
    }
  ]
}
```

表示される入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを使用す [!DNL Sensei Content Framework] るかを決定します。リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 キーワード抽出サービスの場合、`analyzer_id` ID は次のとおりです。
>`Feature:cintel-ner:Service-1a35aefb0f0f4dc0a3b5262370ebc709`

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v1/predict \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "Content-Type: multipart/form-data" \
  -H "cache-control: no-cache,no-cache" \
  -H "x-api-key: {API_KEY}" \
  -F file="{
    \"application-id\": \"1234\", 
    \"language\": \"en\", 
    \"content-type\": \"inline\", 
    \"encoding\": \"utf-8\",
    \"threshold\": 0.01,
    \"top-N\": 10,
    \"custom\": {
        \"min-n\": 2,
        \"entity-types\": [\"PERSON\"]
      },
    \"data\": [{
      \"content-id\": \"abc123\", 
      \"content\": \"But an influential faction on the ATP player council, which is chaired by Novak Djokovic, staged a rebellion against Kermodes regime in the spring, and he will leave the post on Dec 31\"
      }]
    }" \
  -F 'contentAnalyzerRequests={
    "enable_diagnostics":"true",
    "requests":[{
         "analyzer_id": "Feature:cintel-ner:Service-1a35aefb0f0f4dc0a3b5262370ebc709",
         "parameters": {}
    }]
}'
```

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービス ID。 この ID は、どの [!DNL Sensei Content Frameworks] が使用されるかを決定します。 カスタムサービスの場合は、コンテンツ AI チームとコマース AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを持つ JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data` 配列の外部で指定されたグローバルパラメータより優先されます。 この表で説明する残りのプロパティは、`data` 内で上書きできます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトは `inline` です。 | ○ |
| `encoding` | 入力テキストのエンコード形式です。 `utf-8` または `utf-16` を指定できます。 このプロパティのデフォルトは `utf-8` です。 | × |
| `threshold` | スコアのしきい値 (0 ～ 1)。この値を超えると、結果が返されます。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は使用できません）。 値 `0` を使用して、すべての結果を返します。 `threshold` と組み合わせて使用した場合、返される結果の数は、どちらの制限値セットでも小さい方です。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 このプロパティを機能させるには、有効な JSON オブジェクトが必要です。 カスタムパラメータの詳細については、[ 付録 ](#appendix) を参照してください。 | × |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | キーワード抽出サービスで使用されるコンテンツ。 コンテンツは、生のテキスト（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツが S3 上のファイル (「s3-bucket」 content-type) の場合、署名済み URL を渡します。コンテンツがリクエスト本文の一部である場合、データ要素のリストには 1 つのオブジェクトのみ含める必要があります。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |

**応答**

正常な応答は、抽出されたキーワードを `response` 配列に含む JSON オブジェクトを返します。

```json
{
  "status": 200,
  "cas_responses": [
    {
      "status": 200,
      "analyzer_id": "Feature:cintel-ner:Service-1a35aefb0f0f4dc0a3b5262370ebc709",
      "content_id": "",
      "result": {
        "response_type": "feature",
        "response": [
          {
            "feature_value": [
              {
                "feature_value": "success",
                "feature_name": "status"
              },
              {
                "feature_name": "labels",
                "feature_value": [
                  {
                    "feature_name": "atp player",
                    "feature_value": [
                      {
                        "feature_value": "KEYWORD",
                        "feature_name": "type"
                      },
                      {
                        "feature_value": 0.007743432063478832,
                        "feature_name": "score"
                      }
                    ]
                  },
                  {
                    "feature_name": "Novak Djokovic",
                    "feature_value": [
                      {
                        "feature_name": "type",
                        "feature_value": "PERSON"
                      },
                      {
                        "feature_name": "score",
                        "feature_value": 0
                      }
                    ]
                  },
                  {
                    "feature_value": [
                      {
                        "feature_name": "type",
                        "feature_value": "KEYWORD"
                      },
                      {
                        "feature_value": 0.00899321792126428,
                        "feature_name": "score"
                      }
                    ],
                    "feature_name": "player council"
                  },
                  {
                    "feature_value": [
                      {
                        "feature_value": "KEYWORD",
                        "feature_name": "type"
                      },
                      {
                        "feature_value": 0.007743432063478832,
                        "feature_name": "score"
                      }
                    ],
                    "feature_name": "kermodes regime"
                  },
                  {
                    "feature_value": [
                      {
                        "feature_name": "type",
                        "feature_value": "KEYWORD"
                      },
                      {
                        "feature_name": "score",
                        "feature_value": 0.0006052376660884209
                      }
                    ],
                    "feature_name": "atp player council"
                  }
                ]
              }
            ],
            "feature_name": "abc123"
          }
        ]
      }
    }
  ],
  "error": []
}
```

## PDFキーワードの抽出 {#pdf-extraction}

キーワード抽出サービスはPDFをサポートしますが、PDFファイルに新しい AnalyzerID を使用し、ドキュメントの種類を「PDF」に変更する必要があります。 詳しくは、以下の例を参照してください。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて、PDFドキュメントからキーワードを抽出します。

>[!CAUTION]
>
>`analyzer_id` どれを使用す [!DNL Sensei Content Framework] るかを決定します。リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 PDFのキーワード抽出の場合、`analyzer_id` ID は次のようになります。
>`Feature:cintel-ner:Service-7a87cb57461345c280b62470920bcdc5`

```SHELL
curl -w'\n' -i -X POST https://sensei.adobe.io/services/v1/predict \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "Content-Type: multipart/form-data" \
  -H "cache-control: no-cache,no-cache" \
  -H "x-api-key: {API_KEY}" \
  -F file=@TestPDF.pdf \
  -F 'contentAnalyzerRequests={
    "enable_diagnostics":"true",
    "requests":[{
    "analyzer_id": "Feature:cintel-ner:Service-7a87cb57461345c280b62470920bcdc5",
    "parameters": {
      "application-id": "1234",
      "content-type": "file",
      "encoding": "pdf",
      "threshold": "0.01",
      "top-N": "0",
      "custom": {},
      "data": [{
        "content-id": "abc123",
        "content": "file",
        }]
      }
    }]
  }'
```

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービス ID。 この ID は、どの [!DNL Sensei Content Frameworks] が使用されるかを決定します。 カスタムサービスの場合は、コンテンツ AI チームとコマース AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを持つ JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data` 配列の外部で指定されたグローバルパラメータより優先されます。 この表で説明する残りのプロパティは、`data` 内で上書きできます。 | ○ |
| `language` | 入力の言語。 デフォルト値は `en`（英語）です。 | × |
| `content-type` | 入力コンテンツタイプを示すために使用します。 これは `file` に設定する必要があります。 | ○ |
| `encoding` | 入力のエンコード形式。 これは `pdf` に設定する必要があります。 後日、より多くのエンコーディングタイプがサポートされるように設定されます。 | ○ |
| `threshold` | スコアのしきい値 (0 ～ 1)。この値を超えると、結果が返されます。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は使用できません）。 値 `0` を使用して、すべての結果を返します。 `threshold` と組み合わせて使用した場合、返される結果の数は、どちらの制限値セットでも小さい方です。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 このプロパティを機能させるには、有効な JSON オブジェクトが必要です。 カスタムパラメータの詳細については、[ 付録 ](#appendix) を参照してください。 | × |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | これは `file` に設定する必要があります。 | ○ |

**応答**

正常な応答は、抽出されたキーワードを `response` 配列に含む JSON オブジェクトを返します。

```json
{
  "statusCode": 200,
  "body": {
    "type": "JSON",
    "matchType": "strict",
    "json": {
      "status": 200,
      "content_id": "161hw2.pdf",
      "cas_responses": [
        {
          "status": 200,
          "analyzer_id": "Feature:cintel-ner:Service-7a87cb57461345c280b62470920bcdc5",
          "content_id": "161hw2.pdf",
          "result": {
            "response_type": "feature",
            "response": [
              {
                "feature_value": [
                  {
                    "feature_name": "status",
                    "feature_value": "success"
                  },
                  {
                    "feature_value": [
                      {
                        "feature_name": "delbick",
                        "feature_value": [
                          {
                            "feature_name": "score",
                            "feature_value": 0.03673855028832046
                          },
                          {
                            "feature_name": "type",
                            "feature_value": "KEYWORD"
                          }
                        ]
                      },
                      {
                        "feature_name": "Ci",
                        "feature_value": [
                          {
                            "feature_name": "score",
                            "feature_value": 0
                          },
                          {
                            "feature_name": "type",
                            "feature_value": "PERSON"
                          }
                        ]
                      }
                    ],
                    "feature_name": "labels"
                  }
                ],
                "feature_name": "abc123"
              }
            ]
          }
        }
      ],
      "error": []
    }
  }
}
```

PDF抽出の使用に関する詳細とサンプルには、AEM Cloud Service との設定、デプロイ、統合の手順が含まれています。 [CCAIPDF抽出ワーカー github リポジトリ ](https://github.com/adobe/asset-compute-example-workers/tree/master/projects/worker-ccai-pdfextract) を参照してください。

## 付録 {#appendix}

次の表に、`custom` 内から利用できるパラメータを示します。

| 名前 | 説明 | 必須 |
| --- | --- | --- |
| `min-n` | キーワードに必要な単語の最小数。 | × |
| `entity-types` | 返すエンティティのタイプ。 このドキュメントの最初にある名前付きエンティティの認識テーブルを参照してください。 | × |

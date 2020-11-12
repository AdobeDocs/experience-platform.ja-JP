---
keywords: Experience Platform;getting started;content ai;commerce ai;content and commerce ai;keyword extraction;Keyword extraction
solution: Experience Platform
title: キーワード抽出
topic: Developer guide
description: キーワード抽出サービスは、テキストドキュメントを指定すると、ドキュメントの主題を最も記述したキーワードまたはキーパースを自動的に抽出します。 キーワードを抽出するために、名前付きエンティティ認識(NER)と監視されていないキーワード抽出アルゴリズムの組み合わせが使用されます。
translation-type: tm+mt
source-git-commit: e397711baf31092402de83b826887ebef16321eb
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 4%

---


# キーワード抽出

>[!NOTE]
>
>[!DNL Content and Commerce AI] はベータ版です。 このドキュメントは変更されることがあります。

キーワード抽出サービスは、テキストドキュメントを指定すると、ドキュメントの主題を最も記述したキーワードまたはキーパースを自動的に抽出します。 キーワードを抽出するために、名前付きエンティティ認識(NER)と監視されていないキーワード抽出アルゴリズムの組み合わせが使用されます。

で認識される名前付きエンティティ [!DNL Content and Commerce AI] を次の表に示します。

| エンティティ名 | 説明 |
| --- | --- |
| PERSON | 架空の人も含む。 |
| NORP | 国籍、宗教、政治団体。 |
| GPE | 国、市、州。 |
| LOC | GPE以外の場所、山岳地帯、水体。 |
| 顔面 | 建物、空港、高速道路、橋等 |
| ORG | 会社、機関、機関等 |
| 製品 | 物品、車両、食品等 （サービスではありません）。 |
| イベント | ハリケーン、戦闘、戦争、スポーツイベントなどと名付けた |
| WORK_OF_ART | 書名、歌名等 |
| LAW | ドキュメントを法にした。 |
| 言語 | 任意の名前付き言語。 |

>[!NOTE]
>
>PDFを処理する予定がある場合は、このドキュメント内の [PDFキーワードの抽出](#pdf-extraction) に関する説明に進んでください。 また、docx、ppt、amd xmlなどの追加ファイルタイプのサポートは、後でリリースされるように設定されています。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

以下のリクエストは、ペイロードで提供された入力パラメータに基づいてドキュメントからキーワードを抽出する。

入力ファイルの簡略化されたJSON:

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

以下に示す入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。 リクエストを行う前に、適切な情報があることを確認し `analyzer_id` てください。 キーワード抽出サービスの場合、 `analyzer_id` IDは：
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
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービスID。 このIDは、使用するIDを決定 [!DNL Sensei Content Frameworks] します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを持つJSONオブジェクトを含む配列。 この配列の一部として渡されたパラメータは、配列の外部で指定されたグローバルパラメータよりも優先され `data` ます。 次の表に示す残りのプロパティは、内で上書きでき `data`ます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトはで `inline`す。 | ○ |
| `encoding` | 入力テキストのエンコーディング形式です。 ORを指定でき `utf-8` ま `utf-16`す。 このプロパティのデフォルトはで `utf-8`す。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すに `0` は、この値を使用します。 このプロパティのデフォルトはで `0`す。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すに `0` は、この値を使用します。 と組み合わせて使用した場合、返される結果の数は、どちらの制限セットにも該当しない数になります。 `threshold`このプロパティのデフォルトはで `0`す。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 このプロパティを機能させるには、有効なJSONオブジェクトが必要です。 カスタムパラメータの詳細については、 [付録](#appendix) を参照してください。 | × |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | キーワード抽出サービスで使用されるコンテンツ。 コンテンツは生のテキスト（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツがS3上のファイル(「s3-bucket」 content-type)の場合、署名済みURLを渡します。 コンテンツがリクエスト本文の一部である場合、データ要素のリストには1つのオブジェクトしか含めないでください。 複数のオブジェクトが渡された場合は、最初のオブジェクトのみが処理されます。 | ○ |

**応答** 

正常に応答すると、抽出されたキーワードが `response` 配列に含まれるJSONオブジェクトが返されます。

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

## PDFキーワード抽出 {#pdf-extraction}

キーワード抽出サービスはPDFをサポートしますが、PDFファイルに新しいAnalyzerIDを使用し、ドキュメントの種類をPDFに変更する必要があります。 詳しくは、次の例を参照してください。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて、PDFドキュメントからキーワードを抽出します。

>[!CAUTION]
>
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。 リクエストを行う前に、適切な情報があることを確認し `analyzer_id` てください。 PDFキーワード抽出の場合、 `analyzer_id` IDは次のとおりです。
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
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービスID。 このIDは、使用するIDを決定 [!DNL Sensei Content Frameworks] します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを持つJSONオブジェクトを含む配列。 この配列の一部として渡されたパラメータは、配列の外部で指定されたグローバルパラメータよりも優先され `data` ます。 次の表に示す残りのプロパティは、内で上書きでき `data`ます。 | ○ |
| `language` | 入力の言語。 The default value is `en` (english). | × |
| `content-type` | 入力コンテンツタイプを示すために使用します。 これはに設定する必要があり `file`ます。 | ○ |
| `encoding` | 入力のエンコーディング形式です。 これはに設定する必要があり `pdf`ます。 後日、サポートされるように設定されるエンコーディングタイプが増えます。 | ○ |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すに `0` は、この値を使用します。 このプロパティのデフォルトはで `0`す。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すに `0` は、この値を使用します。 と組み合わせて使用した場合、返される結果の数は、どちらの制限セットにも該当しない数になります。 `threshold`このプロパティのデフォルトはで `0`す。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 このプロパティを機能させるには、有効なJSONオブジェクトが必要です。 カスタムパラメータの詳細については、 [付録](#appendix) を参照してください。 | × |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | これはに設定する必要があり `file`ます。 | ○ |

**応答** 

正常に応答すると、抽出されたキーワードが `response` 配列に含まれるJSONオブジェクトが返されます。

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

AEMクラウドサービスの設定、デプロイ、統合の方法に関する手順を含む、PDF抽出の使用に関する詳細と例を示します。 CCAI PDF抽出ワーカーgithubリポジトリにアクセスし [ます](https://github.com/adobe/asset-compute-example-workers/tree/master/projects/worker-ccai-pdfextract)。

## 付録 {#appendix}

次の表に、内から利用できるパラメータを示し `custom`ます。

| 名前 | 説明 | 必須 |
| --- | --- | --- |
| `min-n` | キーワードに必要な単語の最小数。 | × |
| `entity-types` | 返すエンティティのタイプ。 このドキュメントの先頭にある名前付きエンティティ認識テーブルを参照してください。 | × |
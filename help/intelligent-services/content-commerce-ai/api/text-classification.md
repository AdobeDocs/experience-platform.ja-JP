---
keywords: text classification;Text classification
solution: Experience Platform, Intelligent Services
title: テキスト分類APIエンドポイント
topic: Developer guide
description: テキスト分類サービスは、テキストフラグメントを指定した場合、1つ以上のラベルに分類できます。 分類は、単一のラベル、複数のラベル、階層のいずれかです。
translation-type: tm+mt
source-git-commit: de16ebddd8734f082f908f5b6016a1d3eadff04c
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 5%

---


# テキストの分類

>[!NOTE]
>
>コンテンツとコマースAIはベータ版です。 このドキュメントは変更されることがあります。

テキスト分類サービスは、テキストフラグメントを指定した場合、1つ以上のラベルに分類できます。 分類は、単一のラベル、複数のラベル、階層のいずれかです。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて、フラグメントのテキストを分類します。 以下に示す入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。 リクエストを行う前に、適切な情報があることを確認し `analyzer_id` てください。 本サービスのご利用を受けるには、コンテンツおよびコマースAIベータチームにお問い合わせ `analyzer_id` ください。

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
    \"data\": [{
      \"content-id\": \"abc123\", 
      \"content\": \"Server and Workstation Processors, Microcode Update is a self-extracting executable file containing the latest beta microcode updates (System Configuration Data) and software license agreement.\"
      }]
    }" \
  -F 'contentAnalyzerRequests={
    "enable_diagnostics":"true",
    "requests":[{
         "analyzer_id": "Feature:cintel-text-classifier:Service-38a4cc7b286449e6bc1977f59df01b47",
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
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトはで `inline`す。 | × |
| `encoding` | 入力テキストのエンコーディング形式です。 ORを指定でき `utf-8` ま `utf-16`す。 このプロパティのデフォルトはで `utf-8`す。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すに `0` は、この値を使用します。 このプロパティのデフォルトはで `0`す。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すに `0` は、この値を使用します。 と組み合わせて使用した場合、返される結果の数は、どちらの制限セットにも該当しない数になります。 `threshold`このプロパティのデフォルトはで `0`す。 | × |
| `custom` | 渡す任意のカスタムパラメーター。 このプロパティを機能させるには、有効なJSONオブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意のID。 この値が渡されない場合は、自動生成IDが割り当てられます。 | × |
| `content` | テキスト分類サービスで使用されるコンテンツ。 コンテンツは生のテキスト（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツがS3上のファイル(「s3-bucket」 content-type)の場合、署名済みURLを渡します。 | ○ |

**応答** 

成功した応答は、分類されたテキストを応答配列で返します。

```json
{
  "status": 200,
  "cas_responses": [
    {
      "status": 200,
      "analyzer_id": "Feature:cintel-text-classifier:Service-38a4cc7b286449e6bc1977f59df01b47",
      "content_id": "",
      "result": {
        "response_type": "feature",
        "response": [
          {
            "feature_name": "abc123",
            "feature_value": [
              {
                "feature_value": [
                  {
                    "feature_value": 0.6899315714836121,
                    "feature_name": "Embedded & IoT"
                  }
                ],
                "feature_name": "labels"
              },
              {
                "feature_name": "status",
                "feature_value": "success"
              }
            ]
          }
        ]
      }
    }
  ],
  "error": []
}
```

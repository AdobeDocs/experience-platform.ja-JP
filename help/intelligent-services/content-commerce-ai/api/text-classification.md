---
keywords: テキスト分類；テキスト分類
solution: Experience Platform, Intelligent Services
title: コンテンツとコマースAI APIのテキスト分類
topic-legacy: Developer guide
description: テキスト分類サービスは、テキストフラグメントを指定した場合、1つ以上のラベルに分類できます。 分類は、単一のラベル、複数のラベル、階層のいずれかです。
exl-id: f240519a-0d83-4309-91e4-4e48be7955a1
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 4%

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
>`analyzer_id` どの変数を使用 [!DNL Sensei Content Framework] するかを決定します。リクエストを行う前に、適切な`analyzer_id`があることを確認してください。 このサービスの`analyzer_id`を受け取るには、Content and Commerce AIベータチームにお問い合わせください。

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
| `analyzer_id` | リクエストの展開先の[!DNL Sensei]サービスID。 このIDは、[!DNL Sensei Content Frameworks]のうちどれを使用するかを決定します。 カスタムサービスの場合は、Content and Commerce AIチームに連絡して、カスタムIDを設定してください。 | ○ |
| `application-id` | 作成したアプリケーションのID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを持つJSONオブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data`配列の外部で指定されたグローバルパラメータよりも優先されます。 この表で説明する他のプロパティは、`data`内で上書きできます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力が要求本文の一部であるか、S3バケットの署名済みURLであるかを示すために使用されます。 このプロパティのデフォルトは`inline`です。 | × |
| `encoding` | 入力テキストのエンコーディング形式です。 `utf-8`または`utf-16`を指定できます。 このプロパティのデフォルトは`utf-8`です。 | × |
| `threshold` | スコア(0 ～ 1)のしきい値。この値を超えると結果を返す必要があります。 すべての結果を返すには、値`0`を使用します。 このプロパティのデフォルトは`0`です。 | × |
| `top-N` | 返す結果の数です（負の整数は指定できません）。 すべての結果を返すには、値`0`を使用します。 `threshold`と組み合わせて使用した場合、返される結果の数は、どちらの制限セットの中でも小さい方です。 このプロパティのデフォルトは`0`です。 | × |
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

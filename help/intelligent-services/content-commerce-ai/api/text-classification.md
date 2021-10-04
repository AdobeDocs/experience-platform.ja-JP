---
keywords: テキスト分類；テキスト分類
solution: Experience Platform, Intelligent Services
title: コンテンツおよびコマース AI API のテキスト分類
topic-legacy: Developer guide
description: テキスト分類サービスは、テキストフラグメントを指定すると、1 つ以上のラベルに分類できます。 分類は、単一ラベル、複数ラベル、階層のいずれかです。
exl-id: f240519a-0d83-4309-91e4-4e48be7955a1
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 5%

---

# テキストの分類

>[!NOTE]
>
>コンテンツおよびコマース AI はベータ版です。 このドキュメントは変更される場合があります。

テキスト分類サービスは、テキストフラグメントを指定すると、1 つ以上のラベルに分類できます。 分類は、単一ラベル、複数ラベル、階層のいずれかです。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで提供された入力パラメーターに基づいて、フラグメントのテキストを分類します。 表示される入力パラメーターの詳細については、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>`analyzer_id` どれを使用す [!DNL Sensei Content Framework] るかを決定します。リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 このサービスの `analyzer_id` を受け取るには、コンテンツおよびコマース AI ベータチームにお問い合わせください。

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
| `analyzer_id` | リクエストがデプロイされる [!DNL Sensei] サービス ID。 この ID は、どの [!DNL Sensei Content Frameworks] が使用されるかを決定します。 カスタムサービスの場合は、コンテンツ AI チームとコマース AI チームに連絡して、カスタム ID を設定してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを持つ JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメータは、`data` 配列の外部で指定されたグローバルパラメータより優先されます。 この表で説明する残りのプロパティは、`data` 内で上書きできます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力がリクエスト本文に含まれているか、S3 バケットの署名済み URL に含まれているかを示すために使用されます。 このプロパティのデフォルトは `inline` です。 | × |
| `encoding` | 入力テキストのエンコード形式です。 `utf-8` または `utf-16` を指定できます。 このプロパティのデフォルトは `utf-8` です。 | × |
| `threshold` | スコアのしきい値 (0 ～ 1)。この値を超えると、結果が返されます。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は使用できません）。 値 `0` を使用して、すべての結果を返します。 `threshold` と組み合わせて使用した場合、返される結果の数は、どちらの制限値セットでも小さい方です。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 このプロパティを機能させるには、有効な JSON オブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意の ID。 この値が渡されない場合は、自動生成された ID が割り当てられます。 | × |
| `content` | テキスト分類サービスで使用されるコンテンツ。 コンテンツは、生のテキスト（「inline」コンテンツタイプ）にすることができます。 <br> コンテンツが S3 上のファイル (「s3-bucket」 content-type) の場合、署名済み URL を渡します。 | ○ |

**応答**

正常な応答は、分類されたテキストを応答配列で返します。

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

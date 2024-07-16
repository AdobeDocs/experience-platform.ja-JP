---
keywords: テキストの分類；テキストの分類
solution: Experience Platform
title: コンテンツとCommerce AI API のテキストの分類
description: テキスト分類サービスは、テキストフラグメントを指定した場合、それを 1 つ以上のラベルに分類できます。 分類は、単一ラベル、複数ラベル、階層のいずれかにすることができます。
exl-id: f240519a-0d83-4309-91e4-4e48be7955a1
source-git-commit: b124ed97da8bde2a7fc4f10d350c81a47e096f29
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 5%

---

# テキストの分類

>[!NOTE]
>
>コンテンツとCommerce AI はベータ版です。 ドキュメントは変更される場合があります。

テキスト分類サービスは、テキストフラグメントを指定した場合、それを 1 つ以上のラベルに分類できます。 分類は、単一ラベル、複数ラベル、階層のいずれかにすることができます。

**API 形式**

```http
POST /services/v1/predict
```

**リクエスト**

次のリクエストは、ペイロードで指定された入力パラメーターに基づいて、フラグメントからテキストを分類します。 表示される入力パラメーターについて詳しくは、ペイロード例の下の表を参照してください。

>[!CAUTION]
>
>使用 `analyzer_id` る [!DNL Sensei Content Framework] を指定します。 リクエストを行う前に、適切な `analyzer_id` を持っていることを確認してください。 このサービスの `analyzer_id` ールを受け取るには、コンテンツおよびCommerce AI ベータ版チームにお問い合わせください。

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
| `analyzer_id` | リクエストがデプロイされている [!DNL Sensei] サービス ID。 この ID によって、使用される [!DNL Sensei Content Frameworks] が決まります。 カスタムサービスについては、コンテンツおよびCommerce AI チームに連絡して、カスタム ID の設定を依頼してください。 | ○ |
| `application-id` | 作成したアプリケーションの ID。 | ○ |
| `data` | ドキュメントを表す配列内の各オブジェクトを含む JSON オブジェクトを含む配列。 この配列の一部として渡されたパラメーターは、`data` 配列の外部で指定されたグローバルパラメーターよりも優先されます。 この表で説明している残りのプロパティは、`data` 内から上書きできます。 | ○ |
| `language` | 入力テキストの言語。 デフォルト値は `en` です。 | × |
| `content-type` | 入力がリクエスト本文の一部であるか、S3 バケットの署名済み URL であるかを示すために使用されます。 このプロパティのデフォルトは `inline` です。 | × |
| `encoding` | 入力テキストのエンコード形式。 `utf-8` または `utf-16` を指定できます。 このプロパティのデフォルトは `utf-8` です。 | × |
| `threshold` | 結果を返す必要があるしきい値のスコア（0 ～ 1）。 値 `0` を使用して、すべての結果を返します。 このプロパティのデフォルトは `0` です。 | × |
| `top-N` | 返される結果の数（負の整数は不可）。 値 `0` を使用して、すべての結果を返します。 `threshold` と一緒に使用すると、返される結果の数は、どちらの制限セットよりも小さくなります。 このプロパティのデフォルトは `0` です。 | × |
| `custom` | 渡すカスタムパラメーター。 このプロパティを機能させるには、有効な JSON オブジェクトが必要です。 | × |
| `content-id` | 応答で返されるデータ要素の一意の ID。 これが渡されない場合、自動生成された ID が割り当てられます。 | × |
| `content` | テキスト分類サービスで使用されるコンテンツ。 コンテンツは、生のテキスト（「inline」 content-type）にすることができます。 <br> コンテンツが S3 上のファイル（「s3-bucket」 content-type）の場合は、署名済み URL を渡します。 | ○ |

**応答**

応答が成功すると、分類されたテキストが応答配列に返されます。

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

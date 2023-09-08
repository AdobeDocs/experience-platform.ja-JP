---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、オーディエンステンプレートを作成するために使用される API 呼び出しの例を示します。
title: オーディエンステンプレートの作成
source-git-commit: 3f31a54c0cf329d374808dacce3fac597a72aa11
workflow-type: ht
source-wordcount: '624'
ht-degree: 100%

---


# オーディエンステンプレートの作成

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/audience-templates`

Destination SDK を使用して作成した一部の宛先では、オーディエンスメタデータ設定を作成して、宛先のオーディエンスメタデータをプログラムで作成、更新または削除する必要があります。このページでは、`/authoring/audience-templates` API エンドポイントを使用した設定の作成方法を示します。

このエンドポイントを通じて設定できる機能について詳しくは、[オーディエンスメタデータ管理](../functionality/audience-metadata-management.md)を参照してください。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## オーディエンステンプレート API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## オーディエンステンプレートの作成 {#create}

`/authoring/audience-templates` エンドポイントに `POST` リクエストを行うことで、新しいオーディエンステンプレートを作成できます。

**API 形式**

```http
POST /authoring/audience-templates
```

+++リクエスト

以下のリクエストは、ペイロードで提供されるパラメーター設定に基づいて、新しいオーディエンステンプレートを作成します。以下のペイロードには、`/authoring/audience-templates` エンドポイントで使用可能なすべてのパラメーターが含まれます。呼び出しにすべてのパラメーターを追加する必要はなく、テンプレートは API 要件に応じてカスタマイズできることに注意してください。

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/audience-templates \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "metadataTemplate":{
      "name":"string",
      "create":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "update":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "delete":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "validate":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      },
      "notify":{
         "url":"string",
         "httpMethod":"string",
         "headers":[
            {
               "header":"string",
               "value":"string"
            }
         ],
         "requestBody":{
            
         },
         "responseFields":[
            {
               "name":"string",
               "value":"string"
            }
         ],
         "responseErrorFields":[
            {
               "name":"string",
               "value":"string"
            }
         ]
      }
   },
   "validations":[
      {
         "field":"string",
         "regex":"string"
      }
   ]
}'
```

| プロパティ | タイプ | 説明 |
| -------- | ----------- | ----------- |
| `name` | 文字列 | 宛先のオーディエンスメタデータテンプレートの名前。この名前は、Adobe Experience Platform のユーザーインターフェイスであらゆるパートナー固有のエラーメッセージに表示され、その後に `metadataTemplate.create.errorSchemaMap` から解析されたエラーメッセージが続きます。 |
| `url` | 文字列 | API の URL とエンドポイント。プラットフォームでオーディエンスを作成、更新、削除、検証するために使用します。`https://adsapi.snapchat.com/v1/adaccounts/{{customerData.accountId}}/segments` および `https://api.linkedin.com/v2/dmpSegments/{{segment.alias}}` は 2 つの業界の例です。 |
| `httpMethod` | 文字列 | 宛先のオーディエンスをプログラムで作成、更新、削除、検証するためにエンドポイントで使用されるメソッド。例：`POST`、`PUT`、`DELETE` |
| `headers.header` | 文字列 | API への呼び出しに追加する HTTP ヘッダーを指定します。例：`"Content-Type"` |
| `headers.value` | 文字列 | API への呼び出しに追加する HTTP ヘッダーの値を指定します。例：`"application/x-www-form-urlencoded"` |
| `requestBody` | 文字列 | API に送信するメッセージ本文のコンテンツを指定します。`requestBody` オブジェクトに追加する必要があるパラメーターは、API が受け入れるフィールドに応じて異なります。例については、オーディエンスメタデータ機能ドキュメントの[最初のテンプレートの例](../functionality/audience-metadata-management.md#example-1)を参照してください。 |
| `responseFields.name` | 文字列 | 呼び出し時に API が返す応答フィールドを指定します。例については、オーディエンスメタデータ機能ドキュメントの[テンプレートの例](../functionality/audience-metadata-management.md#examples)を参照してください。 |
| `responseFields.value` | 文字列 | 呼び出し時に API が返す応答フィールドの値を指定します。 |
| `responseErrorFields.name` | 文字列 | 呼び出し時に API が返す応答フィールドを指定します。例については、オーディエンスメタデータ機能ドキュメントの[テンプレートの例](../functionality/audience-metadata-management.md#examples)を参照してください。 |
| `responseErrorFields.value` | 文字列 | API 呼び出しに対する宛先からの応答で返されたエラーメッセージを解析します。これらのエラーメッセージは、Adobe Experience Platform のユーザーインターフェイスでユーザーに表示されます。 |
| `validations.field` | 文字列 | 宛先に対する API 呼び出しが実行される前に、いずれかのフィールドに対して検証を実行する必要があるかどうかを示します。例えば、ユーザーのアカウント ID の検証に `{{validations.accountId}}` を使用できます。 |
| `validations.regex` | 文字列 | 検証に合格するために、フィールドをどのように構成するべきかを示します。 |

{style="table-layout:auto"}

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、新しく作成されたオーディエンステンプレートと共に返されます。

+++

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、オーディエンステンプレートを使用するタイミングと、`/authoring/audience-templates` API エンドポイントを使用してオーディエンステンプレートを設定する方法について確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。

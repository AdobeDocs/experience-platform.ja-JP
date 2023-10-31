---
description: このページでは、Adobe Experience Platform Destination SDK を通じて、オーディエンステンプレートを更新するために使用される API 呼び出しの例を示します。
title: オーディエンステンプレートの更新
exl-id: 8185a015-256d-46a7-af33-8475832fb6c1
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 100%

---

# オーディエンステンプレートの更新

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/audience-templates`

このページでは、`/authoring/audience-templates` API エンドポイントを使用して、オーディエンステンプレートを更新するために使用できる API リクエストおよびペイロードの例を示します。

このエンドポイントを通じて設定できる機能について詳しくは、[オーディエンスメタデータ管理](../functionality/audience-metadata-management.md)を参照してください。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## オーディエンステンプレート API 操作の概要 {#get-started}

続行する前に、「[はじめる前に](../getting-started.md)」を参照し、API の呼び出しを正常に行うために必要となる重要な情報（必要な宛先オーサリング権限および必要なヘッダーの取得方法など）を確認してください。

## オーディエンステンプレートの更新 {#create}

更新されたペイロードで `/authoring/audience-templates` エンドポイントに `PUT` リクエストを行うことで、[既存の](create-audience-template.md)オーディエンステンプレートを更新できます。

既存のオーディエンステンプレートおよびその関連する `{INSTANCE_ID}` を取得するには、[オーディエンステンプレートの取得](retrieve-audience-template.md)に関する記事を参照してください。

**API 形式**

```http
PUT /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新するオーディエンステンプレートの ID。既存のオーディエンステンプレートおよびその関連する `{INSTANCE_ID}` を取得するには、[オーディエンステンプレートの取得](retrieve-audience-template.md)を参照してください。 |

以下のリクエストは、ペイロードで提供されるパラメーター設定に基づいて、既存のオーディエンスメタデータテンプレートを更新します。

+++リクエスト

```shell
curl -X PUT https://platform.adobe.io/data/core/activation/authoring/audience-templates/{INSTANCE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "metadataTemplate":{
      "create":{
         "url":"https://api.moviestar.com/v1.0/{{customerData.accountId}}/customaudiences?fields=name,description,account_id&subtype=CUSTOM&name={{segment.name}}&customer_file_source={{segment.metadata.customer_file_source}}&access_token={{authData.accessToken}}",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseFields":[
            {
               "value":"{{response.id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      },
      "update":{
         "url":"https://api.moviestar.com/v1.0/{{segment.alias}}?field=name,description,account_id&access_token={{authData.accessToken}}&customerAudienceId={{segment.alias}}&&name={{segment.name}}&description={{segment.description}}&customer_file_source={{segment.metadata.customer_file_source}}",
         "httpMethod":"POST",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseFields":[
            {
               "value":"{{response.id}}",
               "name":"externalAudienceId"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      },
      "delete":{
         "url":"https://api.moviestar.com/v1.0/{{segment.alias}}?fields=name,description,account_id&access_token={{authData.accessToken}}&customerAudienceId={{segment.alias}}",
         "httpMethod":"DELETE",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      },
      "validate":{
         "url":"https://api.moviestar.com/v1.0/permissions?access_token={{authData.accessToken}}",
         "httpMethod":"GET",
         "headers":[
            {
               "value":"application/x-www-form-urlencoded",
               "header":"Content-Type"
            }
         ],
         "responseFields":[
            {
               "value":"{{response.data[0].permission}}",
               "name":"Id"
            }
         ],
         "responseErrorFields":[
            {
               "value":"{{error.message}}",
               "name":"message"
            }
         ]
      }
   }
}'
```

+++

+++応答

応答が成功すると、HTTP ステータス 200 が、更新されたオーディエンステンプレートの詳細と共に返されます。

+++

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントでは、オーディエンステンプレートを使用するタイミングと、`/authoring/audience-templates` API エンドポイントを使用してオーディエンステンプレートを更新する方法について確認しました。この手順が宛先設定プロセスのどこに当てはまるかを把握するには、[Destination SDK を使用した宛先の設定方法](../guides/configure-destination-instructions.md)を参照してください。

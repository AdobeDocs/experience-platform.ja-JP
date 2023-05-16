---
description: このページでは、Adobe Experience Platform Destination SDKを通じたオーディエンステンプレートの更新に使用される API 呼び出しの例を示します。
title: オーディエンステンプレートの更新
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 36%

---


# オーディエンステンプレートの更新

>[!IMPORTANT]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/audience-templates`

このページでは、 `/authoring/audience-templates` API エンドポイント。

このエンドポイントを通じて設定できる機能について詳しくは、 [audience metadata management](../functionality/audience-metadata-management.md).

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## オーディエンステンプレート API 操作の概要 {#get-started}

続ける前に「[はじめる前に](../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## オーディエンステンプレートの更新 {#create}

以下の項目を更新し、 [既存](create-audience-template.md) オーディエンステンプレートを作成する `PUT` にリクエスト `/authoring/audience-templates` エンドポイントと、更新されたペイロード。

既存のオーディエンステンプレートとそれに対応するオーディエンステンプレートを取得するには `{INSTANCE_ID}`に関する記事を参照してください。 [オーディエンステンプレートの取得](retrieve-audience-template.md).

**API 形式**

```http
PUT /authoring/audience-templates/{INSTANCE_ID}
```

| パラメーター | 説明 |
| -------- | ----------- |
| `{INSTANCE_ID}` | 更新するオーディエンステンプレートの ID。 既存のオーディエンステンプレートとそれに対応するオーディエンステンプレートを取得するには `{INSTANCE_ID}`を参照してください。 [オーディエンステンプレートの取得](retrieve-audience-template.md). |

次のリクエストは、ペイロード内のパラメーター設定に基づいて、既存のオーディエンスメタデータテンプレートを更新します。

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

正常な応答は、HTTP ステータス 200 と、更新されたオーディエンステンプレートの詳細を返します。

+++

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読むと、オーディエンステンプレートを使用するタイミングと、 `/authoring/audience-templates` API エンドポイント。 [Destination SDK を使用して宛先を設定する方法](../guides/configure-destination-instructions.md)を参照して、この手順が宛先を設定するプロセスの中でどのように位置づけられるかを把握します。

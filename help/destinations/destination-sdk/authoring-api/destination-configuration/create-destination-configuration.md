---
description: API 呼び出しを構造化し、Adobe Experience Platform Destination SDKを通じて宛先設定を作成する方法を説明します。
title: 宛先設定の作成
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 57%

---


# 宛先設定の作成

このページの例では、 `/authoring/destinations` API エンドポイント。

このエンドポイントを通じて設定できる機能について詳しくは、次の記事を参照してください。

* [顧客認証設定](../../functionality/destination-configuration/customer-authentication.md)
* [OAuth 2 認証](../../functionality/destination-configuration/oauth2-authentication.md)
* [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md)
* [UI 属性](../../functionality/destination-configuration/ui-attributes.md)
* [スキーマ設定](../../functionality/destination-configuration/schema-configuration.md)
* [ID 名前空間の設定](../../functionality/destination-configuration/identity-namespace-configuration.md)
* [宛先配信](../../functionality/destination-configuration/destination-delivery.md)
* [オーディエンスメタデータの設定](../../functionality/destination-configuration/audience-metadata-configuration.md)
* [オーディエンスメタデータの設定](../../functionality/destination-configuration/audience-metadata-configuration.md)
* [集計ポリシー](../../functionality/destination-configuration/aggregation-policy.md)
* [バッチ設定](../../functionality/destination-configuration/batch-configuration.md)
* [プロファイル選定履歴](../../functionality/destination-configuration/historical-profile-qualifications.md)

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 宛先設定 API 操作の概要 {#get-started}

続ける前に「[はじめる前に](../../getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 宛先設定の作成 {#create}

`/authoring/destinations` エンドポイントに POST リクエストを実行することで、新しい宛先設定を作成できます。

>[!TIP]
>
>**API エンドポイント**：`platform.adobe.io/data/core/activation/authoring/destinations`

**API 形式**

```http
POST /authoring/destinations
```

次のリクエストは、 [!DNL Amazon S3] 宛先の設定。ペイロードで指定されたパラメーターによって設定されます。 以下のペイロードには、`/authoring/destinations` エンドポイントで利用可能なファイルベースの宛先のパラメーターをすべて含みます。

API 呼び出しにすべてのパラメーターを追加する必要はなく、API 要件に従ってペイロードをカスタマイズできることに注意してください。

+++リクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/activation/authoring/destinations \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {ORG_ID}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
{
   "name":"Amazon S3 destination with predefined CSV formatting options",
   "description":"Amazon S3 destination with predefined CSV formatting options",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
      {
         "authType":"S3"
      }
   ],
   "customerDataFields":[
      {
         "name":"bucket",
         "title":"Enter the name of your Amazon S3 bucket",
         "description":"Amazon S3 bucket name",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"path",
         "title":"Enter the path to your S3 bucket folder",
         "description":"Enter the path to your S3 bucket folder",
         "type":"string",
         "isRequired":true,
         "pattern":"^[A-Za-z]+$",
         "readOnly":false,
         "hidden":false
      },
      {
         "name":"compression",
         "title":"Compression format",
         "description":"Select the desired file compression format.",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "enum":[
            "SNAPPY",
            "GZIP",
            "DEFLATE",
            "NONE"
         ]
      },
      {
         "name":"fileType",
         "title":"Select a fileType",
         "description":"Select fileType",
         "type":"string",
         "isRequired":true,
         "readOnly":false,
         "hidden":false,
         "enum":[
            "csv",
            "json",
            "parquet"
         ],
         "default":"csv"
      }
   ],
   "uiAttributes":{
      "documentationLink":"https://www.adobe.com/go/destinations-amazon-s3-en",
      "category":"cloudStorage",
      "icon":{
         "key":"amazonS3"
      },
      "connectionType":"S3",
      "frequency":"Batch"
   },
   "destinationDelivery":[
      {
         "deliveryMatchers":[
            {
               "type":"SOURCE",
               "value":[
                  "batch"
               ]
            }
         ],
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}"
      }
   ],
   "schemaConfig":{
      "profileRequired":true,
      "segmentRequired":true,
      "identityRequired":true
   },
   "batchConfig":{
      "allowMandatoryFieldSelection":true,
      "allowDedupeKeyFieldSelection":true,
      "defaultExportMode":"DAILY_FULL_EXPORT",
      "allowedExportMode":[
         "DAILY_FULL_EXPORT",
         "FIRST_FULL_THEN_INCREMENTAL"
      ],
      "allowedScheduleFrequency":[
         "DAILY",
         "EVERY_3_HOURS",
         "EVERY_6_HOURS",
         "EVERY_8_HOURS",
         "EVERY_12_HOURS",
         "ONCE"
      ],
      "defaultFrequency":"DAILY",
      "defaultStartTime":"00:00",
      "filenameConfig":{
         "allowedFilenameAppendOptions":[
            "SEGMENT_NAME",
            "DESTINATION_INSTANCE_ID",
            "DESTINATION_INSTANCE_NAME",
            "ORGANIZATION_NAME",
            "SANDBOX_NAME",
            "DATETIME",
            "CUSTOM_TEXT"
         ],
         "defaultFilenameAppendOptions":[
            "DATETIME"
         ],
         "defaultFilename":"%DESTINATION%_%SEGMENT_ID%"
      },
      "backfillHistoricalProfileData":true
   }
}'
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | Experience Platform カタログ内の宛先のタイトルを示します。 |
| `description` | 文字列 | アドビが宛先カードの Experience Platform 宛先カタログで使用する説明を入力します。4 ～ 5 文以下を目指します。![宛先の説明を示す Platform UI 画像。](../../assets/authoring-api/destination-configuration/destination-description.png "宛先の説明"){width="100" zoomable="yes"} |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。最初に宛先を設定する際は `TEST` を使用します。 |
| `customerAuthenticationConfigurations.authType` | 文字列 | 宛先サーバーに対するExperience Platform顧客の認証に使用する設定を示します。 詳しくは、 [顧客認証設定](../../functionality/destination-configuration/customer-authentication.md) を参照してください。 |
| `customerDataFields.name` | 文字列 | 導入するカスタムフィールドの名前を記入します。<br/><br/> 詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 ![顧客データフィールドを示す Platform UI 画像。](../../assets/authoring-api/destination-configuration/customer-data-fields.png "顧客データフィールド"){width="100" zoomable="yes"} |
| `customerDataFields.type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 使用できる値は `string`、`object`、`integer` です。<br/><br/> 詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 |
| `customerDataFields.title` | 文字列 | Experience Platform のユーザーインターフェースで顧客に表示される、フィールドの名前を示します。<br/><br/> 詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 |
| `customerDataFields.description` | 文字列 | カスタムフィールドの説明を入力します。詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 |
| `customerDataFields.isRequired` | ブール値 | このフィールドが宛先設定ワークフローで必須かどうかを示します。<br/><br/> 詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 |
| `customerDataFields.enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。<br/><br/> 詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 |
| `customerDataFields.default` | 文字列 | デフォルト値を `enum` リストから定義します。 |
| `customerDataFields.pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドに「`^[A-Za-z]+$`」と入力します。<br/><br/> 詳しくは、 [顧客データフィールド](../../functionality/destination-configuration/customer-data-fields.md) を参照してください。 |
| `uiAttributes.documentationLink` | 文字列 | 宛先用の[宛先のカタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=ja#catalog)にあるドキュメントページを参照します。`https://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。ここでは、`YOURDESTINATION` は宛先の名前です。Moviestar という宛先の場合、`https://www.adobe.com/go/destinations-moviestar-en` を使用します。。このリンクは、Adobeが宛先をライブに設定し、ドキュメントが公開された後にのみ機能します。 <br/><br/> 詳しくは、 [UI 属性](../../functionality/destination-configuration/ui-attributes.md) を参照してください。 ![ドキュメントリンクを示す Platform UI 画像。](../../assets/authoring-api/destination-configuration/documentation-url.png "ドキュメント URL"){width="100" zoomable="yes"} |
| `uiAttributes.category` | 文字列 | Adobe Experience Platform で宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先のカテゴリ](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/destinations/destination-types.html?lang=ja#destination-categories)をお読みください。次のいずれかの値を使用します：`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`<br/><br/> 詳しくは、 [UI 属性](../../functionality/destination-configuration/ui-attributes.md) を参照してください。 |
| `uiAttributes.connectionType` | 文字列 | 宛先に応じた接続のタイプ。サポートされている値。 <ul><li>`Server-to-server`</li><li>`Cloud storage`</li><li>`Azure Blob`</li><li>`Azure Data Lake Storage`</li><li>`S3`</li><li>`SFTP`</li><li>`DLZ`</li></ul> |
| `uiAttributes.frequency` | 文字列 | 宛先でサポートされているデータ書き出しのタイプを指します。 に設定 `Streaming` （API ベースの統合の場合）、または `Batch` を使用して、宛先にファイルを書き出す場合。 |
| `identityNamespaces.externalId.acceptsAttributes` | ブール値 | お客様が、設定中の ID に標準プロファイル属性をマッピングできるかどうかを示します。 |
| `identityNamespaces.externalId.acceptsCustomNamespaces` | ブール値 | 顧客がに属する ID をマッピングできるかどうかを示します [カスタム名前空間](/help/identity-service/namespaces.md#manage-namespaces) を設定する必要があります。 |
| `identityNamespaces.externalId.transformation` | 文字列 | _サンプル設定には表示されません_。例えば、[!DNL Platform] の顧客がプレーンなメールアドレスを属性として持っており、プラットフォームがハッシュ化されたメールのみを受け取る場合に使用します。ここで、適用する必要のある変換（例えば、メールを小文字に変換してからハッシュ化するなど）を行います。 |
| `identityNamespaces.externalId.acceptedGlobalNamespaces` | - | どれが [標準 id 名前空間](/help/identity-service/namespaces.md#standard) （例えば、IDFA）のお客様は、設定中の ID にマッピングできます。 <br> `acceptedGlobalNamespaces` を使用する場合、`"requiredTransformation":"sha256(lower($))"` を使用すれば、メールアドレスまたは電話番号を小文字に変換してハッシュ化できます。 |
| `destinationDelivery.authenticationRule` | 文字列 | [!DNL Platform] の顧客が宛先に接続する方法を示します。使用できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`、<br> です。 <ul><li>Platform の顧客がユーザー名とパスワード、ベアラートークン、または他の認証方法を使用してシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。例えば、`customerAuthenticationConfigurations` で `authType: OAUTH2` や `authType:BEARER` も選択した場合、このオプションを選択することになります。 </li><li> アドビと接続先との間にグローバル認証システムがあり、[!DNL Platform] の顧客が接続先に認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION` を使用してください。この場合、 [資格情報 API](../../credentials-api/create-credential-configuration.md) 設定。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationDelivery.destinationServerId` | 文字列 | この宛先に使用される[宛先サーバーテンプレート](../destination-server/create-destination-server.md)の `instanceId`。 |
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 常に次に設定 `true`. |
| `segmentMappingConfig.mapUserInput` | ブール値 | 宛先アクティベーションワークフローのセグメントマッピング ID をユーザーが入力するかどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentId` | ブール値 | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント ID かどうかを制御します。 |
| `segmentMappingConfig.mapExperiencePlatformSegmentName` | ブール値 | 宛先アクティベーションワークフローのセグメントマッピング ID がExperience Platformセグメント名かどうかを制御します。 |
| `segmentMappingConfig.audienceTemplateId` | ブール値 | この宛先に使用する[オーディエンスメタデータテンプレート](../../metadata-api/create-audience-template.md) の `instanceId`。 |
| `schemaConfig.profileFields` | 配列 | 上記の設定に示すように、定義済みの `profileFields` を追加する際、ユーザーは Adobe Experience Platform 属性を宛先側の定義済み属性にマッピングするオプションを選択できます。 |
| `schemaConfig.profileRequired` | ブール値 | 上記の設定例に示すように、ユーザーが Experience Platform から宛先側のカスタム属性にプロファイル属性をマッピングできる場合、`true` を使用します。 |
| `schemaConfig.segmentRequired` | ブール値 | 常に `segmentRequired:true` を使用します。 |
| `schemaConfig.identityRequired` | ブール値 | ユーザーが、Adobe Experience Platform から目的のスキーマに ID 名前空間をマッピングできるようにする場合、`true` を使用します。 |

{style="table-layout:auto"}

+++

+++応答

成功時の応答は、HTTP ステータス 200 と共に、新しく作成された宛先の詳細を返します。

+++

## API エラー処理

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読んだ後、Destination SDKを使用して新しい宛先設定を作成する方法を学びました `/authoring/destinations` API エンドポイント。

このエンドポイントで実行できる操作について詳しくは、次の記事を参照してください。

* [宛先設定の取得](retrieve-destination-configuration.md)
* [宛先設定の更新](update-destination-configuration.md)
* [宛先設定の削除](delete-destination-configuration.md)

このエンドポイントが宛先オーサリングプロセスに適した場所を理解するには、次の記事を参照してください。

* [Destination SDK を使用したストリーミングの宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-destination-configuration)

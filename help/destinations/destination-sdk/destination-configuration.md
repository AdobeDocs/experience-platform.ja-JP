---
description: この設定を使用すると、宛先名、カテゴリ、説明、ロゴなどの基本情報を指定できます。 また、この設定の設定によって、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができる ID も決まります。
title: 宛先 SDK の宛先設定オプション
exl-id: b7e4db67-2981-4f18-b202-3facda5c8f0b
source-git-commit: 76a596166edcdbf141b5ce5dc01557d2a0b4caf3
workflow-type: tm+mt
source-wordcount: '1727'
ht-degree: 5%

---

# 宛先の設定 {#destination-configuration}

## 概要 {#overview}

この設定を使用すると、宛先名、カテゴリ、説明、ロゴなど、重要な情報を指定できます。 また、この設定の設定によって、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができる ID も決まります。

また、この設定は、宛先サーバーとオーディエンスメタデータを機能させるために必要な他の設定も、この設定に接続します。 [ の後の ](./destination-configuration.md#connecting-all-configurations) の節で、2 つの設定を参照する方法を確認してください。

このドキュメントで説明する機能は、`/authoring/destinations` API エンドポイントを使用して設定できます。 エンドポイントで実行できる操作の完全なリストについては、[ 宛先 API エンドポイントの操作 ](./destination-configuration-api.md) をお読みください。

## 設定例 {#example-configuration}

架空の宛先 Moviestar の設定例を以下に示します。この宛先は、世界の 4 つの場所にエンドポイントを持っています。 宛先は、モバイルの宛先カテゴリに属します。 次の節では、この設定の構築方法を詳しく説明します。

```json
{
   "name":"Moviestar",
   "description":"Moviestar is a fictional destination, used for this example.",
   "status":"TEST",
   "customerAuthenticationConfigurations":[
      {
         "authType":"BEARER"
      }
   ],
   "customerDataFields":[
      {
         "name":"endpointsInstance",
         "type":"string",
         "title":"Select Endpoint",
         "description":"Moviestar manages several instances across the globe for REST endpoints that our customers are provisioned for. Select your endpoint in the dropdown list.",
         "isRequired":true,
         "enum":[
            "US",
            "EU",
            "APAC",
            "NZ"
         ]
      },
      {
         "name":"customerID",
         "type":"string",
         "title":"Moviestar Customer ID",
         "description":"Your customer ID in the Moviestar destination (e.g. abcdef).",
         "isRequired":true,
         "pattern":"^[A-Za-z]+$"
      }
   ],
   "uiAttributes":{
      "documentationLink":"http://www.adobe.com/go/destinations-moviestar-en",
      "category":"mobile",
      "connectionType":"Server-to-server",
      "frequency":"Streaming"
   },
   "identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "Email":{
               
            }
         }
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      }
   },
   "schemaConfig":{
      "profileRequired":false,
      "segmentRequired":true,
      "identityRequired":true
   },
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"9c77000a-4559-40ae-9119-a04324a3ecd4"
      }
   ],
   "segmentMappingConfig":{
      "mapExperiencePlatformSegmentName":false,
      "mapExperiencePlatformSegmentId":false,
      "mapUserInput":false,
      "audienceTemplateId":"cbf90a70-96b4-437b-86be-522fbdaabe9c"
   },
   "aggregation":{
      "aggregationType":"CONFIGURABLE_AGGREGATION",
      "configurableAggregation":{
         "splitUserById":true,
         "maxBatchAgeInSecs":0,
         "maxNumEventsInBatch":0,
         "aggregationKey":{
            "includeSegmentId":true,
            "includeSegmentStatus":true,
            "includeIdentity":true,
            "oneIdentityPerGroup":false,
            "groups":[
               {
                  "namespaces":[
                     "IDFA",
                     "GAID"
                  ]
               },
               {
                  "namespaces":[
                     "EMAIL"
                  ]
               }
            ]
         }
      }
   },
   "backfillHistoricalProfileData":true
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルをExperience Platformします。 |
| `description` | 文字列 | 宛先カタログで、宛先カードの説明をExperience Platformします。 4～5 文以下を目指す。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。宛先を最初に設定する際は、`TEST` を使用します。 |

{style=&quot;table-layout:auto&quot;}

## 顧客認証設定 {#customer-authentication-configurations}

このセクションでは、Experience PlatformユーザーインターフェイスでExperience Platformページを生成し、ユーザーは、宛先に割り当てられたアカウントにアカウントを接続します。 `authType` フィールドに指定した認証オプションに応じて、Experience Platformページが次のように生成されます。

**ベアラ認証**

ユーザーは、宛先から取得した bearer トークンを入力する必要があります。

![ベアラ認証を使用した UI レンダリング](./assets/bearer-authentication-ui.png)

**OAuth 2 認証**

ユーザーは「**[!UICONTROL 宛先に接続]**」を選択して、OAuth 2 認証フローを宛先にトリガーします。

![OAuth 2 認証を使用した UI レンダリング](./assets/oauth2-authentication-ui.png)


| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `customerAuthenticationConfigurations` | 文字列 | サーバーへのExperience Platform顧客の認証に使用する設定を示します。 受け入れられる値については、以下の `authType` を参照してください。 |
| `authType` | 文字列 | 指定できる値は `OAUTH2, BEARER` です。 <br><ul><li> 宛先が OAuth 2 認証をサポートしている場合は、 `OAUTH2` 値を選択し、宛先 SDK OAuth 2 認証ページに示すように、OAuth 2 の必須フィールドを追加します。 さらに、[ 宛先配信セクション ](./destination-configuration.md) で `authenticationRule=CUSTOMER_AUTHENTICATION` を選択する必要があります。 </li><li>bearer 認証の場合、`BEARER` を選択し、[ 宛先配信セクション ](./destination-configuration.md) で `authenticationRule=CUSTOMER_AUTHENTICATION` を選択します。</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 顧客データフィールド {#customer-data-fields}

このセクションでは、パートナーがカスタムフィールドを導入できます。 上記の例の設定では、`customerDataFields` は、ユーザーが認証フローでエンドポイントを選択し、宛先で顧客 ID を示す必要があります。 設定は、次に示すように、認証フローに反映されます。

![カスタムフィールド認証フロー](./assets/custom-field-authentication-flow.png)

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `type` | 文字列 | 導入するカスタムフィールドの種類を示します。 指定できる値は `string`、`object`、`integer` です。 |
| `title` | 文字列 | ユーザーインターフェイスでユーザーに表示されるフィールドの名前をExperience Platformします。 |
| `description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `isRequired` | Boolean | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用してパターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドに `^[A-Za-z]+$` と入力します。 |

{style=&quot;table-layout:auto&quot;}

## UI 属性 {#ui-attributes}

この節では、Adobe Experience Platformユーザーインターフェイスの宛先にAdobeが使用する、上記の設定の UI 要素について説明します。 次を参照してください。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `documentationLink` | 文字列 | 宛先の [ 宛先カタログ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) のドキュメントページを参照します。 `http://www.adobe.com/go/destinations-YOURDESTINATION-en` を使用します。`YOURDESTINATION` は宛先の名前です。 Moviestar という宛先の場合は、`http://www.adobe.com/go/destinations-moviestar-en` |
| `category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指定します。 詳しくは、[ 宛先カテゴリ ](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html) を参照してください。 次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `connectionType` | 文字列 | `Server-to-server` は現在、唯一のオプションです。 |
| `frequency` | 文字列 | `Streaming` は現在、唯一のオプションです。 |

{style=&quot;table-layout:auto&quot;}

## マッピング手順でのスキーマ設定 {#schema-configuration}

![マッピング手順の有効化](./assets/enable-mapping-step.png)

`schemaConfig` のパラメーターを使用して、宛先のアクティベーションワークフローのマッピング手順を有効にします。 以下に説明するパラメーターを使用すると、Experience Platformユーザーがプロファイル属性や ID を宛先の側の目的のスキーマにマッピングできるかどうかを判断できます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `profileFields` | 配列 | *上記の設定例では示していません。* 事前定義済みを追加す `profileFields`ると、Experience Platformユーザーは、Platform 属性を宛先の側の定義済み属性にマッピングするオプションがあります。 |
| `profileRequired` | Boolean | 上記の設定例に示すように、Experience Platformから宛先のカスタム属性にプロファイル属性をマッピングできる場合は、`true` を使用します。 |
| `segmentRequired` | Boolean | 常に `segmentRequired:true` を使用します。 |
| `identityRequired` | Boolean | ユーザーが ID 名前空間をExperience Platformから目的のスキーマにマッピングできる場合は、`true` を使用します。 |

{style=&quot;table-layout:auto&quot;}

## ID と属性 {#identities-and-attributes}

この節のパラメーターは、Experience Platformユーザーインターフェイスのマッピング手順でターゲットの ID と属性を入力する方法を決定します。ユーザーは、XDM スキーマを宛先のスキーマにマッピングします。

顧客が宛先に書き出すことのできる [!DNL Platform] ID を指定する必要があります。 例えば、[!DNL Experience Cloud ID]、ハッシュ化された電子メール、デバイス ID([!DNL IDFA]、[!DNL GAID]) などがあります。 これらの値は [!DNL Platform] ID 名前空間で、顧客が宛先の ID 名前空間にマッピングできます。

ID 名前空間では、[!DNL Platform] と宛先の間に 1 対 1 の対応関係は必要ありません。
例えば、お客様は [!DNL Platform] [!DNL IDFA] 名前空間を宛先の [!DNL IDFA] 名前空間にマッピングできます。また、同じ [!DNL Platform] [!DNL IDFA] 名前空間を宛先の [!DNL Customer ID] 名前空間にマッピングできます。

詳しくは、[ID 名前空間の概要 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja) を参照してください。

![UI でのターゲット ID のレンダリング](./assets/target-identities-ui.png)

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `acceptsAttributes` | Boolean | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `acceptsCustomNamespaces` | Boolean | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `allowedAttributesTransformation` | 文字列 | *サンプルの設定*&#x200B;では示されていません。例えば、[!DNL Platform] の顧客が属性としてプレーンな電子メールアドレスを持ち、プラットフォームがハッシュ化された電子メールのみを受け入れる場合に使用します。 このオブジェクトでは、適用する必要がある変換（例えば、E メールを小文字に変換し、ハッシュ化）を実行できます。 例については、[ 宛先設定 API リファレンス ](./destination-configuration-api.md#update) の `requiredTransformation` を参照してください。 |
| `acceptedGlobalNamespaces` | - | プラットフォームが [ 標準 ID 名前空間 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（例えば、IDFA）を受け入れる場合に使用されるので、Platform ユーザーは、これらの ID 名前空間のみを選択するように制限できます。 |

{style=&quot;table-layout:auto&quot;}

## 宛先の配信 {#destination-delivery}

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationRule` | 文字列 | [!DNL Platform] ユーザーが宛先に接続する方法を示します。 指定できる値は `CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE` です。<br> <ul><li>Platform のお客様が、ユーザー名とパスワード、ベアラートークン、または別の認証方法を使用してシステムにログインする場合は、`CUSTOMER_AUTHENTICATION` を使用します。 例えば、`customerAuthenticationConfigurations` で `authType: OAUTH2` または `authType:BEARER` も選択した場合は、このオプションを選択します。 </li><li> Adobeと宛先の間にグローバル認証システムがあり、`PLATFORM_AUTHENTICATION` 顧客が宛先に接続するための認証資格情報を提供する必要がない場合は、[!DNL Platform] を使用します。 この場合、[Credentials](./credentials-configuration.md) 設定を使用して credentials オブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE` を使用します。 </li></ul> |
| `destinationServerId` | 文字列 | この宛先で使用される [ 宛先サーバー設定 ](./destination-server-api.md) の `instanceId`。 |
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |

{style=&quot;table-layout:auto&quot;}

## セグメントマッピングの設定 {#segment-mapping}

![セグメントマッピングの設定セクション](./assets/segment-mapping-configuration.png)

宛先設定のこの節では、セグメント名や ID などのセグメントメタデータを、宛先との間で共有する方法について説明します。

`audienceTemplateId` を通じて、この設定と [ オーディエンスメタデータ設定 ](./audience-metadata-management.md) を結び付けます。

上記の設定に示したパラメーターは、[ 宛先エンドポイント API リファレンス ](./destination-configuration-api.md) に記載されています。

## この設定が、宛先に必要な情報をすべて接続する方法 {#connecting-all-configurations}

宛先の一部の設定は、宛先サーバーまたはオーディエンスメタデータエンドポイントを通じて設定できます。 宛先の設定エンドポイントは、次の手順で設定を参照することで、これらの設定をすべて接続します。

* `destinationServerId` を使用して、宛先サーバーと、宛先に対して設定されたテンプレート設定を参照します。
* `audienceMetadataId` を使用して、宛先に設定されたオーディエンスメタデータ設定を参照します。


## 集計ポリシー {#aggregation}

![構成テンプレートの集計ポリシー](./assets/aggregation-configuration.png)

このセクションでは、宛先にデータをエクスポートする際にExperience Platformが使用する集計ポリシーを設定できます。

集計ポリシーは、データエクスポートでエクスポートされたプロファイルを組み合わせる方法を決定します。 次の選択肢があります。
* ベストエフォートの集計
* 設定可能な集計（上記の設定で表示）

[ テンプレート ](./message-format.md#using-templating) と [ 集計の主な例 ](./message-format.md#template-aggregation-key) の使用の節を読み、選択した集計ポリシーに基づいて、メッセージ変換テンプレートに集計ポリシーを含める方法を理解してください。

### ベストエフォートの集計 {#best-effort-aggregation}

>[!TIP]
>
>API エンドポイントが受け入れるプロファイルが API 呼び出しあたり 100 件未満の場合は、このオプションを使用します。

このオプションは、リクエストあたりのプロファイル数が少なく、データ量が多いリクエストよりも少ない方がリクエスト数が多い宛先に最適です。

`maxUsersPerRequest` パラメーターを使用して、宛先がリクエストで取得できるプロファイルの最大数を指定します。

### 設定可能な集計 {#configurable-aggregation}

このオプションは、同じ呼び出しで何千ものプロファイルを含む大きなバッチを取る場合に最適です。 また、このオプションを使用すると、複雑な集計ルールに基づいて、書き出されたプロファイルを集計できます。

このオプションを使用すると、次のことができます。
* 宛先に対して API 呼び出しがおこなわれるまでに集計するプロファイルの最大時間と数を設定します。
* 次の項目に基づいて、宛先にマッピングされた書き出し済みプロファイルを集計します。
   * セグメント ID
   * セグメントのステータス
   * ID または ID のグループ

集計パラメーターの詳細については、[ 宛先 API エンドポイントの操作 ](./destination-configuration-api.md) リファレンスページを参照してください。各パラメーターについて説明します。

## 過去のプロファイル認定

宛先の設定で `backfillHistoricalProfileData` パラメーターを使用して、過去のプロファイル認定を宛先に書き出す必要があるかどうかを判断できます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |
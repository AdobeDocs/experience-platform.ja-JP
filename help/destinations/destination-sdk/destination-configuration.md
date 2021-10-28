---
description: この設定を使用すると、宛先名、カテゴリ、説明、ロゴなどの基本情報を指定できます。 また、この設定では、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができる ID も決定します。
title: 宛先 SDK の宛先設定オプション
exl-id: b7e4db67-2981-4f18-b202-3facda5c8f0b
source-git-commit: 0bd57e226155ee68758466146b5d873dc4fdca29
workflow-type: tm+mt
source-wordcount: '1757'
ht-degree: 5%

---

# 宛先の設定 {#destination-configuration}

## 概要 {#overview}

この設定を使用すると、宛先名、カテゴリ、説明などの重要な情報を指定できます。 また、この設定では、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができる ID も決定します。

また、この設定は、宛先サーバーとオーディエンスメタデータの機能に必要な他の設定も、この設定に接続します。 2 つの設定を [下のセクション](./destination-configuration.md#connecting-all-configurations).

このドキュメントで説明する機能は、 `/authoring/destinations` API エンドポイント。 読み取り [宛先 API エンドポイントの操作](./destination-configuration-api.md) エンドポイントで実行できる操作の完全なリストについては、を参照してください。

## 設定例 {#example-configuration}

次に、架空の宛先 Moviestar の設定例を示します。この宛先は、世界の 4 つの場所にエンドポイントを持っています。 宛先は、モバイル宛先カテゴリに属しています。 次の節では、この設定の構築方法を詳しく説明します。

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
| `description` | 文字列 | 宛先カタログで宛先カードの説明をExperience Platformします。 4～5 文以下を目指します。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。用途 `TEST` を設定します。 |

{style=&quot;table-layout:auto&quot;}

## 顧客認証設定 {#customer-authentication-configurations}

宛先設定のこのセクションは、 [新しい宛先の設定](/help/destinations/ui/connect-destination.md) ページを開きます。このページでは、Experience Platformが、宛先のアカウントにExperience Platformを接続できます。 に指定する認証オプションに応じて、 `authType` 」フィールドに値を入力すると、Experience Platformページが次のように生成されます。

**Bearer 認証**

bearer 認証タイプを設定する場合、ユーザーは宛先から取得した bearer トークンを入力する必要があります。

![bearer 認証を使用した UI レンダリング](./assets/bearer-authentication-ui.png)

**OAuth 2 認証**

ユーザーが選択 **[!UICONTROL 宛先に接続]** を使用して、Twitter Cloud Audiences の宛先に対する次の例に示すように、OAuth 2 認証フローを宛先にトリガーします。 宛先エンドポイントに対する OAuth 2 認証の設定について詳しくは、専用の [宛先 SDK OAuth 2 認証ページ](./oauth2-authentication.md).

![OAuth 2 認証を使用した UI レンダリング](./assets/oauth2-authentication-ui.png)


| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `customerAuthenticationConfigurations` | 文字列 | サーバーへのExperience Platform顧客の認証に使用する設定を示します。 詳しくは、 `authType` を参照してください。 |
| `authType` | 文字列 | 指定できる値は次のとおりです。 `OAUTH2, BEARER`. <br><ul><li> 宛先が OAuth 2 認証をサポートしている場合は、 `OAUTH2` の値を入力して、OAuth 2 の必須フィールドを追加します ( [宛先 SDK OAuth 2 認証ページ](./oauth2-authentication.md). また、 `authenticationRule=CUSTOMER_AUTHENTICATION` 内 [宛先の配信セクション](./destination-configuration.md). </li><li>bearer 認証の場合は、「 」を選択します。 `BEARER` を選択し、 `authenticationRule=CUSTOMER_AUTHENTICATION` 内 [宛先の配信セクション](./destination-configuration.md).</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 顧客データフィールド {#customer-data-fields}

このセクションでは、パートナーがカスタムフィールドを導入できます。 上記の例の設定では、 `customerDataFields` では、ユーザーは認証フローでエンドポイントを選択し、顧客 ID と宛先を指定する必要があります。 次に示すように、設定は認証フローに反映されます。

![カスタムフィールド認証フロー](./assets/custom-field-authentication-flow.png)

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 紹介するカスタムフィールドの名前を指定します。 |
| `type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は次のとおりです。 `string`, `object`, `integer`. |
| `title` | 文字列 | ユーザーインターフェイスのユーザーに表示されるフィールドの名前をExperience Platformします。 |
| `description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `isRequired` | Boolean | このフィールドが宛先設定ワークフローで必須かどうかを示します。 |
| `enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、 `^[A-Za-z]+$` を選択します。 |

{style=&quot;table-layout:auto&quot;}

## UI 属性 {#ui-attributes}

この節では、Adobe Experience Platformユーザーインターフェイスの宛先にAdobeが使用する必要がある、上記の設定の UI 要素について説明します。 次を参照してください。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `documentationLink` | 文字列 | ページの [宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog) を設定します。 用途 `http://www.adobe.com/go/destinations-YOURDESTINATION-en`で、 `YOURDESTINATION` は、宛先の名前です。 Moviestar という宛先の場合、 `http://www.adobe.com/go/destinations-moviestar-en` |
| `category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを指します。 詳しくは、 [宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html). 次のいずれかの値を使用します。 `adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `connectionType` | 文字列 | `Server-to-server` は現在唯一の利用可能なオプションです。 |
| `frequency` | 文字列 | `Streaming` は現在唯一の利用可能なオプションです。 |

{style=&quot;table-layout:auto&quot;}

## マッピング手順のスキーマ設定 {#schema-configuration}

![マッピングステップの有効化](./assets/enable-mapping-step.png)

のパラメーターを使用します。 `schemaConfig` ：宛先のアクティベーションワークフローのマッピング手順を有効にします。 以下に説明するパラメーターを使用することで、Experience Platformユーザーがプロファイル属性や ID を宛先の側の目的のスキーマにマッピングできるかどうかを判断できます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `profileFields` | 配列 | *上記の設定例では示していません。* 定義済みの `profileFields`の場合、Experience Platformユーザーは、Platform 属性を宛先の側の事前定義済み属性にマッピングするオプションがあります。 |
| `profileRequired` | ブール値 | 用途 `true` 上記の設定例に示すように、ユーザーがExperience Platformから宛先側のカスタム属性にプロファイル属性をマッピングできる場合。 |
| `segmentRequired` | ブール値 | 常に使用 `segmentRequired:true`. |
| `identityRequired` | ブール値 | 用途 `true` ( ユーザーが、id 名前空間をExperience Platformから目的のスキーマにマッピングできる場合 )。 |

{style=&quot;table-layout:auto&quot;}

## ID と属性 {#identities-and-attributes}

この節のパラメーターは、宛先が受け入れる ID を決定します。 また、この設定により、 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) を使用します。

どちらを指定するかを指定する必要があります [!DNL Platform] id 顧客は、を宛先に書き出すことができます。 以下に例を示します。 [!DNL Experience Cloud ID]，ハッシュ化された電子メール，デバイス ID ([!DNL IDFA], [!DNL GAID]) をクリックします。 これらの値は次のとおりです。 [!DNL Platform] ユーザーが宛先の id 名前空間にマッピングできる id 名前空間。 また、顧客が宛先でサポートされている ID にカスタム名前空間をマッピングできるかどうかを指定することもできます。

ID 名前空間では、 [!DNL Platform] および宛先
例えば、お客様は [!DNL Platform] [!DNL IDFA] 名前空間を [!DNL IDFA] 名前空間を宛先から作成するか、同じ名前空間をマッピングできます。 [!DNL Platform] [!DNL IDFA] 名前空間を [!DNL Customer ID] 名前空間を使用します。

詳しくは、 [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja).

![UI でのターゲット ID のレンダリング](./assets/target-identities-ui.png)

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `acceptsAttributes` | ブール値 | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されています。 |
| `acceptsCustomNamespaces` | ブール値 | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `allowedAttributesTransformation` | 文字列 | *サンプル設定には表示されません*. 例えば、 [!DNL Platform] のお客様は、属性としてプレーンな電子メールアドレスを持っており、プラットフォームはハッシュ化された電子メールのみを受け取ります。 このオブジェクトでは、適用が必要な変換（例えば、E メールを小文字に変換し、ハッシュ化）を適用できます。 例については、 `requiredTransformation` 内 [宛先設定 API リファレンス](./destination-configuration-api.md#update). |
| `acceptedGlobalNamespaces` | - | プラットフォームがを受け入れる場合に使用します [標準 id 名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces) （例えば IDFA）を使用して、Platform ユーザーがこれらの ID 名前空間を選択するように制限できます。 |

{style=&quot;table-layout:auto&quot;}

## 宛先の配信 {#destination-delivery}

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationRule` | 文字列 | 方法を示します [!DNL Platform] のお客様が宛先に接続します。 指定できる値は次のとおりです。 `CUSTOMER_AUTHENTICATION`, `PLATFORM_AUTHENTICATION`, `NONE`. <br> <ul><li>用途 `CUSTOMER_AUTHENTICATION` Platform のお客様が、ユーザー名とパスワード、ベアラートークンまたはその他の認証方法を使用してシステムにログインした場合。 例えば、このオプションを選択する場合は、 `authType: OAUTH2` または `authType:BEARER` in `customerAuthenticationConfigurations`. </li><li> 用途 `PLATFORM_AUTHENTICATION` Adobeと宛先の間にグローバル認証システムがあり、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 [資格情報](./credentials-configuration-api.md) 設定。 </li><li>用途 `NONE` 宛先プラットフォームにデータを送信するために認証が必要ない場合に使用します。 </li></ul> |
| `destinationServerId` | 文字列 | この `instanceId` の [宛先サーバーの設定](./destination-server-api.md) この宛先に使用されます。 |

{style=&quot;table-layout:auto&quot;}

## セグメントマッピングの設定 {#segment-mapping}

![セグメントマッピング設定セクション](./assets/segment-mapping-configuration.png)

宛先設定のこのセクションは、セグメント名や ID などのセグメントメタデータをExperience Platformと宛先の間で共有する方法に関連しています。

を通じて `audienceTemplateId`の場合、この節では、この設定を [オーディエンスメタデータ設定](./audience-metadata-management.md).

上記の設定で示されたパラメーターは、 [宛先エンドポイント API リファレンス](./destination-configuration-api.md).

## 集計ポリシー {#aggregation}

![設定テンプレートの集計ポリシー](./assets/aggregation-configuration.png)

このセクションでは、宛先にデータをエクスポートする際にExperience Platformが使用する集計ポリシーを設定できます。

集計ポリシーは、データエクスポートでエクスポートされたプロファイルを組み合わせる方法を決定します。 次の選択肢があります。
* ベストエフォート集計
* 設定可能な集計（上記の設定で表示）

の節を読む [テンプレートの使用](./message-format.md#using-templating) そして [集計の主な例](./message-format.md#template-aggregation-key) を参照して、選択した集計ポリシーに基づいて、メッセージ変換テンプレートに集計ポリシーを含める方法を理解してください。

### ベストエフォート集計 {#best-effort-aggregation}

>[!TIP]
>
>API エンドポイントが API 呼び出しごとに受け入れるプロファイル数が 100 未満の場合は、このオプションを使用します。

このオプションは、リクエストあたりのプロファイル数が少ない宛先で最適で、より少ないデータでより多くのリクエストを取得する方が、より少ないデータでより少ないリクエストを取得する方が効果的です。

以下を使用： `maxUsersPerRequest` パラメーターを使用して、宛先がリクエストで取得できるプロファイルの最大数を指定します。

### 設定可能な集計 {#configurable-aggregation}

このオプションは、同じ呼び出しで数千のプロファイルを含む大きなバッチを取る場合に最も適しています。 また、このオプションを使用すると、複雑な集計ルールに基づいて、書き出されたプロファイルを集計できます。

このオプションを使用すると、次のことができます。
* 宛先に対する API 呼び出しがおこなわれるまでに集計するプロファイルの最大時間と最大数を設定します。
* 次の条件に基づいて、宛先にマッピングされたエクスポート済みプロファイルを集計します。
   * Segment ID;
   * セグメントのステータス。
   * ID または ID のグループ。

集計パラメータの詳細については、 [宛先 API エンドポイントの操作](./destination-configuration-api.md) 参照ページ。各パラメータについて説明します。

## 過去のプロファイルの選定 {#profile-backfill}

以下を使用して、 `backfillHistoricalProfileData` パラメーターを使用して、過去のプロファイル選定を宛先に書き出す必要があるかどうかを指定します。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `backfillHistoricalProfileData` | ブール値 | 宛先に対してセグメントをアクティブ化する際に、履歴プロファイルデータを書き出すかどうかを制御します。 <br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前に、そのセグメントの対象として認定された過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |

## この設定により、宛先に必要なすべての情報が接続される仕組み {#connecting-all-configurations}

一部の宛先設定は、 [宛先サーバー](./server-and-template-configuration.md) または [オーディエンスメタデータ設定](./audience-metadata-management.md). ここで説明する宛先設定では、次のように、他の 2 つの設定を参照することで、これらの設定をすべて接続します。

* 以下を使用： `destinationServerId` を使用して、目的の宛先に対して設定された宛先サーバーおよびテンプレート設定を参照します。
* 以下を使用： `audienceMetadataId` を使用して、目的の宛先に対して設定されたオーディエンスメタデータを参照します。
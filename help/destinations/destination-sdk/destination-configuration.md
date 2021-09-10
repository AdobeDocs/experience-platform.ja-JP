---
description: この設定を使用すると、宛先名、カテゴリ、説明、ロゴなどの基本情報を指定できます。 また、この設定の設定によって、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができるIDも決まります。
title: 宛先SDKの宛先設定オプション
exl-id: b7e4db67-2981-4f18-b202-3facda5c8f0b
source-git-commit: 9be8636b02a15c8f16499172289413bc8fb5b6f0
workflow-type: tm+mt
source-wordcount: '1527'
ht-degree: 5%

---

# 宛先の設定 {#destination-configuration}

## 概要 {#overview}

この設定を使用すると、宛先名、カテゴリ、説明、ロゴなどの基本情報を指定できます。 また、この設定の設定によって、Experience Platformユーザーが宛先に対して認証する方法、Experience Platformユーザーインターフェイスでの表示方法、宛先に書き出すことができるIDも決まります。

このドキュメントで説明する機能は、`/authoring/destinations` APIエンドポイントを使用して設定できます。 エンドポイントで実行できる操作の完全なリストについては、[宛先APIエンドポイントの操作](./destination-configuration-api.md)をお読みください。

## 設定例 {#example-configuration}

架空の宛先Moviestarの設定例を以下に示します。この宛先は、世界中の4つの場所にエンドポイントを持っています。 宛先は、モバイル宛先カテゴリに属します。 次の節では、この設定の構築方法を詳しく説明します。

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
         "acceptsCustomNamespaces":true
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
   "inputSchemaId":"cc8621770a9243b98aba4df79898b1ed",
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
   }
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 宛先カタログ内の宛先のタイトルをExperience Platformします。 |
| `description` | 文字列 | Adobeが宛先カードの宛先カタログで使用するExperience Platformの説明を入力します。 4～5文以下を目指す。 |
| `status` | 文字列 | 宛先カードのライフサイクルステータスを示します。 指定できる値は、`TEST`、`PUBLISHED`、`DELETED` です。宛先を最初に設定する際には、`TEST`を使用します。 |

{style=&quot;table-layout:auto&quot;}

## 顧客認証設定 {#customer-authentication-configurations}

このセクションでは、Experience Platformユーザーインターフェイスのアカウントページが生成され、ユーザーは、宛先にExperience Platformを接続するアカウントに接続します。 `authType`フィールドに指定したExperience Platformオプションに応じて、次のようにユーザー用の認証ページが生成されます。

**ベアラ認証**

ユーザーは、宛先から取得したbearerトークンを入力する必要があります。

![ベアラ認証を使用したUIレンダリング](./assets/bearer-authentication-ui.png)

**OAuth 2認証**

ユーザーは、「**[!UICONTROL 宛先に接続]**」を選択して、OAuth 2認証フローを宛先にトリガーします。

![OAuth 2認証を使用したUIレンダリング](./assets/oauth2-authentication-ui.png)


| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `customerAuthenticationConfigurations` | 文字列 | サーバーに対するExperience Platform顧客の認証に使用される設定を示します。 使用可能な値については、以下の`authType`を参照してください。 |
| `authType` | 文字列 | 指定できる値は`OAUTH2, BEARER`です。<br><ul><li> 宛先がOAuth 2認証をサポートしている場合は、「宛先SDK OAuth 2認証」ページに示すように、「`OAUTH2`」値を選択し、OAuth 2に必要なフィールドを追加します。 さらに、「[宛先の配信セクション](./destination-configuration.md)」で「`authenticationRule=CUSTOMER_AUTHENTICATION`」を選択する必要があります。 </li><li>bearer認証の場合、`BEARER`を選択し、[宛先配信セクション](./destination-configuration.md)で`authenticationRule=CUSTOMER_AUTHENTICATION`を選択します。</li></ul> |

{style=&quot;table-layout:auto&quot;}

## 顧客データフィールド {#customer-data-fields}

このセクションでは、パートナーがカスタムフィールドを導入できます。 上記の例の設定では、`customerDataFields`は、ユーザーが認証フローでエンドポイントを選択し、宛先を持つ顧客IDを示す必要があります。 次に示すように、設定は認証フローに反映されます。

![カスタムフィールド認証フロー](./assets/custom-field-authentication-flow.png)

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `name` | 文字列 | 導入するカスタムフィールドの名前を指定します。 |
| `type` | 文字列 | 導入するカスタムフィールドのタイプを示します。 指定できる値は`string`、`object`、`integer`です。 |
| `title` | 文字列 | ユーザーインターフェイスでユーザーに表示されるフィールドの名前をExperience Platformします。 |
| `description` | 文字列 | カスタムフィールドの説明を入力します。 |
| `isRequired` | Boolean | 宛先設定ワークフローでこのフィールドが必須かどうかを示します。 |
| `enum` | 文字列 | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `pattern` | 文字列 | 必要に応じて、カスタムフィールドのパターンを適用します。 正規表現を使用してパターンを適用します。 例えば、顧客IDに数値やアンダースコアが含まれない場合は、このフィールドに`^[A-Za-z]+$`と入力します。 |

{style=&quot;table-layout:auto&quot;}

## UI属性 {#ui-attributes}

この節では、Adobe Experience Platformユーザーインターフェイスの宛先にAdobeが使用する、上記の設定のUI要素について説明します。 次を参照してください。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `documentationLink` | 文字列 | 宛先の[宛先カタログ](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/overview.html?lang=en#catalog)のドキュメントページを参照します。 `http://www.adobe.com/go/destinations-YOURDESTINATION-en`を使用します。`YOURDESTINATION`は宛先の名前です。 Moviestarという宛先の場合、`http://www.adobe.com/go/destinations-moviestar-en` |
| `category` | 文字列 | Adobe Experience Platformで宛先に割り当てられたカテゴリを参照します。 詳しくは、[宛先カテゴリ](https://experienceleague.adobe.com/docs/experience-platform/destinations/destination-types.html)を参照してください。 次のいずれかの値を使用します。`adobeSolutions, advertising, analytics, cdp, cloudStorage, crm, customerSuccess, database, dmp, ecommerce, email, emailMarketing, enrichment, livechat, marketingAutomation, mobile, personalization, protocols, social, streaming, subscriptions, surveys, tagManagers, voc, warehouses, payments`. |
| `connectionType` | 文字列 | `Server-to-server` は現在、唯一のオプションです。 |
| `frequency` | 文字列 | `Streaming` は現在、唯一のオプションです。 |

{style=&quot;table-layout:auto&quot;}

## マッピング手順のスキーマ設定 {#schema-configuration}

![マッピング手順の有効化](./assets/enable-mapping-step.png)

`schemaConfig`のパラメーターを使用して、宛先のアクティベーションワークフローのマッピング手順を有効にします。 以下に説明するパラメーターを使用することで、Experience Platformユーザーがプロファイル属性やIDを宛先の側の目的のスキーマにマッピングできるかどうかを判断できます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `profileFields` | 配列 | *上記の設定例では示していません。* 定義済みのを追加す `profileFields`ると、ユーザーはExperience Platform属性を宛先側の定義済み属性にマッピングするオプションを使用できます。 |
| `profileRequired` | Boolean | 上記の設定例に示すように、Experience Platformから宛先側のカスタム属性にプロファイル属性をマッピングできる場合は、`true`を使用します。 |
| `segmentRequired` | Boolean | 常に`segmentRequired:true`を使用します。 |
| `identityRequired` | Boolean | ユーザーがID名前空間をExperience Platformから目的のスキーマにマッピングできる場合は、`true`を使用します。 |

{style=&quot;table-layout:auto&quot;}

## IDと属性 {#identities-and-attributes}

この節のパラメーターは、Experience Platformユーザーインターフェイスのマッピング手順でターゲットのIDと属性を入力する方法を決定します。ユーザーは、XDMスキーマを宛先のスキーマにマッピングします。

Adobeは、顧客が宛先に書き出すことができる[!DNL Platform] IDを把握する必要があります。 例えば、[!DNL Experience Cloud ID]、ハッシュ化された電子メール、デバイスID([!DNL IDFA]、[!DNL GAID])などがあります。 これらの値は、顧客が宛先のID名前空間にマッピングできる[!DNL Platform] ID名前空間です。

ID名前空間では、[!DNL Platform]と宛先の間に1対1の対応関係は必要ありません。
例えば、お客様は、[!DNL Platform] [!DNL IDFA]名前空間を宛先の[!DNL IDFA]名前空間にマッピングするか、同じ[!DNL Platform] [!DNL IDFA]名前空間を宛先の[!DNL Customer ID]名前空間にマッピングできます。

詳しくは、[ID名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)を参照してください。

![UIでのターゲットIDのレンダリング](./assets/target-identities-ui.png)

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `acceptsAttributes` | Boolean | 宛先が標準のプロファイル属性を受け入れるかどうかを示します。 通常、これらの属性はパートナーのドキュメントで強調表示されます。 |
| `acceptsCustomNamespaces` | Boolean | 顧客が宛先にカスタム名前空間を設定できるかどうかを示します。 |
| `allowedAttributesTransformation` | 文字列 | *サンプルの設定*&#x200B;では示されていません。例えば、[!DNL Platform]の顧客が属性としてプレーンなEメールアドレスを持ち、プラットフォームがハッシュ化されたEメールのみを受け入れる場合に使用します。 ここで、適用する必要がある変換（例えば、Eメールを小文字に変換し、ハッシュ化）を指定します。 |
| `acceptedGlobalNamespaces` | - | *サンプルの設定*&#x200B;では示されていません。プラットフォームが[標準のID名前空間](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=en#standard-namespaces)（IDFAなど）を受け入れる場合に使用されるので、PlatformユーザーがこれらのID名前空間を選択するように制限できます。 |

{style=&quot;table-layout:auto&quot;}

## 宛先の配信 {#destination-delivery}

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationRule` | 文字列 | [!DNL Platform]ユーザーが宛先に接続する方法を示します。 指定できる値は`CUSTOMER_AUTHENTICATION`、`PLATFORM_AUTHENTICATION`、`NONE`です。<br> <ul><li>Platformのお客様が、ユーザー名とパスワード、ベアラートークン、または別の認証方法でシステムにログインする場合は、`CUSTOMER_AUTHENTICATION`を使用します。 例えば、`customerAuthenticationConfigurations`で`authType: OAUTH2`または`authType:BEARER`も選択した場合は、このオプションを選択します。 </li><li> Adobeと宛先の間にグローバル認証システムがあり、[!DNL Platform]のお客様が宛先に接続するための認証資格情報を提供する必要がない場合は、`PLATFORM_AUTHENTICATION`を使用します。 この場合、[Credentials](./credentials-configuration.md)設定を使用してcredentialsオブジェクトを作成する必要があります。 </li><li>宛先プラットフォームにデータを送信するために認証が必要ない場合は、`NONE`を使用します。 </li></ul> |
| `destinationServerId` | 文字列 | この宛先に使用される[宛先サーバー設定](./destination-server-api.md)の`instanceId`。 |
| `backfillHistoricalProfileData` | Boolean | 宛先に対してセグメントをアクティブ化した場合に、履歴プロファイルデータを書き出すかどうかを制御します。<br> <ul><li> `true`: [!DNL Platform] は、セグメントがアクティブ化される前にセグメントに適合した過去のユーザープロファイルを送信します。 </li><li> `false`: [!DNL Platform] には、セグメントがアクティブ化された後にセグメントに適合するユーザープロファイルのみが含まれます。 </li></ul> |

{style=&quot;table-layout:auto&quot;}

## セグメントマッピングの設定 {#segment-mapping}

![「 Segment mapping configuration 」セクション](./assets/segment-mapping-configuration.png)

宛先設定のこの節では、セグメント名やIDなどのセグメントメタデータを、Experience Platformと宛先の間で共有する方法について説明します。

`audienceTemplateId`を通じて、この設定を[オーディエンスメタデータ設定](./audience-metadata-management.md)と結び付けます。

上記の設定に示すパラメーターは、[宛先エンドポイントAPIリファレンス](./destination-configuration-api.md)に記載されています。

## 集計ポリシー {#aggregation}

![構成テンプレートの集計ポリシー](./assets/aggregation-configuration.png)

このセクションでは、宛先にデータをエクスポートする際にExperience Platformが使用する集計ポリシーを設定できます。

集計ポリシーは、エクスポートされたプロファイルをデータエクスポートで結合する方法を決定します。 次の選択肢があります。
* ベストエフォート集計
* 設定可能な集計（上の設定で表示）

### ベストエフォート集計 {#best-effort-aggregation}

>[!TIP]
>
>APIエンドポイントがAPI呼び出しに対して受け入れるプロファイルの数が100未満の場合は、このオプションを使用します。

このオプションは、リクエストあたりのプロファイル数を減らし、より少ないデータでより多くのリクエストを取るのに対して、より少ないデータでより多くのリクエストを取るのに最適です。

`maxUsersPerRequest`パラメーターを使用して、宛先がリクエストで取得できるプロファイルの最大数を指定します。

### 設定可能な集計 {#configurable-aggregation}

このオプションは、同じ呼び出しで数千のプロファイルを含む大きなバッチを取る場合に最適です。 また、このオプションを使用すると、複雑な集計ルールに基づいて、エクスポートされたプロファイルを集計できます。

このオプションを使用すると、次の操作を実行できます。
* 宛先に対してAPI呼び出しがおこなわれるまでに集計するプロファイルの最大時間と数を設定します。
* 次の項目に基づいて、宛先にマッピングされたエクスポート済みプロファイルを集計します。
   * セグメントID
   * セグメントステータス
   * IDまたはIDのグループ

集計パラメーターの詳細については、 [宛先APIエンドポイントの操作](./destination-configuration-api.md)リファレンスページを参照してください。各パラメーターについて説明します。

<!--

commenting out the `backfillHistoricalProfileData` parameter, which will only be used after an April release

|`backfillHistoricalProfileData` | Boolean | Controls whether historical profile data is exported when segments are activated to the destination. <br> <ul><li> `true`: [!DNL Platform] sends the historical user profiles that qualified for the segment before the segment is activated. </li><li> `false`: [!DNL Platform] only includes user profiles that qualify for the segment after the segment is activated. </li></ul> |

-->

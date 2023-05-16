---
description: Destination SDKを使用して作成された宛先の宛先配信設定を設定し、書き出したデータの送信先と、データの送信先で使用される認証ルールを指定する方法について説明します。
title: 宛先配信
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 8%

---


# 宛先配信

宛先に書き出されるデータの宛先をより詳細に制御するために、Destination SDKで宛先配信設定を指定できます。

宛先配信セクションは、書き出されたデータの送信先と、データの送信先で使用される認証ルールを示します。

<!-- When configuring a destination, you must specify an authentication rule and one or more `destinationServerId` parameters, corresponding to the destination servers that define where the data will be delivered to. In most cases, the authentication rule that you should use is `CUSTOMER_AUTHENTICATION`.  -->

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、次の宛先設定の概要ページを参照してください。

* [Destination SDK を使用したストリーミングの宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-destination-configuration)

宛先の配信設定は、 `/authoring/destinations` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先で使用できる、サポートされるすべての宛先配信オプションについて説明します。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | ○ |

## サポートされているパラメーター {#supported-parameters}

宛先の配信を設定する際に、次の表で説明するパラメーターを使用して、エクスポートするデータの送信先を定義できます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationRule` | 文字列 | 方法を示します [!DNL Platform] は宛先に接続する必要があります。 サポートされている値。<ul><li>`CUSTOMER_AUTHENTICATION`:Platform のお客様が、以下に説明するいずれかの認証方法を使用してシステムにログインした場合は、このオプションを使用します [ここ](customer-authentication.md).</li><li>`PLATFORM_AUTHENTICATION`:Adobeと宛先の間にグローバル認証システムがある場合、このオプションを使用します。また、 [!DNL Platform] のお客様は、宛先に接続するための認証資格情報を提供する必要はありません。 この場合、 [資格情報 API](../../credentials-api/create-credential-configuration.md) 設定。 </li><li>`NONE`:宛先プラットフォームにデータを送信するために認証が必要ない場合は、このオプションを使用します。 </li></ul> |
| `destinationServerId` | 文字列 | この `instanceId` の [宛先サーバー](../../authoring-api/destination-server/create-destination-server.md) を選択します。 |
| `deliveryMatchers.type` | 文字列 | <ul><li>ファイルベースの宛先の宛先の宛先配信を設定する場合、常にこれをに設定します。 `SOURCE`.</li><li>ストリーミング先の宛先配信を設定する場合、 `deliveryMatchers` セクションは必須ではありません。</li></ul> |
| `deliveryMatchers.value` | 文字列 | <ul><li>ファイルベースの宛先の宛先の宛先配信を設定する場合、常にこれをに設定します。 `batch`.</li><li>ストリーミング先の宛先配信を設定する場合、 `deliveryMatchers` セクションは必須ではありません。</li></ul> |

{style="table-layout:auto"}

## ストリーミング宛先の宛先配信設定 {#destination-delivery-streaming}

次の例は、ストリーミング先に対して宛先の配信設定を設定する方法を示しています。 なお、 `deliveryMatchers` ストリーミング先の場合、セクションは不要です。

>[!BEGINSHADEBOX]

```json
{
   "destinationDelivery":[
      {
         "authenticationRule":"CUSTOMER_AUTHENTICATION",
         "destinationServerId":"{{destinationServerId}}"
      }
   ]
}
```

>[!ENDSHADEBOX]

## ファイルベースの宛先の宛先配信設定 {#destination-delivery-file-based}

次の例は、ファイルベースの宛先に対して宛先の配信設定を設定する方法を示しています。 なお、 `deliveryMatchers` セクションは、ファイルベースの宛先の場合は必須です。

>[!BEGINSHADEBOX]

```json
{
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
   ]
}
```

>[!ENDSHADEBOX]

## 次の手順 {#next-steps}

この記事を読むと、ストリーミングとファイルベースの両方の宛先について、宛先でデータを書き出す場所を設定する方法をより深く理解できるはずです。

その他の宛先コンポーネントについて詳しくは、次の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [UI 属性](ui-attributes.md)
* [顧客データフィールド](customer-data-fields.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間の設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [オーディエンスメタデータの設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
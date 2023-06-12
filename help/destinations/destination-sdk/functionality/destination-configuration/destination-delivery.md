---
description: Destination SDK で作成された宛先に対して、書き出されたデータの移動先や、データが到達する場所で使用される認証ルールを示す、宛先配信設定の設定方法を説明します。
title: 宛先配信
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 100%

---


# 宛先配信

宛先に書き出されたデータが到達する場所をより正確に制御するために、Destination SDK では、宛先配信設定を指定できます。

宛先配信セクションは、書き出されたデータの移動先や、データが到達する場所で使用される認証ルールを示します。

<!-- When configuring a destination, you must specify an authentication rule and one or more `destinationServerId` parameters, corresponding to the destination servers that define where the data will be delivered to. In most cases, the authentication rule that you should use is `CUSTOMER_AUTHENTICATION`.  -->

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、以下の宛先設定の概要ページを参照してください。

* [Destination SDK を使用したストリーミング宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-destination-configuration)

`/authoring/destinations` エンドポイントを介して宛先配信設定を設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての宛先配信オプションについて説明します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○ |

## サポートされるパラメーター {#supported-parameters}

宛先配信設定を設定する際に、以下の表で説明されているパラメーターを使用して、書き出されたデータが送信される場所を定義できます。

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `authenticationRule` | 文字列 | [!DNL Platform] が宛先に接続する方法を示します。サポートされている値：<ul><li>`CUSTOMER_AUTHENTICATION`：Platform の顧客が[こちら](customer-authentication.md)で説明しているいずれかの認証方法でお使いのシステムにログインする場合は、このオプションを使用します。</li><li>`PLATFORM_AUTHENTICATION`：アドビと宛先との間にグローバル認証システムがあり、[!DNL Platform] の顧客が宛先への接続に認証資格情報を提供する必要がない場合は、このオプションを使用します。この場合、[資格情報 API 設定](../../credentials-api/create-credential-configuration.md)を使用して、資格情報オブジェクトを作成する必要があります。 </li><li>`NONE`：宛先プラットフォームにデータを送信するために認証が必要ない場合は、このオプションを使用します。 </li></ul> |
| `destinationServerId` | 文字列 | データの書き出し先にする[宛先サーバー](../../authoring-api/destination-server/create-destination-server.md)の `instanceId`。 |
| `deliveryMatchers.type` | 文字列 | <ul><li>ファイルベースの宛先用に宛先配信を設定する場合は、常に、これを `SOURCE` に設定します。</li><li>ストリーミング宛先用に宛先配信を設定する場合は、`deliveryMatchers` セクションは必須ではありません。</li></ul> |
| `deliveryMatchers.value` | 文字列 | <ul><li>ファイルベースの宛先用に宛先配信を設定する場合は、常に、これを `batch` に設定します。</li><li>ストリーミング宛先用に宛先配信を設定する場合は、`deliveryMatchers` セクションは必須ではありません。</li></ul> |

{style="table-layout:auto"}

## ストリーミング宛先用の宛先配信設定 {#destination-delivery-streaming}

以下の例に、宛先配信設定がどのようにストリーミング宛先用に設定される必要があるかを示します。`deliveryMatchers` セクションは、ストリーミング宛先に必須ではないことに注意してください。

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

## ファイルベースの宛先用の宛先配信設定 {#destination-delivery-file-based}

以下の例に、宛先配信設定がどのようにファイルベースの宛先用に設定される必要があるかを示します。`deliveryMatchers` セクションは、ファイルベースの宛先に必須であることに注意してください。

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

この記事を読むことで、ストリーミングとファイルベースの両方の宛先に対して、宛先がデータを書き出す場所を設定する方法について、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [UI 属性](ui-attributes.md)
* [顧客データフィールド](customer-data-fields.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
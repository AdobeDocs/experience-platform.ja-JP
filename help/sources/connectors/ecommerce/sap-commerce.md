---
title: SAP Commerce Sourceの概要
description: API またはユーザーインターフェイスを使用して SAP CommerceをAdobe Experience Platformに接続する方法について説明します。
last-substantial-update: 2023-07-26T00:00:00Z
badge: ベータ版
exl-id: d2ddfec3-a421-48a7-b765-86ce9162f26f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 14%

---

# [!DNL SAP Commerce]

>[!NOTE]
>
>[!DNL SAP Commerce] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../home.md#terms-and-conditions) を参照してください。

[[!DNL SAP Commerce]](https://www.sap.com/india/products/acquired-brands/what-is-hybris.html) は、B2B および B2C 企業向けのクラウドベースの e コマースプラットフォームソリューションで、SAP カスタマーエクスペリエンスのポートフォリオの一部として利用できます。 [[!DNL SAP]  サブスクリプション請求 ](https://www.sap.com/products/financial-management/subscription-billing.html) は、ポートフォリオの製品であり、標準化された統合によってシンプルな販売および支払いエクスペリエンスで完全なサブスクリプションライフサイクル管理を可能にします。

[!DNL SAP Commerce] ソースを使用すると、以下の [[!DNL SAP]  サブスクリプション請求 ](https://www.sap.com/products/financial-management/subscription-billing.html) ビジネスパートナー API エンドポイントからExperience Platformに顧客および連絡先情報を取り込むことができます。

* [ 顧客 ](https://api.sap.com/api/BusinessPartner_APIs/path/GET_customers)
* [ 連絡先 ](https://api.sap.com/api/BusinessPartner_APIs/path/GET_contacts)

さらに、[!DNL SAP Commerce] が実行されて顧客データが取得される場合、[ 顧客と連絡先の関係 ](https://api.sap.com/api/BusinessPartner_APIs/path/GET_relationships-customer-contacts) API も呼び出されて、顧客の連絡先情報が取得されます。

## IP アドレス許可リスト {#ip-allow-list}

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要がある場合があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件 {#prerequisites}

[!DNL SAP Commerce] データをExperience Platformに取り込むには、まず、次の点を確認する必要があります。

* [!DNL SAP Subscription Billing] アカウント。 有効な請求アカウントをお持ちでない場合は、[!DNL SAP] アカウントマネージャーにお問い合わせください。 詳しくは、[[!DNL SAP] Platform 設定 ](https://help.sap.com/doc/5fd179965d5145fbbe7f2a7aa1272338/latest/en-US/PlatformConfiguration.pdf) ドキュメントを参照してください。

* サ [!DNL SAP] ビスキー。 [!DNL SAP] サービスキーを使用すると、Experience Platformから [!DNL SAP Subscription Billing] API にアクセスできます。 [!DNL SAP Commerce] には、以下が必要です。
   * クライアント ID
   * クライアントシークレット
   * URL。 URL パターンは次のとおりです。`https://subscriptionbilling.authentication.eu10.hana.ondemand.com` この値は、後で API を使用して [ ベース接続を作成 ](../../tutorials/api/create/ecommerce/sap-commerce.md#base-connection) する場合、またはExperience Platform UI を使用して [ アカウントを接続  [!DNL SAP Commerce]  する ](../../tutorials/ui/create/ecommerce/sap-commerce.md#connect-account) 場合に、`region` および `tokenEndpoint` の値を取得するために使用されます。

+++選択して、サービスキーの例を確認します

```json
{ 
    "url": "https://eu10.revenue.cloud.sap/api",
    "uaa": {
        "clientid": "XXX",
        "clientsecret": "XXX",
        "url": "https://subscriptionbilling.authentication.eu10.hana.ondemand.com",
        "identityzone": "subscriptionbilling",
        "identityzoneid": "XXX",
        "tenantid": "XXX",
        "tenantmode": "dedicated",
        "sburl": "https://internal-xsuaa.authentication.eu10.hana.ondemand.com",
        "apiurl": "https://api.authentication.eu10.hana.ondemand.com",
        "verificationkey": "XXX",
        "xsappname": "XXX",
        "subaccountid": "XXX",
        "uaadomain": "authentication.eu10.hana.ondemand.com",
        "zoneid": "XXX",
        "credential-type": "binding-secret"
    },
    "vendor": "SAP"
}
```

+++

## 次の手順

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL SAP Commerce] をExperience Platformに接続する方法について説明しています。

* [ ソース接続とデータフローを作成し、API を使用してExperience Platformに  [!DNL SAP Commerce]  ータを取り込みます ](../../tutorials/api/create/ecommerce/sap-commerce.md)。
* [UI を使用してアカウ  [!DNL SAP Commerce]  トをExperience Platformに接続します ](../../tutorials/ui/create/ecommerce/sap-commerce.md)。
* [UI を使用したソースのデータフローの作成](../../tutorials/ui/dataflow/ecommerce.md)

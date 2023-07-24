---
title: SAP Commerce ソースの概要
description: API またはユーザーインターフェイスを使用して SAP Commerce をAdobe Experience Platformに接続する方法を説明します。
last-substantial-update: 2023-07-26T00:00:00Z
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 99edb8b2bcd4225235038e966a367d91375c961a
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 17%

---

# [!DNL SAP Commerce]

>[!NOTE]
>
>[!DNL SAP Commerce] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

[[!DNL SAP Commerce]](https://www.sap.com/india/products/acquired-brands/what-is-hybris.html)は、B2B および B2C 企業向けのクラウドベースの e コマースプラットフォームソリューションで、SAP Customer Experience ポートフォリオの一部として利用できます。 [[!DNL SAP] サブスクリプション請求](https://www.sap.com/products/financial-management/subscription-billing.html) は、ポートフォリオの下の製品で、標準化された統合による販売と支払いの経験をシンプル化し、完全なサブスクリプションライフサイクル管理を可能にします。

この [!DNL SAP Commerce] ソースを使用すると、 [[!DNL SAP] サブスクリプション請求](https://www.sap.com/products/financial-management/subscription-billing.html) 以下のビジネスパートナー API エンドポイント：

* [顧客](https://api.sap.com/api/BusinessPartner_APIs/path/GET_customers)
* [連絡先](https://api.sap.com/api/BusinessPartner_APIs/path/GET_contacts)

また、 [!DNL SAP Commerce] を実行して顧客データを取得する場合、 [顧客と連絡先の関係](https://api.sap.com/api/BusinessPartner_APIs/path/GET_relationships-customer-contacts) また、API は、顧客の連絡先情報を取得するためにも呼び出されます。

## IP アドレス許可リスト {#ip-allow-list}

ソースコネクタを使用する前に、IP アドレスのリストを許可リストに追加する必要が生じる場合があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## 前提条件 {#prerequisites}

次を持ってくる前に [!DNL SAP Commerce] Experience Platformにデータを送信する場合は、まず、次の条件を満たす必要があります。

* A [!DNL SAP Subscription Billing] アカウント 有効な請求アカウントをまだお持ちでない場合は、 [!DNL SAP] アカウントマネージャー。 詳しくは、 [[!DNL SAP] プラットフォーム設定](https://help.sap.com/doc/5fd179965d5145fbbe7f2a7aa1272338/latest/en-US/PlatformConfiguration.pdf) ドキュメントを参照してください。

* [!DNL SAP] サービスキー。 この [!DNL SAP] サービスキーを使用すると、 [!DNL SAP Subscription Billing] Experience Platformを介した API [!DNL SAP Commerce] には、以下が必要です。
   * クライアント ID
   * クライアントシークレット
   * URL. URL のパターンは次のとおりです。 `https://subscriptionbilling.authentication.eu10.hana.ondemand.com`. この値は後での値の取得に使用されます。 `region` および `tokenEndpoint` いつ [ベース接続を作成](../../tutorials/api/create/ecommerce/sap-commerce.md#base-connection) API を使用する場合、または [接続 [!DNL SAP Commerce] アカウント](../../tutorials/ui/create/ecommerce/sap-commerce.md#connect-account) Platform UI を使用します。

+++「 」を選択して、サービスキーの例を表示します

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

以下のドキュメントでは、API やユーザーインターフェイスを使用して [!DNL SAP Commerce] と Platform を接続する方法について説明します。

* [ソース接続とデータフローを作成して、 [!DNL SAP Commerce] API を使用した Platform へのデータの取得](../../tutorials/api/create/ecommerce/sap-commerce.md).
* [接続 [!DNL SAP Commerce] UI を使用してExperience Platformにアカウント](../../tutorials/ui/create/ecommerce/sap-commerce.md).
* [UI を使用したソースのデータフローの作成](../../tutorials/ui/dataflow/ecommerce.md)

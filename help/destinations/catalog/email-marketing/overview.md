---
keywords: メール;メール;メール;メールの宛先
title: メールマーケティングの宛先の概要
type: Tutorial
description: メールサービスプロバイダー（ESP）を利用すると、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できます。 Experience Platformの配信先としてサポートされているESPについて説明します。
exl-id: e07f8c5a-0424-4de5-810f-3d5711ef4606
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 59%

---

# メールマーケティングの宛先の概要 {#email-marketing-destinations}

## 概要 {#overview}

メールサービスプロバイダー（ESP）を使用すると、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できます。[!DNL Adobe Experience Platform]は、メールマーケティングの宛先に対してオーディエンスをアクティブ化できるようにすることで、ESPと統合します。

## サポートされているメールマーケティングの宛先 {#supported-destinations}

[!DNL Adobe Experience Platform]は、次のメールマーケティング宛先をサポートしています：

* [Adobe Campaign](adobe-campaign.md)
* [Adobe Campaign Managed Cloud Services](adobe-campaign-managed-services.md)
* [Mailchimp の興味カテゴリ](mailchimp-interest-categories.md)
* [Mailchimp タグ](mailchimp-tags.md)
* [（API）Oracle Eloqua](oracle-eloqua-api.md)
* [&#x200B; （API）  [!DNL Salesforce Marketing Cloud]](salesforce-marketing-cloud-exact-target.md)
* [（ファイル）OracleEloqua](oracle-eloqua.md)
* [（ファイル）  [!DNL Salesforce Marketing Cloud]](salesforce-marketing-cloud.md)
* [[!DNL Salesforce Marketing Cloud Account Engagement]](salesforce-marketing-cloud-account-engagement.md)
* [Oracle Responsys](oracle-responsys.md)
* [SendGrid](sendgrid.md)

## 新しいメールマーケティングの宛先への接続 {#connect-destination}

キャンペーンのメールマーケティング宛先にオーディエンスを送信するには、Experience Platformが最初に宛先に接続する必要があります。 新しい宛先の設定について詳しくは、[宛先の作成に関するチュートリアル](../../ui/connect-destination.md)を参照してください。

## メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス {#best-practices}

### ID の選択 {#identity}

[結合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の ID を選択することをお勧めします。これは、ユーザーの ID をキーオフにするフィールドです。一般的には、このフィールドはメールアドレスですが、ロイヤルティプログラム ID や電話番号にすることもできます。スキーマで最も一般的な一意の ID とその XDM フィールドについては、次の表を参照してください。

| 一意の ID | 統合スキーマの XDM フィールド |
|----------------- | ---------------------------|
| メールアドレス | `personalEmail.address` |
| 電話番号 | `mobilePhone.number` |
| ロイヤルティプログラム ID | `Customer-defined XDM field` |

{style="table-layout:auto"}

### その他の宛先属性 {#other-destination-attributes}

「スキーマ」フィールドセレクターで、電子メールの送信先に書き出しする他のフィールドを選択します。次のオプションが推奨されます。

| スキーマ | XDM フィールド |
|------ | ---------|
| 名 | `person.name.firstName` |
| 姓 | `person.name.lastName` |
| 電話番号 | `mobilePhone.number` |
| 住所（市区町村） | `homeAddress.city` |
| 住所（都道府県） | `homeAddress.stateProvince` |
| 住所（郵便番号） | `homeAddress.postalCode` |
| 誕生日 | `person.birthDayAndMonth` |
| セグメントのメンバーシップ | `segmentMembership.status` |

{style="table-layout:auto"}

## メールマーケティングの宛先にオーディエンスをアクティベート {#activate}

カタログ内の一部のメールマーケティング宛先では、宛先とのAPI統合を通じて、ストリーミング形式でプロファイルを書き出します。

その他の宛先は、ファイルをクラウドストレージの場所に書き出します。 エクスポートが完了したら、クラウドストレージの場所からメールマーケティングの宛先にデータをインポートする必要があります。

「[&#x200B; サポートされるメールマーケティング宛先](#supported-destinations)」セクションのリンクに従って、各メールマーケティング宛先に対してオーディエンスをアクティブ化する方法を説明します。

## その他のリソース {#additional-resources}

* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)
* [Flow Service API を使用したメールマーケティングの宛先の作成とデータのアクティブ化](../../api/connect-activate-batch-destinations.md)

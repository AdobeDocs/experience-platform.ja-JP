---
keywords: メール;メール;メール;メールの宛先
title: メールマーケティングの宛先の概要
type: Tutorial
description: メールサービスプロバイダー（ESP）を使用すると、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できます。宛先としてサポートされる ESP をExperience Platformします。
exl-id: e07f8c5a-0424-4de5-810f-3d5711ef4606
source-git-commit: d6ea94b275ab0ed7c0638200188fe7ada7bacf5c
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 76%

---

# メールマーケティングの宛先の概要 {#email-marketing-destinations}

## 概要 {#overview}

メールサービスプロバイダー（ESP）を使用すると、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できます。Adobe Experience Platform は、メールマーケティングの宛先に対してセグメントをアクティブ化できるようにすることで、ESP と統合されます。

## サポートされているメールマーケティングの宛先 {#supported-destinations}

Adobe Experience Platform は、次のメールマーケティングの宛先をサポートしています。

* [Adobe Campaign](adobe-campaign.md)
* [Adobe Campaign Managed Cloud Services](adobe-campaign-managed-services.md)
* [(API)OracleEloqua](oracle-eloqua-api.md)
* [（API）Salesforce Marketing Cloud](salesforce-marketing-cloud-exact-target.md)
* [（ファイル）OracleEloqua](oracle-eloqua.md)
* [（ファイル） SalesforceMarketing Cloud](salesforce-marketing-cloud.md)
* [Oracle Responsys](oracle-responsys.md)
* [SendGrid](sendgrid.md)

## 新しいメールマーケティングの宛先への接続 {#connect-destination}

キャンペーン用のメールマーケティングの宛先に対するセグメントを送信するには、まずは Platform が宛先に接続する必要があります。新しい宛先の設定について詳しくは、[宛先の作成に関するチュートリアル](../../ui/connect-destination.md)を参照してください。

## メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス {#best-practices}

### ID の選択 {#identity}

[ユニオンスキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の ID を選択することをお勧めします。これは、ユーザーの ID をキーオフにするフィールドです。一般的には、このフィールドはメールアドレスですが、ロイヤルティプログラム ID や電話番号にすることもできます。スキーマで最も一般的な一意の ID とその XDM フィールドについては、次の表を参照してください。

| 一意の ID | 統合スキーマの XDM フィールド |
|----------------- | ---------------------------|
| メールアドレス | `personalEmail.address` |
| 電話番号 | `mobilePhone.number` |
| ロイヤリティープログラム ID | `Customer-defined XDM field` |

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

## メールマーケティングの宛先に対してセグメントをアクティブ化 {#activate}

カタログの書き出しプロファイルの一部の電子メールマーケティングの宛先は、宛先との API 統合を通じて、ストリーミング方式で行われます。

クラウドストレージの場所に、その他の宛先の書き出しファイルを書き出します。 書き出しが完了したら、クラウドストレージの場所から電子メールマーケティングの宛先にデータを読み込む必要があります。

リンク先の [サポートされる電子メールマーケティングの宛先](#supported-destinations) の節を参照して、各電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法を確認してください。

## その他のリソース {#additional-resources}

* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)
* [Flow Service API を使用したメールマーケティングの宛先の作成とデータのアクティブ化](../../api/connect-activate-batch-destinations.md)

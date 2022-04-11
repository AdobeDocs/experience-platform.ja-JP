---
keywords: メール;メール;メール;メールの宛先
title: メールマーケティングの宛先の概要
type: Tutorial
description: メールサービスプロバイダー（ESP）を使用すると、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できます。
exl-id: e07f8c5a-0424-4de5-810f-3d5711ef4606
source-git-commit: 9d2e98c834eddcacf67de7caafef4717e38d80f8
workflow-type: ht
source-wordcount: '387'
ht-degree: 100%

---

# メールマーケティングの宛先の概要 {#email-marketing-destinations}

## 概要 {#overview}

メールサービスプロバイダー（ESP）を使用すると、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できます。Adobe Experience Platform は、メールマーケティングの宛先に対してセグメントをアクティブ化できるようにすることで、ESP と統合されます。

Platform は、セグメントを `.csv` ファイル形式で書き出し、目的の場所に配信します。[!DNL Platform] で有効になっているストレージの場所から、お使いのメールマーケティングプラットフォームでのデータの読み込みスケジュールを設定します。データを読み込むプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## サポートされているメールマーケティングの宛先 {#supported-destinations}

Adobe Experience Platform は、次のメールマーケティングの宛先をサポートしています。

* [Adobe Campaign](adobe-campaign.md)
* [Oracle Eloqua](oracle-eloqua.md)
* [Oracle Responsys](oracle-responsys.md)
* [Salesforce Marketing Cloud](salesforce-marketing-cloud.md)

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

### その他の宛先属性

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

## ストレージの場所から宛先へのデータの読み込み {#import-data-into-destination}

ストレージの場所から宛先にデータを読み込む方法については、個々のメールマーケティングの宛先に関する記事を参照してください。

* [Adobe Campaign](adobe-campaign.md)
* [Oracle Eloqua](oracle-eloqua.md)
* [Oracle Responsys](oracle-responsys.md)
* [Salesforce Marketing Cloud](salesforce-marketing-cloud.md)

## メールマーケティングの宛先に対してセグメントをアクティブ化 {#activate}

メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、[プロファイルの一括書き出しの宛先に対してオーディエンスデータをアクティブ化する](../../ui/activate-batch-profile-destinations.md)を参照してください。

## その他のリソース

* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)
* [Flow Service API を使用したメールマーケティングの宛先の作成とデータのアクティブ化](../../api/connect-activate-batch-destinations.md)

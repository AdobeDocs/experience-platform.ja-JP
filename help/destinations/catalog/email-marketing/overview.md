---
keywords: 電子メール；電子メール；電子メールの宛先
title: 電子メールマーケティングの宛先の概要
type: Tutorial
description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
exl-id: e07f8c5a-0424-4de5-810f-3d5711ef4606
source-git-commit: 9d2e98c834eddcacf67de7caafef4717e38d80f8
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 39%

---

# 電子メールマーケティングの宛先の概要 {#email-marketing-destinations}

## 概要 {#overview}

電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。Adobe Experience Platformは、電子メールマーケティングの宛先に対するセグメントをアクティブ化できるようにすることで、ESP と統合されます。

Platform は、セグメントを `.csv` ファイルを作成し、希望の場所に配信します。 電子メールマーケティングプラットフォームで、 [!DNL Platform]. データをインポートするプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## サポートされる電子メールマーケティングの宛先 {#supported-destinations}

Adobe Experience Platformは、次の電子メールマーケティングの宛先をサポートしています。

* [Adobe Campaign](adobe-campaign.md)
* [Oracle Eloqua](oracle-eloqua.md)
* [Oracle Responsys](oracle-responsys.md)
* [Salesforce Marketing Cloud](salesforce-marketing-cloud.md)

## 新しい電子メールマーケティングの宛先への接続 {#connect-destination}

キャンペーンの電子メールマーケティングの宛先にセグメントを送信するには、まず Platform が宛先に接続する必要があります。 詳しくは、 [宛先の作成チュートリアル](../../ui/connect-destination.md) を参照してください。

## 電子メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス {#best-practices}

### ID の選択 {#identity}

Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). これは、ユーザー ID をキーオフにするフィールドです。 最も一般的に、このフィールドは電子メールアドレスですが、ロイヤリティープログラム ID や電話番号を指定することもできます。スキーマで最も一般的な一意の識別子とその XDM フィールドについては、次の表を参照してください。

| 一意の ID | 統合スキーマの XDM フィールド |
|----------------- | ---------------------------|
| 電子メールアドレス | `personalEmail.address` |
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

## ストレージの場所から宛先にデータをインポートします {#import-data-into-destination}

ストレージの場所から宛先にデータをインポートする方法については、個々の電子メールマーケティングの宛先に関する記事をお読みください。

* [Adobe Campaign](adobe-campaign.md)
* [OracleEloqua](oracle-eloqua.md)
* [OracleResponsys](oracle-responsys.md)
* [SalesforceMarketing Cloud](salesforce-marketing-cloud.md)

## 電子メールマーケティングの宛先へのセグメントのアクティブ化 {#activate}

電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md).

## その他のリソース

* [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md)
* [フローサービス API を使用した電子メールマーケティングの宛先の作成とデータのアクティブ化](../../api/connect-activate-batch-destinations.md)

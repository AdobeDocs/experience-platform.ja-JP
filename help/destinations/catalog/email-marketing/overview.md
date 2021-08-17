---
keywords: 電子メール；電子メール；電子メールの宛先
title: 電子メールマーケティングの宛先の概要
type: Tutorial
description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
exl-id: e07f8c5a-0424-4de5-810f-3d5711ef4606
source-git-commit: 802b1844bec1e577e978da5d5a69de87278c04b9
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 39%

---

# 電子メールマーケティングの宛先の概要 {#email-marketing-destinations}

## 概要 {#overview}

電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。Adobe Experience Platformは、電子メールマーケティングの宛先に対してセグメントをアクティブ化できるようにすることで、ESPと統合されます。

Platformは、セグメントを`.csv`ファイルとして書き出し、目的の場所に配信します。 [!DNL Platform]で有効になっているストレージの場所から、電子メールマーケティングプラットフォームにデータのインポートをスケジュールします。 データをインポートするプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## サポートされる電子メールマーケティングの宛先 {#supported-destinations}

Adobe Experience Platformは、次の電子メールマーケティングの宛先をサポートしています。

* [Adobe Campaign](adobe-campaign.md)
* [Oracle Eloqua](oracle-eloqua.md)
* [Oracle Responsys](oracle-responsys.md)
* [Salesforce Marketing Cloud](salesforce-marketing-cloud.md)

## 新しい電子メールマーケティングの宛先への接続 {#connect-destination}

キャンペーンの電子メールマーケティングの宛先にセグメントを送信するには、まずPlatformが宛先に接続する必要があります。 新しい宛先の設定について詳しくは、[宛先の作成に関するチュートリアル](../../ui/connect-destination.md)を参照してください。

## 電子メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス {#best-practices}

### IDの選択 {#identity}

Adobeでは、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。 これは、ユーザーIDをキーオフにするフィールドです。 最も一般的に、このフィールドは電子メールアドレスですが、ロイヤリティープログラム ID や電話番号を指定することもできます。スキーマで最も一般的な一意の識別子とそのXDMフィールドについては、次の表を参照してください。

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

## ストレージの場所から宛先にデータをインポートする {#import-data-into-destination}

ストレージの場所から宛先にデータをインポートする方法については、個々の電子メールマーケティングの宛先に関する記事を参照してください。

* [Adobe Campaign](adobe-campaign.md)
* [OracleEloqua](oracle-eloqua.md)
* [OracleResponsys](oracle-responsys.md)
* [SalesforceMarketing Cloud](salesforce-marketing-cloud.md)

## 電子メールマーケティングの宛先へのセグメントのアクティブ化 {#activate}

電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## その他のリソース

* [宛先へのデータのアクティブ化](../../ui/activate-destinations.md)
* [フローサービスAPIを使用した、電子メールマーケティングの宛先の作成とデータのアクティブ化](../../api/email-marketing.md)

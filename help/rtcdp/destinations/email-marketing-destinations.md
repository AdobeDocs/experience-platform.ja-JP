---
title: 電子メールマーケティングの宛先
seo-title: 電子メールマーケティングの宛先
description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
seo-description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
translation-type: tm+mt
source-git-commit: 50e6b39c1eb0bda4f3b30991515fb1c13fa9ff87

---


# 電子メールマーケティングの宛先 {#email-marketing-destinations}

電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。アドビのリアルタイム顧客データプラットフォームは、電子メールマーケティングの宛先に対するセグメントをアクティブ化できるようにすることで、ESP と統合されます。

キャンペーンの電子メールマーケティングの宛先にセグメントを送信するには、まず Adobe Real-time CDP が宛先に接続する必要があります。

電子メールマーケティングの宛先への接続は、3 つの手順で行います。各手順について、このページで詳しく説明します。

次の節で説明する接続フローで、Amazon S3 または SFTP に接続します。Real-time CDP は、セグメントを `.csv` または `.txt` ファイルとして書き出し、希望の場所に配信します。Real-time CDP で有効になっているストレージの場所から、電子メールマーケティングプラットフォームにデータのインポートをスケジュールします。データをインポートするプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## 手順 1 — 宛先の接続 {#connect-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;で、接続する電子メールマーケティングの宛先を選択し、「**[!UICONTROL 宛先の接続]**」を選択します。

   ![宛先に接続](/help/rtcdp/destinations/assets/connect-destination.png)

2. 「接続」ウィザードで、ストレージの場所の&#x200B;**[!UICONTROL 接続タイプ]**&#x200B;を選択します。**Amazon S3**、**SFTP（パスワード）**、**SFTP（SSH キー）**&#x200B;の中から選択できます。接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

**S3 接続**&#x200B;の場合、アクセスキー ID とシークレットアクセスキーを指定する必要があります。

**SFTP（パスワード）** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。

**SFTP（SSH キー）** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。

## 手順 2 — 書き出したファイルの宛先属性として使用するスキーマフィールドの選択 {#destination-attributes}

この手順では、電子メールマーケティングの宛先に書き出しするフィールドを選択します。

![宛先属性](/help/rtcdp/destinations/assets/destination-attributes.png)

### ID {#identity}

[ユニオンスキーマ](../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。これは、ユーザーの ID をキーオフにするフィールドです。最も一般的に、このフィールドは電子メールアドレスですが、ロイヤリティープログラム ID や電話番号を指定することもできます。統合スキーマで最も一般的な一意の識別子とその XDM フィールドについては、次の表を参照してください。

| 一意の識別子 | 統合スキーマの XDM フィールド |
---------|----------
| 電子メールアドレス | `personalEmail.address` |
| 電話番号 | `mobilePhone.number` |
| ロイヤリティープログラム ID | `Customer-defined XDM field` |

### その他の宛先属性

「スキーマ」フィールドセレクターで、電子メールの送信先に書き出しする他のフィールドを選択します。次のオプションが推奨されます。

| スキーマ | XDM フィールド |
---------|----------
| 名 | `person.name.firstName` |
| 姓 | `person.name.lastName` |
| 電話番号 | `mobilePhone.number` |
| 住所（市区町村） | `homeAddress.city` |
| 住所（都道府県） | `homeAddress.stateProvince` |
| 住所（郵便番号） | `homeAddress.postalCode` |
| 誕生日 | `person.birthDayAndMonth` |

## 手順 3 — ストレージの場所からインポート先にデータをインポートひます

ストレージの場所から宛先にデータをインポートする方法については、個々の電子メールマーケティングの宛先に関する記事を参照してください。

* [Adobe Campaign](/help/rtcdp/destinations/adobe-campaign-destination.md#import-data-into-campaign)
* [Salesforce Marketing Cloud](/help/rtcdp/destinations/salesforce-marketing-cloud-destination.md#import-data-into-salesforce)
* [Oracle Eloqua](/help/rtcdp/destinations/oracle-eloqua-destination.md#import-data-into-eloqua)
* [Oracle Responsys](/help/rtcdp/destinations/oracle-responsys-destination.md#import-data-into-responsys)

## 電子メールマーケティングの宛先へのセグメントのアクティブ化

電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。
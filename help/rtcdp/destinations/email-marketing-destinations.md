---
title: 電子メールマーケティングの宛先
seo-title: 電子メールマーケティングの宛先
description: 電子メールサービスプロバイダー(ESP)を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
seo-description: 電子メールサービスプロバイダー(ESP)を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
translation-type: tm+mt
source-git-commit: 463212a8fabb9dd5962b4d3f523a6f2d88bb1d9d

---


# 電子メールマーケティングの宛先 {#email-marketing-destinations}

電子メールサービスプロバイダー(ESP)を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。 Adobe Real-time Customer Data Platformは、電子メールマーケティングの宛先に対するセグメントをアクティブ化できるようにすることで、ESPと統合されます。

キャンペーンの電子メールマーケティングの宛先にセグメントを送信するには、Adobe Real-time CDPが最初に宛先に接続する必要があります。

電子メールマーケティングの宛先への接続は、3つの手順で行います。 各手順について、このページで詳しく説明します。

次の節で説明する接続先フローで、Amazon S3またはSFTPに接続します。 リアルタイムCDPは、セグメントをファイルとし `.csv` て書き出 `.txt` し、希望の場所に配信します。 Real-time CDPで有効になっているストレージの場所から、電子メールマーケティングプラットフォームにデータのインポートをスケジュールします。 データをインポートするプロセスは、パートナーごとに異なります。 詳しくは、個々のリンク先の記事を参照してください。

## 手順1 — 接続先 {#connect-destination}

1. 接続/ **[!UICONTROL 宛先で]**、接続する電子メールマーケティングの宛先を選択し、「接続先」を **[!UICONTROL 選択します]**。

   ![宛先に接続](/help/rtcdp/destinations/assets/connect-destination.png)

2. 接続ウィザードで、保存場所の **[!UICONTROL 接続の種類]** を選択します。 Amazon S3 **、** SFTP with Password **、** SFTP with SSH Keyの中から選択できます ****。 接続タイプに応じて以下の情報を入力し、「接続」を選択し **[!UICONTROL ます]**。

**S3接続の場合**、アクセスキーIDとシークレットアクセスキーを指定する必要があります。

パスワ **ード接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名およびパスワードを指定する必要があります。

SSHキー **接続を使用するSFTPの場合は** 、ドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

## 手順2 — 書き出したファイルで出力先属性として使用するスキーマフィールドを選択する {#destination-attributes}

この手順では、電子メールマーケティングの宛先にエクスポートするフィールドを選択します。

![宛先属性](/help/rtcdp/destinations/assets/destination-attributes.png)

### ID {#identity}

ユニオンスキーマから一意の識別子を選択することをお [勧めします](https://www.adobe.io/apis/experienceplatform/home/profile-identity-segmentation/profile-identity-segmentation-services.html#!api-specification/markdown/narrative/technical_overview/unified_profile_architectural_overview/unified_profile_architectural_overview.md)。 これは、ユーザーのIDをキーオフにするフィールドです。 最も一般的に、このフィールドは電子メールアドレスですが、忠誠度プログラムIDや電話番号を指定することもできます。 統合スキーマで最も一般的な一意の識別子とそのXDMフィールドについては、次の表を参照してください。

| 一意識別子 | 統合スキーマのXDMフィールド |
---------|----------
| Email Address | `personalEmail.address` |
| 電話 | `mobilePhone.number` |
| 忠誠度プログラムID | `Customer-defined XDM field` |

### その他の宛先属性

「スキーマ」フィールドセレクターで、電子メールの送信先にエクスポートする他のフィールドを選択します。 次のオプションが推奨されます。

| スキーマ | XDMフィールド |
---------|----------
| 名 | `person.name.firstName` |
| 姓 | `person.name.lastName` |
| 電話 | `mobilePhone.number` |
| 住所（市区町村） | `homeAddress.city` |
| 住所の状態 | `homeAddress.stateProvince` |
| 住所の郵便番号 | `homeAddress.postalCode` |
| 誕生日 | `person.birthDayAndMonth` |

## 手順3 — ストレージの場所からインポート先にデータをインポートする

保存場所から宛先にデータをインポートする方法については、個々の電子メールマーケティングの宛先に関する記事を参照してください。

* [Adobe Campaign](/help/rtcdp/destinations/adobe-campaign-destination.md#import-data-into-campaign)
* [Salesforce Marketing Cloud](/help/rtcdp/destinations/salesforce-marketing-cloud-destination.md#import-data-into-salesforce)
* [Oracle Eloqua](/help/rtcdp/destinations/oracle-eloqua-destination.md#import-data-into-eloqua)
* [Oracle Responsys](/help/rtcdp/destinations/oracle-responsys-destination.md#import-data-into-responsys)

## 電子メールマーケティングの宛先へのセグメントのアクティブ化

電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、「宛先へのデータのアクテ [ィブ化」を参照してくださ](/help/rtcdp/destinations/activate-destinations.md)い。
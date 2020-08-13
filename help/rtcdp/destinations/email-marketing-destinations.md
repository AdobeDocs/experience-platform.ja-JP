---
title: 電子メールマーケティングの宛先
seo-title: 電子メールマーケティングの宛先
description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
seo-description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
translation-type: tm+mt
source-git-commit: 6850a1ee5a578a3dccce9f9decd8f6a368705f4a
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 48%

---


# 電子メールマーケティングの宛先 {#email-marketing-destinations}

電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。アドビのリアルタイム顧客データプラットフォームは、電子メールマーケティングの宛先に対するセグメントをアクティブ化できるようにすることで、ESP と統合されます。

キャンペーンの電子メールマーケティングの宛先にセグメントを送信するには、まず アドビのリアルタイム CDP が宛先に接続する必要があります。

電子メールマーケティングの宛先への接続は、3 つの手順で行います。各手順について、このページで詳しく説明します。

次の節で説明する接続フローで、Amazon S3 または SFTP に接続します。リアルタイム CDP は、セグメントを `.csv` または `.txt` ファイルとして書き出し、希望の場所に配信します。リアルタイム CDP で有効になっているストレージの場所から、電子メールマーケティングプラットフォームにデータのインポートをスケジュールします。データをインポートするプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## 手順 1 — 宛先の接続 {#connect-destination}

1. **[!UICONTROL 接続]** / **[!UICONTROL 宛先]**&#x200B;で、接続先の電子メールマーケティング先を選択し、「接続先 ****」を選択します。

   ![宛先に接続](/help/rtcdp/destinations/assets/connect-email-marketing.png)

2. In the **[!UICONTROL Authentication]** step, if you had previously set up a connection to your email marketing destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to your email marketing destination. 「 **[!UICONTROL Connection type]** 」セレクターで、 **AmazonS3**、SFTP with Password **、****** SFTP with SSH Keyを選択できます。 接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

   **S3接続の場合**、AmazonアクセスキーIDとシークレットアクセスキーを指定する必要があります。

   For **SFTP with Password** connections, you must provide Domain, Port, Username, and Password for your SFTP server.

   For **SFTP with SSH Key** connections, you must provide Domain, Port, Username, and SSH Key for your SFTP server.

3. 「 **[!UICONTROL 設定]** 」( **[!UICONTROL Setup]** )手順で、新しい保存先の **[!UICONTROL 名前と説明(Description]** )を入力し、書き出したファイルのファイル形式( **** File format)を入力します。 <br>
前の手順でストレージとしてAmazonS3を選択した場合は、ファイルが配信されるクラウドストレージーに、 **[!UICONTROL バケット名]** と **[!UICONTROL フォルダーパス]** を挿入します。 For the SFTP storage option, insert the **[!UICONTROL Folder path]** where the files will be delivered. <br>
また、この手順では、この宛先に適用する **[!UICONTROL マーケティングの使用例]** を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](/help/rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](/help/data-governance/policies/overview.md#core-actions)。 <br>
   ![電子メールの設定手順](/help/rtcdp/destinations/assets/email-setup-step.png)

## 手順2 — エクスポート先に含めるセグメントメンバーを選択します。 {#select-segments}

On the **[!UICONTROL Select Segments]** page, select which segments to send to the destination. フィールドの詳細については、以下の節を参照してください。

![セグメントの選択](/help/rtcdp/destinations/assets/email-select-segments.png)

## 手順3 — ファイル名を設定する

ファイル名の編集オプションの詳細については、チュートリアルの「 [Activate destinations](/help/rtcdp/destinations/activate-destinations.md#configure) 」の設定手順を参照してください。

## Step 4 - Select attributes - Select which schema fields to use as destination attributes in your exported files {#destination-attributes}

この手順では、電子メールマーケティングの宛先に書き出しするフィールドを選択します。

![宛先属性](/help/rtcdp/destinations/assets/recommended-attributes.png)

この手順の詳細については、チュートリアルの「属性を [選択](/help/rtcdp/destinations/activate-destinations.md#select-attributes) 」の手順を参照してください。

### ID {#identity}

[ユニオンスキーマ](../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。これは、ユーザーの ID をキーオフにするフィールドです。最も一般的に、このフィールドは電子メールアドレスですが、ロイヤリティープログラム ID や電話番号を指定することもできます。スキーマ内で最も一般的な一意のIDとそのXDMフィールドについては、次の表を参照してください。

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
| セグメントのメンバーシップ | `segmentMembership.status` |

## 手順 5 — ストレージの場所からインポート先にデータをインポートひます

ストレージの場所から宛先にデータをインポートする方法については、個々の電子メールマーケティングの宛先に関する記事を参照してください。

* [Adobe Campaign](/help/rtcdp/destinations/adobe-campaign-destination.md#import-data-into-campaign)
* [Salesforce Marketing Cloud](/help/rtcdp/destinations/salesforce-marketing-cloud-destination.md#import-data-into-salesforce)
* [Oracle Eloqua](/help/rtcdp/destinations/oracle-eloqua-destination.md#import-data-into-eloqua)
* [Oracle Responsys](/help/rtcdp/destinations/oracle-responsys-destination.md#import-data-into-responsys)

## 電子メールマーケティングの宛先へのセグメントのアクティブ化

電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)」を参照してください。

## その他のリソース

* [宛先へのデータのアクティブ化](/help/rtcdp/destinations/activate-destinations.md)
* [Flow Service APIを使用して、電子メールマーケティングの宛先を作成し、データをアクティブ化する](https://docs.adobe.com/content/help/en/experience-platform/tutorials/destinations/email-marketing-api.html)
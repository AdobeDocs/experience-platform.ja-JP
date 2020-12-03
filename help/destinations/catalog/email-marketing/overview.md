---
keywords: email;Email;e-mail;email destinations
title: 電子メールマーケティングの宛先
seo-title: 電子メールマーケティングの宛先
type: Tutorial
description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
seo-description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
translation-type: tm+mt
source-git-commit: 0bb1622895b1e0f97fc47b5c61d456bc369746c8
workflow-type: tm+mt
source-wordcount: '827'
ht-degree: 38%

---


# 電子メールマーケティングの宛先 {#email-marketing-destinations}

電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。リアルタイム顧客データプラットフォームは、電子メールマーケティングの宛先に対してセグメントをアクティブ化できるので、ESPと統合できます。

キャンペーン用の電子メールマーケティングの宛先にセグメントを送信するには、リアルタイムCDPが最初に宛先に接続する必要があります。

電子メールマーケティングの宛先への接続は、3 つの手順で行います。各手順について、このページで詳しく説明します。

次の節で説明する接続フローで、Amazon S3 または SFTP に接続します。リアルタイム CDP は、セグメントを `.csv` または `.txt` ファイルとして書き出し、希望の場所に配信します。リアルタイム CDP で有効になっているストレージの場所から、電子メールマーケティングプラットフォームにデータのインポートをスケジュールします。データをインポートするプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## 宛先の設定 {#connect-destination}

**[!UICONTROL 接続]** / **[!UICONTROL 宛先]**&#x200B;で、接続先の電子メールマーケティング先を選択し、「 **[!UICONTROL 設定]**」を選択します。

![宛先に接続](../../assets/catalog/email-marketing/overview/connect-email-marketing.png)

In the **[!UICONTROL Authentication]** step, if you had previously set up a connection to your email marketing destination, select **[!UICONTROL Existing Account]** and select your existing connection. Or, you can select **[!UICONTROL New Account]** to set up a new connection to your email marketing destination. 「 **[!UICONTROL Connection type]** 」セレクターで、AmazonS3、パスワード付きSFTP、またはSSHキー付きSFTPのいずれかを選択できます。 接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

- **S3接続の場合**、AmazonアクセスキーIDとシークレットアクセスキーを指定する必要があります。
- For **SFTP with Password** connections, you must provide Domain, Port, Username, and Password for your SFTP server.
- For **SFTP with SSH Key** connections, you must provide Domain, Port, Username, and SSH Key for your SFTP server.

必要に応じて、RSA形式の公開鍵を添付し、「 **[!UICONTROL Key]** 」セクションの下に書き出したファイルに暗号化を追加できます。 この公開鍵は、Base64エンコードされた文字列 **として書き込む** 必要があります。

「 **[!UICONTROL 設定]** 」(Setup)ステップで、新しい保存先の名前と説明、および書き出すファイルのファイル形式を入力します。

前の手順でストレージとしてAmazonS3を選択した場合は、ファイルを配信するクラウドストレージーの保存先にバケット名とフォルダーパスを挿入します。 「SFTPストレージ」オプションで、ファイルが配信されるフォルダーパスを挿入します。

また、この手順では、この宛先に適用するマーケティングの使用例を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](../../../rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](../../../data-governance/policies/overview.md#core-actions)。

![電子メールの設定手順](../../assets/catalog/email-marketing/overview/email-setup-step.png)

## エクスポート先に含めるセグメントメンバーを選択します {#select-segments}

On the **[!UICONTROL Select Segments]** page, select which segments to send to the destination. フィールドの詳細については、以下の節を参照してください。

![セグメントの選択](../../assets/common/email-select-segments.png)

## ファイル名の設定

セグメントのスケジュールとファイル名の編集オプションについて詳しくは、「アクティブな宛先」チュートリアルの [設定](../../ui/activate-destinations.md#configure) 手順を参照してください。

## Select attributes - Select which schema fields to use as destination attributes in your exported files {#destination-attributes}

この手順では、電子メールマーケティングの宛先にエクスポートするフィールドを選択し、どのフィールドが必須かをマークします。

![宛先属性](../../assets/catalog/email-marketing/overview/recommended-attributes.png)

この手順の詳細については、チュートリアルの「属性を [選択](../../ui/activate-destinations.md#select-attributes) 」の手順を参照してください。

### ID {#identity}

[ユニオンスキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。これは、ユーザーの ID をキーオフにするフィールドです。最も一般的に、このフィールドは電子メールアドレスですが、ロイヤリティープログラム ID や電話番号を指定することもできます。スキーマ内で最も一般的な一意のIDとそのXDMフィールドについては、次の表を参照してください。

| 一意の識別子 | 統合スキーマの XDM フィールド |
----------------- | ---------------------------
| 電子メールアドレス | `personalEmail.address` |
| 電話番号 | `mobilePhone.number` |
| ロイヤリティープログラム ID | `Customer-defined XDM field` |

### その他の宛先属性

「スキーマ」フィールドセレクターで、電子メールの送信先に書き出しする他のフィールドを選択します。次のオプションが推奨されます。

| スキーマ | XDM フィールド |
------ | ---------
| 名 | `person.name.firstName` |
| 姓 | `person.name.lastName` |
| 電話番号 | `mobilePhone.number` |
| 住所（市区町村） | `homeAddress.city` |
| 住所（都道府県） | `homeAddress.stateProvince` |
| 住所（郵便番号） | `homeAddress.postalCode` |
| 誕生日 | `person.birthDayAndMonth` |
| セグメントのメンバーシップ | `segmentMembership.status` |

## ストレージーの場所からインポート先にデータをインポートする

ストレージの場所から宛先にデータをインポートする方法については、個々の電子メールマーケティングの宛先に関する記事を参照してください。

- [Adobe Campaign](./adobe-campaign.md#import-data-into-campaign)
- [Oracle Eloqua](./oracle-eloqua.md#import-data-into-eloqua)
- [Oracle Responsys](./oracle-responsys.md#import-data-into-responsys)
- [Salesforce Marketing Cloud](./salesforce-marketing-cloud.md#import-data-into-salesforce)

## 電子メールマーケティングの宛先へのセグメントのアクティブ化

電子メールマーケティングの宛先に対してセグメントをアクティブ化する方法については、「[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## その他のリソース

- [宛先へのデータのアクティブ化](../../ui/activate-destinations.md)
- [Flow Service APIを使用して、電子メールマーケティングの宛先を作成し、データをアクティブ化する](../../api/email-marketing.md)
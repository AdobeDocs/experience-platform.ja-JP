---
keywords: 電子メール；電子メール；電子メール；電子メールの宛先
title: 電子メールマーケティングの宛先の概要
type: チュートリアル
description: 電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。
translation-type: tm+mt
source-git-commit: 7d579d85d427c45f39d000288ed883c7ffd003bf
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 33%

---


# 電子メールマーケティングの宛先の概要 {#email-marketing-destinations}

電子メールサービスプロバイダー（ESP）を使用すると、プロモーション電子メールキャンペーンの送信など、電子メールマーケティング活動を管理できます。Adobe Experience Platformは、電子メールマーケティングの宛先に対してセグメントをアクティブ化できるようにすることで、ESPと統合されています。

キャンペーンの電子メールマーケティングの宛先にセグメントを送信するには、プラットフォームはまず宛先に接続する必要があります。

電子メールマーケティングの宛先への接続は、3 つの手順で行います。各手順について、このページで詳しく説明します。

次の節で説明する接続フローで、Amazon S3 または SFTP に接続します。プラットフォームは、セグメントを`.csv`または`.txt`ファイルとして書き出し、希望の場所に配信します。 Platformで有効になっているストレージーの場所から、電子メールマーケティングプラットフォームでデータのインポートをスケジュールします。 データをインポートするプロセスは、パートナーごとに異なります。詳しくは、個々の宛先に関する記事を参照してください。

## 宛先を構成{#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、接続先の電子メールマーケティング先を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![宛先に接続](../../assets/catalog/email-marketing/overview/connect-email-marketing.png)

**[!UICONTROL 認証]**&#x200B;手順で、電子メールマーケティングの宛先への接続を設定済みの場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、電子メールマーケティングの宛先への新しい接続を設定できます。 **[!UICONTROL 接続タイプ]**&#x200B;セレクターで、AmazonS3、パスワード付きSFTP、またはSSHキー付きSFTPのいずれかを選択できます。 接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

- **S3接続**&#x200B;の場合、AmazonアクセスキーIDとシークレットアクセスキーを指定する必要があります。
- **SFTPとパスワード**&#x200B;接続の場合、SFTPサーバーのドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
- **SSHキー**&#x200B;接続を使用するSFTPの場合、SFTPサーバーのドメイン、ポート、ユーザー名、SSHキーを指定する必要があります。

必要に応じて、RSA形式の公開鍵を添付し、**[!UICONTROL [キー]**]セクションの下にある書き出したファイルに暗号化を追加できます。 この公開鍵&#x200B;**は、Base64エンコードされた文字列として書かれる必要があります。**

**[!UICONTROL セットアップ]**&#x200B;の手順で、新しい宛先の名前と説明、および書き出したファイルのファイル形式を入力します。

前の手順でストレージとしてAmazonS3を選択した場合は、ファイルを配信するクラウドストレージーの保存先にバケット名とフォルダーパスを挿入します。 「SFTPストレージ」オプションで、ファイルが配信されるフォルダーパスを挿入します。

また、この手順では、この宛先に適用するマーケティングアクションを選択できます。 マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。 Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![電子メールの設定手順](../../assets/catalog/email-marketing/overview/email-setup-step.png)

## エクスポート先に含めるセグメントメンバーを選択{#select-segments}

**[!UICONTROL セグメントを選択]**&#x200B;ページで、送信先に送信するセグメントを選択します。 フィールドの詳細については、以下の節を参照してください。

![セグメントの選択](../../assets/common/email-select-segments.png)

## ファイル名の設定

セグメントのスケジュールとファイル名の編集オプションについて詳しくは、「宛先のアクティブ化」チュートリアルの[設定](../../ui/activate-destinations.md#configure)の手順を参照してください。

## 属性の選択 — 書き出したファイルの保存先属性として使用するスキーマフィールドを選択します。 {#destination-attributes}

この手順では、電子メールマーケティングの宛先にエクスポートするフィールドを選択し、どのフィールドが必須かをマークします。

![宛先属性](../../assets/catalog/email-marketing/overview/recommended-attributes.png)

この手順の詳細については、チュートリアルの[属性を選択](../../ui/activate-destinations.md#select-attributes)の手順を参照してください。

## ID {#identity}

[ユニオンスキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の ID を選択することをお勧めします。これは、ユーザーの ID をキーオフにするフィールドです。最も一般的に、このフィールドは電子メールアドレスですが、ロイヤリティープログラム ID や電話番号を指定することもできます。スキーマ内で最も一般的な一意のIDとそのXDMフィールドについては、次の表を参照してください。

| 一意の ID | 統合スキーマの XDM フィールド |
----------------- | ---------------------------
| 電子メールアドレス | `personalEmail.address` |
| 電話番号 | `mobilePhone.number` |
| ロイヤリティープログラム ID | `Customer-defined XDM field` |

## その他の宛先属性

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
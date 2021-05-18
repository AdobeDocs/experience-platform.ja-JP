---
keywords: 電子メール；電子メール；電子メール；電子メールの送信先；oracleの検索；oracle
title: OracleEloqua接続
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
exl-id: 6eaa79ff-8874-423b-bdff-aa04f6101a53
source-git-commit: 70be44e919070df910d618af4507b600ad51123c
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 30%

---

# [!DNL Oracle Eloqua] connection

[[!DNL Oracle Eloqua] は](https://www.oracle.com/cx/marketing/automation/)[!DNL Oracle]、 が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメントデータを[!DNL Oracle Eloqua]に送信するには、まずAdobe Experience Platformの宛先](#connect-destination)を[接続し、次に[ストレージの場所から[!DNL Oracle Eloqua]にデータインポート](#import-data-into-eloqua)を設定する必要があります。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## IPアドレス許可リスト{#allow-list}

SFTPストレージを使用して電子メールマーケティングの宛先を設定する場合、Adobeでは許可リストに特定のIP範囲を追加することを推奨します。

許可リストにAdobeIPを追加する必要がある場合は、[IPアドレスの許可リストを参照して、クラウドストレージの宛先](../cloud-storage/ip-address-allow-list.md)を確認してください。

## 宛先に接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Oracle Eloqua]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「[!UICONTROL アクティブ化]」と「[!UICONTROL 設定]」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

![Eloqua に接続](../../assets/catalog/email-marketing/oracle-eloqua/catalog.png)

**[!UICONTROL アカウント]**&#x200B;の手順で、クラウドストレージの接続先への接続を以前に設定した場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続の1つを選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。[!DNL Oracle Eloqua]の場合は、**[!UICONTROL パスワード]**&#x200B;のSFTPと&#x200B;**[!UICONTROL SSHキー]**&#x200B;のSFTPのどちらかを選択できます。

![Eloquaアカウントの接続](../../assets/catalog/email-marketing/oracle-eloqua/connection-type.png)

接続タイプに応じて、以下の情報を入力し、「**[!UICONTROL 宛先への接続]**」を選択します。

- **[!UICONTROL SFTPとパスワード]**&#x200B;接続の場合、[!UICONTROL ドメイン]、[!UICONTROL ポート]、[!UICONTROL ユーザー名]、[!UICONTROL パスワード]を指定する必要があります。
- **[!UICONTROL SSHキー]**&#x200B;接続を使用するSFTPの場合、[!UICONTROL ドメイン]、[!UICONTROL ポート]、[!UICONTROL ユーザー名]、[!UICONTROL SSHキー]を指定する必要があります。

必要に応じて、RSA形式の公開鍵を添付し、PGP/GPGを使用した暗号化を&#x200B;**[!UICONTROL Key]**&#x200B;セクションのエクスポートファイルに追加することができます。 公開鍵は、[!DNL Base64]エンコードされた文字列として書き込む必要があります。

![宛先に接続する](../../assets/catalog/email-marketing/oracle-eloqua/account-info.png)

**[!UICONTROL 認証]**&#x200B;の手順で、宛先に関する関連情報を次のように入力します。
- **[!UICONTROL 名前]**：宛先の名前を選択します。
- **[!UICONTROL 説明]**：宛先の説明を入力します。
- **[!UICONTROL Folder Path]**:ストレージー上の場所に、PlatformがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするパスを指定します。
- **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
- **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

![Eloqua の基本情報](../../assets/catalog/email-marketing/oracle-eloqua/basic-information.png)

上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」をクリックします。これで宛先が作成され、宛先への[セグメントをアクティブ化](../../ui/activate-destinations.md)できます。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 宛先属性 {#destination-attributes}

[セグメント](../../ui/activate-destinations.md)を[!DNL Oracle Eloqua]宛先にアクティブ化する場合、Adobeでは[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することを推奨します。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳細については、[書き出したファイルの保存先属性として使用するスキーマフィールドの選択](./overview.md#destination-attributes)を参照してください。

## エクスポートされたデータ{#exported-data}

[!DNL Oracle Eloqua]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## [!DNL Oracle Eloqua] {#import-data-into-eloqua}へのデータインポートの設定

[!DNL Platform]を[!DNL SFTP]ストレージに接続した後、ストレージの場所から[!DNL Oracle Eloqua]にデータインポートを設定する必要があります。 これを達成する方法については、[!DNL Oracle Eloqua Help Center]の[連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm)を参照してください。

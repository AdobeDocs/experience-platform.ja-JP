---
keywords: 電子メール；電子メール；電子メールの宛先；oracleeloqua;oracle
title: OracleEloqua接続
description: Oracle Eloqua は、Oracle が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。
exl-id: 6eaa79ff-8874-423b-bdff-aa04f6101a53
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 26%

---

# [!DNL Oracle Eloqua] 接続

[[!DNL Oracle Eloqua] は](https://www.oracle.com/cx/marketing/automation/)[!DNL Oracle]、 が提供するマーケティング自動処理向けの SaaS（サービスとしてのソフトウェア）プラットフォームで、B2B マーケターや組織がマーケティングキャンペーンや販売リードジェネレーションを管理するのを支援します。

セグメントデータを[!DNL Oracle Eloqua]に送信するには、まずAdobe Experience Platformで宛先](#connect-destination)に接続し、次にストレージの場所から[](#import-data-into-eloqua)にデータインポート[!DNL Oracle Eloqua]を設定する必要があります。[

## 書き出しタイプ {#export-type}

**プロファイルベースの**  — セグメントのすべてのメンバーを、目的のスキーマフィールド(例：電子メールアドレス、電話番号、姓)。オーディエンスアクティベーションワークフローの属性を選択画面 [から選択します](../../ui/activate-batch-profile-destinations.md#select-attributes)。

## IPアドレス許可リスト {#allow-list}

SFTPストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定のIP範囲を許可リストに追加することをお勧めします。

許可リストにAdobeIPを追加する必要がある場合は、[クラウドストレージの宛先](../cloud-storage/ip-address-allow-list.md)のIPアドレス許可リストを参照してください。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

この宛先では、次の接続タイプをサポートしています。

* **[!UICONTROL SFTP（パスワード）]**
* **[!UICONTROL SFTP（SSHキー）]**

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL パスワード]**&#x200B;を使用したSFTP接続の場合は、次を指定する必要があります。
   * [!UICONTROL ドメイン]
   * [!UICONTROL ポート]
   * [!UICONTROL ユーザー名]
   * [!UICONTROL パスワード]
* **[!UICONTROL SFTP（SSHキー]**&#x200B;を使用）で接続する場合は、次の情報を指定する必要があります。
   * [!UICONTROL ドメイン]
   * [!UICONTROL ポート]
   * [!UICONTROL ユーザー名]
   * [!UICONTROL SSHキー]

* 必要に応じて、RSA形式の公開鍵を添付し、**[!UICONTROL Key]**&#x200B;セクションで、PGP/GPGを使用した暗号化を書き出したファイルに追加できます。 公開鍵は、[!DNL Base64]エンコードされた文字列として書き込む必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:Platformが書き出しデータをCSVまたはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
* **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

<!--

Commenting out Amazon S3 bucket part for now until support is clarified

- **[!UICONTROL Bucket name]**: Your Amazon S3 bucket, where Platform will deposit the data export. Your input must be between 3 and 63 characters long. Must begin and end with a letter or number. Must contain only lowercase letters, numbers, or hyphens ( - ). Must not be formatted as an IP address (for example, 192.100.1.1).

-->

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[プロファイルの一括書き出し先へのオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md)を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してセグメントをアクティブ化する場合、Adobeでは、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、電子メールマーケティングの宛先](overview.md#best-practices)に対してオーディエンスをアクティブ化する際のベストプラクティス[を参照してください。

## エクスポートされたデータ {#exported-data}

[!DNL Oracle Eloqua]の宛先の場合、Platformは指定したストレージの場所に、タブ区切りの`.csv`ファイルを作成します。 ファイルの詳細については、『セグメントのアクティベーションのチュートリアル』の「[セグメントのアクティベーションの検証](../../ui/activate-batch-profile-destinations.md#verify)」を参照してください。

## [!DNL Oracle Eloqua]へのデータインポートの設定 {#import-data-into-eloqua}

[!DNL Platform]を[!DNL SFTP]ストレージに接続した後、ストレージの場所から[!DNL Oracle Eloqua]へのデータインポートを設定する必要があります。 これをおこなう方法については、[!DNL Oracle Eloqua Help Center]の「連絡先またはアカウントのインポート](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAA/Help/DataImportExport/Tasks/ImportingContactsOrAccounts.htm)」を参照してください。[

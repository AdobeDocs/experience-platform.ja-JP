---
keywords: SFTP;sftp
title: SFTP接続
description: 区切り形式のデータファイルをAdobe Experience Platformから定期的にエクスポートするには、SFTPサーバーへのライブ送信接続を作成します。
translation-type: tm+mt
source-git-commit: 4f0047e7ac4c83e3e17ea0a077bbeb09c86d1db6
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 7%

---


# SFTP接続

区切り形式のデータファイルをAdobe Experience Platformから定期的にエクスポートするには、SFTPサーバーへのライブ送信接続を作成します。

>[!IMPORTANT]
>
> AdobeはSFTPサーバーへのデータのエクスポートをサポートしていますが、データのエクスポートに推奨されるクラウドストレージの場所は[!DNL Amazon S3]と[!DNL Azure Blob]です。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

![SFTPプロファイルベースのエクスポートタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先の接続 {#connect-destination}

SFTPを含むクラウドストレージの接続先への接続方法については、[クラウドストレージの接続先ワークフロー](./workflow.md)を参照してください。

SFTP の宛先については、宛先の作成ワークフローの&#x200B;**認証**&#x200B;手順で次の情報を入力します 。

* **ホスト**:SFTPストレージの場所のアドレス
* **ユーザー名**:SFTPストレージの場所にログインするためのユーザー名
* **パスワード**:SFTPストレージの場所にログインするためのパスワード

## エクスポートされたデータ{#exported-data}

SFTPの送信先の場合、プラットフォームは、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## IPアドレス許可リスト

許可リストにAdobeIPを追加する必要がある場合は、[IPアドレスの許可リストを参照して、クラウドストレージの宛先](./ip-address-allow-list.md)を確認してください。
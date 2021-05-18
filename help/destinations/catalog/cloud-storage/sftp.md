---
keywords: SFTP;sftp
title: SFTP接続
description: 区切り形式のデータファイルをAdobe Experience Platformから定期的にエクスポートするには、SFTPサーバーへのライブ送信接続を作成します。
exl-id: 27abfc38-ec19-4321-b743-169370d585a0
source-git-commit: a21abb44bb9cbe6fefa0ff70a1ff19e31cc0c7de
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 8%

---

# SFTP接続

## 概要 {#overview}

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

[!DNL SFTP]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## IPアドレス許可リスト

許可リストにAdobeIPを追加する必要がある場合は、[IPアドレスの許可リストを参照して、クラウドストレージの宛先](./ip-address-allow-list.md)を確認してください。

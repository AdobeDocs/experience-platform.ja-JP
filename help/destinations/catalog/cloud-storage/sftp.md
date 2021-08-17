---
keywords: SFTP;sftp
title: SFTP接続
description: SFTPサーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 27abfc38-ec19-4321-b743-169370d585a0
source-git-commit: b7392596c7ed96032dc8ad6bb8e423640f562394
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---

# SFTP接続

## 概要 {#overview}

SFTPサーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。

>[!IMPORTANT]
>
> AdobeではSFTPサーバーへのデータの書き出しがサポートされていますが、データを書き出すための推奨されるクラウドストレージの場所は[!DNL Amazon S3]と[!DNL Azure Blob]です。

## 書き出しタイプ {#export-type}

**プロファイルベースの**  — セグメントのすべてのメンバーを、目的のスキーマフィールド(例：電子メールアドレス、電話番号、姓)。宛先のアクティベーションワークフローの属性を選択画面 [から選択します](../../ui/activate-batch-profile-destinations.md)。

![SFTPプロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **ホスト**:SFTPストレージの場所のアドレス
* **ユーザー名**:SFTPストレージの場所にログインするユーザー名
* **パスワード**:SFTPストレージの場所にログインするためのパスワード
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出すファイルをホストする宛先フォルダーのパスを入力します。

必要に応じて、RSA形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、[!DNL Base64]エンコードされた文字列として書き込む必要があります。

## エクスポートされたデータ {#exported-data}

[!DNL SFTP]の宛先の場合、Platformは指定したストレージの場所に、タブ区切りの`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションのチュートリアルの「 [プロファイルのバッチ書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) 」を参照してください。

## IPアドレス許可リスト

許可リストにAdobeIPを追加する必要がある場合は、[クラウドストレージの宛先](ip-address-allow-list.md)のIPアドレス許可リストを参照してください。

---
keywords: SFTP;sftp
title: SFTP 接続
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 27abfc38-ec19-4321-b743-169370d585a0
source-git-commit: b7392596c7ed96032dc8ad6bb8e423640f562394
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---

# SFTP 接続

## 概要 {#overview}

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。

>[!IMPORTANT]
>
> Adobeは SFTP サーバーへのデータの書き出しをサポートしていますが、データを書き出す際に推奨されるクラウドストレージの場所は [!DNL Amazon S3] と [!DNL Azure Blob] です。

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。宛先のアクティベーションワークフローの属性を選択画面 [から選択します](../../ui/activate-batch-profile-destinations.md)。

![SFTP プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **ホスト**:SFTP ストレージの場所のアドレス
* **ユーザー名**:SFTP ストレージの場所にログインするユーザー名
* **パスワード**:SFTP ストレージの場所にログインするためのパスワード
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする宛先フォルダーのパスを入力します。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、[!DNL Base64] エンコードされた文字列として書き込む必要があります。

## エクスポートされたデータ {#exported-data}

[!DNL SFTP] の宛先の場合、Platform は指定したストレージの場所に、タブ区切りの `.csv` ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションのチュートリアルの「[ プロファイルのバッチ書き出し先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md)」を参照してください。

## IP アドレス許可リスト

許可リストにAdobeIP を追加する必要がある場合は、[ クラウドストレージの宛先 ](ip-address-allow-list.md) の IP アドレス許可リストを参照してください。

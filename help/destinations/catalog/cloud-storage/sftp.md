---
keywords: SFTP;sftp
title: SFTP 接続
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 27abfc38-ec19-4321-b743-169370d585a0
source-git-commit: bf46f4e6549fcbd975a9f0a6034040ed2e9b34e6
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# SFTP 接続

## 概要 {#overview}

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。

>[!IMPORTANT]
>
> Adobeは SFTP サーバーへのデータの書き出しをサポートしていますが、データを書き出す際に推奨されるクラウドストレージの場所は次のとおりです [!DNL Amazon S3] および [!DNL Azure Blob].

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md).

![SFTP プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_sftp_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、Base64 エンコードされた文字列として書き込む必要があります。"
>text="Learn more in documentation"

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **ホスト**:SFTP ストレージの場所のアドレス
* **ユーザー名**:SFTP ストレージの場所にログインするユーザー名
* **パスワード**:SFTP ストレージの場所にログインするためのパスワード
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。

## 書き出されたデータ {#exported-data}

の場合 [!DNL SFTP] 宛先、Platform は、 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （セグメントのアクティベーションに関するチュートリアル）。

## IP アドレスの許可リスト

参照： [クラウドストレージの宛先の IP アドレス許可リスト](ip-address-allow-list.md) 許可リストにAdobeIP を追加する必要がある場合。

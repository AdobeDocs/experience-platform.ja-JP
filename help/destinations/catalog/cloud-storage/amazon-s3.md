---
keywords: Amazon S3;S3 destination;s3;amazon s3
title: Amazon S3 接続
description: Amazon Web Services(AWS)S3 ストレージへのライブアウトバウンド接続を作成し、CSV データファイルをAdobe Experience Platformから独自の S3 バケットに定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: b4810dfef7b0d437744ca14a32bd4f5746e8d002
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# [!DNL Amazon S3] 接続 {#s3-connection}

## 概要 {#overview}

へのライブアウトバウンド接続を作成します [!DNL Amazon Web Services] (AWS)S3 ストレージを使用し、Adobe Experience Platformから独自の S3 バケットに CSV データファイルを定期的に書き出します。

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 [宛先のアクティベーションワークフロー](../../ui/activate-segment-streaming-destinations.md#mapping).

![Amazon S3 プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!DNL Amazon S3]アクセスキー** および **[!DNL Amazon S3]秘密鍵**:In [!DNL Amazon S3]、 `access key - secret access key` ペアを使用して、 [!DNL Amazon S3] アカウント 詳しくは、 [Amazon Web Servicesドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html).
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL バケット名]**:名前を入力 [!DNL Amazon S3] この宛先で使用するバケット。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。

>[!TIP]
>
>「宛先の接続」ワークフローでは、書き出したセグメントファイルごとにAmazon S3 ストレージにカスタムフォルダーを作成できます。 読み取り [マクロを使用して、ストレージの場所にフォルダーを作成する](overview.md#use-macros) 」を参照してください。

### 必須 [!DNL Amazon S3] 権限 {#required-s3-permission}

データを正常に接続してに書き出すには、以下を実行します。 [!DNL Amazon S3] ストレージの場所、次の IAM (Identity and Access Management) ユーザーを作成する [!DNL Platform] in [!DNL Amazon S3] 次のアクションに対する権限を割り当てます。

* `s3:DeleteObject`
* `s3:GetBucketLocation`
* `s3:GetObject`
* `s3:ListBucket`
* `s3:PutObject`
* `s3:ListMultipartUploadParts`

<!--

Commenting out this note, as write permissions are assigned through the s3:PutObject permission.

>[!IMPORTANT]
>
>Platform needs `write` permissions on the bucket object where the export files will be delivered.

-->

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

## 書き出されたデータ {#exported-data}

の場合 [!DNL Amazon S3] 宛先、 [!DNL Platform] を作成 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （セグメントのアクティベーションに関するチュートリアル）。

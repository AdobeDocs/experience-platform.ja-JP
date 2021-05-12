---
keywords: AmazonS3;S3宛先；s3;amazon s3
title: AmazonS3接続
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
exl-id: 6a2a2756-4bbf-4f82-88e4-62d211cbbb38
source-git-commit: 49a59e5b081243679f5d94b03a63d30df22cdc6a
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 12%

---

# [!DNL Amazon S3] connection  {#s3-connection}

## 概要 {#overview}

[!DNL Amazon Web Services] (AWS) S3ストレージへのライブアウトバウンド接続を作成し、タブ区切りファイルまたはCSVデータファイルをAdobe Experience PlatformからS3バケットに定期的にエクスポートします。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

![AmazonS3プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先の接続 {#connect-destination}

[を含むクラウドストレージの接続先への接続方法については、[!DNL Amazon S3]クラウドストレージの接続先ワークフロー](./workflow.md)を参照してください。

[!DNL Amazon S3]宛先に対して、宛先を作成ワークフローで次の情報を入力します。

* **[!DNL Amazon S3]アクセスキーと [!DNL Amazon S3] 秘密鍵**:で、 [!DNL Amazon S3]ペアを生成して、プラットフォームに `access key - secret access key`  [!DNL Amazon S3] アカウントへのアクセスを許可します。詳しくは、[AmazonWebサービスドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。

>[!TIP]
>
>接続先のワークフローでは、書き出したセグメントファイルごとに、AmazonS3ストレージにカスタムフォルダを作成できます。 「[マクロを使用してストレージーの場所](./workflow.md#use-macros)にフォルダーを作成する」を読み、手順を確認します。

## 必要な[!DNL Amazon S3]権限{#required-s3-permission}

データを[!DNL Amazon S3]ストレージの場所に正常に接続して書き出すには、[!DNL Amazon S3]に[!DNL Platform]のIdentity and Access Management (IAM)ユーザーを作成し、次の操作に対する権限を割り当てます。

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


## エクスポートされたデータ{#exported-data}

[!DNL Amazon S3]宛先の場合、[!DNL Platform]は、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

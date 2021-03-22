---
keywords: AmazonS3;S3宛先；s3;amazon s3
title: AmazonS3接続
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
translation-type: tm+mt
source-git-commit: 709908196bb5df665c7e7df10dc58ee9f3b0edbf
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 14%

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

>[!IMPORTANT]
>
>プラットフォームは、エクスポートファイルを配信するバケットオブジェクトに対して`write`権限が必要です。

## エクスポートされたデータ{#exported-data}

[!DNL Amazon S3]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

---
keywords: Amazon S3;S3 destination;s3;amazon s3
title: Amazon S3 の宛先
seo-title: Amazon S3 の宛先
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
seo-description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
translation-type: tm+mt
source-git-commit: f2fdc3b75d275698a4b1e4c8969b1b840429c919
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# [!DNL Amazon S3] destination

## 概要

Create a live outbound connection to your [!DNL Amazon Web Services] (AWS) S3 storage to periodically export tab-delimited or CSV data files from Adobe Experience Platform into your own S3 buckets.

## 書き出しタイプ {#export-type}

**プロファイルベース** — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

![AmazonS3プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/amazon-s3/catalog.png)

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](./workflow.md) for instructions on how to connect to your cloud storage destinations, including [!DNL Amazon S3].

For [!DNL Amazon S3] destinations, enter the following information in the create destination workflow:

* **[!DNL Amazon S3]アクセスキーと [!DNL Amazon S3] 秘密鍵**:では、 [!DNL Amazon S3]ペアを作成して、お客様のア `access key - secret access key`[!DNL Amazon S3] カウントにリアルタイムCDPアクセスを許可します。 詳しくは、 [Amazonウェブサービスのドキュメントを参照してください](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)。

>[!IMPORTANT]
>
> Real-time CDP には、書き出しファイルの配信先となるバケットオブジェクトに対する `write` 権限が必要です。

## 書き出されたデータ {#exported-data}

For [!DNL Amazon S3] destinations, Real-time CDP creates a tab-delimited `.txt` or `.csv` file in the storage location that you provided. これらのファイルについて詳しくは、セグメントアクティベーションチュートリアルの [電子メールマーケティングの宛先とクラウドストレージの宛先](../../ui/activate-destinations.md#esp-and-cloud-storage) （英語）を参照してください。

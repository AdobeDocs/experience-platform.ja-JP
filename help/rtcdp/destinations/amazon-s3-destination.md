---
keywords: Amazon S3;S3 destination;s3;amazon s3
title: Amazon S3 の宛先
seo-title: Amazon S3 の宛先
description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
seo-description: Amazon Web Services（AWS）S3 ストレージへのライブアウトバウンド接続を作成し、タブ区切りのデータファイルまたは CSV データファイルを Adobe Experience Platform から S3 バケットへと定期的に書き出します。
translation-type: tm+mt
source-git-commit: d0a04c61bfe4024a2bb45ea7babab9073fcd6c22
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 33%

---


# [!DNL Amazon S3] destination

## 概要

Create a live outbound connection to your [!DNL Amazon Web Services] (AWS) S3 storage to periodically export tab-delimited or CSV data files from Adobe Experience Platform into your own S3 buckets.

## 書き出しタイプ {#export-type}

**プロファイルエクスポート** — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](/help/rtcdp/destinations/activate-destinations.md#select-attributes)。

![AmazonS3プロファイルベースの書き出しタイプ](/help/rtcdp/destinations/assets/aws-export-type.png)

## 宛先の接続 {#connect-destination}

See [Cloud storage destinations workflow ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)for instructions on how to connect to your cloud storage destinations, including [!DNL Amazon S3].

For [!DNL Amazon S3] destinations, enter the following information in the create destination workflow:

* **[!DNL Amazon S3]アクセスキーと[!DNL Amazon S3]秘密鍵**:で、 [!DNL Amazon S3]ペアを生成して、Adobeに対してリアルタイムCDPアクセスを `access key - secret access key`[!DNL Amazon S3] アカウントに付与します。 詳しくは、 [Amazonウェブサービスのドキュメントを参照してください](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)。

>[!IMPORTANT]
>
>アドビのリアルタイム CDP には、書き出しファイルの配信先となるバケットオブジェクトに対する `write` 権限が必要です。

## 書き出されたデータ {#exported-data}

For [!DNL Amazon S3] destinations, Adobe Real-time CDP creates a tab-delimited `.txt` or `.csv` file in the storage location that you provided. これらのファイルについて詳しくは、セグメントアクティベーションチュートリアルの [電子メールマーケティングの宛先とクラウドストレージの宛先](/help/rtcdp/destinations/activate-destinations.md#esp-and-cloud-storage) （英語）を参照してください。

<!--

Expect a new file to be created in your storage location every day. The file format is:

`amazon-s3_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv`

```
amazon-s3_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
amazon-s3_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
amazon-s3_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

The presence of these files in your storage location is confirmation of successful activation. To understand how the exported files are structured, you can [download a sample .csv file](/help/rtcdp/destinations/assets/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv). This sample file includes the profile attributes `person.firstname`, `person.lastname`, `person.gender`, `person.birthyear`, and `personalEmail.address`.

-->

---
keywords: cloud storage destination;cloud storage
title: クラウドストレージの宛先
seo-title: クラウドストレージの宛先
description: リアルタイムCDPは、セグメントをデータファイルとしてAmazonS3、AWSKinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
seo-description: リアルタイムCDPは、セグメントをデータファイルとしてAmazonS3、AWSKinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
translation-type: tm+mt
source-git-commit: 0bb1622895b1e0f97fc47b5c61d456bc369746c8
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 36%

---


# クラウドストレージの宛先 {#cloud-storage-destinations}

リアルタイムCDPは、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 This enables you to send audiences and their profile attributes to your internal systems, via CSV or tab-delimited files for [!DNL Amazon S3] and SFTP. 宛先 [!DNL AWS Kinesis] と [!DNL Azure Event Hubs] 宛先の場合、データはJSON形式でExperience Platformからストリーミングされます。

![Adobe Cloud のストレージの保存先](../../assets/catalog/cloud-storage/cloud-storage-destinations.png)

クラウドストレージの宛先への接続方法について詳しくは、「[クラウドストレージの宛先を作成するためのワークフロー](./workflow.md)」を参照してください。

## データのエクスポートのタイプ

**プロファイルベースの書き出し** - オーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。

## 利用可能なCloudストレージの宛先

- [Amazon S3 の宛先](./amazon-s3.md)
- [SFTP の宛先](./sftp.md)

## 使用可能なクラウドストレージストリーミング先

- [AmazonKinesis駅](./amazon-kinesis.md)
- [Azureイベントハブの宛先](./azure-event-hubs.md)
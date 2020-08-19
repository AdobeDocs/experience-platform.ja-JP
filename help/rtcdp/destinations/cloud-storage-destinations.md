---
keywords: cloud storage destination;cloud storage
title: クラウドストレージの宛先
seo-title: クラウドストレージの宛先
description: AdobeReal-time CDPは、セグメントをデータファイルとしてAmazonS3、AWSKinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
seo-description: AdobeReal-time CDPは、セグメントをデータファイルとしてAmazonS3、AWSKinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
translation-type: tm+mt
source-git-commit: 15323134f0c626cad2c4e90b3e1c0662cf7e57dd
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 35%

---


# クラウドストレージの宛先 {#cloud-storage-destinations}

AdobeReal-time CDPは、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 This enables you to send audiences and their profile attributes to your internal systems, via CSV or tab-delimited files for [!DNL Amazon S3] and SFTP. 宛先 [!DNL AWS Kinesis] と [!DNL Azure Event Hubs] 宛先の場合、データはJSON形式でExperience Platformからストリーミングされます。

![Adobe Cloud のストレージの保存先](/help/rtcdp/destinations/assets/cloud-storage-destinations.png)

クラウドストレージの宛先への接続方法について詳しくは、「[クラウドストレージの宛先を作成するためのワークフロー](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)」を参照してください。

## データのエクスポートのタイプ

**プロファイルベースの書き出し** - オーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。

## 利用可能なCloudストレージの宛先

* [Amazon S3 の宛先](/help/rtcdp/destinations/amazon-s3-destination.md)
* [SFTP の宛先](/help/rtcdp/destinations/sftp-destination.md)

## 使用可能なクラウドストレージストリーミング先

* [AmazonKinesis駅](/help/rtcdp/destinations/amazon-kinesis-destination.md)
* [Azureイベントハブの宛先](/help/rtcdp/destinations/azure-event-hubs-destination.md)
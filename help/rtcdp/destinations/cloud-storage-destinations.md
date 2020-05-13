---
title: クラウドストレージの宛先
seo-title: クラウドストレージの宛先
description: Adobe Real-time CDPは、セグメントをデータファイルとしてAmazon S3、AWS Kinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
seo-description: Adobe Real-time CDPは、セグメントをデータファイルとしてAmazon S3、AWS Kinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
translation-type: tm+mt
source-git-commit: a18f89531cf024f61b054b47a660bd26766bebf6
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 34%

---


# クラウドストレージの宛先 {#cloud-storage-destinations}

Adobe Real-time CDPは、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 これにより、Amazon S3およびSFTP用のCSVファイルまたはタブ区切りファイルを使用して、オーディエンスとそのプロファイル属性を社内システムに送信できます。 AWS KinesisとAzureイベントハブの宛先の場合、データはJSON形式でエクスペリエンスプラットフォームからストリーミングされます。

![Adobe Cloud のストレージの保存先](/help/rtcdp/destinations/assets/cloud-storage-destinations.png)

クラウドストレージの宛先への接続方法について詳しくは、「[クラウドストレージの宛先を作成するためのワークフロー](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)」を参照してください。

## データのエクスポートのタイプ

**プロファイルベースの書き出し** - オーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。

## 利用可能なCloudストレージの宛先

* [Amazon S3 の宛先](/help/rtcdp/destinations/amazon-s3-destination.md)
* [SFTP の宛先](/help/rtcdp/destinations/sftp-destination.md)

## 使用可能なクラウドストレージストリーミング先

* [Amazon Kinesisの宛先](/help/rtcdp/destinations/amazon-kinesis-destination.md)
* [Azure EventHubsの宛先](/help/rtcdp/destinations/azure-event-hubs-destination.md)
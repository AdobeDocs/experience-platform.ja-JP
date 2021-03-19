---
keywords: クラウドのストレージ先；クラウドのストレージ
title: クラウドストレージの宛先の概要
description: Adobe Experience Platformは、セグメントをデータファイルとしてAmazonS3、AWSKinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
translation-type: tm+mt
source-git-commit: 4f636de9f0cac647793564ce37c6589d096b61f7
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 22%

---


# クラウドストレージの宛先の概要 {#cloud-storage-destinations}

Adobe Experience Platformは、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 これにより、オーディエンスとそのプロファイル属性を、[!DNL Amazon S3]、[!DNL Azure Blob]、SFTP用のCSVまたはタブ区切りファイルを使用して、社内システムに送信できます。 [!DNL Amazon Kinesis]と[!DNL Azure Event Hubs]の宛先の場合、データはJSON形式でExperience Platformからストリーミングされます。

![Adobeクラウドのストレージ先](../../assets/catalog/cloud-storage/cloud-storage-destinations.png)

クラウドストレージの宛先への接続方法について詳しくは、「[クラウドストレージの宛先を作成するためのワークフロー](./workflow.md)」を参照してください。

## データのエクスポートのタイプ

**プロファイルベースの書き出し** - オーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。

## 利用可能なクラウドストレージの宛先

- [AmazonS3接続](./amazon-s3.md)
- [Azure Blob接続](./azure-blob.md)
- [SFTP接続](./sftp.md)

## 利用可能なクラウドストレージストリーミング先

- [AmazonKinesis接続](./amazon-kinesis.md)
- [Azureイベントハブ接続](./azure-event-hubs.md)
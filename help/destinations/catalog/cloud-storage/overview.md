---
keywords: クラウドのストレージ先；クラウドのストレージ
title: クラウドストレージの宛先の概要
description: Adobe Experience Platformは、セグメントをデータファイルとしてAmazonS3、AWSKinesis、Azureイベントハブ、またはSFTPクラウドストレージの場所に配信できます。
translation-type: tm+mt
source-git-commit: e13a19640208697665b0a7e0106def33fd1e456d
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 34%

---


# クラウドストレージの宛先の概要 {#cloud-storage-destinations}

Adobe Experience Platformは、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 これにより、オーディエンスとそのプロファイル属性を[!DNL Amazon S3]およびSFTP用のCSVファイルまたはタブ区切りファイルを使用して、社内システムに送信できます。 [!DNL AWS Kinesis]と[!DNL Azure Event Hubs]の宛先の場合、データはJSON形式でExperience Platformからストリーミングされます。

![Adobeクラウドのストレージ先](../../assets/catalog/cloud-storage/cloud-storage-destinations.png)

クラウドストレージの宛先への接続方法について詳しくは、「[クラウドストレージの宛先を作成するためのワークフロー](./workflow.md)」を参照してください。

## データのエクスポートのタイプ

**プロファイルベースの書き出し** - オーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。

## 利用可能なクラウドストレージの宛先

- [Amazon S3 の宛先](./amazon-s3.md)
- [Azure Blobの宛先](./azure-blob.md)
- [SFTP の宛先](./sftp.md)

## 利用可能なクラウドストレージストリーミング先

- [AmazonKinesis駅](./amazon-kinesis.md)
- [Azureイベントハブの宛先](./azure-event-hubs.md)
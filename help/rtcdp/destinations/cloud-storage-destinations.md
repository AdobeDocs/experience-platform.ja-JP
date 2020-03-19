---
title: クラウドのストレージの宛先
seo-title: クラウドのストレージの宛先
description: Adobe Real-time CDPは、セグメントをデータファイルとしてAmazon S3またはSFTPクラウドのストレージの場所に配信できます。 今後のリリースでは、クラウドストレージの宛先をさらに追加する予定です。
seo-description: Adobe Real-time CDPは、セグメントをデータファイルとしてAmazon S3またはSFTPクラウドのストレージの場所に配信できます。 今後のリリースでは、クラウドストレージの宛先をさらに追加する予定です。
translation-type: tm+mt
source-git-commit: 8c3ce7e7258ef2597e8089cf7928e42471278591

---


# クラウドのストレージの宛先 {#cloud-storage-destinations}

Adobe Real-time CDPは、セグメントをデータファイルとしてAmazon S3またはSFTPクラウドのストレージの場所に配信できます。 これにより、オーディエンスとそのプロファイル属性をCSVまたはタブ区切りファイル経由で内部システムに送信できます。

![Adobe Cloudのストレージの保存先](/help/rtcdp/destinations/assets/cloud-storage-destinations.png)

Adobe Real-time CDPは、現在、 [Amazon S3と](/help/rtcdp/destinations/amazon-s3-destination.md) SFTPの2つのクラウドストレージの宛先をサポートしています [](/help/rtcdp/destinations/sftp-destination.md)。 今後のリリースでは、クラウドストレージの宛先をさらに追加する予定です。

クラウドストレージの宛先への接続方法について詳しくは、「クラウドストレージの宛先を作成す [るためのワークフロー」を参照してくださ](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)い。

## データのエクスポートの種類

**プロファイルベースのエクスポート** — オーディエンス内の個人に関する詳細をエクスポートします。 これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。


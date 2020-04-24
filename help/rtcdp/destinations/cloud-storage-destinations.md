---
title: クラウドストレージの宛先
seo-title: クラウドストレージの宛先
description: Adobe Real-time CDP は、セグメントをデータファイルとして Amazon S3 または SFTP クラウドのストレージの場所に配信できます。今後のリリースでは、クラウドストレージの宛先をさらに追加する予定です。
seo-description: Adobe Real-time CDP は、セグメントをデータファイルとして Amazon S3 または SFTP クラウドのストレージの場所に配信できます。今後のリリースでは、クラウドストレージの宛先をさらに追加する予定です。
translation-type: tm+mt
source-git-commit: f3c6c27b7ad07ada0df18aabe0e8503253b38342

---


# クラウドストレージの宛先 {#cloud-storage-destinations}

Adobe Real-time CDP は、セグメントをデータファイルとして Amazon S3 または SFTP クラウドのストレージの場所に配信できます。これにより、オーディエンスとそのプロファイル属性を CSV またはタブ区切りファイル経由で内部システムに送信できます。

![Adobe Cloud のストレージの保存先](/help/rtcdp/destinations/assets/cloud-storage-destinations.png)

Adobe Real-time CDP は、現在、[Amazon S3](/help/rtcdp/destinations/amazon-s3-destination.md) と [SFTP](/help/rtcdp/destinations/sftp-destination.md) の 2 つのクラウドストレージの宛先をサポートしています。今後のリリースでは、クラウドストレージの宛先をさらに追加する予定です。

クラウドストレージの宛先への接続方法について詳しくは、「[クラウドストレージの宛先を作成するためのワークフロー](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)」を参照してください。

## データのエクスポートのタイプ

**プロファイルベースの書き出し** - オーディエンスの個人に関する情報を書き出します。これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。


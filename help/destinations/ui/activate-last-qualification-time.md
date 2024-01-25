---
title: 新しいベータクラウドストレージの宛先で最終認定時間 XDM 属性を使用します。
description: 新しいベータクラウドストレージの宛先で XDM 属性の最終認定時間を使用する方法を説明します。
badgeBeta: label="ベータ版" type="Informative"
exl-id: d077ea10-5ff2-4acc-8ee6-78ea6cd752d1
source-git-commit: 7130ac46a7768ea6e71bf73eb970bf2890323d0f
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 10%

---

# 新しいベータクラウドストレージの宛先で最終認定時間 XDM 属性を使用します。 {#last-qualification-time}

>[!IMPORTANT]
> 
>このページでは、ベータ版の機能について説明します。 機能とドキュメントは変更される場合があります。このベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。

## 前提条件 {#prerequisites}

最終認定時間 (`lastQualificationTime`) XDM 属性を使用する場合は、以下の 6 つのクラウドストレージの宛先のいずれかにデータを書き出す必要があります。

* [[!DNL ADLS Gen 2]](/help/destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md)
* [[!DNL Azure Blob]](/help/destinations/catalog/cloud-storage/azure-blob.md)
* [[!DNL Data Landing Zon]e](/help/destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL Google Cloud Storage]](/help/destinations/catalog/cloud-storage/google-cloud-storage.md)
* [SFTP](/help/destinations/catalog/cloud-storage/sftp.md)

## 最終認定時間 XDM 属性の使用方法 {#how-to-use}

上記の 6 つのクラウドストレージコネクタのいずれかを使用している場合は、XDM 属性の最終認定時間を [マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) の最新のタイムスタンプを使用して、プロファイルがセグメントの対象として認定された時点での、書き出されたファイルに列を作成するためのアクティベーションワークフローに関する情報です。 これにより、特定の測定または分析の使用例に役立ち、特定のオーディエンスをアクティブ化するタイミングをより深く把握できます。

を追加します。 `lastQualificationTime` をファイルのエクスポートに追加する場合は、現在、値を手動で挿入する必要があります `xdm: segmentMembership.ups.seg_id.lastQualificationTime` を「ソース」フィールドに挿入します。 ターゲットフィールドを編集して、 `lastQualificationTime` またはこの列に名前を付ける他の値。 これはベータ版機能なので、 `xdm: segmentMembership.ups.seg_id.lastQualificationTime` 値は将来変更される可能性があります。

![マッピング手順に貼り付けた XDM 属性の最終選定時間を示す画面記録](/help/destinations/ui/last-qualification-time.gif)

## 詳細 {#more-information}

ワークフロー内のすべての手順や必要な権限を含む、ファイルベースの宛先に対するデータのアクティブ化に関する詳細については、 [ファイルベースの宛先のアクティブ化チュートリアル](/help/destinations/ui/activate-batch-profile-destinations.md).

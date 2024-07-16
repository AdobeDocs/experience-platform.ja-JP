---
title: 新しいベータ版のクラウドストレージ宛先での最終選定時間 XDM 属性の使用
description: 新しいベータ版クラウドストレージ宛先での最終選定時間 XDM 属性の使用方法を説明します
badgeBeta: label="ベータ版" type="Informative"
exl-id: d077ea10-5ff2-4acc-8ee6-78ea6cd752d1
source-git-commit: 7130ac46a7768ea6e71bf73eb970bf2890323d0f
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 10%

---

# 新しいベータ版のクラウドストレージ宛先での最終選定時間 XDM 属性の使用 {#last-qualification-time}

>[!IMPORTANT]
> 
>このページでは、ベータ版の機能について説明します。 機能とドキュメントは変更される場合があります。このベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。

## 前提条件 {#prerequisites}

最終選定時間（`lastQualificationTime`） XDM 属性を使用するには、以下に示す 6 つのクラウドストレージ宛先のいずれかにデータを書き出す必要があります。

* [[!DNL ADLS Gen 2]](/help/destinations/catalog/cloud-storage/adls-gen2.md)
* [[!DNL Amazon S3]](/help/destinations/catalog/cloud-storage/amazon-s3.md)
* [[!DNL Azure Blob]](/help/destinations/catalog/cloud-storage/azure-blob.md)
* [[!DNL Data Landing Zon]e](/help/destinations/catalog/cloud-storage/data-landing-zone.md)
* [[!DNL Google Cloud Storage]](/help/destinations/catalog/cloud-storage/google-cloud-storage.md)
* [SFTP](/help/destinations/catalog/cloud-storage/sftp.md)

## 最終選定時間 XDM 属性の使用方法 {#how-to-use}

上記の 6 つのクラウドストレージコネクタのいずれかを使用している場合、アクティベーションワークフローの [ マッピング手順 ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) で最終選定時間の XDM 属性を使用して、プロファイルがセグメントに選定されたときの最新のタイムスタンプで、書き出されたファイルに列を作成できます。 これにより、特定の測定または分析のユースケースに役立つほか、特定のオーディエンスをアクティブ化するタイミングをより正確に把握できます。

ファイルの書き出しに `lastQualificationTime` を追加するには、以下に示すように、現在は値 `xdm: segmentMembership.ups.seg_id.lastQualificationTime` をソースフィールドに手動で挿入する必要があります。 また、ターゲットフィールドを編集して、`lastQualificationTime` や、この列に名前を付けるその他の値に変更することもできます。 これはベータ版の機能なので、`xdm: segmentMembership.ups.seg_id.lastQualificationTime` 値の構文は将来的に変更される可能性があります。

![ 最後の選定時間を示す画面記録 XDM 属性をマッピングステップに貼り付け ](/help/destinations/ui/last-qualification-time.gif)

## 詳細 {#more-information}

ワークフローのすべての手順や必要な権限など、ファイルベースの宛先に対するデータのアクティブ化について詳しくは、[ ファイルベースの宛先のアクティブ化に関するチュートリアル ](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

---
keywords: クラウドストレージの宛先；クラウドストレージ
title: クラウドストレージの宛先の概要
description: Adobe Experience Platformは、セグメントをデータファイルとしてAmazon S3、AWS Kinesis、Azure Event Hubs、または SFTP クラウドのストレージの場所に配信できます。
exl-id: d29f0a6e-b323-4f78-bbd0-dee2f1e0fedb
source-git-commit: 818c9a6f5cd39db1405827b657872b99254ad1e5
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 2%

---

# クラウドストレージの宛先の概要 {#cloud-storage-destinations}

## 概要 {#overview}

Adobe Experience Platformは、セグメントをデータファイルとしてクラウドストレージの場所に配信できます。 これにより、オーディエンスとそのプロファイル属性を、 [!DNL Amazon S3], [!DNL Azure Blob] と SFTP に含まれます。 の場合 [!DNL Amazon Kinesis] および [!DNL Azure Event Hubs] の宛先、データはでExperience Platformからストリーミングされる [!DNL JSON] 形式

![Adobeクラウドストレージの宛先](../../assets/catalog/cloud-storage/cloud-storage-destinations.png)

## サポートされるクラウドストレージの宛先 {#supported-destinations}

Adobe Experience Platformは、次のクラウドストレージの宛先をサポートしています。

* [Amazon Kinesis接続](amazon-kinesis.md)
* [Amazon S3 接続](amazon-s3.md)
* [Azure BLOB 接続](azure-blob.md)
* [Azure Event Hubs 接続](azure-event-hubs.md)
* [SFTP 接続](sftp.md)

## 新しいクラウドストレージの宛先に接続 {#connect-destination}

キャンペーンのクラウドストレージの宛先にセグメントを送信するには、まず Platform が宛先に接続する必要があります。 詳しくは、 [宛先の作成チュートリアル](../../ui/connect-destination.md) を参照してください。


## マクロを使用して、ストレージの場所にフォルダーを作成する {#use-macros}

>[!NOTE]
>
> この節で説明する機能は、現在、 [Amazon S3](amazon-s3.md) の宛先のみ。

ストレージの場所にあるセグメントファイルごとにカスタムフォルダーを作成するには、フォルダーパスの入力フィールドにマクロを使用します。 次に示すように、入力フィールドの末尾にマクロを挿入します。

![マクロを使用してストレージにフォルダーを作成する方法](../../assets/catalog/cloud-storage/workflow/macros-folder-path.png)

以下の例では、サンプルセグメントを参照しています `Luxury Audience` ID を持つ `25768be6-ebd5-45cc-8913-12fb3f348615`.

**マクロ 1:`%SEGMENT_NAME%`**

入力： `acme/campaigns/2021/%SEGMENT_NAME%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/Luxury Audience`

**マクロ 2:`%SEGMENT_ID%`**

入力： `acme/campaigns/2021/%SEGMENT_ID%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/25768be6-ebd5-45cc-8913-12fb3f348615`

**マクロ 3:`%SEGMENT_NAME%/%SEGMENT_ID%`**

入力： `acme/campaigns/2021/%SEGMENT_NAME%/%SEGMENT_ID%`
ストレージの場所のフォルダーパス： `acme/campaigns/2021/Luxury Audience/25768be6-ebd5-45cc-8913-12fb3f348615`

## データのエクスポートのタイプ {#export-type}

クラウドストレージの宛先のサポート **プロファイルベースの書き出し**. つまり、オーディエンスの個人に関する詳細を書き出します。 これらの詳細はパーソナライゼーションに必要で、属性、イベント、セグメントのメンバーシップなどを含めることができます。
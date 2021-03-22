---
keywords: IPアドレス、IP範囲、許可リスト先
title: 'クラウドストレージ先のIPアドレス許可リスト '
type: ドキュメント
description: このページには、許可リストに追加できるIP範囲が含まれています。このIP範囲は、Experience PlatformからSFTPサーバー、AmazonS3、またはAzure Blobストレージに安全にデータをエクスポートするために使用できます。
translation-type: tm+mt
source-git-commit: 7d7568de57cf79843a833a05b9bdfa6eb048bdbc
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 0%

---


# クラウドストレージの宛先のIPアドレス許可リスト{#ip-address-allow-list}

## 概要 {#overview}

>[!IMPORTANT]
>
> * Adobeでは、このページにブックマークを付け、3か月ごとに再度アクセスして、最新のIPアドレスを確認することをお勧めします。 Adobeは、新しいIP範囲の通知を提供しません。
> * AdobeはSFTPサーバーへのデータのエクスポートをサポートしていますが、データのエクスポートに推奨されるクラウドストレージの場所は[!DNL Amazon S3]と[!DNL Azure Blob]です。


このページには、許可リストに追加できるIP範囲があり、Experience Platformから[SFTPサーバー](./sftp.md)、[AmazonS3](./amazon-s3.md)、または[Azure BLOB](./azure-blob.md)ストレージに安全にデータをエクスポートできます。

ネットワークアクセス制御は、ネットワークファイアウォールを通じて定義できます。 適切なIP範囲を指定することで、データ転送サービスのトラフィックを許可できます。

Adobeでは、クラウドストレージの宛先接続を操作する前に、許可リストに次のIP範囲を追加することをお勧めします。 地域固有のIP範囲を許可リストに追加できないと、クラウドストレージの宛先接続を使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。

## すべてのお客様に必須

* `52.247.108.70`

## 米国のお客様

* `52.252.71.64/29`

## EMEAのお客様

* `51.137.8.208/29`

## APACのお客様

* `20.53.201.168/29`
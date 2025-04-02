---
title: ファイルベースのクラウドストレージの宛先の IP アドレスの許可リスト
type: Documentation
description: このページでは、Experience Platformからクラウドストレージの宛先に安全にデータを書き出すために、許可リストに追加できる IP 範囲を提供します。
exl-id: 0b8086aa-786e-4244-b2a5-a3f57ad59a8b
source-git-commit: 7cf15550d7619e247052efc4d9b4c72c5d32641a
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 5%

---

# 許可リストに加える ファイルベースのクラウドストレージの宛先の IP アドレス。 {#ip-address-allow-list-cloud-storage}

>[!IMPORTANT]
>
> * Adobeでは、このページをブックマークに追加し、3 か月ごとに再訪問して最新の IP アドレスを確認することをお勧めします。 Adobeは新しい IP 範囲の通知を行いません。
> * Adobeでは SFTP サーバーへのデータの書き出しをサポートしていますが、データを書き出す際に推奨されるクラウドストレージの場所は [!DNL Amazon S3] と [!DNL Azure Blob] です。

## 適用性 {#applicability}

このページの IP 範囲の情報は、宛先カタログの次のファイルベースのクラウドストレージコネクタに適用されます。

* [[!UICONTROL Amazon S3]](./amazon-s3.md)
* [[!UICONTROL Google Cloud Storage]](google-cloud-storage.md)
* [SFTP](./sftp.md)

>[!IMPORTANT]
>
>このページで説明している IP 範囲は、ファイルベースのクラウドストレージ宛先 [!UICONTROL Azure Blob]、[!UICONTROL Azure Data Lake Storage Gen2] および [!UICONTROL  データランディングゾーン ] では *サポートされていません*。

## 概要 {#overview}

このページでは、Experience Platformから複数のクラウドストレージの宛先に安全にデータを書き出すために、許可リストに追加できる IP 範囲を提供します。

ネットワークファイアウォールを介して、ネットワークアクセス制御を定義できます。 適切な IP 範囲を指定することで、データ転送サービスのトラフィックを許可できます。

Adobeでは、クラウドストレージの宛先接続を使用する前に、次の IP 範囲を許可リストに追加することをお勧めします。 地域固有の IP 範囲を許可リストに追加しないと、クラウドストレージの宛先接続を使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。

## すべてのお客様に必須 {#all-customers}

* `52.247.108.70`

## AWSを使用している米国のお客様 {#aws}

以下の IP 範囲は、Amazon Web Services（AWS）で動作するExperience Platformのお客様に適用されます。 詳しくは、[Experience Platform Multi-Cloud の概要 ](../../../landing/multi-cloud.md) を参照してください。

>[!NOTE]
>
>この IP 範囲は、AWSで稼動しているお客様がファイルベースの宛先を使用してAmazon S3 にデータを書き出す場合はサポートされません。

* `66.117.18.0/24`

## 米国のお客様 {#us-customers}

* `52.252.71.64/29`

## カナダのお客様 {#canada-customers}

* `20.220.135.16/29`

## EMEA のお客様 {#emea-customers}

* `51.137.8.208/29`

## 英国の顧客 {#uk-customers}

* `20.26.133.96/29`

## APAC のお客様 {#apac-customers}

* `20.53.201.168/29`

---
keywords: IP アドレス，IP 範囲，許可リスト 許可リストに加える,
title: クエリサービスの IP アドレスの許可リスト
description: このページでは、許可リストに追加できる IP 範囲を提供します。
exl-id: f6745e0f-d387-45f2-9f72-054e721016ff
source-git-commit: 3a00f98b7463f7fb35aa53f703d67193f18400cf
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 7%

---

# IP アドレスの許可リスト {#ip-address-allow-list}

>[!IMPORTANT]
>
> * Adobeでは、このページをブックマークに追加し、3 か月ごとに再訪問して最新の IP アドレスを確認することをお勧めします。 Adobeは新しい IP 範囲を通知しません。
> * Adobeでは SFTP サーバーへのデータの書き出しをサポートしていますが、データを書き出す際に推奨されるクラウドストレージの場所は [!DNL Amazon S3] と [!DNL Azure Blob] です。

## 概要 {#overview}

このページでは、Experience Platformから [SFTP サーバー ](../destinations/catalog/cloud-storage/sftp.md) にデータを安全に書き出すために、許可リストに追加できる IP アドレスを提供します。

ネットワークファイアウォールを介して、ネットワークアクセス制御を定義できます。 適切な IP 範囲を指定することで、データ転送サービスのトラフィックを許可できます。

Adobeでは、地域に応じて次の IP 範囲を許可リストに追加することをお勧めします。 地域固有の IP 範囲を許可リストに追加しないと、エラーやパフォーマンス低下が発生する場合があります。

## VA7：米国およびアメリカのお客様 {#us-americas}

* 20.14.241.153

## NLD2: EMEA のお客様 {#emea}

* 20.101.233.128

## AUS5:APAC のお客様 {#apac}

* 20.248.220.69

## CAN2：カナダのお客様 {#can2}

* 4.172.1.139

## GBR9:United Kingdon のお客様 {#gbr9}

* 20.108.200.9


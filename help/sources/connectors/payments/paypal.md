---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: PayPalコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 52a01c5f90a9643691a25e2d0a5f03a7f0334a7d
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 13%

---


# （ベータ版） [!DNL PayPal] コネクタ

>[!NOTE]
>コネクタ [!DNL PayPal] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティ支払い申込書からデータを取り込むためのサポートを提供します。 支払いプロバイダのサポートは以下のとおりで [!DNL PayPal]す。

## IPアドレス許可リスト

ソースコネクタを使用する前に、許可リストに次のIPアドレスを追加する必要があります。 地域固有のIPアドレスを許可リストに追加できないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下したりする可能性があります。

### 米国東部地域

- `20.41.2.0/23`
- `20.41.4.0/26`
- `20.44.17.80/28`
- `20.49.102.16/29`
- `40.70.148.160/28`
- `52.167.107.224/28`

### 西ヨーロッパ地域

- `13.69.67.192/28`
- `13.69.107.112/28`
- `13.69.112.128/28`
- `40.74.24.192/26`
- `40.74.26.0/23`
- `40.113.176.232/29`
- `52.236.187.112/28`

### オーストラリア東部

- `13.70.74.144/28`
- `20.37.193.0/25`
- `20.37.193.128/26`
- `20.37.198.224/29`
- `40.79.163.80/28`
- `40.79.171.160/28`

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL PayPal] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

## API [!DNL PayPal] を [!DNL Platform] 使用した接続

- [Flow Service APIを使用してPayPalコネクタを作成する](../../tutorials/api/create/payments/paypal.md)
- [Flow Service APIを使用して支払申込書を調査します](../../tutorials/api/explore/payments.md)
- [Flow Service APIを使用して、支払い申請からデータを収集する](../../tutorials/api/collect/payments.md)

## UI [!DNL PayPal] を [!DNL Platform] 使用して接続

- [UI での PayPal ソースコネクタの作成](../../tutorials/ui/create/payments/paypal.md)
- [UIで支払コネクタのデータフローを構成します](../../tutorials/ui/dataflow/payments.md)
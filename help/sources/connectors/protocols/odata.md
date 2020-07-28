---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 汎用ODataコネクタ
topic: overview
translation-type: tm+mt
source-git-commit: 52a01c5f90a9643691a25e2d0a5f03a7f0334a7d
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 9%

---


# （ベータ版） [!DNL Generic OData] コネクタ

>[!NOTE]
>コネクタ [!DNL Generic OData] はベータ版です。 ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) 「」を参照してください。

Adobe Experience Platform allows data to be ingested from external sources while providing you with the ability to structure, label, and enhance incoming data using [!DNL Platform] services. アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのプロトコルアプリケーションからデータを取り込むためのサポートを提供します。 プロトコルプロバイダーのサポートには以下が含まれ [!DNL Generic OData]ます。

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

次のドキュメントは、APIまたはユーザーインターフェイス [!DNL Generic OData] を [!DNL Platform] 使用した接続方法に関する情報を提供しています。

## API [!DNL Generic OData] を [!DNL Platform] 使用した接続

- [Flow Service APIを使用して汎用ODataコネクタを作成する](../../tutorials/api/create/protocols/odata.md)
- [Flow Service APIを使用したプロトコルアプリケーションの調査](../../tutorials/api/explore/protocols.md)
- [Flow Service APIを使用したプロトコルアプリケーションからのデータ収集](../../tutorials/api/collect/protocols.md)

## UI [!DNL Generic OData] を [!DNL Platform] 使用して接続

- [UIで汎用ODataソースコネクタを作成する](../../tutorials/ui/create/protocols/odata.md)
- [UIでのプロトコルコネクタ用のデータフローの設定](../../tutorials/ui/dataflow/protocols.md)
---
keywords: Experience Platform；ホーム；人気のトピック；Apache Hadoop分散ファイルシステム；Apache hadoop;hdfs;HDFS
solution: Experience Platform
title: Flow Service API を使用した Apache HDFS ベース接続の作成
type: Tutorial
description: Flow Service API を使用して Apache Hadoop分散ファイルシステムをAdobe Experience Platformに接続する方法について説明します。
exl-id: 04fa65db-073c-48e1-b981-425185ae08aa
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 43%

---

# [!DNL Flow Service] API を使用した [!DNL Apache] HDFS ベース接続の作成

>[!NOTE]
>
>Apache HDFS コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[&#x200B; ソースの概要 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Apache Hadoop Distributed File System] （以下「[!DNL HDFS]」）のベース接続を作成する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL HDFS] ています。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | URL は、[!DNL HDFS] に匿名で接続するために必要な認証パラメーターを定義します。 この値の取得方法について詳しくは、[&#x200B; この  [!DNL HDFS]  ドキュメント &#x200B;](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html) を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL AdWords] の接続仕様 ID は `54e221aa-d342-4707-bcff-7a4bceef0001` です。 |

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL HDFS] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL HDFS] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "HDFS test connection",
        "description": "A test connection for an HDFS source",
        "auth": {
            "specName": "Anonymous Authentication",
            "params": {
                "url": "{URL}"
                }
        },
        "connectionSpec": {
            "id": "54e221aa-d342-4707-bcff-7a4bceef0001",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.url` | [!DNL HDFS] に匿名で接続するために必要な認証パラメーターを定義する URL |
| `connectionSpec.id` | [!DNL HDFS] 接続仕様 ID: `54e221aa-d342-4707-bcff-7a4bceef0001`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6a6a880a-2b15-4051-aa88-0a2b1570516d",
    "etag": "\"1801bb7d-0000-0200-0000-5ed6ad580000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL HDFS] 接続を作成し、接続の一意の ID 値を取得しました。 次のチュートリアルでは、この ID を使用した、[Flow Service API を使用したサードパーティのクラウドストレージの調査 &#x200B;](../../explore/cloud-storage.md) 方法を説明します。

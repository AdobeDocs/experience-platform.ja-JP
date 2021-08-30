---
keywords: Experience Platform；ホーム；人気のあるトピック；ApacheHadoop分散ファイルシステム；Apachehadoop;hdfs;HDFS
solution: Experience Platform
title: フローサービスAPIを使用したApache HDFSベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用して、ApacheHadoop分散ファイルシステムをAdobe Experience Platformに接続する方法について説明します。
exl-id: 04fa65db-073c-48e1-b981-425185ae08aa
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL Apache] HDFSベース接続を作成する

>[!NOTE]
>
>Apache HDFSコネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Apache Hadoop Distributed File System]（以下「[!DNL HDFS]」と呼びます）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL HDFS]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | URLは、[!DNL HDFS]に匿名で接続するために必要な認証パラメーターを定義します。 この値の取得方法の詳細については、[this [!DNL HDFS] document](https://hadoop.apache.org/docs/r1.2.1/HttpAuthentication.html)を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL AdWords]の接続仕様IDは次のとおりです。`54e221aa-d342-4707-bcff-7a4bceef0001`. |

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL HDFS]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL HDFS]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `auth.params.url` | [!DNL HDFS]に匿名で接続するために必要な認証パラメーターを定義するURL |
| `connectionSpec.id` | [!DNL HDFS]接続仕様ID:`54e221aa-d342-4707-bcff-7a4bceef0001`. |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6a6a880a-2b15-4051-aa88-0a2b1570516d",
    "etag": "\"1801bb7d-0000-0200-0000-5ed6ad580000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL HDFS]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/cloud-storage.md)を使用してサードパーティのクラウドストレージを調べる方法を学ぶ際に使用できます。[

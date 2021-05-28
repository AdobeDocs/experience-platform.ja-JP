---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure;Azure File Storage;Azureファイルストレージ
solution: Experience Platform
title: フローサービスAPIを使用したAzureファイルストレージソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAzureファイルストレージをAdobe Experience Platformに接続する方法を説明します。
exl-id: 0c585ae2-be2d-4167-b04b-836f7e2c04a9
source-git-commit: e150f05df2107d7b3a2e95a55dc4ad072294279e
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 35%

---

# [!DNL Flow Service] APIを使用して[!DNL Azure File Storage]ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Azure File Storage]を[!DNL Experience Platform]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Azure File Storage]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Azure File Storage]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アクセスする[!DNL Azure File Storag]インスタンスのエンドポイント。 |
| `userId` | [!DNL Azure File Storage]エンドポイントへの十分なアクセス権を持つユーザー。 |
| `password` | [!DNL Azure File Storage]インスタンスのパスワード |
| 接続仕様ID | 接続の作成に必要な一意の識別子。 [!DNL Azure File Storage]の接続仕様IDは次のとおりです。`be5ec48c-5b78-49d5-b8fa-7c89ec4569b8` |

使い始める方法の詳細については、[このAzure File Storageドキュメント](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続では、ソースを指定し、そのソースの資格情報を含めます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、[!DNL Azure File Storage]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Azure File Storage]接続を作成するには、一意の接続仕様IDをPOSTリクエストの一部として指定する必要があります。 [!DNL Azure File Storage]の接続仕様IDは`be5ec48c-5b78-49d5-b8fa-7c89ec4569b8`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
        -d '{
        "name": "Azure File Storage connection",
        "description": "An Azure File Storage test connection",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                    "host": "{HOST}",
                    "userId": "{USER_ID}",
                    "password": "{PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "be5ec48c-5b78-49d5-b8fa-7c89ec4569b8",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | アクセスする[!DNL Azure File Storage]インスタンスのエンドポイント。. |
| `auth.params.userId` | [!DNL Azure File Storage]エンドポイントへの十分なアクセス権を持つユーザー。 |
| `auth.params.password` | [!DNL Azure File Storage]アクセスキー。 |
| `connectionSpec.id` | [!DNL Azure File Storage]接続仕様ID:`be5ec48c-5b78-49d5-b8fa-7c89ec4569b8`. |

**応答** 

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f9377f50-607a-4818-b77f-50607a181860",
    "etag": "\"2f0276fa-0000-0200-0000-5eab3abb0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Azure File Storage]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/cloud-storage.md)を使用してサードパーティのクラウドストレージを調べる方法を学ぶ際に使用できます。[

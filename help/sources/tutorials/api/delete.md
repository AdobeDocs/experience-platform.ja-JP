---
keywords: Experience Platform；ホーム；人気の高いトピック；フローサービス；アカウントの削除；削除；api
solution: Experience Platform
title: Flow Service APIを使用したアカウントの削除
topic: 概要
type: チュートリアル
description: Flow Service APIを使用してアカウントを削除する方法を説明します。
translation-type: tm+mt
source-git-commit: 37be5f5ffa4640d7d4442a24cc257069237f15cb
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 27%

---


# Flow Service APIを使用したアカウントの削除

Adobe Experience Platformは、[!DNL Platform]サービスを使用して、外部ソースからデータを取り込むと同時に、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Flow Service] は、Adobe Experience Platform内のさまざまな異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、ユーザーインターフェイスとRESTful APIを提供し、サポートされるすべてのソースを接続できます。

このチュートリアルでは、[[!DNL Flow Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して削除する手順を説明します。

## はじめに

このチュートリアルでは、有効な接続IDが必要です。 有効な接続IDがない場合は、[ソースの概要](../../home.md)から選択したコネクタを選択し、このチュートリアルを試みる前に説明した手順に従ってください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して接続を正しく削除するために必要な追加情報については、以下の節で説明します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の詳細を検索する

>[!NOTE]
>このチュートリアルでは、[Azure Blobソースコネクタ](../../connectors/cloud-storage/blob.md)を例として使用しますが、概要の手順は、[使用可能なソースコネクタ](../../home.md)のいずれかに適用されます。

接続情報を更新する最初の手順は、接続IDを使用して接続の詳細を取得することです。

**API 形式**

```http
GET /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 取得する接続の一意の`id`値。 |

**リクエスト**

次の例は、接続IDに関する情報を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/dd3631cd-d0ea-4fea-b631-cdd0ea6fea21' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、資格情報、一意の識別子(`id`)、バージョンを含む、接続の現在の詳細が返されます。

```json
{
    "items": [
        {
            "createdAt": 1603514659165,
            "updatedAt": 1603514659165,
            "createdBy": "{CREATED_BY}",
            "updatedBy": "{UPDATED_BY}",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT",
            "sandboxId": "{SANDBOX_ID}",
            "sandboxName": "{SANDBOX_NAME}",
            "id": "dd3631cd-d0ea-4fea-b631-cdd0ea6fea21",
            "name": "Test Azure Blob Connector",
            "description": "A test connector for Azure Blob",
            "connectionSpec": {
                "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
                "version": "1.0"
            },
            "state": "enabled",
            "auth": {
                "specName": "ConnectionString",
                "params": {
                    "connectionString": "xxxx"
                }
            },
            "version": "\"07001eed-0000-0200-0000-5f93b1250000\"",
            "etag": "\"07001eed-0000-0200-0000-5f93b1250000\""
        }
    ]
}
```

## 接続の削除

既存の接続IDを取得したら、[!DNL Flow Service] APIに対するDELETEリクエストを実行します。

**API 形式**

```http
DELETE /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 削除する接続の一意の`id`値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform-int.adobe.io/data/foundation/flowservice/connections/dd3631cd-d0ea-4fea-b631-cdd0ea6fea21' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

接続に対してルックアップ(GET)要求を試行すると、削除を確認できます。

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して既存のアカウントを削除できます。

ユーザーインターフェイスを使用してこれらの操作を実行する手順については、UI](../../tutorials/ui/delete-accounts.md)でのアカウントの削除に関するチュートリアルを参照してください[

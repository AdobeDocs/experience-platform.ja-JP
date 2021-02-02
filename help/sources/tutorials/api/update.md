---
keywords: Experience Platform；ホーム；人気の高いトピック；フローサービス；接続の更新
solution: Experience Platform
title: Flow Service APIを使用して接続情報を更新する
topic: overview
type: Tutorial
description: 状況によっては、既存のソース接続の詳細を更新する必要がある場合があります。 Flow Service APIを使用すると、既存のバッチ接続またはストリーミング接続（名前、説明、秘密鍵証明書など）の詳細を追加、編集および削除できます。
translation-type: tm+mt
source-git-commit: ece2ae1eea8426813a95c18096c1b428acfd1a71
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 14%

---


# Flow Service APIを使用して接続情報を更新する

状況によっては、既存のソース接続の詳細を更新する必要がある場合があります。 [!DNL Flow Service] 既存のバッチ接続またはストリーミング接続（名前、説明、秘密鍵証明書など）の詳細を追加、編集および削除できます。

このチュートリアルでは、[[!DNL Flow Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して既存の接続の詳細と秘密鍵証明書を更新する手順を説明します。

## はじめに

このチュートリアルでは、有効な接続IDが必要です。 有効な接続IDがない場合は、[ソースの概要](../../home.md)から選択したコネクタを選択し、このチュートリアルを試みる前に説明した手順に従ってください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して接続の情報を正しく更新するために必要な追加情報については、以下の節で説明します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] APIを呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。 次に示すように、すべての[!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の詳細を検索する

>[!NOTE]
>このチュートリアルでは、例として[Salesforceソースコネクタ](../../connectors/crm/salesforce.md)を使用しますが、概要の手順は、[使用可能なソースコネクタ](../../home.md)のいずれかに適用されます。

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
    'https://platform.adobe.io/data/foundation/flowservice/connections/139f6a5f-a78b-4744-9f6a-5fa78bd74431' \
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
            "createdAt": 1597973312000,
            "updatedAt": 1597973312000,
            "createdBy": "{CREATED_BY}",
            "updatedBy": "{UPDATED_BY}",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT}",
            "sandboxName": "{SANDBOX_NAME}",
            "id": "139f6a5f-a78b-4744-9f6a-5fa78bd74431",
            "name": "E2E_SF Base_Connection",
            "connectionSpec": {
                "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
                "version": "1.0"
            },
            "state": "enabled",
            "auth": {
                "specName": "Basic Authentication",
                "params": {
                    "securityToken": "{SECURITY_TOKEN}",
                    "password": "{PASSWORD}",
                    "username": "my-salesforce-account",
                    "environmentUrl": "login.salesforce.com"
                }
            },
            "version": "\"1400dd53-0000-0200-0000-5f3f23450000\"",
            "etag": "\"1400dd53-0000-0200-0000-5f3f23450000\""
        }
    ]
}
```

## 接続の更新

既存の接続IDを取得したら、[!DNL Flow Service] APIに対するPATCHリクエストを実行します。

>[!IMPORTANT]
>PATCHリクエストには`If-Match`ヘッダーを使用する必要があります。 このヘッダーの値は、接続の一意のバージョンです。

**API 形式**

```http
PATCH /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 更新する接続の一意の`id`値。 |

**リクエスト**

次の要求は、接続を更新するための新しい情報を提供します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/connections/139f6a5f-a78b-4744-9f6a-5fa78bd74431' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: 1400dd53-0000-0200-0000-5f3f23450000' \
    -d '[
        {
            "op": "replace",
            "path": "/auth/params",
            "value": {
                "username": "salesforce-connector-username",
                "password": "{NEW_PASSWORD}",
                "securityToken": "{NEW_SECURITY_TOKEN}"
            }
        },
        {
            "op": "replace",
            "path": "/name",
            "value": "Test salesforce connection"
        },
        {
            "op": "add",
            "path": "/description",
            "value": "A test salesforce connection"
        }
    ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | 接続の更新に必要なアクションを定義するために使用される操作呼び出し。 次の操作があります。`add`、`replace`、`remove`。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答** 

応答が成功すると、接続IDと更新されたeタグが返されます。

```json
{
    "id": "139f6a5f-a78b-4744-9f6a-5fa78bd74431",
    "etag": "\"3600e378-0000-0200-0000-5f40212f0000\""
}
```

## 更新された接続の詳細を検索します

[!DNL Flow Service] APIにGETリクエストを行うことで行った変更を確認するために、更新したのと同じ接続IDを取得できます。

**API 形式**

```http
GET /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 取得する接続の一意の`id`値。 |

**リクエスト**

次の要求は、接続IDに関する更新された情報を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/139f6a5f-a78b-4744-9f6a-5fa78bd74431' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常に応答すると、新しい名前、説明、バージョンなど、接続IDの更新された詳細が返されます。

```json
{
    "items": [
        {
            "createdAt": 1597973312000,
            "updatedAt": 1598038319627,
            "createdBy": "{CREATED_BY}",
            "updatedBy": "{UPDATED_BY}",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT}",
            "sandboxName": "{SANDBOX_NAME}",
            "id": "139f6a5f-a78b-4744-9f6a-5fa78bd74431",
            "name": "Test salesforce connection",
            "description": "A test salesforce connection",
            "connectionSpec": {
                "id": "cfc0fee1-7dc0-40ef-b73e-d8b134c436f5",
                "version": "1.0"
            },
            "state": "enabled",
            "auth": {
                "specName": "Basic Authentication",
                "params": {
                    "securityToken": "{NEW_SECURITY_TOKEN}",
                    "password": "{PASSWORD}",
                    "username": "salesforce-connector-username"
                }
            },
            "version": "\"3600e378-0000-0200-0000-5f40212f0000\"",
            "etag": "\"3600e378-0000-0200-0000-5f40212f0000\""
        }
    ]
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して、接続に関連付けられた資格情報と情報を更新できます。 ソースコネクタの使用に関する詳細は、[ソースの概要](../../home.md)を参照してください。

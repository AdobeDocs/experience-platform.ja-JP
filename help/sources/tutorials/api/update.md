---
keywords: Experience Platform;ホーム;人気の高いトピック;フローサービス;アカウントの更新
solution: Experience Platform
title: Flow Service API を使用したアカウントの更新
topic-legacy: overview
type: Tutorial
description: このチュートリアルでは、Flow Service API を使用してアカウントの詳細と資格情報を更新する手順を説明します。
exl-id: a93385fd-ed36-457f-8882-41e37f6f209d
source-git-commit: 95f455bd03b7baefe0133a9818c9d048f36f9d38
workflow-type: ht
source-wordcount: '523'
ht-degree: 100%

---

# Flow Service API を使用したアカウントの更新

状況によっては、既存のソース接続の詳細を更新する必要が生じる場合があります。[!DNL Flow Service] には、既存のバッチまたはストリーミング接続（名前、説明、資格情報など）の詳細を追加、編集および削除する機能が用意されています。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、接続の詳細と資格情報を更新する手順を説明します。

## はじめに

このチュートリアルでは、既存の接続と有効な接続 ID が必要です。既存の接続がない場合は、このチュートリアルを試す前に、[ソースの概要](../../home.md)からソースを選択し、説明されている手順に従ってください。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ソース](../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)を参照してください。

## 接続の詳細を検索

接続を更新する最初の手順は、接続 ID を使用して詳細を取得することです。接続の現在の詳細を取得するには、[!DNL Flow Service] API に GET リクエストを実行して、更新したい接続の接続 ID を指定します。

**API 形式**

```http
GET /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 取得したい接続の一意の `id` 値。 |

**リクエスト**

次のリクエストでは、接続に関する情報を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/139f6a5f-a78b-4744-9f6a-5fa78bd74431' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、資格情報や一意の ID（`id`）、およびバージョンを含む接続の現在の詳細が返されます。接続を更新するには、バージョンの値が必要です。

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

## 接続を更新

接続の名前、説明および資格情報を更新するには、[!DNL Flow Service] API に対して PATCH リクエストを実行し、接続 ID、バージョンおよび使用する新しい情報を提供します。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCH リクエストを行う際に必要です。このヘッダーの値は、更新する接続の一意のバージョンです。

**API 形式**

```http
PATCH /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 更新したい接続の一意の `id` 値。 |

**リクエスト**

次のリクエストでは、新しい名前と説明、一連の新しい資格情報を提供して接続を更新します。

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
| `op` | 接続の更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

リクエストが成功した場合は、接続 ID と更新された etag が返されます。更新を検証するには、接続 ID を指定する際に [!DNL Flow Service] API へ GET リクエストを行います。

```json
{
    "id": "139f6a5f-a78b-4744-9f6a-5fa78bd74431",
    "etag": "\"3600e378-0000-0200-0000-5f40212f0000\""
}
```

## 次の手順

このチュートリアルでは、接続に関連付けられた資格情報と情報を、[!DNL Flow Service] API を使用して更新しました。ソースコネクタの使用について詳しくは、[ソースの概要](../../home.md)を参照してください。

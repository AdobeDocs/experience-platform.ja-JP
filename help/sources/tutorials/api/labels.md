---
title: API を使用したソースデータフローへのユーザーアクセスを管理するためのアクセスラベルの適用
description: Flow Service API を使用してアクセスラベルを適用し、ソースデータフローへのユーザーアクセスを管理する方法を説明します。
exl-id: 572d6838-3e4c-4fd5-89fa-32cad6280325
source-git-commit: f57fa04e668fa9c61b9b15778e74969edffae0fa
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 11%

---

# API を使用したソースデータフローへのユーザーアクセスを管理するためのアクセスラベルの適用

Real-Time CDPの [ 属性ベースのアクセス制御 ](../../../access-control/abac/overview.md) で提供される機能を使用して、ソースデータフローにラベルを適用できます。 この機能を使用すると、組織内のユーザーのサブセットのみが特定のソースデータフローにアクセスできるようになります。

アクセスラベルを特定のデータフローに追加すると、そのラベルが割り当てられた役割へのアクセス権を持つユーザーのみが、そのデータフローを表示および編集できます。 ソースデータフローがラベルでマークされていない場合、組織に属するすべてのユーザーに表示されます。 例えば、C12 ラベルをデータフローに適用する場合、C12 ラベルを持たない役割に割り当てられたユーザーは、C12 ラベルを持つデータフローを表示および編集できません。

[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用してソースデータフローにアクセスラベルを適用する方法については、このガイドを参照してください。

## 基本を学ぶ

アクセス制御ラベルを使用する前に、まず属性ベースのアクセス制御の機能を熟知しておいてください。 詳しくは、次のドキュメントを参照してください。

* [属性ベースのアクセス制御の概要](../../../access-control/abac/overview.md)
* [属性ベースのアクセス制御エンドツーエンドガイド](../../../access-control/abac/end-to-end-guide.md)
* [属性ベースのアクセス制御 API ガイド](../../../access-control/abac/api/overview.md)
* [データ使用ラベルの用語集](../../../data-governance/labels/reference.md)

## ソースデータフローへのアクセスラベルの適用

>[!NOTE]
>
>* フロー実行にラベルを適用することはできません。 ただし、フロー実行は、親データフローに適用するラベルを継承します。
>
>* データフローに対する表示アクセス権がない場合、対応するフロー実行も表示できません。

データフローにラベルを追加するには、`/flows` エンドポイントに対してPATCH リクエストを実行し、更新するデータフローの ID を指定します。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{FLOW_ID}` | 更新するデータフローの ID。 |

**リクエスト**

>[!TIP]
>
>PATCH リクエストを行うには、更新するデータフローの version/etag を `if-match` ヘッダーパラメーターとして指定します。

次のリクエストでは、ID `84224def-1e2a-4d95-9ea2-132d697ed2aa` のデータフローに C12 ラベルが追加されます。

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/flowservice/flows/84224def-1e2a-4d95-9ea2-132d697ed2aa' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'if-match: "c5002e0e-0000-0200-0000-67a3c3b70000"'
  -d '[
    {
        "op": "add",
        "path": "/labels",
        "value": ["core/C12"]
    }
]'
```

| プロパティ | 説明 |
| --- | --- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するデータフローの部分。 |
| `value` | プロパティの更新に使用する新しい値。 |



**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。更新を検証するには、[!DNL Flow Service] API へ GET リクエストを行い、その際にフロー ID を指定します。

```json
{
    "id": "84224def-1e2a-4d95-9ea2-132d697ed2aa",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

データフローへのアクセスラベルの設定が正常に完了すると、そのラベルへのアクセス権を持たないユーザーは、データフローを取得できなくなります。 例えば、C12 ラベルをプロビジョニングされていないユーザーが、ID `84224def-1e2a-4d95-9ea2-132d697ed2aa` を使用してデータフローを取得するGET リクエストを行うと、次の応答が返されます。

```json
{
    "type": "https://ns.adobe.com/aep/errors/FLOW-1439-404",
    "title": "Resource not found",
    "status": 404,
    "report": {
        "detailed-message": "The requested flows resource 84224def-1e2a-4d95-9ea2-132d697ed2aa is not found. Verify the resource ID before trying again.",
        "id": "84224def-1e2a-4d95-9ea2-132d697ed2aa",
        "request-id": "{REQUEST_ID}",
        "type": "flows"
    },
    "errorMessage": "The requested flows resource 84224def-1e2a-4d95-9ea2-132d697ed2aa is not found. Verify the resource ID before trying again.",
    "errorDetails": "The requested flows resource 84224def-1e2a-4d95-9ea2-132d697ed2aa is not found. Verify the resource ID before trying again."
}
```

同様に、C12 ラベルへのアクセス権を持たないユーザーは、更新されたデータフローに対してPATCHまたはDELETEのリクエストを行えなくなり、次の応答が返されます。

```json
{
    "type": "https://ns.adobe.com/aep/errors/FLOW-2120-403",
    "title": "Forbidden",
    "status": 403,
    "report": {
        "detailed-message": "You do not have sufficient permissions to perform the operation. Please contact your administrator to resolve permissions and try again.",
        "request-id": "{REQUEST_ID}"
    },
    "errorMessage": "You do not have sufficient permissions to perform the operation. Please contact your administrator to resolve permissions and try again.",
    "errorDetails": "You do not have sufficient permissions to perform the operation. Please contact your administrator to resolve permissions and try again."
}
```

## 次の手順

これで、ソースデータフローにアクセスラベルを適用する方法がわかりました。 組織内の特定のユーザーのグループのみが特定のソースデータフローにアクセスできるようになりました。 詳しくは、次のドキュメントを参照してください。

* [UI でのソースデータフローへのアクセスラベルの適用](../ui/labels.md)
* [アクセス制御の概要](../../../access-control/home.md)

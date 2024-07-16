---
keywords: Experience Platform；ホーム；人気のトピック；api；属性ベースのアクセス制御；属性ベースのアクセス制御
solution: Experience Platform
title: 役割 API エンドポイント
description: 属性ベースのアクセス制御 API の/roles エンドポイントを使用すると、Adobe Experience Platformのロールをプログラムで管理できます。
role: Developer
exl-id: 049f7a18-7d06-437b-8ce9-25d7090ba782
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 27%

---

# 役割エンドポイント

>[!NOTE]
>
>ユーザートークンが渡される場合、トークンのユーザーには、リクエストされた組織の「組織管理者」の役割が必要です。

役割は、管理者、スペシャリストまたはエンドユーザーが組織内のリソースに対して持つアクセスを定義します。 役割ベースのアクセス制御環境では、ユーザーアクセスプロビジョニングは、共通の責任とニーズによってグループ化されます。役割には特定の権限セットがあり、必要な表示または書き込みアクセスの範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。

属性ベースのアクセス制御 API の `/roles` エンドポイントを使用すると、組織内の役割をプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、属性ベースのアクセス制御 API の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 役割のリストの取得 {#list}

`/roles` エンドポイントに対してGETリクエストを行うことで、組織に属する既存のすべての役割をリストできます。

**API 形式**

```http
GET /roles/
```

**リクエスト**

次のリクエストは、組織に属する役割のリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/roles \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、組織内の役割のリスト（それぞれの役割タイプ、権限セット、サブジェクト属性に関する情報を含む）が返されます。

```json
{
  "roles": [
    {
      "id": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
      "name": "Administrator Role",
      "description": "Role for administrator type of responsibilities and access",
      "roleType": "user-defined",
      "permissionSets": [
        "manage-datasets",
        "manage-schemas"
      ],
      "sandboxes": [
        "prod"
      ],
      "subjectAttributes": {
        "labels": [
          "core/S1"
        ]
      },
      "createdBy": "{CREATED_BY}",
      "createdAt": 1648153201825,
      "modifiedBy": "{MODIFIED_BY}",
      "modifiedAt": 1648153201825,
      "etag": null
    }
  ],
  "_page": {
    "limit": 1,
    "count": 1
  },
  "_links": {
    "next": {
      "href": "https://platform.adobe.io:443/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
      "templated": true
    },
    "page": {
      "href": "https://platform.adobe.io:443/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
      "templated": true
    },
    "subjects": {
      "href": "https://platform.adobe.io:443/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
      "templated": true
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 役割に対応する ID。 この ID は自動生成されます。 |
| `name` | 役割の名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| `sandboxes` | このプロパティには、特定の役割用にプロビジョニングされた組織内のサンドボックスが表示されます。 |
| `subjectAttributes` | サブジェクトと、サブジェクトがアクセス権を持つ Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | クエリされた役割に適用されたデータ使用ラベルを表示します。 |

## 役割の検索 {#lookup}

個々のロールを検索するには、リクエストパスに対応するロールを含むGETリク `roleId` ストを実行します。

**API 形式**

```http
GET /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 検索する役割の ID。 |

**リクエスト**

次のリクエストは、`{ROLE_ID}` の情報を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、クエリされた役割 ID の詳細（役割タイプ、権限セット、サブジェクト属性に関する情報を含む）が返されます。

```json
{
  "id": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
  "name": "Administrator Role",
  "description": "Role for administrator type of responsibilities and access",
  "roleType": "user-defined",
  "permissionSets": [
    "manage-datasets",
    "manage-schemas"
  ],
  "sandboxes": [
    "prod"
  ],
  "subjectAttributes": {
    "labels": [
      "core/S1"
    ]
  },
  "createdBy": "{CREATED_BY}",
  "createdAt": 1648153201825,
  "modifiedBy": "{MODIFIED_BY}",
  "modifiedAt": 1648153201825,
  "etag": null
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 役割に対応する ID。 この ID は自動生成されます。 |
| `name` | 役割の名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| `sandboxes` | このプロパティには、特定の役割用にプロビジョニングされた組織内のサンドボックスが表示されます。 |
| `subjectAttributes` | サブジェクトと、サブジェクトがアクセス権を持つ Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | クエリされた役割に適用されたデータ使用ラベルを表示します。 |

## 役割 ID による件名の検索

また、{ROLE_ID} を指定しながら `/roles` エンドポイントに対してGETリクエストを行うことで、件名を取得できます。

**API 形式**

```http
GET /roles/{ROLE_ID}/subjects
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 検索する件名に関連付けられた役割の ID。 |

**リクエスト**

次のリクエストは、`3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809` に関連付けられた件名を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809/subjects \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、対応するサブジェクト ID とサブジェクトタイプを含む、クエリされた役割 ID に関連付けられたサブジェクトが返されます。

```json
{
  "items": [
      {
          "roleId": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
          "subjectType": "user",
          "subjectId": "03Z07HFQCCUF3TUHAX274206@AdobeID"
      },
      {
          "roleId": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
          "subjectType": "user",
          "subjectId": "PIRJ7WE5T3QT9Z4TCLVH86DE@AdobeID"
      },
      {
          "roleId": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
          "subjectType": "user",
          "subjectId": "WHPWE00MC26SHZ7AKBFG403D@AdobeID"
      },
  ]
  "_page": {
    "limit": 0,
    "count": 0
  },
  "_links": {
      "self": {
          "href": "/roles/{ROLE_ID}/subjects",
          "templated": false,
          "type": null,
          "method": null
      },
      "page": {
          "href": "/roles/{ROLE_ID}/subjects?limit={limit}&start={start}&orderBy={orderBy}&property={property}",
          "templated": true,
          "type": null,
          "method": null
      }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `roleId` | クエリされた件名に関連付けられた役割 ID。 |
| `subjectType` | クエリされた件名のタイプ。 |
| `subjectId` | クエリされたサブジェクトに対応する ID。 |

## 役割の作成 {#create}

新しいロールを作成するには、ロールの名前、説明、ロールタイプの値を指定して、`/roles` エンドポイントにPOSTリクエストを行います。

**API 形式**

```http
POST /roles/
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/access-control/administration/roles \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -d'{
    "name": "Administrator Role",
    "description": "Role for administrator type of responsibilities and access",
    "roleType": "user-defined"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 役割の名前。 役割に関する情報を検索する際に使用できるので、役割の名前はわかりやすい名前にしてください。 |
| `description` | （オプション）役割に関する詳細情報を提供するために含めることができる説明的な値。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |

**応答**

応答が成功すると、新しく作成された役割、対応する役割 ID、役割のタイプ、権限セットおよびサブジェクト属性に関する情報が返されます。

```json
{
  "id": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
  "name": "Administrator Role",
  "description": "Role for administrator type of responsibilities and access",
  "roleType": "user-defined",
  "permissionSets": [
    "manage-datasets",
    "manage-schemas"
  ],
  "sandboxes": [
    "prod"
  ],
  "subjectAttributes": {
    "labels": [
      "core/S1"
    ]
  },
  "createdBy": "{CREATED_BY}",
  "createdAt": 1648153201825,
  "modifiedBy": "{MODIFIED_BY}",
  "modifiedAt": 1648153201825,
  "etag": null
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 役割に対応する ID。 この ID は自動生成されます。 |
| `name` | 役割の名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| `sandboxes` | このプロパティには、特定の役割用にプロビジョニングされた組織内のサンドボックスが表示されます。 |
| `subjectAttributes` | サブジェクトと、サブジェクトがアクセス権を持つ Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | クエリされた役割に適用されたデータ使用ラベルを表示します。 |

## 役割の更新 {#patch}

ロールのプロパティを更新するには、適用したいオペレーションに対応するロール ID と値を指定して、`/roles` エンドポイントにPATCHリクエストを行います。

**API 形式**

```http
PATCH /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 更新する役割の ID。 |

**リクエスト**

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/{ROLE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -d'{
    "operations": [
      {
        "op": "add",
        "path": "/description",
        "value": "Role with permission sets for admin type of access"
      }
    ]
  }'
```

| 運用 | 説明 |
| --- | --- |
| `op` | 役割の更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

応答が成功すると、更新された役割が、更新するように選択したプロパティの新しい値を含めて返されます。

```json
{
  "id": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
  "name": "Administrator Role",
  "description": "Role with permission sets for admin type of access",
  "roleType": "user-defined",
  "permissionSets": [
    "manage-datasets",
    "manage-schemas"
  ],
  "sandboxes": [
    "prod"
  ],
  "subjectAttributes": {
    "labels": [
      "core/S1"
    ]
  },
  "createdBy": "{CREATED_BY}",
  "createdAt": 1648153201825,
  "modifiedBy": "{MODIFIED_BY}",
  "modifiedAt": 1648153201825,
  "etag": null
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 役割に対応する ID。 この ID は自動生成されます。 |
| `name` | 役割の名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| `sandboxes` | このプロパティには、特定の役割用にプロビジョニングされた組織内のサンドボックスが表示されます。 |
| `subjectAttributes` | サブジェクトと、サブジェクトがアクセス権を持つ Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | クエリされた役割に適用されたデータ使用ラベルを表示します。 |

## 役割 ID による役割の更新 {#put}

`/roles` エンドポイントにPUTリクエストを行い、更新するロールに対応するロール ID を指定することで、ロールを更新できます。

**API 形式**

```http
PUT /roles/{ROLE_ID}
```

**リクエスト**

次のリクエストは、役割 ID `3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809` の名前、説明、役割タイプを更新します。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -d'{
    "name": "Administrator role for ACME",
    "description": "New administrator role for ACME",
    "roleType": "user-defined"
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | 更新された役割の名前。 |
| `description` | 役割の更新された説明。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |

**応答**

応答が成功すると、名前、説明、役割タイプに新しい値を含む、更新された役割が返されます。

```json
{
  "id": "3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809",
  "name": "Administrator role for ACME",
  "description": "New administrator role for ACME",
  "roleType": "user-defined",
  "permissionSets": [
    "manage-datasets",
    "manage-schemas"
  ],
  "sandboxes": [
    "prod"
  ],
  "subjectAttributes": {
    "labels": [
      "core/S1"
    ]
  },
  "createdBy": "{CREATED_BY}",
  "createdAt": 1648153201825,
  "modifiedBy": "{MODIFIED_BY}",
  "modifiedAt": 1648153201825,
  "etag": null
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 役割に対応する ID。 この ID は自動生成されます。 |
| `name` | 役割の名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | 役割に指定されたタイプ。 役割タイプの値は、`user-defined` および `system-defined` です。 |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| `sandboxes` | このプロパティには、特定の役割用にプロビジョニングされた組織内のサンドボックスが表示されます。 |
| `subjectAttributes` | サブジェクトと、サブジェクトがアクセス権を持つ Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | クエリされた役割に適用されたデータ使用ラベルを表示します。 |

## 役割 ID で件名を更新

ロールに関連付けられたサブジェクトを更新するには、更新するサブジェクトのロール ID を指定したうえで、`/roles` エンドポイントに対してPATCHリクエストを行います。

**API 形式**

```http
PATCH /roles/{ROLE_ID}/subjects
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 更新するサブジェクトに関連付けられたロールの ID。 |

**リクエスト**

次のリクエストは、`{ROLE_ID}` に関連付けられた件名を更新します。

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/access-control/administration/roles/<ROLE_ID>/subjects' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "op": "add",
        "path": "/user",
        "value": "{USER ID}"
    }
]' 
```

| 運用 | 説明 |
| --- | --- |
| `op` | 役割の更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

応答が成功すると、サブジェクトの新しい値を含め、更新された役割が返されます。

```json
{
  "subjects": [
    [
      {
        "subjectId": "03Z07HFQCCUF3TUHAX274206@AdobeID",
        "subjectType": "user"
      }
    ]
  ],
  "_page": {
    "limit": 1,
    "count": 1
  },
  "_links": {
    "self": {
      "href": "https://platform.adobe.io:443/data/foundation/access-control/administration/roles/{ROLE_ID}/subjects",
      "templated": true
    },
    "page": {
      "href": "https://platform.adobe.io:443/data/foundation/access-control/administration/roles/{ROLE_ID}/subjects?limit={limit}&start={start}&orderBy={orderBy}&property={property}",
      "templated": true
    }
  }
}
```

## 役割の削除 {#delete}

ロールを削除するには、削除するロールの ID を指定したうえで、`/roles` エンドポイントに対してDELETEリクエストを行います。

**API 形式**

```http
DELETE /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 削除する役割の ID。 |

**リクエスト**

次のリクエストは、ID が `{ROLE_ID}` の役割を削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/{ROLE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答** 

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

ロールに対して検索（GET）リクエストを試行することで、削除を確認できます。 役割が管理から削除されたので、HTTP ステータス 404 （見つかりません）が返されます。

## API 資格情報の追加 {#apicredential}

API 資格情報を追加するには、主体のロール ID を指定 `/roles` て、エンドポイントにPATCHリクエストを行います。

**API 形式**

```shell
curl --location --request PATCH 'https://platform.adobe.io/data/foundation/access-control/administration/roles/<ROLE_ID>/subjects' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-api-key: {API_KEY}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'Content-Type: application/json' \
--data-raw '[
    {
        "op": "add",
        "path": "/api-integration",
        "value": "{TECHNICAL ACCOUNT ID}"
    }
]'   
```

| 運用 | 説明 |
| --- | --- |
| `op` | 役割の更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 追加するパラメーターのパス。 |
| `value` | パラメーターを追加する値。 |

**応答** 

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

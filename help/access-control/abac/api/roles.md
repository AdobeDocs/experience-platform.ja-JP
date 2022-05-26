---
keywords: Experience Platform；ホーム；人気の高いトピック；api；属性ベースのアクセス制御；属性ベースのアクセス制御
solution: Experience Platform
title: 役割 API エンドポイント
description: 属性ベースのアクセス制御 API の/roles エンドポイントを使用すると、Adobe Experience Platformの役割をプログラムで管理できます。
hide: true
hidefromtoc: true
exl-id: 049f7a18-7d06-437b-8ce9-25d7090ba782
source-git-commit: 19f1e8df8cd8b55ed6b03f80e42810aefd211474
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 10%

---

# 役割エンドポイント

>[!IMPORTANT]
>
>現在、米国ベースの医療関連のお客様向けに、属性ベースのアクセス制御が限定的なリリースで提供されています。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

役割は、管理者、スペシャリスト、またはエンドユーザーが組織のリソースに対して持つアクセス権を定義します。 役割ベースのアクセス制御環境では、ユーザーアクセスプロビジョニングは、共通の責務とニーズを通じてグループ化されます。 ロールには、特定の権限のセットがあり、組織のメンバーは、必要な表示または書き込みアクセスの範囲に応じて、1 つ以上のロールに割り当てることができます。

この `/roles` 属性ベースのアクセス制御 API のエンドポイントを使用すると、組織内の役割をプログラムで管理できます。

## はじめに

このガイドで使用される API エンドポイントは、属性ベースのアクセス制御 API の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## 役割のリストの取得 {#list}

に対してGETリクエストを実行することで、組織に属する既存の役割をすべてリストできます `/roles` endpoint.

**API 形式**

```http
GET /roles/
```

**リクエスト**

次のリクエストでは、組織に属するロールのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/roles \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、組織内の役割のリスト（それぞれの役割のタイプ、権限セット、件名属性に関する情報を含む）を返します。

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
| `name` | ロールの名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、権限セットを役割に割り当てることができます。 これにより、権限のグループを含む事前定義済みの役割からカスタム役割を作成できます。 |
| `sandboxes` | このプロパティは、特定の役割用にプロビジョニングされている組織内のサンドボックスを表示します。 |
| `subjectAttributes` | 主体と、主体がアクセスできる Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | 問い合わせられた役割に適用されたデータ使用ラベルを表示します。 |

## 役割の検索 {#lookup}

個々のロールを検索するには、対応する `roleId` リクエストパス内で使用します。

**API 形式**

```http
GET /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 検索する役割の ID。 |

**リクエスト**

次のリクエストでは、 `{ROLE_ID}`.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、問い合わせられた役割 ID の詳細（役割のタイプ、権限セット、件名属性に関する情報を含む）を返します。

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
| `name` | ロールの名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、権限セットを役割に割り当てることができます。 これにより、権限のグループを含む事前定義済みの役割からカスタム役割を作成できます。 |
| `sandboxes` | このプロパティは、特定の役割用にプロビジョニングされている組織内のサンドボックスを表示します。 |
| `subjectAttributes` | 主体と、主体がアクセスできる Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | 問い合わせられた役割に適用されたデータ使用ラベルを表示します。 |

## 役割 ID で件名を検索する

また、 `/roles` {ROLE_ID} を提供する際にエンドポイントが発生しました。

**API 形式**

```http
GET /roles/{ROLE_ID}/subjects
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 検索する主体に関連付けられている役割の ID。 |

**リクエスト**

次のリクエストでは、 `3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809`.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809/subjects \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

正常な応答は、問い合わせられた役割 ID に関連付けられた件名を返します。これには、対応する件名 ID と件名タイプが含まれます。

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
| `roleId` | 問い合わせられた件名に関連付けられた役割 ID。 |
| `subjectType` | 問い合わせ対象のタイプ。 |
| `subjectId` | 問い合わせ対象に対応する ID。 |

## ロールの作成 {#create}

新しいロールを作成するには、 `/roles` エンドポイントを使用して、役割の名前、説明、役割タイプの値を指定します。

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
| `name` | ロールの名前。 役割の名前がわかりやすいことを確認します。これを使用して、役割に関する情報を検索できます。 |
| `description` | （オプション）役割に関する詳細を提供するために含めることができる説明的な値です。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |

**応答**

正常な応答は、新しく作成された役割と、対応する役割 ID、役割の種類、権限セット、件名属性に関する情報を返します。

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
| `name` | ロールの名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、権限セットを役割に割り当てることができます。 これにより、権限のグループを含む事前定義済みの役割からカスタム役割を作成できます。 |
| `sandboxes` | このプロパティは、特定の役割用にプロビジョニングされている組織内のサンドボックスを表示します。 |
| `subjectAttributes` | 主体と、主体がアクセスできる Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | 問い合わせられた役割に適用されたデータ使用ラベルを表示します。 |

## ロールの更新 {#patch}

ロールのプロパティを更新するには、 `/roles` エンドポイントを使用して、適用する操作に対応する役割 ID と値を指定できます。

**API 形式**

```http
PATCH /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 更新するロールの ID。 |

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
| `op` | ロールの更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、更新された役割を返します。更新対象として選択したプロパティの新しい値も含まれます。

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
| `name` | ロールの名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、権限セットを役割に割り当てることができます。 これにより、権限のグループを含む事前定義済みの役割からカスタム役割を作成できます。 |
| `sandboxes` | このプロパティは、特定の役割用にプロビジョニングされている組織内のサンドボックスを表示します。 |
| `subjectAttributes` | 主体と、主体がアクセスできる Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | 問い合わせられた役割に適用されたデータ使用ラベルを表示します。 |

## ロール ID によるロールの更新 {#put}

ロールを更新するには、 `/roles` エンドポイントを選択し、更新するロールに対応するロール ID を指定します。

**API 形式**

```http
PUT /roles/{ROLE_ID}
```

**リクエスト**

次のリクエストでは、ロール ID の名前、説明およびロールタイプを更新します。 `3dfa045d-de58-4dfd-8ea9-e4e2c1b6d809`.

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
| `name` | ロールの更新された名前。 |
| `description` | ロールの更新された説明。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |

**応答**

正常な場合は、更新されたロールが返されます。このロールの名前、説明、ロールタイプの新しい値も含まれます。

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
| `name` | ロールの名前。 |
| `description` | description プロパティは、役割に関する追加情報を提供します。 |
| `roleType` | ロールの指定されたタイプ。 役割タイプに指定できる値は次のとおりです。 `user-defined` および `system-defined`. |
| `permissionSets` | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、権限セットを役割に割り当てることができます。 これにより、権限のグループを含む事前定義済みの役割からカスタム役割を作成できます。 |
| `sandboxes` | このプロパティは、特定の役割用にプロビジョニングされている組織内のサンドボックスを表示します。 |
| `subjectAttributes` | 主体と、主体がアクセスできる Platform リソースとの相関関係を示す属性。 |
| `subjectAttributes.labels` | 問い合わせられた役割に適用されたデータ使用ラベルを表示します。 |

## ロール ID で件名を更新

ロールに関連付けられたサブジェクトを更新するには、にPATCHリクエストを実行します `/roles` エンドポイントを使用して、更新するサブジェクトのロール ID を指定します。

**API 形式**

```http
PATCH /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 更新するサブジェクトに関連付けられているロールの ID。 |

**リクエスト**

次のリクエストでは、 `{ROLE_ID}`.

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
        "path": "/subjects",
        "value": "New subjects"
      }
    ]
  }'
```

| 運用 | 説明 |
| --- | --- |
| `op` | ロールの更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、問い合わせられた役割 ID に関連付けられた更新された件名を返します。

```json
{
  "subjects": [
    {
      "subjectId": "string",
      "subjectType": "user"
    }
  ],
  "_page": {
    "limit": 0,
    "count": 0
  },
  "_links": {
    "next": {
      "href": "string",
      "templated": true
    },
    "page": {
      "href": "string",
      "templated": true
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `subjectId` | 件名の ID。 |
| `subjectType` | 件名のタイプ。 |

## ロールの削除 {#delete}

ロールを削除するには、 `/roles` エンドポイントを使用して、削除するロールの ID を指定します。

**API 形式**

```http
DELETE /roles/{ROLE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {ROLE_ID} | 削除するロールの ID。 |

**リクエスト**

次のリクエストでは、ID がの役割が削除されます。 `{ROLE_ID}`.

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/access-control/administration/roles/{ROLE_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答** 

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

ロールに対してルックアップ (GET) リクエストを試行して、削除を確認できます。 役割が管理から削除されたので、HTTP ステータス 404（見つかりません）が表示されます。

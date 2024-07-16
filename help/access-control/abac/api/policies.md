---
keywords: Experience Platform；ホーム；人気のトピック；api；属性ベースのアクセス制御；属性ベースのアクセス制御
solution: Experience Platform
title: アクセス制御ポリシー API エンドポイント
description: 属性ベースのアクセス制御 API の/policies エンドポイントを使用すると、Adobe Experience Platformでポリシーをプログラムで管理できます。
role: Developer
exl-id: 07690f43-fdd9-4254-9324-84e6bd226743
source-git-commit: c16ce1020670065ecc5415bc3e9ca428adbbd50c
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 10%

---

# アクセス制御ポリシーエンドポイント

>[!NOTE]
>
>ユーザートークンが渡される場合、トークンのユーザーには、リクエストされた組織の「組織管理者」の役割が必要です。

アクセス制御ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを規定するステートメントです。 これらのポリシーは、ローカルまたはグローバルのいずれかであり、他のポリシーを上書きできます。 属性ベースのアクセス制御 API の `/policies` エンドポイントを使用すると、ポリシーおよびそれぞれのサブジェクト条件を管理するルールに関する情報など、ポリシーをプログラムに基づいて管理できます。

>[!IMPORTANT]
>
>このエンドポイントを、データ使用ポリシーの管理に使用される [Policy Service API](../../../data-governance/api/policies.md) の `/policies` エンドポイントと混同しないでください。

## はじめに

このガイドで使用する API エンドポイントは、属性ベースのアクセス制御 API の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## ポリシーのリストの取得 {#list}

`/policies` エンドポイントに対してGETリクエストを実行し、組織内の既存のポリシーをすべてリストします。

**API 形式**

```http
GET /policies
```

**リクエスト**

次のリクエストは、既存のポリシーのリストを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/policies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

応答が成功すると、既存のポリシーのリストが返されます。

```json
{
  {
      "id": "7019068e-a3a0-48ce-b56b-008109470592",
      "imsOrgId": "{IMS_ORG}",
      "createdBy": "{CREATED_BY}",
      "createdAt": 1652892767559,
      "modifiedBy": "{MODIFIED_BY}",
      "modifiedAt": 1652895736367,
      "name": "schema-field",
      "description": "schema-field",
      "status": "inactive",
      "subjectCondition": null,
      "rules": [
          {
              "effect": "Deny",
              "resource": "/orgs/{IMS_ORG}/sandboxes/xql/schemas/*/schema-fields/*",
              "condition": "{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}",
              "actions": [
                  "com.adobe.action.read",
                  "com.adobe.action.write",
                  "com.adobe.action.view"
              ]
          },
          {
              "effect": "Permit",
              "resource": "/orgs/{IMS_ORG}/sandboxes/*/schemas/*/schema-fields/*",
              "condition": "{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}",
              "actions": [
                  "com.adobe.action.delete"
              ]
          },
          {
              "effect": "Deny",
              "resource": "/orgs/{IMS_ORG}/sandboxes/delete-sandbox-adfengine-test-8/segments/*",
              "condition": "{\"!\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"custom/\",{\"var\":\"resource.labels\"}]}]}",
              "actions": [
                  "com.adobe.action.write"
              ]
          }
      ],
      "_etag": "\"0300593f-0000-0200-0000-62852ff80000\""
  },
  {
      "id": "13138ef6-c007-495d-837f-0a248867e219",
      "imsOrgId": "{IMS_ORG}",
      "createdBy": "{CREATED_BY}",
      "createdAt": 1652859368555,
      "modifiedBy": "{MODIFIED_BY}",
      "modifiedAt": 1652890780206,
      "name": "Documentation-Copy",
      "description": "xyz",
      "status": "active",
      "subjectCondition": null,
      "rules": [
          {
              "effect": "Permit",
              "resource": "orgs/{IMS_ORG}/sandboxes/ro-sand/schemas/*/schema-fields/*",
              "condition": "{\"!\":[{\"or\":[{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"!\":[{\"and\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}]}]}]}]}",
              "actions": [
                  "com.adobe.action.read"
              ]
          },
          {
              "effect": "Deny",
              "resource": "orgs/{IMS_ORG}/sandboxes/*/segments/*",
              "condition": "{\"!\":[{\"or\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"custom/\",{\"var\":\"resource.labels\"}]}]}]}",
              "actions": [
                  "com.adobe.action.read"
              ]
          }
      ],
      "_etag": "\"0300d43c-0000-0200-0000-62851c9c0000\""
  },
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | ポリシーに対応する ID。 この識別子は自動生成され、ポリシーの参照、更新、削除に使用できます。 |
| `imsOrgId` | クエリされたポリシーにアクセスできる組織。 |
| `createdBy` | ポリシーを作成したユーザーの ID。 |
| `createdAt` | ポリシーが作成された時間。 `createdAt` プロパティは、unix エポックタイムスタンプで表示されます。 |
| `modifiedBy` | ポリシーを最後に更新したユーザーの ID。 |
| `modifiedAt` | ポリシーが最後に更新された時刻。 `modifiedAt` プロパティは、unix エポックタイムスタンプで表示されます。 |
| `name` | ポリシーの名前。 |
| `description` | （任意）特定のポリシーの詳細を提供するために追加できるプロパティ。 |
| `status` | ポリシーの現在のステータスです。このプロパティは、ポリシーが現在 `active` か `inactive` かを定義します。 |
| `subjectCondition` | サブジェクトに適用される条件。 サブジェクトとは、アクションを実行するためにリソースへのアクセスを要求する、特定の属性を持つユーザーのことです。 この場合、`subjectCondition` れは件名属性に適用されるクエリのような条件です。 |
| `rules` | ポリシーを定義する一連のルール。 ルールでは、サブジェクトがリソースに対してアクションを正常に実行するために、どの属性の組み合わせが許可されるかを定義します。 |
| `rules.effect` | `action`、`condition`、`resource` の値を検討した後に発生する効果。 使用可能な値は `permit`、`deny`、`indeterminate` です。 |
| `rules.resource` | サブジェクトがアクセスできるまたはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API があります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマには、そのスキーマに対するアクションが許可されるか許可されないかに影響する、特定のラベルを適用できます。 |
| `rules.action` | クエリされたリソースに対してサブジェクトが実行できるアクション。 使用可能な値：`read`、`create`、`edit`、`delete` |

## ID によるポリシーの詳細の検索 {#lookup}

リクエストパスでポリシー ID を指定して `/policies` エンドポイントにGETリクエストを送信し、その個々のポリシーに関する情報を取得します。

**API 形式**

```http
GET /policies/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {POLICY_ID} | 取得するポリシーの ID。 |

**リクエスト**

次のリクエストは、個々のポリシーに関する情報を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/access-control/administration/policies/13138ef6-c007-495d-837f-0a248867e219 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

リクエストが成功すると、クエリされたポリシー ID に関する情報が返されます。

```json
{
  "policies": [
    {
      "id": "7019068e-a3a0-48ce-b56b-008109470592",
      "imsOrgId": "5555467B5D8013E50A494220@AdobeOrg",
      "createdBy": "example@AdobeID",
      "createdAt": 1652892767559,
      "modifiedBy": "example@AdobeID",
      "modifiedAt": 1652895736367,
      "name": "schema-field",
      "description": "schema-field",
      "status": "inactive",
      "subjectCondition": null,
      "rules": [
        {
          "effect": "Deny",
          "resource": "/orgs/5555467B5D8013E50A494220@AdobeOrg/sandboxes/xql/schemas/*/schema-fields/*",
          "condition": "{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}",
          "actions": [
            "com.adobe.action.read",
            "com.adobe.action.write",
            "com.adobe.action.view"
          ]
        },
        {
          "effect": "Permit",
          "resource": "/orgs/5555467B5D8013E50A494220@AdobeOrg/sandboxes/*/schemas/*/schema-fields/*",
          "condition": "{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}",
          "actions": [
            "com.adobe.action.delete"
          ]
        },
        {
          "effect": "Deny",
          "resource": "/orgs/5555467B5D8013E50A494220@AdobeOrg/sandboxes/delete-sandbox-adfengine-test-8/segments/*",
          "condition": "{\"!\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"custom/\",{\"var\":\"resource.labels\"}]}]}",
          "actions": [
            "com.adobe.action.write"
          ]
        }
      ],
      "etag": "\"0300593f-0000-0200-0000-62852ff80000\""
    }
  ]
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | ポリシーに対応する ID。 この識別子は自動生成され、ポリシーの参照、更新、削除に使用できます。 |
| `imsOrgId` | クエリされたポリシーにアクセスできる組織。 |
| `createdBy` | ポリシーを作成したユーザーの ID。 |
| `createdAt` | ポリシーが作成された時間。 `createdAt` プロパティは、unix エポックタイムスタンプで表示されます。 |
| `modifiedBy` | ポリシーを最後に更新したユーザーの ID。 |
| `modifiedAt` | ポリシーが最後に更新された時刻。 `modifiedAt` プロパティは、unix エポックタイムスタンプで表示されます。 |
| `name` | ポリシーの名前。 |
| `description` | （任意）特定のポリシーの詳細を提供するために追加できるプロパティ。 |
| `status` | ポリシーの現在のステータスです。このプロパティは、ポリシーが現在 `active` か `inactive` かを定義します。 |
| `subjectCondition` | サブジェクトに適用される条件。 サブジェクトとは、アクションを実行するためにリソースへのアクセスを要求する、特定の属性を持つユーザーのことです。 この場合、`subjectCondition` れは件名属性に適用されるクエリのような条件です。 |
| `rules` | ポリシーを定義する一連のルール。 ルールでは、サブジェクトがリソースに対してアクションを正常に実行するために、どの属性の組み合わせが許可されるかを定義します。 |
| `rules.effect` | `action`、`condition`、`resource` の値を検討した後に発生する効果。 使用可能な値は `permit`、`deny`、`indeterminate` です。 |
| `rules.resource` | サブジェクトがアクセスできるまたはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API があります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマには、そのスキーマに対するアクションが許可されるか許可されないかに影響する、特定のラベルを適用できます。 |
| `rules.action` | クエリされたリソースに対してサブジェクトが実行できるアクション。 使用可能な値：`read`、`create`、`edit`、`delete` |


## ポリシーを作成する {#create}

新しいポリシーを作成するには、`/policies` エンドポイントに対してPOSTリクエストを実行します。

**API 形式**

```http
POST /policies
```

**リクエスト**

次のリクエストは、`acme-integration-policy` という名前の新しいポリシーを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/access-control/administration/policies \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -d'{
      "name": "acme-integration-policy",
      "description": "Policy for ACME",
      "imsOrgId": "{IMS_ORG}",
      "rules": [
        {
          "effect": "Permit",
          "resource": "/orgs/{IMS_ORG}/sandboxes/*",
          "condition": "{\"or\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"!\":[{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}]}]}",
          "actions": [
            "com.adobe.action.read"
          ]
        }
      ]
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | ポリシーの名前。 |
| `description` | （任意）特定のポリシーの詳細を提供するために追加できるプロパティ。 |
| `imsOrgId` | ポリシーを含む組織。 |
| `rules` | ポリシーを定義する一連のルール。 ルールでは、サブジェクトがリソースに対してアクションを正常に実行するために、どの属性の組み合わせが許可されるかを定義します。 |
| `rules.effect` | `action`、`condition`、`resource` の値を検討した後に発生する効果。 使用可能な値は `permit`、`deny`、`indeterminate` です。 |
| `rules.resource` | サブジェクトがアクセスできるまたはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API があります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマには、そのスキーマに対するアクションが許可されるか許可されないかに影響する、特定のラベルを適用できます。 |
| `rules.action` | クエリされたリソースに対してサブジェクトが実行できるアクション。 使用可能な値：`read`、`create`、`edit`、`delete` |

**応答**

リクエストが成功すると、一意のポリシー ID と関連するルールを含む、新しく作成されたポリシーが返されます。

```json
{
    "id": "c3863937-5d40-448d-a7be-416e538f955e",
    "imsOrgId": "{IMS_ORG}",
    "createdBy": "{CREATED_BY}",
    "createdAt": 1652988384458,
    "modifiedBy": "{MODIFIED_BY}",
    "modifiedAt": 1652988384458,
    "name": "acme-integration-policy",
    "description": "Policy for ACME",
    "status": "active",
    "subjectCondition": null,
    "rules": [
        {
            "effect": "Permit",
            "resource": "/orgs/{IMS_ORG}/sandboxes/*",
            "condition": "{\"or\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"!\":[{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}]}]}",
            "actions": [
                "com.adobe.action.read"
            ]
        }
    ],
    "_etag": null
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | ポリシーに対応する ID。 この識別子は自動生成され、ポリシーの参照、更新、削除に使用できます。 |
| `name` | ポリシーの名前。 |
| `rules` | ポリシーを定義する一連のルール。 ルールでは、サブジェクトがリソースに対してアクションを正常に実行するために、どの属性の組み合わせが許可されるかを定義します。 |
| `rules.effect` | `action`、`condition`、`resource` の値を検討した後に発生する効果。 使用可能な値は `permit`、`deny`、`indeterminate` です。 |
| `rules.resource` | サブジェクトがアクセスできるまたはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API があります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマには、そのスキーマに対するアクションが許可されるか許可されないかに影響する、特定のラベルを適用できます。 |
| `rules.action` | クエリされたリソースに対してサブジェクトが実行できるアクション。 使用可能な値：`read`、`create`、`edit`、`delete` |


## ポリシー ID でポリシーを更新 {#put}

個々のポリシーの規則を更新するには、リクエストパスで更新するポリシーの ID を指定したうえで、`/policies` エンドポイントに対してPUTリクエストを行います。

**API 形式**

```http
PUT /policies/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {POLICY_ID} | 更新するポリシーの ID。 |

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/access-control/administration/policies/8cf487d7-3642-4243-a8ea-213d72f694b9 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -d'{
      "id": "8cf487d7-3642-4243-a8ea-213d72f694b9",
      "imsOrgId": "{IMS_ORG}",
      "name": "test-2",
      "rules": [
      {
        "effect": "Deny",
        "resource": "/orgs/{IMS_ORG}/sandboxes/*",
        "condition": "{\"or\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"!\":[{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}]}]}",
        "actions": [
          "com.adobe.action.read"
        ]
      }
    ]
  }'
```

**応答**

応答が成功すると、更新されたポリシーが返されます。

```json
{
    "id": "8cf487d7-3642-4243-a8ea-213d72f694b9",
    "imsOrgId": "{IMS_ORG}",
    "createdBy": "{CREATED_BY}",
    "createdAt": 1652988866647,
    "modifiedBy": "{MODIFIED_BY}",
    "modifiedAt": 1652989297287,
    "name": "test-2",
    "description": null,
    "status": "active",
    "subjectCondition": null,
    "rules": [
        {
            "effect": "Deny",
            "resource": "/orgs/{IMS_ORG}/sandboxes/*",
            "condition": "{\"or\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"!\":[{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}]}]}",
            "actions": [
                "com.adobe.action.read"
            ]
        }
    ],
    "_etag": null
}
```

## ポリシープロパティの更新 {#patch}

個々のポリシーのプロパティを更新するには、リクエストパスで更新するポリシーの ID を指定したうえで、`/policies` エンドポイントに対してPATCHリクエストを行います。

**API 形式**

```http
PATCH /policies/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {POLICY_ID} | 更新するポリシーの ID。 |

**リクエスト**

次のリクエストでは、ポリシー ID `c3863937-5d40-448d-a7be-416e538f955e` の `/description` の値が置き換えられます。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/access-control/administration/policies/c3863937-5d40-448d-a7be-416e538f955e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
  -d'{
    "operations": [
      {
        "op": "replace",
        "path": "/description",
        "value": "Pre-set policy to be applied for ACME"
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

応答が成功すると、クエリされたポリシー ID と更新された説明が返されます。

```json
{
    "id": "c3863937-5d40-448d-a7be-416e538f955e",
    "imsOrgId": "{IMS_ORG}",
    "createdBy": "acp_accessControlService",
    "createdAt": 1652988384458,
    "modifiedBy": "acp_accessControlService",
    "modifiedAt": 1652988384458,
    "name": "acme-integration-policy",
    "description": "Pre-set policy to be applied for ACME",
    "status": "active",
    "subjectCondition": null,
    "rules": [
        {
            "effect": "Permit",
            "resource": "/orgs/{IMS_ORG}/sandboxes/*",
            "condition": "{\"or\":[{\"adobe.match_any_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]},{\"!\":[{\"adobe.match_all_labels_by_prefix\":[{\"var\":\"subject.roles.labels\"},\"core/\",{\"var\":\"resource.labels\"}]}]}]}",
            "actions": [
                "com.adobe.action.read"
            ]
        }
    ],
    "_etag": null
}
```

## ポリシーの削除 {#delete}

ポリシーを削除するには、削除するポリシーの ID を指定したうえで、`/policies` エンドポイントに対してDELETEリクエストを行います。

**API 形式**

```http
DELETE /policies/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {POLICY_ID} | 削除するポリシーの ID。 |

**リクエスト**

次のリクエストは、ID が `c3863937-5d40-448d-a7be-416e538f955e` のポリシーを削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/access-control/administration/policies/c3863937-5d40-448d-a7be-416e538f955e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答** 

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

削除を確認するには、ポリシーに対してルックアップ（GET）リクエストを試みます。 ポリシーが管理から削除されたので、HTTP ステータス 404 （見つかりません）が返されます。

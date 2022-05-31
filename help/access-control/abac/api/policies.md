---
keywords: Experience Platform；ホーム；人気の高いトピック；api；属性ベースのアクセス制御；属性ベースのアクセス制御
solution: Experience Platform
title: ポリシー API エンドポイント
description: 属性ベースのアクセス制御 API の/policies エンドポイントを使用すると、Adobe Experience Platformでポリシーをプログラムで管理できます。
exl-id: 07690f43-fdd9-4254-9324-84e6bd226743
source-git-commit: 567bfe089fd96cb08cb8ea7c90d065c804be9413
workflow-type: tm+mt
source-wordcount: '1413'
ht-degree: 11%

---

# ポリシーエンドポイント

>[!IMPORTANT]
>
>現在、米国ベースの医療関連のお客様向けに、属性ベースのアクセス制御が限定的なリリースで提供されています。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

ポリシーとは、属性を統合して、許容される許容されるアクションと許容されないアクションを確立するステートメントです。 ポリシーは、ローカルまたはグローバルに設定でき、他のポリシーを上書きできます。 この `/policies` 属性ベースのアクセス制御 API のエンドポイントを使用すると、ポリシーを制御するルールに関する情報や、それぞれの件名条件を含むポリシーをプログラムで管理できます。

## はじめに

このガイドで使用される API エンドポイントは、属性ベースのアクセス制御 API の一部です。 先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## ポリシーのリストの取得 {#list}

に対するGETリクエスト `/policies` endpoint ：組織内の既存のポリシーをすべてリストします。

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

正常な応答は、既存のポリシーのリストを返します。

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
| `imsOrgId` | 照会されたポリシーにアクセスできる組織。 |
| `createdBy` | ポリシーを作成したユーザーの ID。 |
| `createdAt` | ポリシーが作成された時刻。 この `createdAt` プロパティは unix エポックタイムスタンプで表示されます。 |
| `modifiedBy` | ポリシーを最後に更新したユーザーの ID。 |
| `modifiedAt` | ポリシーが最後に更新された時刻。 この `modifiedAt` プロパティは unix エポックタイムスタンプで表示されます。 |
| `name` | ポリシーの名前。 |
| `description` | （オプション）特定のポリシーに関する詳細情報を提供するために追加できるプロパティです。 |
| `status` | ポリシーの現在のステータスです。このプロパティは、ポリシーが現在 `active` または `inactive`. |
| `subjectCondition` | 件名に適用される条件。 件名とは、特定の属性を持ち、アクションを実行するためにリソースへのアクセスを要求するユーザーです。 この場合、 `subjectCondition` は、件名の属性に適用されるクエリに似た条件です。 |
| `rules` | ポリシーを定義する一連のルール。 ルールは、主体がリソースに対するアクションを正しく実行するために許可する属性の組み合わせを定義します。 |
| `rules.effect` | の値を考慮した後に生じる効果 `action`, `condition` および `resource`. 次の値を指定できます。 `permit`, `deny`または `indeterminate`. |
| `rules.resource` | 主体がアクセスできる、またはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API などがあります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマに対するアクションが許容されるか、許容されないかに影響する特定のラベルをスキーマに適用できます。 |
| `rules.action` | 問い合わせられたリソースに対して主体が実行できるアクション。 次の値を指定できます。 `read`, `create`, `edit`、および `delete`. |

## ポリシーの詳細を ID で検索します {#lookup}

に対するGETリクエスト `/policies` エンドポイントを使用して、個々のポリシーに関する情報を取得するためのリクエストパスにポリシー ID を指定する際に使用されます。

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
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | ポリシーに対応する ID。 この識別子は自動生成され、ポリシーの参照、更新、削除に使用できます。 |
| `imsOrgId` | 照会されたポリシーにアクセスできる組織。 |
| `createdBy` | ポリシーを作成したユーザーの ID。 |
| `createdAt` | ポリシーが作成された時刻。 この `createdAt` プロパティは unix エポックタイムスタンプで表示されます。 |
| `modifiedBy` | ポリシーを最後に更新したユーザーの ID。 |
| `modifiedAt` | ポリシーが最後に更新された時刻。 この `modifiedAt` プロパティは unix エポックタイムスタンプで表示されます。 |
| `name` | ポリシーの名前。 |
| `description` | （オプション）特定のポリシーに関する詳細情報を提供するために追加できるプロパティです。 |
| `status` | ポリシーの現在のステータスです。このプロパティは、ポリシーが現在 `active` または `inactive`. |
| `subjectCondition` | 件名に適用される条件。 件名とは、特定の属性を持ち、アクションを実行するためにリソースへのアクセスを要求するユーザーです。 この場合、 `subjectCondition` は、件名の属性に適用されるクエリに似た条件です。 |
| `rules` | ポリシーを定義する一連のルール。 ルールは、主体がリソースに対するアクションを正しく実行するために許可する属性の組み合わせを定義します。 |
| `rules.effect` | の値を考慮した後に生じる効果 `action`, `condition` および `resource`. 次の値を指定できます。 `permit`, `deny`または `indeterminate`. |
| `rules.resource` | 主体がアクセスできる、またはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API などがあります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマに対するアクションが許容されるか、許容されないかに影響する特定のラベルをスキーマに適用できます。 |
| `rules.action` | 問い合わせられたリソースに対して主体が実行できるアクション。 次の値を指定できます。 `read`, `create`, `edit`、および `delete`. |


## ポリシーを作成する {#create}

新しいポリシーを作成するには、 `/policies` endpoint.

**API 形式**

```http
POST /policies
```

**リクエスト**

次のリクエストでは、という名前の新しいポリシーが作成されます。 `acme-integration-policy`.

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
            "read"
          ]
        }
      ]
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | ポリシーの名前。 |
| `description` | （オプション）特定のポリシーに関する詳細情報を提供するために追加できるプロパティです。 |
| `imsOrgId` | ポリシーを含む組織。 |
| `rules` | ポリシーを定義する一連のルール。 ルールは、主体がリソースに対するアクションを正しく実行するために許可する属性の組み合わせを定義します。 |
| `rules.effect` | の値を考慮した後に生じる効果 `action`, `condition` および `resource`. 次の値を指定できます。 `permit`, `deny`または `indeterminate`. |
| `rules.resource` | 主体がアクセスできる、またはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API などがあります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマに対するアクションが許容されるか、許容されないかに影響する特定のラベルをスキーマに適用できます。 |
| `rules.action` | 問い合わせられたリソースに対して主体が実行できるアクション。 次の値を指定できます。 `read`, `create`, `edit`、および `delete`. |

**応答**

リクエストが成功すると、新しく作成されたポリシーが返されます。この中には、一意のポリシー ID と関連するルールが含まれます。

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
                "read"
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
| `rules` | ポリシーを定義する一連のルール。 ルールは、主体がリソースに対するアクションを正しく実行するために許可する属性の組み合わせを定義します。 |
| `rules.effect` | の値を考慮した後に生じる効果 `action`, `condition` および `resource`. 次の値を指定できます。 `permit`, `deny`または `indeterminate`. |
| `rules.resource` | 主体がアクセスできる、またはアクセスできないアセットまたはオブジェクト。  リソースには、ファイル、アプリケーション、サーバー、API などがあります。 |
| `rules.condition` | リソースに適用される条件。 例えば、リソースがスキーマの場合、スキーマに対するアクションが許容されるか、許容されないかに影響する特定のラベルをスキーマに適用できます。 |
| `rules.action` | 問い合わせられたリソースに対して主体が実行できるアクション。 次の値を指定できます。 `read`, `create`, `edit`、および `delete`. |


## ポリシー ID によるポリシーの更新 {#put}

個々のポリシーのルールを更新するには、 `/policies` エンドポイントを使用して、リクエストパスで更新するポリシーの ID を指定する際に使用するエンドポイントを返します。

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
          "read"
        ]
      }
    ]
  }'
```

**応答**

正常な応答は、更新されたポリシーを返します。

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
                "read"
            ]
        }
    ],
    "_etag": null
}
```

## ポリシーのプロパティを更新 {#patch}

個々のポリシーのプロパティを更新するには、 `/policies` エンドポイントを使用して、リクエストパスで更新するポリシーの ID を指定する際に使用するエンドポイントを返します。

**API 形式**

```http
PATCH /policies/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {POLICY_ID} | 更新するポリシーの ID。 |

**リクエスト**

次のリクエストでは、 `/description` ポリシー ID 内 `c3863937-5d40-448d-a7be-416e538f955e`.

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
| `op` | ロールの更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、クエリされたポリシー ID と、更新された説明を返します。

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
                "read"
            ]
        }
    ],
    "_etag": null
}
```

## ポリシーを削除する {#delete}

ポリシーを削除するには、 `/policies` エンドポイントを使用して、削除するポリシーの ID を指定します。

**API 形式**

```http
DELETE /policies/{POLICY_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {POLICY_ID} | 削除するポリシーの ID。 |

**リクエスト**

次のリクエストでは、ID がのポリシーを削除します。 `c3863937-5d40-448d-a7be-416e538f955e`.

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/access-control/administration/policies/c3863937-5d40-448d-a7be-416e538f955e \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答** 

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

ポリシーに対して検索 (GET) リクエストを試行すると、削除を確認できます。 ポリシーが管理から削除されているので、HTTP ステータス 404(Not Found) が表示されます。

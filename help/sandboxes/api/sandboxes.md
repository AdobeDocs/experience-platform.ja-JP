---
keywords: Experience Platform；ホーム；人気のトピック；サンドボックス開発者ガイド
solution: Experience Platform
title: サンドボックス管理 API エンドポイント
description: Sandbox API の/sandboxes エンドポイントを使用すると、Adobe Experience Platformのサンドボックスをプログラムで管理できます。
role: Developer
exl-id: 0ff653b4-3e31-4ea5-a22e-07e18795f73e
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1477'
ht-degree: 49%

---

# サンドボックス管理エンドポイント

Adobe Experience Platformのサンドボックスは、実稼動環境に影響を与えることなく機能のテスト、実験の実行、カスタム設定を可能にする、独立した開発環境を提供します。 [!DNL Sandbox] API の `/sandboxes` エンドポイントを使用すると、Experience Platformのサンドボックスをプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## サンドボックスのリストの取得 {#list}

`/sandboxes` エンドポイントに対してGET リクエストを実行することで、組織に属するすべてのサンドボックス（アクティブかどうかにかかわらず）をリストできます。

**API 形式**

```http
GET /sandboxes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{QUERY_PARAMS}` | 結果をフィルタリングするオプションのクエリパラメーター。 詳しくは、[ クエリパラメーター ](./appendix.md#query) の節を参照してください。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes?&limit=4&offset=1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

成功応答は、組織に属するサンドボックスのリスト（`name`、`title`、`state`、`type` などの詳細を含むを返します。

```json
{
    "sandboxes": [
        {
            "name": "prod",
            "title": "Production",
            "state": "active",
            "type": "production",
            "region": "VA7",
            "isDefault": true,
            "eTag": 2,
            "createdDate": "2019-09-04 04:57:24",
            "lastModifiedDate": "2019-09-04 04:57:24",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        },
        {
            "name": "dev",
            "title": "Development",
            "state": "active",
            "type": "development",
            "region": "VA7",
            "isDefault": false,
            "eTag": 1,
            "createdDate": "2019-09-03 22:27:48",
            "lastModifiedDate": "2019-09-03 22:27:48",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        },
        {
            "name": "stage",
            "title": "Staging",
            "state": "active",
            "type": "development",
            "region": "VA7",
            "isDefault": false,
            "eTag": 1,
            "createdDate": "2019-09-03 22:27:48",
            "lastModifiedDate": "2019-09-03 22:27:48",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        },
        {
            "name": "dev-2",
            "title": "Development 2",
            "state": "creating",
            "type": "development",
            "region": "VA7",
            "isDefault": false,
            "eTag": 1,
            "createdDate": "2019-09-07 10:16:02",
            "lastModifiedDate": "2019-09-07 10:16:02",
            "createdBy": "{USER_ID}",
            "modifiedBy": "{USER_ID}"
        }
    ],
    "_page": {
        "limit": 4,
        "count": 4
    },
    "_links": {
        "next": {
            "href": "https://platform.adobe.io:443/data/foundation/sandbox-management/sandboxes/?limit={limit}&offset={offset}",
            "templated": true
        },
        "prev": {
            "href": "https://platform.adobe.io:443/data/foundation/sandbox-management/sandboxes?offset=0&limit=1",
            "templated": null
        },
        "page": {
            "href": "https://platform.adobe.io:443/data/foundation/sandbox-management/sandboxes?offset=1&limit=1",
            "templated": null
        }
    }
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | サンドボックスの名前。このプロパティは、API 呼び出しでルックアップに使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態です。サンドボックスの状態は、次のいずれかになります。<br/><ul><li>`creating`: サンドボックスが作成されましたが、まだシステムによるプロビジョニングが行われています。</li><li>`active`：サンドボックスが作成され、アクティブになります。</li><li>`failed`: エラーが発生したので、サンドボックスをシステムでプロビジョニングできなかったため、無効になりました。</li><li>`deleted`：サンドボックスは手動で無効になりました。</li></ul> |
| `type` | サンドボックスタイプ。 現在サポートされているサンドボックスのタイプには、`development` と `production` があります。 |
| `isDefault` | このサンドボックスが組織のデフォルトの実稼動サンドボックスであるかどうかを示すブール値プロパティ。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |

## サンドボックスの検索 {#lookup}

個々のサンドボックスを検索するには、リクエストパスにサンドボックスの `name` プロパティを含む GET リクエストを作成する必要があります。

**API 形式**

```http
GET /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 検索するサンドボックスの `name` プロパティです。 |

**リクエスト**

次のリクエストは、「dev-2」という名前のサンドボックスを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

成功した応答は、サンドボックスの詳細（`name`、`title`、`state`、`type`）を返します。

```json
{
    "name": "dev-2",
    "title": "Development 2",
    "state": "creating",
    "type": "development",
    "region": "VA7",
    "isDefault": false,
    "eTag": 1,
    "createdDate": "2019-09-07 10:16:02",
    "lastModifiedDate": "2019-09-07 10:16:02",
    "createdBy": "{USER_ID}",
    "modifiedBy": "{USER_ID}"
}
```

| プロパティ | 説明 |
| --- | --- |
| `name` | サンドボックスの名前。このプロパティは、API 呼び出しでルックアップに使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態。サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**：サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**：サンドボックスが作成され、アクティブです。</li><li>**失敗**：エラーが原因で、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除**：サンドボックスは手動で無効にされています。</li></ul> |
| `type` | サンドボックスタイプ。 現在サポートされているサンドボックスのタイプには、`development` と `production` があります。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブール型のプロパティです。通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |

## サンドボックスの作成 {#create}

>[!NOTE]
>
>新しいサンドボックスが作成されたら、新しいサンドボックスの使用を開始する前に、まず [Adobe Admin Console](https://adminconsole.adobe.com/) でその新しいサンドボックスを製品プロファイルに追加する必要があります。製品プロファイルにサンドボックスをプロビジョニングする方法について詳しくは、[製品プロファイルの権限の管理](../../access-control/ui/permissions.md)に関するドキュメントを参照してください。

`/sandboxes` エンドポイントに POST リクエストを実行することで、新しい開発用サンドボックスまたは実稼動用サンドボックスを作成できます。

### 開発サンドボックスを作成

開発用サンドボックスを作成するには、リクエストペイロードで `type` 属性に値 `development` を指定する必要があります。

**API 形式**

```http
POST /sandboxes
```

**リクエスト**

次のリクエストは、「acme-dev」という名前の新しい開発用サンドボックスを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "acme-dev",
    "title": "Acme Business Group dev",
    "type": "development"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 今後のリクエストでサンドボックスにアクセスするために使用される識別子。この値は一意である必要があります。ベストプラクティスはできる限りわかりやすい値にすることです。この値には、スペースや特殊文字を含めることはできません。 |
| `title` | Experience Platform ユーザーインターフェイスでの表示用に使用される、人間が判読できる名前。 |
| `type` | 作成するサンドボックスのタイプ。実稼動以外のサンドボックスの場合、この値は `development` にする必要があります。 |

**応答**

正常な応答は、新しく作成されたサンドボックスの詳細を返し、`state` が「作成中」であることを示します。

```json
{
    "name": "acme-dev",
    "title": "Acme Business Group dev",
    "state": "creating",
    "type": "development",
    "region": "VA7"
}
```

>[!NOTE]
>
>サンドボックスのプロビジョニングには約 30 秒かかり、その後、サンドボックスの `state` が「アクティブ」または「失敗」になります。

### 実稼動用サンドボックスの作成

実稼動用サンドボックスを作成するには、リクエストペイロードで `type` 属性に値 `production` を指定する必要があります。

**API 形式**

```http
POST /sandboxes
```

**リクエスト**

次のリクエストは、「acme」という名前の新しい実稼動サンドボックスを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H `Accept: application/json` \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "acme",
    "title": "Acme Business Group",
    "type": "production"
}'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | 今後のリクエストでサンドボックスにアクセスするために使用される識別子。この値は一意である必要があります。ベストプラクティスはできる限りわかりやすい値にすることです。この値には、スペースや特殊文字を含めることはできません。 |
| `title` | Experience Platform ユーザーインターフェイスでの表示用に使用される、人間が判読できる名前。 |
| `type` | 作成するサンドボックスのタイプ。実稼動サンドボックスの場合、この値は `production` である必要があります。 |

**応答**

正常な応答は、新しく作成されたサンドボックスの詳細を返し、`state` が「作成中」であることを示します。

```json
{
    "name": "acme",
    "title": "Acme Business Group",
    "state": "creating",
    "type": "production",
    "region": "VA7"
}
```

>[!NOTE]
>
>サンドボックスのプロビジョニングには約 30 秒かかり、その後、サンドボックスの `state` が「アクティブ」または「失敗」になります。

## サンドボックスの更新 {#put}

リクエストパスにサンドボックスの`name`を含む PATCH リクエストを作成し、リクエストペイロードで更新するプロパティを作成することで、サンドボックス内の 1 つ以上のフィールドを更新できます。

>[!NOTE]
>
>現在、更新できるのはサンドボックスの `title` プロパティのみです。

**API 形式**

```http
PATCH /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 更新するサンドボックスの`name`プロパティ。 |

**リクエスト**

次のリクエストは、「acme」という名前のサンドボックスの `title` プロパティを更新します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/acme \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json'
  -d '{
    "title": "Acme Business Group prod"
  }'
```

**応答**

成功した応答は、新しく更新されたサンドボックスの詳細を含む HTTP ステータス 200（OK）を返します。

```json
{
    "name": "acme",
    "title": "Acme Business Group prod",
    "state": "active",
    "type": "production",
    "region": "VA7"
}
```

## サンドボックスのリセット {#reset}

サンドボックスには、デフォルト以外のすべてのリソースをサンドボックスから削除する「ファクトリリセット」機能があります。 サンドボックスをリセットするには、そのサンドボックスの `name` をリクエストパスに含む PUT リクエストを作成する必要があります。

**API 形式**

```http
PUT /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |
| `validationOnly` | 実際のリクエストを行わずに、サンドボックスのリセット操作に関するプリフライトチェックを実行できるオプションのパラメーター。 このパラメーターを `validationOnly=true` に設定すると、リセットしようとしているサンドボックスにAdobe Analytics、Adobe Audience Managerまたはセグメント共有のデータが含まれているかどうかを確認できます。 |

**リクエスト**

次のリクエストでは、「acme-dev」という名前のサンドボックスがリセットされます。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/acme-dev?validationOnly=true \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json'
  -d '{
    "action": "reset"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `action` | サンドボックスをリセットするには、このパラメーターをリクエストペイロードで値「reset」と共に指定する必要があります。 |

**応答**

>[!NOTE]
>
>サンドボックスがリセットされると、システムでプロビジョニングされるまでに約 30 秒かかります。

正常な応答は、更新されたサンドボックスの詳細を返し、`state` が「リセット中」であることを示します。

```json
{
    "id": "d8184350-dbf5-11e9-875f-6bf1873fec16",
    "name": "acme-dev",
    "title": "Acme Business Group dev",
    "state": "resetting",
    "type": "development",
    "region": "VA7"
}
```

デフォルトの実稼動用サンドボックスとユーザー作成の実稼動用サンドボックスは、その中にホストされている ID グラフがAdobe Analyticsでも [ クロスデバイス分析（CDA） ](https://experienceleague.adobe.com/docs/analytics/components/cda/overview.html?lang=ja) 機能に使用されている場合や、その中にホストされている ID グラフがAdobe Audience Managerでも [People Based Destinations （PBD） ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html?lang=ja) 機能に使用されている場合は、リセットできません。

サンドボックスがリセットされない可能性のある例外のリストを以下に示します。

```json
{
    "status": 400,
    "title": "Sandbox `{SANDBOX_NAME}` cannot be reset. The identity graph hosted in this sandbox is also being used by Adobe Analytics for the Cross Device Analytics (CDA) feature.",
    "type": "http://ns.adobe.com/aep/errors/SMS-2074-400"
},
{
    "status": 400,
    "title": "Sandbox `{SANDBOX_NAME}` cannot be reset. The identity graph hosted in this sandbox is also being used by Adobe Audience Manager for the People Based Destinations (PBD) feature.",
    "type": "http://ns.adobe.com/aep/errors/SMS-2075-400"
},
{
    "status": 400,
    "title": "Sandbox `{SANDBOX_NAME}` cannot be reset. The identity graph hosted in this sandbox is also being used by Adobe Audience Manager for the People Based Destinations (PBD) feature, as well by Adobe Analytics for the Cross Device Analytics (CDA) feature.",
    "type": "http://ns.adobe.com/aep/errors/SMS-2076-400"
},
{
    "status": 400,
    "title": "Warning: Sandbox `{SANDBOX_NAME}` is used for bi-directional segment sharing with Adobe Audience Manager or Audience Core Service.",
    "type": "http://ns.adobe.com/aep/errors/SMS-2077-400"
}
```

リクエストに `ignoreWarnings` パラメーターを追加することで、[!DNL Audience Manager] または [!DNL Audience Core Service] との双方向のセグメント共有に使用される実稼動用サンドボックスのリセットに進むことができます。

**API 形式**

```http
PUT /sandboxes/{SANDBOX_NAME}?ignoreWarnings=true
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |
| `ignoreWarnings` | 検証チェックをスキップし、[!DNL Audience Manager] または [!DNL Audience Core Service] との双方向のセグメント共有に使用される実稼動サンドボックスを強制的にリセットできるオプションのパラメーターです。 このパラメーターは、デフォルトの実稼動サンドボックスには適用できません。 |

**リクエスト**

次のリクエストでは、「acme」という名前の実稼動サンドボックスがリセットされます。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/acme?ignoreWarnings=true \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json'
  -d '{
    "action": "reset"
  }'
```

**応答**

正常な応答は、更新されたサンドボックスの詳細を返し、`state` が「リセット中」であることを示します。

```json
{
    "id": "d8184350-dbf5-11e9-875f-6bf1873fec16",
    "name": "acme",
    "title": "Acme Business Group prod",
    "state": "resetting",
    "type": "production",
    "region": "VA7"
}
```

## サンドボックスの削除 {#delete}

>[!IMPORTANT]
>
>デフォルトの実稼動サンドボックスは削除できません。

サンドボックスを削除するには、リクエストパスにサンドボックスの `name` を含む DELETE リクエストを実行します。

>[!NOTE]
>
>この API 呼び出しを行うと、サンドボックスの `status` プロパティが「削除済み」に更新され、アクティベートが解除されます。 サンドボックスが削除された後も、GET リクエストを使用して、その詳細を取得できます。

**API 形式**

```http
DELETE /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 削除するサンドボックスの `name`。 |
| `validationOnly` | 実際のリクエストを行わずに、サンドボックスの削除操作に関するプリフライトチェックを実行できるオプションのパラメーター。 このパラメーターを `validationOnly=true` に設定すると、リセットしようとしているサンドボックスにAdobe Analytics、Adobe Audience Managerまたはセグメント共有のデータが含まれているかどうかを確認できます。 |
| `ignoreWarnings` | 検証チェックをスキップし、[!DNL Audience Manager] または [!DNL Audience Core Service] との双方向のセグメント共有に使用される、ユーザー作成の実稼動用サンドボックスを強制的に削除できるオプションのパラメーターです。 このパラメーターは、デフォルトの実稼動サンドボックスには適用できません。 |

**リクエスト**

次のリクエストでは、「acme」という名前の実稼動サンドボックスが削除されます。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/acme?ignoreWarnings=true \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答**

リクエストが成功した場合は、サンドボックスの更新された情報が返され、サンドボックスの `state` が「deleted」になったことが示されます。

```json
{
    "name": "acme",
    "title": "Acme Business Group prod",
    "state": "deleted",
    "type": "development",
    "region": "VA7"
}
```

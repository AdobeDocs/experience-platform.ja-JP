---
keywords: Experience Platform；ホーム；人気の高いトピック；サンドボックス開発者ガイド
solution: Experience Platform
title: サンドボックス管理 API エンドポイント
description: Sandbox API の/sandboxes エンドポイントを使用すると、Adobe Experience Platformでサンドボックスをプログラムで管理できます。
exl-id: 0ff653b4-3e31-4ea5-a22e-07e18795f73e
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '1489'
ht-degree: 54%

---

# サンドボックス管理エンドポイント

Adobe Experience Platform のサンドボックスは、独立した開発環境を提供し、実稼働環境に影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。この `/sandboxes` エンドポイント [!DNL Sandbox] API を使用すると、Platform のサンドボックスをプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Sandbox] API](https://www.adobe.io/experience-platform-apis/references/sandbox) の一部です。先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## サンドボックスのリストの取得 {#list}

に対してGETリクエストをおこなうことで、IMS 組織（アクティブまたはその他）に属するすべてのサンドボックスをリストできます `/sandboxes` endpoint.

**API 形式**

```http
GET /sandboxes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。詳しくは、 [クエリパラメーター](./appendix.md#query) を参照してください。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes?&limit=4&offset=1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
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
| `name` | サンドボックスの名前。このプロパティは、API 呼び出しで参照目的に使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態です。サンドボックスの状態は、次のいずれかになります。<br/><ul><li>`creating`:サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>`active`:サンドボックスが作成され、アクティブです。</li><li>`failed`:エラーが発生したため、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>`deleted`:サンドボックスは手動で無効にされました。</li></ul> |
| `type` | サンドボックスタイプ。 現在サポートされているサンドボックスのタイプは次のとおりです。 `development` および `production`. |
| `isDefault` | このサンドボックスが組織のデフォルトの実稼動サンドボックスであるかどうかを示すブール型プロパティです。 |
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
| `name` | サンドボックスの名前。このプロパティは、API 呼び出しで参照目的に使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態。サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**：サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**：サンドボックスが作成され、アクティブです。</li><li>**失敗**：エラーが原因で、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除**：サンドボックスは手動で無効にされています。</li></ul> |
| `type` | サンドボックスタイプ。 現在サポートされているサンドボックスのタイプは次のとおりです。 `development` および `production`. |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブール型のプロパティです。通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |

## サンドボックスの作成 {#create}

>[!NOTE]
>
>新しいサンドボックスが作成されたら、新しいサンドボックスの使用を開始する前に、まず [Adobe Admin Console](https://adminconsole.adobe.com/) でその新しいサンドボックスを製品プロファイルに追加する必要があります。製品プロファイルにサンドボックスをプロビジョニングする方法について詳しくは、[製品プロファイルの権限の管理](../../access-control/ui/permissions.md)に関するドキュメントを参照してください。

に対してPOSTリクエストを実行することで、新しい開発または実稼動サンドボックスを作成できます `/sandboxes` endpoint.

### 開発サンドボックスの作成

開発サンドボックスを作成するには、 `type` 値を持つ属性 `development` リクエストペイロード内。

**API 形式**

```http
POST /sandboxes
```

**リクエスト**

次のリクエストは、「acme-dev」という名前の新しい開発サンドボックスを作成します。

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
| `title` | Platform ユーザーインターフェイスでの表示用に使用される、わかりやすい名前。 |
| `type` | 作成するサンドボックスのタイプ。実稼動以外のサンドボックスの場合、この値は `development`. |

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
>サンドボックスのプロビジョニングには約 30 秒かかり、その後システムでプロビジョニングされます `state` は「アクティブ」または「失敗」になります。

### 実稼働用サンドボックスの作成

実稼動用サンドボックスを作成するには、 `type` 値を持つ属性 `production` リクエストペイロード内。

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
| `title` | Platform ユーザーインターフェイスでの表示用に使用される、わかりやすい名前。 |
| `type` | 作成するサンドボックスのタイプ。実稼働用サンドボックスの場合、この値は `production`. |

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
>サンドボックスのプロビジョニングには約 30 秒かかり、その後システムでプロビジョニングされます `state` は「アクティブ」または「失敗」になります。

## サンドボックスの更新 {#put}

リクエストパスにサンドボックスの`name`を含む PATCH リクエストを作成し、リクエストペイロードで更新するプロパティを作成することで、サンドボックス内の 1 つ以上のフィールドを更新できます。

>[!NOTE]
>
>現在、サンドボックスの`title`プロパティのみを更新できます。

**API 形式**

```http
PATCH /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 更新するサンドボックスの`name`プロパティ。 |

**リクエスト**

次のリクエストは、 `title` プロパティのプロパティです。

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

サンドボックスには「工場出荷時リセット」機能があり、この機能によりサンドボックスからデフォルト以外のすべてのリソースが削除されます。 サンドボックスをリセットするには、そのサンドボックスの `name` をリクエストパスに含む PUT リクエストを作成する必要があります。

**API 形式**

```http
PUT /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |
| `validationOnly` | 実際のリクエストをおこなわずに、サンドボックスのリセット操作に対してプリフライトチェックを実行できるオプションのパラメーターです。 このパラメーターをに設定します。 `validationOnly=true` をクリックして、リセットしようとしているサンドボックスに、Adobe Analytics、Adobe Audience Manager、またはセグメント共有データが含まれているかどうかを確認します。 |

**リクエスト**

次のリクエストは、「acme-dev」という名前のサンドボックスをリセットします。

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
>サンドボックスがリセットされると、システムによってプロビジョニングされるまでに約 30 秒かかります。

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

デフォルトの実稼働サンドボックスと、その中でホストされている ID グラフがAdobe Analyticsで [クロスデバイス分析 (CDA)](https://experienceleague.adobe.com/docs/analytics/components/cda/overview.html?lang=ja) 機能を使用するか、その中でホストされている ID グラフがAdobe Audience Managerで [People Based Destinations (PBD)](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html?lang=ja) 機能。

サンドボックスのリセットを妨げる可能性のある例外のリストを次に示します。

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

を使用した双方向のセグメント共有に使用される実稼動用サンドボックスのリセットに進むことができます。 [!DNL Audience Manager] または [!DNL Audience Core Service] を `ignoreWarnings` パラメーターをリクエストに追加します。

**API 形式**

```http
PUT /sandboxes/{SANDBOX_NAME}?ignoreWarnings=true
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |
| `ignoreWarnings` | 検証チェックをスキップし、との双方向のセグメント共有に使用される実稼動サンドボックスを強制的にリセットできるオプションのパラメーター。 [!DNL Audience Manager] または [!DNL Audience Core Service]. このパラメーターは、デフォルトの実稼動サンドボックスには適用できません。 |

**リクエスト**

次のリクエストは、「acme」という名前の実稼動用サンドボックスをリセットします。

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
>この API 呼び出しを実行すると、サンドボックスの `status` プロパティが「deleted」に更新され、サンドボックスが非アクティブになります。サンドボックスが削除された後も、GET リクエストを使用して、その詳細を取得できます。

**API 形式**

```http
DELETE /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | 削除するサンドボックスの `name`。 |
| `validationOnly` | 実際のリクエストをおこなわずに、サンドボックスの削除操作に対して事前フライトチェックを実行できるオプションのパラメーターです。 このパラメーターをに設定します。 `validationOnly=true` をクリックして、リセットしようとしているサンドボックスに、Adobe Analytics、Adobe Audience Manager、またはセグメント共有データが含まれているかどうかを確認します。 |
| `ignoreWarnings` | 検証チェックをスキップし、との双方向のセグメント共有に使用されるユーザーが作成した実稼動サンドボックスを強制的に削除できるオプションのパラメーターです。 [!DNL Audience Manager] または [!DNL Audience Core Service]. このパラメーターは、デフォルトの実稼動サンドボックスには適用できません。 |

**リクエスト**

次のリクエストは、「acme」という名前の実稼動用サンドボックスを削除します。

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

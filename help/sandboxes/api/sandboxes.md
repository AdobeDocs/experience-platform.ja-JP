---
keywords: Experience Platform；ホーム；人気のあるトピック；サンドボックス開発者ガイド
solution: Experience Platform
title: サンドボックス管理APIエンドポイント
topic-legacy: developer guide
description: サンドボックスAPIの/sandboxesエンドポイントを使用すると、Adobe Experience Platformのサンドボックスをプログラムで管理できます。
source-git-commit: f84898a87a8a86783220af7f74e17f464a780918
workflow-type: tm+mt
source-wordcount: '1323'
ht-degree: 53%

---

# サンドボックス管理エンドポイント

Adobe Experience Platform のサンドボックスは、独立した開発環境を提供し、実稼働環境に影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。[!DNL Sandbox] APIの`/sandboxes`エンドポイントを使用すると、Platformのサンドボックスをプログラムで管理できます。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Sandbox] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sandbox-api.yaml) の一部です。続行する前に、[はじめにのガイド](./getting-started.md)を参照して、関連ドキュメントへのリンク、このドキュメントのAPI呼び出し例の読み方、およびExperience PlatformAPIを正しく呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## サンドボックスのリストの取得 {#list}

`/sandboxes`エンドポイントにGETリクエストをおこなうことで、IMS組織（アクティブまたはその他）に属するすべてのサンドボックスをリストできます。

**API 形式**

```http
GET /sandboxes?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{QUERY_PARAMS}` | 結果をフィルターするオプションのクエリパラメーター。詳しくは、[クエリパラメーター](./appendix.md#query)の節を参照してください。 |

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes?&limit=4&offset=1 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `name` | サンドボックスの名前。このプロパティは、API呼び出しで参照目的で使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態です。サンドボックスの状態は、次のいずれかになります。<br/><ul><li>`creating`:サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>`active`:サンドボックスが作成され、アクティブです。</li><li>`failed`:エラーが原因で、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>`deleted`:サンドボックスは手動で無効になっています。</li></ul> |
| `type` | サンドボックスのタイプ。 現在サポートされているサンドボックスの種類は、`development`と`production`です。 |
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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `name` | サンドボックスの名前。このプロパティは、API呼び出しで参照目的で使用されます。 |
| `title` | サンドボックスの表示名。 |
| `state` | サンドボックスの現在の処理状態。サンドボックスの状態は、次のいずれかになります。 <ul><li>**作成**：サンドボックスは作成されましたが、システムによって引き続きプロビジョニングされています。</li><li>**アクティブ**：サンドボックスが作成され、アクティブです。</li><li>**失敗**：エラーが原因で、サンドボックスはシステムでプロビジョニングできず、無効になっています。</li><li>**削除**：サンドボックスは手動で無効にされています。</li></ul> |
| `type` | サンドボックスのタイプ。 現在サポートされているサンドボックスのタイプは次のとおりです。`development`と`production`が表示されます。 |
| `isDefault` | このサンドボックスが組織のデフォルトのサンドボックスであるかどうかを示すブール型のプロパティです。通常、これは実稼働用サンドボックスです。 |
| `eTag` | サンドボックスの特定のバージョンの識別子。バージョン管理とキャッシュの効率化に使用され、この値はサンドボックスに変更が追加されるたびに更新されます。 |

## サンドボックスの作成 {#create}

`/sandboxes`エンドポイントにPOSTリクエストを送信することで、新しい開発用サンドボックスまたは実稼動用サンドボックスを作成できます。

### 開発用サンドボックスの作成

開発用サンドボックスを作成するには、リクエストペイロードで`type`属性の値を`development`に指定する必要があります。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `type` | 作成するサンドボックスのタイプ。実稼動以外のサンドボックスの場合、この値は`development`にする必要があります。 |

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
>サンドボックスのプロビジョニングには約30秒かかり、その後、 `state`が「アクティブ」または「失敗」になります。

### 実稼動用サンドボックスの作成

実稼動用サンドボックスを作成するには、リクエストペイロードに`type`属性と値`production`を指定する必要があります。

**API 形式**

```http
POST /sandboxes
```

**リクエスト**

次のリクエストは、「acme」という名前の新しい実稼動用サンドボックスを作成します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `type` | 作成するサンドボックスのタイプ。実稼動用サンドボックスの場合、この値は`production`にする必要があります。 |

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
>サンドボックスのプロビジョニングには約30秒かかり、その後、 `state`が「アクティブ」または「失敗」になります。

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

次のリクエストは、「acme」という名前のサンドボックスの`title`プロパティを更新します。

```shell
curl -X PATCH \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/acme \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

>[!IMPORTANT]
>
>デフォルトの実稼動用サンドボックスは、ホストされているIDグラフがAdobe Analyticsで[クロスデバイス分析(CDA)](https://experienceleague.adobe.com/docs/analytics/components/cda/overview.html)機能にも使用されている場合、またはAdobe Audience ManagerでホストされているIDグラフが[People Based Destinations(PBD)](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html)機能にも使用されている場合は、リセットできません。

開発サンドボックスには「出荷時リセット」機能があり、この機能によりサンドボックスからデフォルト以外のすべてのリソースが削除されます。サンドボックスをリセットするには、そのサンドボックスの `name` をリクエストパスに含む PUT リクエストを作成する必要があります。

**API 形式**

```http
PUT /sandboxes/{SANDBOX_NAME}
```

| パラメーター | 説明 |
| --- | --- |
| `{SANDBOX_NAME}` | リセットするサンドボックスの `name` プロパティです。 |

**リクエスト**

次のリクエストは、「acme-dev」という名前のサンドボックスをリセットします。

```shell
curl -X PUT \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/acme-dev \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json'
  -d '{
    "action": "reset"
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `action` | サンドボックスをリセットするには、このパラメーターをリクエストペイロードで値「reset」と共に指定する必要があります。 |

**応答**

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

>[!NOTE]
>
>サンドボックスがリセットされると、システムによってプロビジョニングされるまで約30秒かかります。 プロビジョニングが完了すると、サンドボックスの `state` は「アクティブ」または「失敗」になります。

次の表に、サンドボックスのリセットを妨げる可能性のある例外を示します。

| エラーコード | 説明 |
| --- | --- |
| `2074-400` | このサンドボックスでホストされているIDグラフは、Adobe Analyticsでクロスデバイス分析(CDA)機能にも使用されているので、このサンドボックスをリセットできません。 |
| `2075-400` | このサンドボックスでホストされているIDグラフは、Adobe Audience ManagerでPeople Based Destinations(PBD)機能にも使用されているので、このサンドボックスをリセットできません。 |
| `2076-400` | このサンドボックスでホストされるIDグラフは、Adobe Audience ManagerでPeople Based Destinations(PBD)機能にも、Adobe Analyticsでクロスデバイス分析(CDA)機能にも使用されているので、このサンドボックスをリセットできません。 |
| `2077-400` | 警告：サンドボックス`{SANDBOX_NAME}`は、Adobe Audience ManagerまたはAudience Core Serviceとの双方向のセグメント共有に使用されます。 |

## サンドボックスの削除 {#delete}

>[!IMPORTANT]
>
>デフォルトの実稼動用サンドボックスは削除できません。

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

**リクエスト**

次のリクエストは、「acme-dev」という名前のサンドボックスを削除します。

```shell
curl -X DELETE \
  https://platform.adobe.io/data/foundation/sandbox-management/sandboxes/dev-2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}'
```

**応答**

リクエストが成功した場合は、サンドボックスの更新された情報が返され、サンドボックスの `state` が「deleted」になったことが示されます。

```json
{
    "name": "acme-dev",
    "title": "Acme Business Group dev",
    "state": "deleted",
    "type": "development",
    "region": "VA7"
}
```

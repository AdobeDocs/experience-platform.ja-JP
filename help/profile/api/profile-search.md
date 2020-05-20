---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 95e002c60389ca7e4c1dcf32bbcf6f552cd55d95
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 1%

---


# プロファイル検索

プロファイル検索は、様々なデータソースに含まれる設定可能なフィールドを検索およびインデックス付けして、ほぼリアルタイムで返すために使用します。

このガイドは、プロファイル検索をより深く理解するのに役立つ情報を提供し、APIを使用して基本的なアクションを実行するためのサンプルAPI呼び出しを含みます。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 先に進む前に、 [リアルタイムのお客様向けプロファイル開発ガイドを参照してください](getting-started.md)。

特に、プロファイル開発ガイドの「 [はじめに](getting-started.md) 」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを成功させるために必要なヘッダーに関する重要な情報が含まれています。

### 検索結果の取得

検索エンドポイントは、必要なパラメーターの値と追加のオプションのクエリパラメーターに基づいて検索結果を取得するために使用でき `schema.name` ます。 複数のパラメーターを使用でき、アンパサンド(&amp;)で区切ります。

**API形式**

```http
GET /search?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `schema.name` | **必須。** 検索するコンテンツを含むスキーマクラスの名前。ドット表記形式で記述されます。 現在、のみがサポートさ `schema.name=_xdm.context.segmentdefinition` れています。 |
| `limit` | 返す検索結果の数。 デフォルト値は 50 です。 |
| `page` | クエリのページ番号付けに使用するページ番号。 |
| `s` | Microsoftが [Luceneの検索構文を実装するためのクエリ](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)。 検索語句を指定しない場合、に関連付けられているすべてのレコード `schema.name` が返されます。 詳しくは、このドキュメントの「 [検索パラメーター](#search-parameters) 」セクションを参照してください。 |
| `namespace` | パラメーターで指定されたスキーマクラスで検索する実際のデータを識別し `schema.name` ます。 |
| `organization.type` | 応答の内容を決定します。 返されるコンテンツの形式は、で使用される値に応じて異なり `schema.name`ます。 の場合 `_xdm.context.segmentdefinition`、有効な値は `hierarchy` またはで `hierarchyinfo`す。 |
| `organization.id` | **を指定した場合は必須`organization.type`です。** 階層の階層と組み合わせて使用する場合、指定した組織の階層 `organization.type` を示します。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search?schema.name=_xdm.context.segmentdefinition \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、検索条件を満たすオブジェクトの配列を返します。 この例では、 `schema.name` パラメーターが存在するので、セグメント定義のリストが返され `_xdm.context.segmentdefinition`ます。

```json
{
  "aam-hierarchy": {
    "_xdm.context.segmentdefinition": [
      {
        "isFolder": true,
        "id": "100023",
        "name": "Segment Definition 1",
        "description": "Sample description.",
        "parentFolderId": "99970"
      },
      {
        "isFolder": false,
        "id": "1000028",
        "name": "Segment Definition 2",
        "description": "Sample description.",
        "parentFolderId": "103584"
      }
    ]
  },
  "page": {
    "totalCount": 2,
    "totalPages": 1,
    "pageOffset": "1",
    "pageSize": 2,
    "limit": 2
  }
}
```

### プロビジョニング要求の作成

エンドポイントにPOSTリクエストを行うことで、スキーマのプロファイル検索を有効にするプロビジョニングリクエストを作成でき `/search/provisioning/component/init` ます。

**リクエスト**

>[!CAUTION]
>このPOST要求にはペイロードが含まれないので、Content-Typeヘッダーは必要ありません。 さらに、すべてのデータがグローバルサンドボックスに送信されるので、Sandboxヘッダーはありません。

```shell
curl -X POST \
    https://platform.adobe.io/data/core/ups/search/provisioning/component/init \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' 
```

**応答**

応答が成功すると、HTTPステータス201（作成済み）と次のメッセージが返されます。

```plaintext
The request has been fulfilled and resulted in a new resource being created.
```

### 構成要求の処理

設定エンドポイントは、新しいIMS組織に対して、適切なインデックス、インデクサー、およびデータソース接続を設定するために使用できます。 設定要求を処理するには、次の2つのプロパティが必要です。 `databaseName` と `containerName`。

`databaseName` 設定する組織のプロファイルデータベースの名前を表します。

`containerName` は、data connectorが設定するコンテナの名前を表します。これは、設定時に読み取られます。 には2つの値があり `containerName`ます。1つまたは両方をPOSTリクエストで使用できます。
- `_xdm.content.segmentdefinition`
- `_experience.audiencemanager.segmentfolder`

### 設定要求の作成

次のAPI呼び出しは、要求ペイロードで指定されたパラメーターに基づいて、必要なデータソース、インデクサーおよびインデックスを生成します。

**API形式**

```http
POST /search/configure
```

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/search/configure \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "databaseName": {DATABASE_NAME},
    "containerName": "_xdm.context.segmentdefinition" "_experience.audiencemanager.segmentfolder"
  }'
```

**応答**

成功した応答は、HTTPステータス202(Accepted)と平文メッセージを返す。

```plaintext
The request has been accepted for processing, but the processing has not been completed.
```

### 設定要求の削除

次のAPI呼び出しは、一致するインデックスエントリを削除し、要求ペイロードで指定されたパラメーターに基づいてインデクサーとデータソースを削除します。

>[!NOTE]
>インデックス自体は共有リソースなので削除されません。

**API形式**

```http
DELETE /search/configure
```

**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/ups/search/configure \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "databaseName": {DATABASE_NAME},
    "containerName": "_xdm.context.segmentdefinition" "_experience.audiencemanager.segmentfolder"
  }'
```

**応答**

成功した応答は、HTTPステータス202(Accepted)と平文メッセージを返す。

```plaintext
The request has been accepted for processing, but the processing has not been completed.
```

## 次の手順

このガイドを読むと、リアルタイム顧客プロファイル検索のしくみをより深く理解できます。 リアルタイム顧客プロファイルの詳細については、 [リアルタイム顧客プロファイルの概要を参照してください](../home.md)。 セグメント化について詳しくは、 [セグメント化の概要を参照してください](../../segmentation/home.md)。

## 付録 {#appendix}

### 検索パラメータ {#search-parameters}

次の表に、検索APIを使用する場合の検索パラメータの動作方法の詳細をリストします。

| 検索クエリ | 説明 |
|------------ | -----------|
| ～を食べる | 任意の単語を検索します。 これは、検索可能なフィールドのいずれかで「foo」という単語が見つかった場合に結果を返します。 |
| fooとbar | ブール検索。 「foo」と「bar」の **両方が検索可能なフィールドのいずれかで見つかった場合** 、結果が返されます。 |
| foo OR bar | ブール検索。 「foo」という単語 **または** 「bar」という単語が検索可能なフィールドのいずれかに見つかった場合、結果が返されます。 |
| foo NOTバー | ブール検索。 「foo」という単語が見つかったが、「bar」という単語が検索可能なフィールドのどれにも見つからない場合、結果が返されます。 |
| name: fooとbar | ブール検索。 「foo」と「bar」 **の両方が** 「name」フィールドに見つかる場合、結果が返されます。 |
| run* | ワイルドカード検索。 アスタリスク(*)を使用すると、0文字以上の任意の文字と一致します。つまり、検索可能なフィールドのコンテンツに「run」という開始を含む単語が含まれている場合、結果が返されます。 例えば、「runs」、「running」、「runner」または「runt」という語が表示された場合に結果が返されます。 |
| カム？ | ワイルドカード検索。 疑問符(?)の使用 は1文字のみに一致します。つまり、「cam」と追加の文字を含む検索可能なフィールドの開始の内容がある場合、結果が返されます。 例えば、「camp」または「cams」という語が表示される場合は結果が返され、「camera」または「campfire」という語が表示される場合は結果が返されません。 |
| &quot;blue abrasor&quot; | フレーズ検索。 検索可能なフィールドのコンテンツに「blue abrasor（青いかさ）」という完全なフレーズが含まれている場合、結果が返されます。 |
| blue\～ | あいまい検索。 必要に応じて、チルダ(～)の後に0 ～ 2の数値を入力し、編集距離を指定できます。 例えば、「blue\～1」は、「blue」、「blues」または「glue」を返します。 あいまい検索 **は用語にのみ適用でき** 、語句には適用できません。 ただし、1つのフレーズ内の各単語の末尾にチルダを追加することはできます。 例えば、「camping\～ in\～ the\～ summer\～」は「camping in the summer」と一致します。 |
| &quot;ホテルエアポート&quot;\\～5 | 近接検索。 このタイプの検索は、ドキュメント内でお互いに近い用語を検索するために使用されます。 例えば、このフレーズ `"hotel airport"~5` では、ドキュメント内の5語以内に「hotel」と「airport」という用語があります。 |
| `/a[0-9]+b$/` | 正規式検索。 このタイプの検索では、RegExpクラスで説明されているように、スラッシュ「/」の間の内容に基づいて一致を見つけます。 例えば、「motel」または「hotel」を含むドキュメントを検索するには、を指定し `/[mh]otel/`ます。 正規式検索は、単一の単語に対して一致します。 |

クエリ構文に関する詳細なドキュメントについては、 [Luceneクエリ構文ドキュメントを参照してください](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)。

---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 95e002c60389ca7e4c1dcf32bbcf6f552cd55d95

---


# プロファイル検索

プロファイル検索は、様々なデータソースに含まれる設定可能なフィールドを検索し、インデックスを作成して、ほぼリアルタイムで返すのに使用します。

このガイドには、プロファイル検索をより深く理解するのに役立つ情報が記載されており、APIを使用して基本的なアクションを実行するためのサンプルAPI呼び出しが含まれています。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 続行する前に、リアルタイムのお客様 [プロファイル開発ガイドを参照してください](getting-started.md)。

特に、プロファイル開発ガ [イドの](getting-started.md) 「はじめに」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIを正常に呼び出すために必要なヘッダーに関する重要な情報が含まれています。

### 検索結果の取得

検索エンドポイントは、必要なパラメーターの値と追加のオプションのパラメーターの値に基づいて検索結 `schema.name` 果を取得するために使用できます。 複数のパラメーターを使用する場合は、アンパサンド(&amp;)で区切ります。

**API形式**

```http
GET /search?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `schema.name` | **必須.** 検索する内容を含むスキーマクラスの名前。ドット表記形式で記述されます。 現在は、のみがサポ `schema.name=_xdm.context.segmentdefinition` ートされています。 |
| `limit` | 返す検索結果の数。 デフォルト値は 50 です。 |
| `page` | 検索したクエリのページ番号。 |
| `s` | MicrosoftのLuceneの検索構文の実装に適 [合するクエリ](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)。 検索語句が指定されていない場合、に関連付けられているすべてのレコ `schema.name` ードが返されます。 詳しくは、このパラメーターの「検索パラメー [ター](#search-parameters) 」セクションを参照してください。ドキュメント |
| `namespace` | パラメーターで指定されたスキーマクラスで検索する実際のデータを指定 `schema.name` します。 |
| `organization.type` | 応答の内容を決定します。 返されるコンテンツの形式は、で使用される値によって異なりま `schema.name`す。 の有 `_xdm.context.segmentdefinition`効な値は、または `hierarchy` です `hierarchyinfo`。 |
| `organization.id` | **を指定した場合は`organization.type`必須です。** 階層の階層と組み合わせて使用する場合、指定した組織の階層を `organization.type` 示します。 |

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

正常に応答すると、検索条件を満たすオブジェクトの配列が返されます。 この例では、パラメーターがで `schema.name` あったので、 `_xdm.context.segmentdefinition`セグメント定義のリストが返されます。

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

エンドポイントにPOSTリクエストを行うことで、プロファイルでのスキーマ検索を有効にするプロビジョニングリクエストを作成で `/search/provisioning/component/init` きます。

**リクエスト**

>[!CAUTION]
>このPOSTリクエストにはペイロードが含まれていないので、Content-Typeヘッダーは必要ありません。 また、すべてのデータがグローバルサンドボックスに送信されるので、サンドボックスヘッダーはありません。

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

### 設定要求の処理

設定エンドポイントは、新しいIMS組織の適切なインデックス、インデクサー、およびデータソース接続を設定するために使用できます。 設定要求を処理するには、次の2つのプロパティが必要です。 `databaseName` と `containerName`

`databaseName` 設定する組織のプロファイルデータベースの名前を表します。

`containerName` は、設定時に読み取られるコンテナの名前で、data connectorによって設定されるデータの名前を表します。 POSTリクエストでは、次の2 `containerName`つの値を使用できます。一方または両方を使用できます。
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
>インデックス自体は共有リソースなので、削除されません。

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

このガイドを読むと、リアルタイム顧客プロファイル検索の仕組みがわかります。 リアルタイム顧客プロファイルの詳細については、リアルタイム顧客 [プロファイルの概要を参照してください](../home.md)。 セグメント化について詳しくは、セグメント化の概要を参照 [してください](../../segmentation/home.md)。

## 付録 {#appendix}

### 検索パラメータ {#search-parameters}

次の表に、検索APIを使用する際の検索リストの動作に関する詳細を示します。

| 検索クエリ | 説明 |
|------------ | -----------|
| foo | 任意の単語を検索します。 「foo」という単語が検索可能なフィールドのいずれかで見つかった場合、結果が返されます。 |
| foo AND bar | ブール検索。 「foo」と「bar」の両方が **** 、検索可能なフィールドのいずれかで見つかった場合、結果が返されます。 |
| foo OR bar | ブール検索。 検索可能なフィールドの **いずれか** に「foo」という語または「bar」という語が見つかった場合、結果が返されます。 |
| foo NOTバー | ブール検索。 「foo」という単語が見つかったが、「bar」という単語が検索可能なフィールドのいずれにも見つからない場合、結果が返されます。 |
| 名前：foo AND bar | ブール検索。 「name」フィールドに「 **foo** 」と「bar」の両方が見つかった場合、結果が返されます。 |
| run* | ワイルドカード検索。 アスタリスク(*)を使用すると、0文字以上に一致します。つまり、検索可能なフィールドのコンテンツに「run」という開始を含む単語が含まれている場合、結果が返されます。 例えば、「runs」、「running」、「runner」または「runt」という単語が表示された場合、結果が返されます。 |
| カム？ | ワイルドカード検索。 疑問符(?)は1文字のみに一致します。つまり、検索可能なフィールドの内容が「cam」と追加の開始を含む場合、結果が返されます。 例えば、「camp」または「cams」という単語が表示される場合は結果を返し、「camera」または「campfire」という単語が表示される場合は結果を返しません。 |
| 「青傘」 | フレーズ検索。 検索可能なフィールドのコンテンツに「青い傘」という完全なフレーズが含まれている場合、結果が返されます。 |
| blue\～ | あいまい検索。 必要に応じて、チルダ(～)の後に0 ～ 2の数値を入力し、編集距離を指定できます。 例えば、「blue\～1」は、「blue」、「blues」または「glue」を返します。 あいまい検索は用語に **のみ適用** でき、フレーズには適用できません。 ただし、フレーズ内の各単語の末尾にチルダを追加することはできます。 例えば、「camping\～ in\～ the\～ summer\～」は「camping in the summer」と一致します。 |
| &quot;ホテル空港&quot;\～5 | 近接検索。 このタイプの検索は、ドキュメント内で互いに近い用語を検索するために使用されます。 例えば、このフレーズ `"hotel airport"~5` では、ドキュメント内の5語以内に「hotel」と「airport」という用語が見つかります。 |
| `/a[0-9]+b$/` | 通常の式検索。 このタイプの検索では、RegExpクラスで説明されているように、スラッシュ「/」の間の内容に基づいて一致を見つけます。 例えば、「motel」または「hotel」を含むドキュメントを検索するには、を指定しま `/[mh]otel/`す。 正規式検索は、単一の単語に対して一致します。 |

クエリ構文に関する詳細なドキュメントについては、 [Luceneクエリ構文ドキュメントをお読みください](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)。

---
title: セグメント検索 API エンドポイント
description: Adobe Experience Platform Segmentation Service API では、セグメント検索を使用して、様々なデータソースに含まれるフィールドを検索し、ほぼリアルタイムで返します。 このガイドでは、セグメント検索をより深く理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しが含まれています。
role: Developer
exl-id: bcafbed7-e4ae-49c0-a8ba-7845d8ad663b
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 45%

---

# セグメント検索エンドポイント

セグメント検索は、様々なデータソースに含まれるフィールドを検索し、ほぼリアルタイムで返すために使用されます。

このガイドでは、セグメント検索をより深く理解するのに役立つ情報を提供し、API を使用して基本的なアクションを実行するためのサンプル API 呼び出しが含まれています。

## はじめに

このガイドで使用するエンドポイントは、[!DNL Adobe Experience Platform Segmentation Service] API の一部です。 続行する前に、[&#x200B; はじめる前に &#x200B;](./getting-started.md) を参照して、必要なヘッダーやサンプル API 呼び出しの読み取り方法など、API の呼び出しを正常に実行するために必要な重要な情報を確認してください。

「はじめに」の節で説明している必要なヘッダーに加えて、セグメント検索エンドポイントへのすべてのリクエストには、次の追加ヘッダーが必要です。

- x-ups-search-version: &quot;1.0&quot;

### 複数の名前空間での検索

この検索エンドポイントを使用すると、様々な名前空間を検索して、検索数の結果のリストを返すことができます。 複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

**API 形式**

```http
GET /search/namespaces?schema.name={SCHEMA}
GET /search/namespaces?schema.name={SCHEMA}&s={SEARCH_TERM}
```

| パラメーター | 説明 |
| ---------- | ----------- | 
| `schema.name={SCHEMA}` | **（必須）** ここで、{SCHEMA} は検索オブジェクトに関連付けられたスキーマクラス値を表します。 現在は、`_xdm.context.segmentdefinition` のみがサポートされています。 |
| `s={SEARCH_TERM}` | *（任意）*{SEARCH_TERM} は、Microsoftの [Lucene の検索構文 &#x200B;](https://docs.microsoft.com/ja-JP/azure/search/query-lucene-syntax) 実装に準拠するクエリを表します。 検索語句が指定されていない場合、`schema.name` に関連付けられているすべてのレコードが返されます。詳しい説明は、このドキュメントの [&#x200B; 付録 &#x200B;](#appendix) に記載されています。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search/namespaces?schema.name=_xdm.context.segmentdefinition \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-ups-search-version: 1.0' 
```

**応答**

正常な応答は、HTTP ステータス 200 と次の情報を返します。

```json
{
  "namespaces": [
    {
      "namespace": "AAMTraits",
      "displayName": "AAMTraits",
      "count": 45
    },
    {
      "namespace": "AAMSegments",
      "displayName": "AAMSegment",
      "count": 10
    },
    {
      "namespace": "SegmentsAISegments",
      "displayName": "SegmentSAISegment",
      "count": 3
    }
  ],
  "totalCount": 3,
  "status": {
    "message": "Success"
  }
}
```

### 個々のエンティティの検索

この検索エンドポイントを使用すると、指定した名前空間内のすべてのフルテキストインデックス付きオブジェクトのリストを取得できます。 複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

**API 形式**

```http
GET /search/entities?schema.name={SCHEMA}&namespace={NAMESPACE}
GET /search/entities?schema.name={SCHEMA}&namespace={NAMESPACE}&s={SEARCH_TERM}
GET /search/entities?schema.name={SCHEMA}&namespace={NAMESPACE}&entityId={ENTITY_ID}
```

| パラメーター | 説明 |
| ---------- | ----------- | 
| `schema.name={SCHEMA}` | **（必須）** ここで、{SCHEMA} には検索オブジェクトに関連付けられたスキーマクラス値が含まれます。 現在は、`_xdm.context.segmentdefinition` のみがサポートされています。 |
| `namespace={NAMESPACE}` | **（必須）** 検索 {NAMESPACE} る名前空間が含まれています。 |
| `s={SEARCH_TERM}` | *（任意）* Microsoftの {SEARCH_TERM}Lucene の検索構文 [&#x200B; 実装に準拠するクエリが &#x200B;](https://docs.microsoft.com/ja-JP/azure/search/query-lucene-syntax) に含まれます。 検索語句が指定されていない場合、`schema.name` に関連付けられているすべてのレコードが返されます。詳しい説明は、このドキュメントの [&#x200B; 付録 &#x200B;](#appendix) に記載されています。 |
| `entityId={ENTITY_ID}` | *（オプション）* 検索を、{ENTITY_ID} で指定した指定フォルダー内に制限します。 |
| `limit={LIMIT}` | *（オプション）*{LIMIT} は、返される検索結果の数を表します。 デフォルト値は 50 です。 |
| `page={PAGE}` | *（任意）*{PAGE} は、検索したクエリの結果をページ分割するために使用されるページ番号を表します。 ページ番号は **0** から始まることに注意してください。 |


**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search/entities?schema.name=_xdm.context.segmentdefinition&namespace=AAMSegments \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-ups-search-version: 1.0' 
```

**応答**

応答に成功すると、HTTP ステータス 200 と、検索クエリに一致する結果が返されます。

```json
{
  "entities": [
    {
       "id": "1012667",
       "base64EncodedSourceId": "RFVGamdydHpEdy01ZTE1ZGJlZGE4YjAxMzE4YWExZWY1MzM1",
       "sourceId": "DUFjgrtzDw-5e15dbeda8b01318aa1ef533",
       "isFolder": true,
       "parentFolderId": "974139",
       "name": "aam-47995 verification (100)"
    },
    {
       "id": "14653311",
       "base64EncodedSourceId": "REVGamduLVgzdy01ZTE2ZjRhNjc1ZDZhMDE4YThhZDM3NmY1",
       "sourceId": "DEFjgn-X3w-5e16f4a675d6a018a8ad376f",
       "isFolder": false,
       "parentFolderId": "324050",
       "name": "AAM - Heavy equipment",
       "description": "AAM - Acme Equipment"
    }
 
 ],
  "page": {
    "totalCount": 2,
    "totalPages": 1,
    "pageOffset": 0,
    "pageSize": 10
  },
  "status": {
    "message": "Success"
  }
}
```

### 検索オブジェクトに関する構造情報を取得する

この検索エンドポイントを使用して、リクエストされた検索オブジェクトに関する構造情報を取得できます。

**API 形式**

```http
GET /search/taxonomy?schema.name={SCHEMA}&namespace={NAMESPACE}&entityId={ENTITY_ID}
```

| パラメーター | 説明 |
| ---------- | ----------- | 
| `schema.name={SCHEMA}` | **（必須）** ここで、{SCHEMA} には検索オブジェクトに関連付けられたスキーマクラス値が含まれます。 現在は、`_xdm.context.segmentdefinition` のみがサポートされています。 |
| `namespace={NAMESPACE}` | **（必須）** 検索 {NAMESPACE} る名前空間が含まれています。 |
| `entityId={ENTITY_ID}` | **（必須）** 構造情報を取得する検索オブジェクトの ID。{ENTITY_ID} で指定します。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search/taxonomy?schema.name=_xdm.context.segmentdefinition&namespace=AAMSegments&entityId=porsche11037 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-ups-search-version: 1.0' 
```

**応答**

リクエストされた検索オブジェクトに関する詳細な構造情報とともに、応答が成功すると、HTTP ステータス 200 が返されます。

```json
{
    "taxonomy": [
        {
            "id": "0",
            "base64EncodedSourceId": "RFVGZ01BLTVlNjgzMGZjMzk3YjQ1MThhYWExYTA4Zg2",
            "name": "AAMTraits for Cars",
            "parentFolderId": "root"
        },
        {
            "id": "150561",
            "base64EncodedSourceId": "RFVGamdpRk1BZy01ZTY4MzBmYzM5N2I0NTE4YWFhMWEwOGY1",
            "name": "Fast Cars",
            "parentFolderId": "carTraits"
        },
        {
            "id": "porsche11037",
            "base64EncodedSourceId": "REFGZ01CLTVlNjczMGZjMzk3YjQ1MThhZGIxYTA4Zg==",
            "name": "Porsche",
            "parentFolderId": "redCarsFolderId"
        }
    ],
    "status": {
        "message": "Success"
    }
}
```

## 次の手順

このガイドを読むことで、セグメント検索の仕組みについて理解が深まりました。

## 付録 {#appendix}

次の節では、検索用語の仕組みについて詳しく説明します。 検索クエリは、次のように記述されます：`s={FieldName}:{SearchExpression}`。 例えば、AAMまたは [!DNL Platform] という名前のセグメント定義を検索するには、検索クエリ `s=segmentName:AAM%20OR%20Platform` を使用します。

>[!NOTE]
>
>ベストプラクティスとして、検索式は、上記の例のようにHTML エンコードする必要があります。

### 検索フィールド {#search-fields}

次の表に、検索クエリパラメーター内で検索できるフィールドを示します。

| フィールド名 | 説明 |
| ---------- | ----------- |
| folderId | 指定した検索のフォルダー ID を持つ 1 つ以上のフォルダー。 |
| folderLocation | 指定した検索のフォルダーの場所を持つ 1 つまたは複数の場所。 |
| parentFolderId | 指定した検索の親フォルダー ID を持つセグメント定義またはフォルダー。 |
| segmentId | 指定した検索のセグメント ID に一致するセグメント定義。 |
| segmentName | 指定した検索のセグメント名に一致するセグメント定義。 |
| segmentDescription | 指定した検索のセグメント説明に一致するセグメント定義。 |

### 検索式 {#search-expression}

次の表に、Segment Search API を使用する場合の検索クエリの仕組みを示します。

>[!NOTE]
>
>次の例は、よりわかりやすくするために、HTML以外でエンコードされた形式で表示されています。 ベストプラクティスとして、HTMLが検索式をエンコードします。

| 検索式の例 | 説明 |
| ------------------------- | ----------- |
| foo | 任意の単語を検索します。検索可能なフィールドのいずれかで「foo」という単語が見つかった場合、結果が返されます。 |
| foo AND bar | ブール値検索。検索可能なフィールドのいずれかで「foo」という単語と「bar」という単語の&#x200B;**両方**&#x200B;が見つかった場合、結果が返されます。 |
| foo OR bar | ブール値検索。検索可能なフィールドのいずれかで「foo」という単語と「bar」という単語の&#x200B;**いずれか**&#x200B;が見つかった場合、結果が返されます。 |
| foo NOT bar | ブール値検索。「foo」という単語が見つかり、「bar」という単語が検索可能なフィールドのいずれにも見つからない場合、結果が返されます。 |
| name: foo AND bar | ブール値検索。「name」フィールドに「foo」と「bar」の&#x200B;**両方**&#x200B;が見つかった場合、結果が返されます。 |
| run* | ワイルドカード検索。アスタリスク（*）の使用は、0 文字以上と一致します。つまり、検索可能なフィールドのコンテンツに「run」で開始する単語が含まれている場合、結果が返されます。例えば、「runs」、「running」、「runner」または「runt」という単語が表示された場合、結果が返されます。 |
| cam? | ワイルドカード検索。疑問符（?）は 1 文字のみに一致します。つまり、検索可能なフィールドに、「cam」で開始し、追加の文字（1 文字）が含まれるコンテンツがある場合、結果が返されます。例えば、「camp」または「cams」という単語が表示される場合は結果を返し、「camera」または「campfire」という単語が表示される場合は結果を返しません。 |
| 「blue umbrella」 | フレーズ検索。検索可能なフィールドのコンテンツに「blue umbrella」という完全なフレーズが含まれている場合、結果が返されます。 |
| blue\~ | あいまい検索。必要に応じて、チルダ（～）の後に 0 ～ 2 の数値を入力し、編集距離を指定できます。例えば、「blue\～1」は、「blue」、「blues」または「glue」を返します。あいまい検索は用語に **のみ**&#x200B;適用でき、フレーズには適用されません。ただし、フレーズ内の各単語の末尾にチルダを追加することはできます。例えば、「camping\～ in\～ the\～ summer\～」は「camping in the summer」と一致します。 |
| &quot;hotel airport&quot;\~5 | 近接検索。このタイプの検索は、ドキュメント内で互いに近接している単語を検索するために使用されます。例えば、フレーズ `"hotel airport"~5` では、ドキュメント内で互いに 5 語以内に近接する「hotel」と「airport」という単語を見つけます。 |
| `/a[0-9]+b$/` | 正規表現検索。このタイプの検索では、RegExp クラスで説明されているように、スラッシュ「/」の間の内容に基づいて一致を見つけます。例えば、「motel」または「hotel」を含むドキュメントを検索するには、`/[mh]otel/` と指定します。正規表現検索は、単一の単語に対して照合されます。 |

クエリ構文に関する詳細なドキュメントについては、[Lucene クエリ構文のドキュメント](https://docs.microsoft.com/ja-JP/azure/search/query-lucene-syntax)をお読みください。

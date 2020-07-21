---
keywords: Experience Platform;segmentation;segmentation service;troubleshooting;API;seg;
solution: Adobe Experience Platform
title: セグメント検索エンドポイント
topic: guide
translation-type: tm+mt
source-git-commit: 995fadef9abacf22d0561e0590dfbe172adf0a43
workflow-type: tm+mt
source-wordcount: '1138'
ht-degree: 2%

---


# セグメント検索エンドポイント

セグメント検索は、様々なデータソースに含まれるフィールドを検索し、ほぼリアルタイムで返す場合に使用します。

このガイドは、セグメント検索をより深く理解するのに役立つ情報を提供し、APIを使用して基本的なアクションを実行するためのサンプルAPI呼び出しを含みます。

## はじめに

このガイドで使用されるエンドポイントは、 [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。 先に進む前に、 [入門ガイドを参照して](./getting-started.md) 、必要なヘッダーやAPI呼び出し例を読む方法など、APIを正しく呼び出すために必要な重要な情報を確認してください。

「はじめに」の節で説明されている必須ヘッダーに加えて、セグメント検索エンドポイントへのすべてのリクエストには、次の追加ヘッダーが必要です。

- x-ups-search-version: &quot;1.0&quot;

### 複数の名前空間に対する検索

この検索エンドポイントは、様々な名前空間を対象に検索を行い、検索数の結果のリストを返すのに使用できます。 複数のパラメーターを使用でき、アンパサンド(&amp;)で区切ります。

**API形式**

```http
GET /search/namespaces?schema.name={SCHEMA}
GET /search/namespaces?schema.name={SCHEMA}&s={SEARCH_TERM}
```

| パラメーター | 説明 |
| ---------- | ----------- | 
| `schema.name={SCHEMA}` | **（必須）** {スキーマ}は、検索オブジェクトに関連付けられたスキーマクラス値を表します。 現在、のみがサポートさ `_xdm.context.segmentdefinition` れています。 |
| `s={SEARCH_TERM}` | *（オプション）* {SEARCH_TERM}は、Microsoftによる [Luceneの検索構文の実装に準拠するクエリを表します](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)。 検索語句を指定しない場合、に関連付けられているすべてのレコード `schema.name` が返されます。 詳しくは、このドキュメントの [付録](#appendix) 「」を参照してください。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search/namespaces?schema.name=_xdm.context.segmentdefinition \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-ups-search-version: 1.0' 
```

**応答**

応答が成功すると、HTTPステータス200が次の情報と共に返されます。

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

この検索エンドポイントは、指定した名前空間内にあるすべてのフルテキストインデックス付きオブジェクトのリストを取得するために使用できます。 複数のパラメーターを使用でき、アンパサンド(&amp;)で区切ります。

**API形式**

```http
GET /search/entities?schema.name={SCHEMA}&namespace={NAMESPACE}
GET /search/entities?schema.name={SCHEMA}&namespace={NAMESPACE}&s={SEARCH_TERM}
GET /search/entities?schema.name={SCHEMA}&namespace={NAMESPACE}&entityId={ENTITY_ID}
```

| パラメーター | 説明 |
| ---------- | ----------- | 
| `schema.name={SCHEMA}` | **（必須）** {スキーマ}には、検索オブジェクトに関連付けられたスキーマクラス値が含まれます。 現在、のみがサポートさ `_xdm.context.segmentdefinition` れています。 |
| `namespace={NAMESPACE}` | **（必須）** {名前空間}には、検索対象の名前空間が含まれています。 |
| `s={SEARCH_TERM}` | *（オプション）* {SEARCH_TERM}には、Microsoftによる [Luceneの検索構文の実装に準拠するクエリが含まれています](https://docs.microsoft.com/en-us/azure/search/query-lucene-syntax)。 検索語句を指定しない場合、に関連付けられているすべてのレコード `schema.name` が返されます。 詳しくは、このドキュメントの [付録](#appendix) 「」を参照してください。 |
| `entityId={ENTITY_ID}` | *（オプション）* {ENTITY_ID}で指定した指定フォルダー内での検索を制限します。 |
| `limit={LIMIT}` | *（オプション）* {LIMIT}は、返す検索結果の数を表します。 デフォルト値は 50 です。 |
| `page={PAGE}` | *（オプション）* {PAGE}は、検索したクエリのページ番号の結果を示すページ番号です。 ページ番号の開始は **0にあることに注意してください**。 |


**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search/entities?schema.name=_xdm.context.segmentdefinition&namespace=AAMSegments \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-ups-search-version: 1.0' 
```

**応答**

応答が成功すると、検索クエリに一致する結果のHTTPステータス200が返されます。

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

### 検索オブジェクトの構造情報の取得

この検索エンドポイントは、要求された検索オブジェクトに関する構造情報を取得するために使用できます。

**API形式**

```http
GET /search/taxonomy?schema.name={SCHEMA}&namespace={NAMESPACE}&entityId={ENTITY_ID}
```

| パラメーター | 説明 |
| ---------- | ----------- | 
| `schema.name={SCHEMA}` | **（必須）** {スキーマ}には、検索オブジェクトに関連付けられたスキーマクラス値が含まれます。 現在、のみがサポートさ `_xdm.context.segmentdefinition` れています。 |
| `namespace={NAMESPACE}` | **（必須）** {名前空間}には、検索対象の名前空間が含まれています。 |
| `entityId={ENTITY_ID}` | **（必須）** {ENTITY_ID}で指定された、構造情報を取得する検索オブジェクトのID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/core/ups/search/taxonomy?schema.name=_xdm.context.segmentdefinition&namespace=AAMSegments&entityId=porsche11037 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'Content-Type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'x-ups-search-version: 1.0' 
```

**応答**

応答が成功すると、HTTPステータス200が返され、要求された検索オブジェクトに関する詳細な構造情報が返されます。

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

このガイドを読むと、セグメント検索のしくみをより深く理解できます。

## 付録 {#appendix}

以下の節では、検索用語の仕組みについて詳しく説明します。 検索クエリは、次の方法で書き込まれます。 `s={FieldName}:{SearchExpression}`. したがって、AAMという名前のセグメントを検索する場合や、次の検索クエリ [!DNL Platform]を使用する場合などが考えられます。 `s=segmentName:AAM%20OR%20Platform`.

> !![NOTE] ベストプラクティスとして、上の例のように、検索式はHTMLエンコードする必要があります。

### 検索フィールド {#search-fields}

次の表に、検索クエリパラメータ内で検索できるフィールドを示します。

| フィールド名 | 説明 |
| ---------- | ----------- |
| folderId | 指定した検索のフォルダIDを持つフォルダ。 |
| folderLocation | 指定した検索のフォルダーの場所を持つ場所。 |
| parentFolderId | 指定した検索の親フォルダーIDを持つセグメントまたはフォルダー。 |
| segmentId | セグメントは、指定した検索のセグメントIDと一致します。 |
| segmentName | このセグメントは、指定した検索のセグメント名と一致します。 |
| segmentDescription | このセグメントは、指定した検索のセグメントの説明と一致します。 |

### 検索式 {#search-expression}

次の表に、セグメント検索APIを使用する場合の検索クエリの機能に関する詳細リストを示します。

>!![NOTE] 次の例は、HTML以外のエンコード形式で表示され、わかりやすくなっています。 ベストプラクティスとして、検索式をHTMLエンコードしてください。

| 検索式の例 | 説明 |
| ------------------------- | ----------- |
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

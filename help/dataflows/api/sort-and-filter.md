---
title: フローサービス API での応答の並べ替えとフィルタリング
description: このチュートリアルでは、フローサービス API のクエリパラメーターを使用した並べ替えとフィルタリングの構文について説明します。これには、高度な使用例が含まれます。
source-git-commit: ccca81357bd7d7083abd3f395aa547c85ebf65fd
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 7%

---

# フローサービス API での応答の並べ替えとフィルタリング

[ フローサービス API](https://www.adobe.io/experience-platform-apis/references/flow-service/) でリスト (GET) リクエストを実行する場合、クエリパラメーターを使用して応答を並べ替えたり、フィルターしたりできます。 このガイドでは、様々な使用例でこれらのパラメーターを使用する方法について説明します。

## 並べ替え

`orderby` クエリパラメーターを使用して、回答を並べ替えることができます。 API では、次のリソースを並べ替えることができます。

* [接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Connections)
* [ソース接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Source-connections)
* [ターゲット接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Target-connections)
* [流れ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Flows)
* [実行](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Runs)

パラメーターを使用するには、並べ替えの基準とする特定のプロパティに値を設定する必要があります（例：`?orderby=name`）。 値の前に、昇順の場合はプラス記号 (`+`)、降順の場合はマイナス記号 (`-`) を付けることができます。 並べ替えプレフィックスが指定されない場合、リストはデフォルトで昇順で並べ替えられます。

```http
GET /flows?orderby=name
GET /flows?orderby=-name
```

並べ替えパラメーターとフィルタリングパラメーターを組み合わせるには、「and」記号 (`&`) を使用します。

```http
GET /flows?property=state==enabled&orderby=createdAt
```

## フィルタリング

キーと値の式で `property` パラメーターを使用して、応答をフィルタリングできます。 例えば、`?property=id==12345` は、`id` プロパティが `12345` と完全に等しいリソースのみを返します。

フィルタリングは、そのプロパティの有効なパスがわかっている限り、エンティティ内の任意のプロパティに一般的に適用できます。

>[!NOTE]
>
>プロパティが配列項目内にネストされている場合は、パス内の配列に角括弧 (`[]`) を追加する必要があります。 例については、[ 配列のプロパティ ](#arrays) のフィルタリングの節を参照してください。

**ソース・テーブル名が次の場合に、すべてのソース接続を返し `lead`ます：**

```http
GET /sourceConnections?property=params.tableName==lead
```

**特定のセグメント ID のすべてのフローを返す：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a
```

### フィルターの組み合わせ

クエリで「および」文字 (`&`) で区切られている場合は、1 つのクエリに複数の `property` フィルターを含めることができます。 フィルターを組み合わせる場合は、AND 関係が想定されます。つまり、エンティティが応答に含めるには、すべてのフィルターを満たす必要があります。

**セグメント ID の有効なすべてのフローを返す：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a&property=state==enabled
```

### 配列プロパティのフィルタリング {#arrays}

配列プロパティの名前に `[]` を付けると、配列内の項目のプロパティに基づいてフィルタリングできます。

**特定のソース接続に関連付けられたフローを返す：**

```http
GET /flows?property=sourceConnectionIds[]==9874984,6980696
```

**特定のセレクター値 ID を含む変換を含む戻りフロー。**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a
```

**特定の値を持つ列を持つソース接続を返 `name` します。**

```http
GET /sourceConnections?property=params.columns[].name==firstName
```

**セグメント ID でフィルタリングして、宛先のフロー実行 ID を検索します。**

```http
GET /runs?property=metrics.recordSummary.targetSummaries[].entitySummaries[].id==segment:068d6e2c-b546-4c73-bfb7-9a9d33375659
```

### `count`

フィルタリングクエリに `count` クエリパラメーターを付け、値 `true` を付けて結果の数を返すことができます。 API 応答には `count` プロパティが含まれ、その値はフィルターされた項目の合計数を表します。 この呼び出しでは、実際にフィルターされた項目は返されません。

**システム内の有効なフローの数を返す：**

```http
GET /flows?property=state==enabled&count=true
```

上記のクエリへの応答は、次のようになります。

```json
{
  "count": 95
}
```

### リソース別のフィルタリング可能なプロパティ

取得するフローサービスエンティティに応じて、様々なプロパティを使用してフィルタリングできます。 以下の表では、フィルタリングの使用例で一般的に使用される各リソースのルートレベルのフィールドを分類します。

**`connectionSpec`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/connectionSpecs?property=id==736873,9485095` |
| `name` | `/connectionSpecs?property=name==TestConn` |
| `providerId` | `/connectionSpecs?property=providerId==3897933` |
| `attributes.{ATTRIBUTE_NAME}` | `/connectionSpecs?property=attributes.sampleAttribute="abc"` |

{style=&quot;table-layout:auto&quot;}

**`flowSpec`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/flowSpecs?property=id==736873,9485095` |
| `name` | `/flowSpecs?property=name==TestConn` |
| `providerId` | `/flowSpecs?property=providerId==3897933` |

{style=&quot;table-layout:auto&quot;}

**`connection`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/connections?property=id==736873,9485095` |
| `name` | `/connections?property=name==TestConn` |
| `description` | `/connections?property=description==Test%20description` |
| `connectionSpec.id` | `/connections?property=connectionSpec.id==938903,849048` |
| `state` | `/connections?property=state==enabled` |

{style=&quot;table-layout:auto&quot;}

**`sourceConnection`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/sourceConnections?property=id==736873,9485095` |
| `connectionSpec.id` | `/sourceConnections?property=connectionSpec.id==938903,849048` |
| `baseConnectionId` | `/sourceConnections?property=baseConnectionId==983908,4908095` |

{style=&quot;table-layout:auto&quot;}

**`targetConnection`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/targetConnections?property=id==736873,9485095` |
| `connectionSpec.id` | `/targetConnections?property=connectionSpec.id==938903,849048` |
| `baseConnectionId` | `/targetConnections?property=baseConnectionId==983908,4908095` |

{style=&quot;table-layout:auto&quot;}

**`flow`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/flows?property=id==736873,9485095` |
| `name` | `/flows?property=name==TestFlow` |
| `description` | `/flows?property=description==Test%20description` |
| `flowSpec.id` | `/flows?property=flowSpec.id==938903,849048` |
| `state` | `/flows?property=state==enabled` |
| `sourceConnectionIds` | `/flows?property=sourceConnectionIds[]==9874984,6980696` |
| `targetConnectionIds` | `/flows?property=targetConnectionIds[]==598590,690666` |

{style=&quot;table-layout:auto&quot;}

**`run`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/runs?property=id==736873,9485095` |
| `flowId` | `/runs?property=flowId==8749844` |
| `state` | `/runs?property=state==inProgress` |

{style=&quot;table-layout:auto&quot;}

## 次の手順

このガイドでは、`orderby` および `property` クエリパラメーターを使用して、フローサービス API で応答を並べ替えたり、フィルタリングしたりする方法について説明しました。 Platform の一般的なワークフローに API を使用する手順のガイドについては、[sources](../../sources/home.md) および [destinations](../../destinations/home.md) のドキュメントに含まれる API チュートリアルを参照してください。

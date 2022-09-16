---
title: フローサービス API での応答の並べ替えとフィルタリング
description: このチュートリアルでは、フローサービス API のクエリパラメーターを使用した並べ替えとフィルタリングの構文について説明します。高度な使用例もいくつかあります。
exl-id: 029c3199-946e-4f89-ba7a-dac50cc40c09
source-git-commit: ef8db14b1eb7ea555135ac621a6c155ef920e89a
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 7%

---

# フローサービス API での応答の並べ替えとフィルタリング

リスト (GET) リクエストを [フローサービス API](https://www.adobe.io/experience-platform-apis/references/flow-service/)に値を入力する場合は、クエリーパラメーターを使用して応答を並べ替えたり、フィルター処理したりできます。 このガイドでは、様々な使用例でこれらのパラメーターを使用する方法について説明します。

## 並べ替え

回答は、 `orderby` クエリパラメーター。 API では、次のリソースを並べ替えることができます。

* [接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Connections)
* [ソース接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Source-connections)
* [ターゲット接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Target-connections)
* [流れ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Flows)
* [実行](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Runs)

パラメーターを使用するには、並べ替えの基準となる特定のプロパティにその値を設定する必要があります ( 例： `?orderby=name`) をクリックします。 値の前にプラス記号 (`+`) を使用します。`-`) を降順に表示します。 並べ替えプレフィックスが指定されない場合、リストはデフォルトで昇順で並べ替えられます。

```http
GET /flows?orderby=name
GET /flows?orderby=-name
```

並べ替えパラメーターとフィルタリングパラメーターを組み合わせるには、「and」記号 (`&`) をクリックします。

```http
GET /flows?property=state==enabled&orderby=createdAt
```

## フィルター

回答は、 `property` パラメーターにキーと値の式を含める必要があります。 例： `?property=id==12345` が返すリソースのみが `id` プロパティが完全に等しい `12345`.

フィルタリングは、そのプロパティの有効なパスがわかっている限り、エンティティ内の任意のプロパティに一般的に適用できます。

>[!NOTE]
>
>プロパティが配列項目内にネストされている場合は、角括弧 (`[]`) をパス内の配列に追加します。 詳しくは、 [配列プロパティのフィルタリング](#arrays) 例：

**ソース・テーブル名が次の値であるすべてのソース接続を返す `lead`:**

```http
GET /sourceConnections?property=params.tableName==lead
```

**特定のセグメント ID のすべてのフローを返す：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a
```

### フィルターの組み合わせ

複数 `property` フィルターは、「および」文字 (`&`) をクリックします。 フィルターを組み合わせる際には AND 関係が想定されます。つまり、応答に含めるには、エンティティがすべてのフィルターを満たす必要があります。

**セグメント ID の有効なフローをすべて返す：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a&property=state==enabled
```

### 配列プロパティのフィルタリング {#arrays}

配列内の項目のプロパティに基づいてフィルタリングするには、次を追加します。 `[]` を配列プロパティの名前に追加します。

**特定のソース接続に関連付けられたフローを返す：**

```http
GET /flows?property=sourceConnectionIds[]==9874984,6980696
```

**特定のセレクター値 ID を含む変換を持つ戻りフローを返します。**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a
```

**特定の `name` 値：**

```http
GET /sourceConnections?property=params.columns[].name==firstName
```

**セグメント ID でフィルタリングして、宛先のフロー実行 ID を検索します。**

```http
GET /runs?property=metrics.recordSummary.targetSummaries[].entitySummaries[].id==segment:068d6e2c-b546-4c73-bfb7-9a9d33375659
```

### `count`

任意のフィルタークエリを `count` 値がのクエリパラメーター `true` 結果の数を返します。 API 応答には `count` プロパティの値は、フィルターされた項目の合計数を表します。 この呼び出しでは、実際にフィルターされた項目は返されません。

**システム内の有効なフローの数を返します。**

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

取得するフローサービスエンティティに応じて、様々なプロパティを使用してフィルタリングをおこなうことができます。 以下の表では、使用例のフィルタリングで一般的に使用される各リソースのルートレベルのフィールドを分類します。

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

このガイドでは、 `orderby` および `property` フローサービス API で応答を並べ替えたり、フィルタリングしたりするクエリパラメーター。 Platform の一般的なワークフローで API を使用する手順に関するガイドについては、 [ソース](../../sources/home.md) および [宛先](../../destinations/home.md) ドキュメント。

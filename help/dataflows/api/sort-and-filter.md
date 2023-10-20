---
title: フローサービス API での応答の並べ替えとフィルタリング
description: このチュートリアルでは、フローサービス API のクエリパラメーターを使用した並べ替えとフィルタリングの構文について説明します。高度な使用例もいくつかあります。
exl-id: 029c3199-946e-4f89-ba7a-dac50cc40c09
source-git-commit: c7ff379b260edeef03f8b47f932ce9040eef3be2
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 3%

---

# フローサービス API での応答の並べ替えとフィルタリング

リスト (GET) リクエストを [フローサービス API](https://www.adobe.io/experience-platform-apis/references/flow-service/)に値を入力する場合は、クエリーパラメーターを使用して応答を並べ替えたり、フィルター処理したりできます。 このガイドでは、様々な使用例でこれらのパラメーターを使用する方法について説明します。

## 並べ替え

回答を並べ替えるには、 `orderby` クエリパラメーター。 API では、次のリソースを並べ替えることができます。

* [接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Connections)
* [ソース接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Source-connections)
* [ターゲット接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Target-connections)
* [流れ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Flows)
* [実行](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Runs)

パラメーターを使用するには、並べ替えの基準となる特定のプロパティに値を設定する必要があります ( 例： `?orderby=name`) をクリックします。 値の前にプラス記号 (`+`) を使用します。`-`) を降順に並べ替えます。 並べ替えプレフィックスが指定されない場合、リストはデフォルトで昇順で並べ替えられます。

```http
GET /flows?orderby=name
GET /flows?orderby=-name
```

並べ替えパラメーターとフィルタリングパラメーターを組み合わせるには、「and」記号 (`&`) をクリックします。

```http
GET /flows?property=state==enabled&orderby=createdAt
```

## フィルター

回答は、 `property` パラメーターにキーと値の式を含める必要があります。 例： `?property=id==12345` が返すリソースのみが `id` プロパティが次と完全に等しい `12345`.

フィルタリングは、そのプロパティの有効なパスがわかっている限り、エンティティ内の任意のプロパティに一般的に適用できます。

>[!NOTE]
>
>プロパティが配列項目内にネストされている場合は、角括弧 (`[]`) をパス内の配列に追加します。 詳しくは、 [配列プロパティのフィルタリング](#arrays) 例：

**ソース・テーブル名が次の値であるすべてのソース接続を返します。 `lead`:**

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

配列内の項目のプロパティに基づいてフィルタリングするには、次の項目を追加します。 `[]` を配列プロパティの名前に追加します。

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

任意のフィルタークエリを `count` の値を持つクエリパラメーター `true` 結果の数を返します。 API 応答には、 `count` プロパティの値は、フィルターされた項目の合計数を表します。 この呼び出しでは、実際にフィルターされた項目は返されません。

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

{style="table-layout:auto"}

**`flowSpec`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/flowSpecs?property=id==736873,9485095` |
| `name` | `/flowSpecs?property=name==TestConn` |
| `providerId` | `/flowSpecs?property=providerId==3897933` |

{style="table-layout:auto"}

**`connection`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/connections?property=id==736873,9485095` |
| `name` | `/connections?property=name==TestConn` |
| `description` | `/connections?property=description==Test%20description` |
| `connectionSpec.id` | `/connections?property=connectionSpec.id==938903,849048` |
| `state` | `/connections?property=state==enabled` |

{style="table-layout:auto"}

**`sourceConnection`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/sourceConnections?property=id==736873,9485095` |
| `connectionSpec.id` | `/sourceConnections?property=connectionSpec.id==938903,849048` |
| `baseConnectionId` | `/sourceConnections?property=baseConnectionId==983908,4908095` |

{style="table-layout:auto"}

**`targetConnection`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/targetConnections?property=id==736873,9485095` |
| `connectionSpec.id` | `/targetConnections?property=connectionSpec.id==938903,849048` |
| `baseConnectionId` | `/targetConnections?property=baseConnectionId==983908,4908095` |

{style="table-layout:auto"}

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

{style="table-layout:auto"}

**`run`**

| プロパティ | 例 |
| --- | --- |
| `id` | `/runs?property=id==736873,9485095` |
| `flowId` | `/runs?property=flowId==8749844` |
| `state` | `/runs?property=state==inProgress` |

{style="table-layout:auto"}

## ユースケース {#use-cases}

この節では、フィルタリングと並べ替えを使用して、特定のコネクタに関する情報を返す方法や、問題のデバッグに役立つ方法の具体的な例を示します。 追加したい使用例が他にある場合は、次をAdobeしてください： **[!UICONTROL 詳細なフィードバックオプション]** をクリックして、リクエストを送信します。

**特定の宛先への接続のみを返すフィルター**

フィルターを使用して、特定の宛先への接続のみを返すことができます。 まず、 `connectionSpecs` 次のようなエンドポイント：

```http
GET /connectionSpecs
```

次に、目的のを検索します。 `connectionSpec` 調べて `name` パラメーター。 例えば、Amazon Ads、Pega、SFTP などを `name` パラメーター。 対応する `id` が `connectionSpec` 次の API 呼び出しでで検索できる

例えば、宛先をフィルタリングして、Amazon S3 接続への既存の接続のみを返します。

```http
GET /connections?property=connectionSpec.id==4890fc95-5a1f-4983-94bb-e060c08e3f81
```

**データフローを宛先にのみ返すためのフィルター**

クエリ時に `/flows` エンドポイントでは、すべてのソースと宛先のデータフローを返す代わりに、フィルターを使用して、宛先にデータフローのみを返すことができます。 これをおこなうには、 `isDestinationFlow` をクエリパラメーターとして次のように指定します。

```http
GET /flows?property=inheritedAttributes.properties.isDestinationFlow==true
```

**データフローを特定のソースまたは宛先にのみ返すためのフィルター**

データフローをフィルタリングして、特定の宛先にデータフローを返したり、特定のソースからのみデータフローを返したりできます。 例えば、宛先をフィルタリングして、Amazon S3 接続への既存の接続のみを返します。

```http
GET /flows?property=inheritedAttributes.targetConnections[].connectionSpec.id==4890fc95-5a1f-4983-94bb-e060c08e3f81
```

**特定の期間のデータフローのすべての実行を取得するフィルター**

データフローのデータフロー実行をフィルタリングして、次に示すように、特定の時間間隔での実行のみを確認できます。

```
GET /runs?property=flowId==<flow-id>&property=metrics.durationSummary.startedAtUTC>1593134665781&property=metrics.durationSummary.startedAtUTC<1653134665781
```

**失敗したデータフローのみを返すフィルター**

デバッグの目的で、次のように、特定のソースまたは宛先のデータフローに対して失敗したデータフローの実行をすべてフィルタリングして表示できます。

```http
GET /runs?property=flowId==<flow-id>&property=metrics.statusSummary.status==Failed
```

## 次の手順

このガイドでは、 `orderby` および `property` フローサービス API で応答を並べ替えたり、フィルタリングしたりするクエリパラメーター。 Platform の一般的なワークフローで API を使用する手順に関するガイドについては、 [ソース](../../sources/home.md) および [宛先](../../destinations/home.md) ドキュメント。

---
title: Flow Service API での応答の並べ替えとフィルタリング
description: このチュートリアルでは、Flow Service API のクエリパラメーターを使用した並べ替えとフィルタリングの構文について、いくつかの高度なユースケースを含めて説明します。
exl-id: 029c3199-946e-4f89-ba7a-dac50cc40c09
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 2%

---

# Flow Service API での応答の並べ替えとフィルタリング

[Flow Service API](https://www.adobe.io/experience-platform-apis/references/flow-service/) でリスト（GET）リクエストを実行する際に、クエリパラメーターを使用して応答を並べ替えたりフィルタリングしたりできます。 このガイドでは、様々なユースケースでこれらのパラメーターを使用する方法のリファレンスを提供します。

## 並べ替え

`orderby` クエリパラメーターを使用して、応答を並べ替えることができます。 次のリソースは、API で並べ替えることができます。

* [接続](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Connections)
* [Source接続 ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Source-connections)
* [ ターゲット接続 ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Target-connections)
* [ フロー ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Flows)
* [ 実行中 ](https://www.adobe.io/experience-platform-apis/references/flow-service/#tag/Runs)

パラメーターを使用するには、その値を、並べ替えの基準にする特定のプロパティ（例：`?orderby=name`）に設定する必要があります。 値の前には、昇順の場合はプラス記号（`+`）を、降順の場合はマイナス記号（`-`）を付けることができます。 順序付けのプレフィックスが指定されていない場合、デフォルトではリストが昇順で並べ替えられます。

```http
GET /flows?orderby=name
GET /flows?orderby=-name
```

「and」記号（`&`）を使用して、並べ替えパラメーターをフィルタリングパラメーターと組み合わせることもできます。

```http
GET /flows?property=state==enabled&orderby=createdAt
```

## フィルタリング

キー値式を持つ `property` パラメーターを使用して、応答をフィルタリングできます。 例えば、`?property=id==12345` は、`id` プロパティが完全に `12345` に等しいリソースのみを返します。

フィルタリングは、エンティティ内の任意のプロパティに一般的に適用できます（そのプロパティへの有効なパスが分かっている場合）。

>[!NOTE]
>
>プロパティが配列項目内にネストされている場合は、パスの配列に角括弧（`[]`）を追加する必要があります。 例については、[ 配列プロパティのフィルタリング ](#arrays) の節を参照してください。

**ソーステーブル名が `lead` であるすべてのソース接続を返します。**

```http
GET /sourceConnections?property=params.tableName==lead
```

**特定のセグメント ID のすべてのフローを返す：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a
```

### フィルターの組み合わせ

「and」文字（`&`）で区切られている場合は、1 つのクエリに複数の `property` フィルターを含めることができます。 フィルターを組み合わせる場合、AND 関係が想定されます。つまり、エンティティを応答に含めるには、エンティティがすべてのフィルターを満たす必要があります。

**セグメント ID に対して有効なすべてのフローを返す：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a&property=state==enabled
```

### 配列プロパティのフィルタリング {#arrays}

配列プロパティの名前に `[]` を追加することで、配列内の項目のプロパティに基づいてフィルタリングできます。

**特定のソース接続に関連付けられたフローを返します：**

```http
GET /flows?property=sourceConnectionIds[]==9874984,6980696
```

**特定のセレクター値 ID を含む変換があるフローを返します：**

```http
GET /flows?property=transformations[].params.segmentSelectors.selectors[].value.id==5722a16f-5e1f-4732-91b6-3b03943f759a
```

**特定の `name` 値を持つ列を持つソース接続を返します。**

```http
GET /sourceConnections?property=params.columns[].name==firstName
```

**セグメント ID でフィルタリングして、宛先のフロー実行 ID を検索します。**

```http
GET /runs?property=metrics.recordSummary.targetSummaries[].entitySummaries[].id==segment:068d6e2c-b546-4c73-bfb7-9a9d33375659
```

### `count`

結果の数を返 `count` 値 `true` を使用して、フィルタリングクエリにクエリパラメーターを追加できます。 API 応答には、フィルターされた項目の合計数を値が表す `count` プロパティが含まれています。 この呼び出しでは、実際のフィルター済み項目は返されません。

**システム内の有効なフローの数を返す：**

```http
GET /flows?property=state==enabled&count=true
```

上記のクエリに対する応答は、次のようになります。

```json
{
  "count": 95
}
```

### リソース別のフィルタリング可能なプロパティ

取得するフローサービスエンティティに応じて、様々なプロパティをフィルタリングに使用できます。 次の表は、ユースケースのフィルタリングで一般的に使用される、各リソースのルートレベルフィールドを分類しています。

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

フィルタリングと並べ替えを使用して、特定のコネクタに関する情報を返したり、問題のデバッグを支援したりする方法について、具体的な例については、この節を参照してください。 Adobeで追加したい他のユースケースがある場合は、ページの **[!UICONTROL 詳細なフィードバックオプション]** を使用してリクエストを送信してください。

**特定の宛先への接続のみを返すフィルター**

フィルターを使用して、特定の宛先にのみ接続を返すことができます。 まず、次のように `connectionSpecs` エンドポイントをクエリします。

```http
GET /connectionSpecs
```

次に、`name` パラメーターを調べて、目的の `connectionSpec` を検索します。 例えば、`name` パラメーターでAmazon Ads、Pega、SFTP などを検索します。 対応する `id` は、次の API 呼び出しで検索できる `connectionSpec` です。

例えば、宛先をフィルタリングして、既存の接続のみをAmazon S3 接続に返します。

```http
GET /connections?property=connectionSpec.id==4890fc95-5a1f-4983-94bb-e060c08e3f81
```

**データフローを宛先のみに返すフィルター**

`/flows` エンドポイントに対してクエリを実行する場合、すべてのソースと宛先のデータフローを返す代わりに、フィルターを使用して、宛先へのデータフローのみを返すことができます。 これを行うには、以下のように、`isDestinationFlow` をクエリパラメーターとして使用します。

```http
GET /flows?property=inheritedAttributes.properties.isDestinationFlow==true
```

**データフローを特定のソースまたは宛先にのみ返すフィルター**

データフローをフィルタリングして、特定の宛先へのデータフローまたは特定のソースからのデータフローのみを返すことができます。 例えば、宛先をフィルタリングして、既存の接続のみをAmazon S3 接続に返します。

```http
GET /flows?property=inheritedAttributes.targetConnections[].connectionSpec.id==4890fc95-5a1f-4983-94bb-e060c08e3f81
```

**特定の期間のデータフローのすべての実行を取得するフィルター**

データフローのデータフロー実行をフィルタリングして、次のように特定の時間間隔の実行のみを表示できます。

```
GET /runs?property=flowId==<flow-id>&property=metrics.durationSummary.startedAtUTC>1593134665781&property=metrics.durationSummary.startedAtUTC<1653134665781
```

**失敗したデータフローのみを返すフィルター**

デバッグ目的で、次のように、特定のソースまたは宛先データフローに対して失敗したすべてのデータフロー実行をフィルタリングして確認できます。

```http
GET /runs?property=flowId==<flow-id>&property=metrics.statusSummary.status==Failed
```

## 次の手順

このガイドでは、`orderby` および `property` クエリパラメーターを使用して、Flow Service API で応答を並べ替えたりフィルタリングしたりする方法について説明しました。 Experience Platformの一般的なワークフローで API を使用する手順のガイドについては、[sources](../../sources/home.md) および [destinations](../../destinations/home.md) ドキュメントに含まれている API チュートリアルを参照してください。

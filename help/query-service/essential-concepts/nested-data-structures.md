---
keywords: Experience Platform;クエリサービス;クエリサービス;ネストされたデータ構造;ネストされたデータ;
title: クエリサービスでのネストされたデータ構造の使用
description: このドキュメントでは、CTAS 文と INSERT INTO 文を使用して、ネストされたデータフィールドを処理および変換する実際の例について説明します。
exl-id: 593379fb-88ad-4b14-8d2e-aa6d18129974
source-git-commit: d3ea7ee751962bb507c91e1afea0da35da60a66d
workflow-type: tm+mt
source-wordcount: '795'
ht-degree: 100%

---

# クエリサービスでのネストされたデータ構造の使用

Adobe Experience Platform クエリサービスでは、ネストされたデータフィールドの使用をサポートしています。エンタープライズデータ構造の複雑さにより、このデータの変換や処理が複雑になる場合があります。このドキュメントでは、ネストされたデータ構造を含む複雑なデータタイプを含むデータセットを作成、処理、変換する方法の例について説明します。

クエリサービスは、Experience Platform によって管理されるすべてのデータセットに対して SQL クエリを実行するための [!DNL PostgreSQL] インターフェイスを提供します。Platform では、構造体、配列、マップと、深くネストされた構造体、配列、マップなど、テーブル列でのプリミティブデータタイプまたは複合データタイプの使用をサポートしています。 データセットには、列のデータタイプがネストされた構造の配列と同じくらい複雑なネストされた構造や、キーと値のペアの値が複数レベルのネストを持つ構造になるマップのマップを含めることもできます。

## はじめに

このチュートリアルでは、サードパーティの PSQL クライアントまたはクエリエディターツールを使用して、Experience Platform ユーザーインターフェイス（UI）内でクエリの書き込み、検証、実行を行う必要があります。UI を使用してクエリを実行する方法について詳しくは、[クエリエディター UI ガイド](../ui/user-guide.md)を参照してください。クエリサービスに接続できるサードパーティのデスクトップクライアントの詳細なリストについては、[クライアント接続の概要](../clients/overview.md)を参照してください。

`INSERT INTO` と `CTAS` の構文についてもよく理解しておく必要があります。使用に関する具体的な情報については、[SQL 構文リファレンスドキュメント](../sql/syntax.md)の [`INSERT INTO`](../sql/syntax.md#insert-into) および [`CTAS`](../sql/syntax.md#create-table-as-select) の節を参照してください。

## データセットの作成

クエリサービスは、テーブルを選択として作成（`CTAS`）機能を提供し、`SELECT` 文の出力に基づいて、またはこの場合のように、Adobe Experience Platform の既存の XDM スキーマへの参照を使用してテーブルを作成します。 この例のために作成した `Final_subscription` の XDM スキーマを以下に示します。

![final_subscription スキーマの図。](../images/best-practices/final-subscription-schema.png)

次の例は、`final_subscription_test2` データセットの作成に使用する SQL を示しています。`final_subscription_test2` は、`Final_subscription` スキーマを使用して作成します。`SELECT` 句を使用してソースからデータを抽出し、いくつかの行に入力します。

```sql
CREATE TABLE final_subscription_test2 with(schema='Final_subscription') AS (
        SELECT struct(userid, collect_set(subscription) AS subscription) AS _lumaservices3 FROM(
            SELECT user AS userid,
                   struct( last(eventtime) AS last_eventtime,
                           last(status) AS last_status,
                           offer_id, 
                           subsid AS subscription_id)
                   AS subscription
             FROM (
                   SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , TIMESTAMP eventtime
 
                   FROM
                        xbox_subscription_event
                   UNION   
                   SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , TIMESTAMP eventtime
                   FROM
                        office365_subscription_event
             ) 
             GROUP BY user,subsid,offer_id
             ORDER BY user ASC
       ) GROUP BY userid)
```

初期データセット `final_subscription_test2` では、構造体データタイプを使用して `subscription` フィールドと各ユーザーに固有の `userid` フィールドの両方が含まれます。`subscription` フィールドは、ユーザーの製品購読を表します。複数の購読が存在する可能性がありますが、テーブルには 1 行につき 1 つの購読の情報しか含めることができません。

## INSERT INTO を使用してネストされたデータフィールドの更新

`final_subscription_test2` データセットを作成した後、`INSERT INTO` 文を使用して追加データをテーブルに追加します。データをコピーする場合、ソースとターゲットのデータタイプが一致する必要があります。または、ソースデータタイプはターゲットデータタイプに対して `CAST` である必要があります。次に、次の SQL を使用して、増分データをターゲットデータセットに追加します。

```sql
INSERT INTO final_subscription_test
      SELECT struct(userid, collect_set(subscription) AS subscription) AS _lumaservices3 FROM(
            SELECT user AS userid,
                   struct( last(eventtime) AS last_eventtime,
                           last(status) AS last_status,
                           offer_id, 
                           subsid AS subscription_id)
                   AS subscription
             FROM  SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , TIMESTAMP eventtime
 
                   FROM
                        xbox_subscription_event
                   UNION   
                   SELECT _lumaservices3.msftidentities.userid user
                        , _lumaservices3.subscription.subscription_id subsid
                        , _lumaservices3.subscription.subscription_status status
                        , _lumaservices3.subscription.offer_id offer_id
                        , timestamp eventtime
                   FROM
                        office365_subscription_event
             ) 
             GROUP BY user,subsid,offer_id
             ORDER BY user ASC
       ) GROUP BY userid)
```

## ネストされたデータセットからのデータの処理

データセットからユーザーのアクティブな購読のリストを見つけるには、配列の要素を複数の行と列に分割するクエリを記述する必要があります。これを行うには、購読情報がデータセット内にネストされた配列内に保持されるので、最初にデータモデルの形状を理解する必要があります。

PSQL `\d` コマンドを使用して、必要な購読データへとレベルごとに移動します。`final_subscription_test2` データセットの構造を以下の表に示します。複雑なデータタイプは、テキスト、ブール値、タイムスタンプなどの一般的なタイプの値ではないので、一目で認識できます。

| 列 | タイプ |
|--------|-------|
| `_lumaservices3` | final_subscription_test2__lumaservices3 |

`\d final_subscription_test2__lumaservices3` コマンドを使用すると、次の列のフィールドが表示されます。

| 列 | タイプ |
|---------|-------|
| `userid` | テキスト |
| `subscription` | _lumaservices3_subscription_e[] |

`subscription` は構造体要素の配列です。`\d _lumaservices3_subscription_e[]` コマンドを使用すると、そのフィールドが表示されます。

| 列 | タイプ |
|---------|-------|
| `last_eventtime` | タイムスタンプ |
| `last_status` | テキスト |
| `offer_id` | テキスト |
| `subscription_id` | テキスト |

ネストされた購読フィールドをクエリするには、最初に `subscription` 配列の要素を複数の行に分割し、explode 関数を使用して結果を返す必要があります。次の SQL の例では、`userid` に基づいて、あるユーザーのアクティブな購読を返します。

```sql
SELECT userid, subs AS active_subscription FROM (
    SELECT _lumaservices3.userid AS userid, explode(_lumaservices3.subscription) AS subs 
    FROM final_subscription_test2
)
WHERE subs.last_status='Active';
```

このシンプル化されたサンプルソリューションでは、1 つのアクティブなユーザー購読のみが許可されます。現実的には、1 人のユーザーに対して多数のアクティブな購読が存在する可能性があります。次の例では、前のクエリを変更して、複数の同時にアクティブな購読を許可します。

```sql
SELECT userid, collect_list(subs) AS active_subscriptions FROM (
     SELECT
          _lumaservices3.userid AS userid,
          explode(_lumaservices3.subscription) AS subs
     FROM final_subscription_test2
     )
WHERE subs.last_status='Active' 
GROUP BY userid ;
```

この SQL の例はますます複雑になっていますが、アクティブな購読の `collect_list` は、出力がソースと同じ順序になることを保証しません。ユーザーのアクティブな購読のリストを作成するには、GROUP BY またはシャッフルを使用してリストの結果を集計する必要があります。

## 次の手順

このドキュメントを参照すると、Adobe Experience Platform クエリサービスで複雑なデータタイプを使用するデータセットを処理または変換する方法を理解できます。データレイク内のデータセットで SQL クエリを実行する方法について詳しくは、[クエリ実行のガイダンス](../best-practices/writing-queries.md)を参照してください。

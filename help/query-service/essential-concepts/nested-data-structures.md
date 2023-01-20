---
keywords: Experience Platform；クエリサービス；クエリサービス；ネストされたデータ構造；ネストされたデータ；
title: クエリサービスでのネストされたデータ構造の使用
description: このドキュメントでは、CTAS および INSERT INTO 文を使用してネストされたデータフィールドを処理および変換する作業例を示します。
exl-id: 593379fb-88ad-4b14-8d2e-aa6d18129974
source-git-commit: d3ea7ee751962bb507c91e1afea0da35da60a66d
workflow-type: tm+mt
source-wordcount: '795'
ht-degree: 2%

---

# クエリサービスでのネストされたデータ構造の使用

Adobe Experience Platformクエリサービスでは、ネストされたデータフィールドの使用がサポートされています。 エンタープライズデータ構造の複雑さにより、このデータの変換や処理が複雑になる場合があります。 このドキュメントでは、ネストされたデータ構造を含む複雑なデータ型のデータセットを作成、処理、変換する方法の例を示します。

クエリサービスは、 [!DNL PostgreSQL] Experience Platformが管理するすべてのデータセットに対して SQL クエリを実行するインターフェイス。 Platform では、構造体、配列、マップ、深くネストされた構造体、配列、マップなどのテーブル列で、プリミティブデータ型または複雑なデータ型の使用がサポートされています。 データセットには、列のデータ型がネストされた構造体の配列と同じくらい複雑な構造を含めることも、キーと値のペアの値が複数レベルのネストを持つ構造体になるマップのマップを含めることもできます。

## はじめに

このチュートリアルでは、サードパーティの PSQL クライアントまたはクエリエディターツールを使用して、Experience Platformユーザーインターフェイス (UI) 内でクエリの書き込み、検証、実行をおこなう必要があります。 UI を使用してクエリを実行する方法の詳細については、 [クエリエディター UI ガイド](../ui/user-guide.md). サードパーティのデスクトップクライアントがクエリサービスに接続できる詳細なリストについては、 [クライアント接続の概要](../clients/overview.md).

また、 `INSERT INTO` および `CTAS` 構文と同じです。 使用に関する具体的な情報については、 [`INSERT INTO`](../sql/syntax.md#insert-into) および [`CTAS`](../sql/syntax.md#create-table-as-select) セクション [SQL 構文リファレンスドキュメント](../sql/syntax.md).

## データセットの作成

クエリサービスは、「テーブルを選択として作成」(`CTAS`) 機能を使用して、 `SELECT` 文、またはこの場合と同様に、Adobe Experience Platformの既存の XDM スキーマへの参照を使用します。 以下に、の XDM スキーマを示します。 `Final_subscription` を作成しました。

![final_subscription スキーマの図。](../images/best-practices/final-subscription-schema.png)

次の例は、 `final_subscription_test2` データセット。 `final_subscription_test2` は `Final_subscription` スキーマ。 データは、 `SELECT` 句を使用して、一部の行を設定します。

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

最初のデータセット内 `final_subscription_test2`の場合、構造体データ型は、 `subscription` フィールドと `userid` 各ユーザーに固有の この `subscription` 「 」フィールドは、ユーザーの製品購読を表します。 複数の購読を設定できますが、テーブルには 1 行につき 1 つの購読の情報のみを含めることができます。

## INSERT INTO を使用して、ネストされたデータフィールドを更新します

次の期間の後 `final_subscription_test2` データセットが作成されている場合、 `INSERT INTO` ステートメントは、テーブルに追加のデータを追加するために使用されます。 データをコピーする場合、ソースとターゲットのデータタイプが一致する必要があります。 または、ソースデータタイプが `CAST` をターゲットデータ型に追加します。 次に、次の SQL を使用して、増分データをターゲットデータセットに追加します。

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

## ネストされたデータセットからデータを処理する

ユーザーのアクティブなサブスクリプションのリストをデータセットから調べるには、配列の要素を複数の行と列に分割するクエリを記述する必要があります。 これをおこなうには、まず、サブスクリプション情報がデータセット内にネストされた配列内に保持されるので、データモデルの形状を理解する必要があります。

PSQL `\d` コマンドを使用して、必要なサブスクリプションデータにレベルごとに移動します。 次の表に、 `final_subscription_test2` データセット。 複雑なデータ型は、テキスト、ブール値、タイムスタンプなどの一般的な型の値ではないので、一目で認識できます。

| 列 | タイプ |
|--------|-------|
| `_lumaservices3` | final_subscription_test2__lumaservices3 |

次の列のフィールドは、 `\d final_subscription_test2__lumaservices3` コマンドを使用します。

| 列 | タイプ |
|---------|-------|
| `userid` | テキスト |
| `subscription` | _lumaservices3_subscription_e[] |

`subscription` は構造体要素の配列です。 フィールドは、 `\d _lumaservices3_subscription_e[]` コマンドを使用します。

| 列 | タイプ |
|---------|-------|
| `last_eventtime` | timestamp |
| `last_status` | テキスト |
| `offer_id` | テキスト |
| `subscription_id` | テキスト |

購読のネストされたフィールドに対してクエリを実行するには、まず、 `subscription` 配列を複数の行に分割し、explode 関数を使用して結果を返します。 次の SQL の例では、ユーザーのアクティブな購読を `userid`.

```sql
SELECT userid, subs AS active_subscription FROM (
    SELECT _lumaservices3.userid AS userid, explode(_lumaservices3.subscription) AS subs 
    FROM final_subscription_test2
)
WHERE subs.last_status='Active';
```

このシンプル化されたサンプルソリューションでは、1 人のアクティブなユーザーサブスクリプションのみを使用できます。 現実的には、1 人のユーザーに対して多数のアクティブな購読が存在する可能性があります。 次の使用例は、複数の同時アクティブなサブスクリプションを許可するように前のクエリを変更します。

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

この SQL の例はますます複雑になっていますが、 `collect_list` アクティブなサブスクリプションでは、出力がソースと同じ順序になるとは限りません。 ユーザーのアクティブな購読のリストを作成するには、GROUP BY を使用するか、シャッフリングを使用してリストの結果を集計する必要があります。

## 次の手順

このドキュメントでは、Adobe Experience Platformクエリサービスで複雑なデータ型を使用するデータセットを処理または変換する方法について説明します。 詳しくは、 [クエリの実行ガイダンス](../best-practices/writing-queries.md) を参照してください。

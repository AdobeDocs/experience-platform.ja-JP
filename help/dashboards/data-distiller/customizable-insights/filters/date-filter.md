---
title: 日付フィルターの作成
description: 日付でカスタムインサイトをフィルタリングする方法を説明します。
source-git-commit: 17ad52864bbca09844c0241b6451e6811bd8f413
workflow-type: tm+mt
source-wordcount: '662'
ht-degree: 0%

---

# 日付フィルターの作成 {#create-date-filter}

日付でインサイトをフィルタリングするには、日付制約を受け入れる SQL クエリにパラメーターを追加する必要があります。 これは、query pro mode insight 作成ワークフローの一部として行われます。 を参照してください。 [query pro モードのドキュメント](#query-pro-mode) を参照して、インサイト用に SQL を入力する方法を確認してください。

クエリパラメーターを使用すると、実行時に追加する値のプレースホルダーとして機能する動的データを操作できます。 これらのプレースホルダー値は UI を通じて更新でき、技術の低いユーザーが日付範囲に基づいてインサイトを更新できるようになります。

クエリパラメーターに慣れていない場合は、のドキュメントを参照してください [パラメーター化クエリの実装方法に関するガイダンス](../../../../query-service/ui/parameterized-queries.md).

## ダッシュボードへの日付フィルターの適用 {#apply-date-filter}

日付フィルターを適用するには、次を選択します **[!UICONTROL フィルターを追加]**、次に **[!UICONTROL 日付フィルター]** ダッシュボード表示のドロップダウンメニューから。

![「フィルターを追加」とそのドロップダウンメニューがハイライト表示されたカスタムダッシュボード。](../../../images/customizable-insights/add-filter.png)

## SQL を編集して日付クエリパラメーターを含める {#include-date-parameters}

次に、SQL に日付範囲を許可するクエリパラメーターが含まれていることを確認します。 まだ SQL にクエリパラメーターを組み込んでいない場合は、インサイトを編集してこれらのパラメーターを含めます。 方法については、ドキュメントを参照してください [インサイトを編集](../query-pro-mode.md#edit).

>[!TIP]
>
>を追加することをお勧めします。 `$START_DATE` および `$END_DATE` 日付フィルターを有効にする各グラフの SQL 文のパラメーター。

>[!NOTE]
>
>日付フィルターは、時間制約をサポートしていません。 このフィルターは日付範囲にのみ適用されます。 つまり、24 時間の期間内に複数のレポートがある場合、同じ日に異なる時間を区別することはできません。 このため、時間コンポーネントを日付としてキャストすることをお勧めします。

分析するデータモデルまたはテーブルに時間の構成要素がある場合は、データを日付でグループ化してから、これらの日付フィルターを適用できます。

以下の SQL 文の例は、組み込み方法を示しています `$START_DATE` および `$END_DATE` パラメーターと使用方法 `cast` 時間コンポーネントを日付としてフレーム化します。

```sql
SELECT Sum(personalization_consent_count) AS Personalization,
       Sum(datacollection_consent_count)  AS Datacollection,
       Sum(datasharing_consent_count)     AS Datasharing
FROM   fact_daily_consent_aggregates f
       INNER JOIN dim_consent_valued
               ON f.consent_value_id = d.consent_value_id
WHERE  f.date BETWEEN Upper(Coalesce(Cast('$START_DATE' AS date), '')) AND Upper
                      (
                             Coalesce(Cast('$END_DATE' AS date), ''))
       AND ( ( Upper(Coalesce($consent_value_filter, '')) IN ( '', 'NULL' ) )
              OR ( f.consent_value_id IN ( $consent_value_filter ) ) )
LIMIT  0; 
```

次のスクリーンショットは、SQL 文とクエリパラメーターのキー値のペアに組み込まれた日付制約を示しています。

>[!NOTE]
>
>query pro モードで文を作成する場合、SQL 文を実行してグラフを作成するには、各パラメータにサンプル値を指定する必要があります。 ステートメントを作成するときに指定するサンプル値は、実行時に日付（またはグローバル） フィルターに選択する実際の値に置き換えられます。

![この [!UICONTROL SQL を入力] SQL で日付パラメーターがハイライト表示されたダイアログ](../../../images/customizable-insights/sql-date-parameters.png)

## 各インサイトでの日付パラメーターの有効化 {#enable-date-parameters}

適切なパラメーターをインサイトの SQL に組み込むと、 `Start_date` および `End_date` 変数は、ウィジェットコンポーザーで切り替えとして使用できるようになりました。 を参照してください。 [pro モードウィジェット母集団セクションのクエリ](#populate-widget) インサイトの編集方法について詳しくは、を参照してください。

ウィジェットコンポーザーで「トグル」を選択して、を有効にします `Start_date` および `End_date` パラメーター。

![Start_date と End_date がハイライトされたウィジェットコンポーザー](../../../images/customizable-insights/widget-composer-date-filter-toggles.png)

次に、ドロップダウンメニューから適切なクエリパラメーターを選択します。

![Start_date ドロップダウンメニューがハイライト表示されているウィジェットコンポーザー。](../../../images/customizable-insights/widget-composer-date-filter-dropdown.png)

最後に、を選択します **[!UICONTROL 保存して閉じる]** をクリックしてダッシュボードに戻ります。 開始日および終了日パラメーターを持つすべてのインサイトに対して、日付フィルターが有効になりました。

## 日付フィルターの使用

カスタムの日付フィルターを使用するには、カレンダーアイコンを選択し、カレンダービューから開始と終了を選択します。

>[!IMPORTANT]
>
>日付フィルターを追加するだけでは、グラフは変更されません。 選択した開始日と終了日を含めるように、各インサイトを編集する必要があります。

![日付フィルターカレンダーがハイライト表示されたカスタムダッシュボード。](../../../images/customizable-insights/date-filter.png)

ダッシュボードから日付範囲を選択すると、SQL に日付パラメーターを持つインサイトのウィジェットコンポーザーに日付フィルターオプションが表示されます。

>[!NOTE]
>
>ダッシュボードで日付範囲を選択すると、インサイト作成ワークフローの一部として日付フィルターの切り替えが表示されます。

## 日付フィルターの削除 {#delete-date-filter}

日付フィルターを削除するには、「フィルターを削除」アイコン（![フィルターを削除アイコン。](../../../images/customizable-insights/delete-filter-icon.png)）に設定します。

![フィルター削除アイコンがハイライト表示されたカスタムダッシュボード。](../../../images/customizable-insights/delete-date-filter.png)

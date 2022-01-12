---
keywords: Experience Platform、ホーム、人気の高いトピック、アカウントの監視、データフローの監視、データフロー、宛先
description: 宛先を使用すると、Adobe Experience Platformから数え切れないほどの外部パートナーに対して、データをアクティブ化できます。 このチュートリアルでは、宛先ユーザーインターフェイスを使用して宛先のデータフローを監視するExperience Platformを説明します。
solution: Experience Platform
title: UI での宛先のデータフローの監視
topic-legacy: overview
type: Tutorial
exl-id: 8eb7bb3c-f2dc-4dbc-9cf5-3d5d3224f5f1
source-git-commit: dc7de355284e2f1f52939ca7a80344345ce92c43
workflow-type: tm+mt
source-wordcount: '1879'
ht-degree: 4%

---

# UI での宛先のデータフローの監視

宛先を使用すると、Adobe Experience Platformから数え切れないほどの外部パートナーに対して、データをアクティブ化できます。 Platform では、データフローに透明性を提供することで、宛先へのデータフローの追跡プロセスを容易にします。

監視ダッシュボードは、データがアクティブ化される宛先を含む、データフローのジャーニーを視覚的に表します。 このチュートリアルでは、Experience Platformワークスペースで直接データフローを監視する方法、または監視ダッシュボードを使用して、宛先ユーザーインターフェイスを使用して、宛先のデータフローを監視する方法について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [データフロー](../home.md)：データフローは、Platform 間でデータを移動するデータジョブを表します。データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   - [データフローの実行](../../sources/notifications.md):データフローの実行は、選択したデータフローの頻度設定に基づく、定期的にスケジュールされたジョブです。
- [宛先](../../destinations/home.md):宛先は、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用例に対して、Platform からデータをシームレスにアクティブ化できる、一般的に使用されるアプリケーションとの事前定義済みの統合です。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## 宛先ワークスペースでのデータフローの監視 {#monitor-dataflows-in-the-destinations-workspace}

内 **[!UICONTROL 宛先]** Platform UI 内のワークスペースで、 **[!UICONTROL 参照]** 「 」タブをクリックし、表示する宛先の名前を選択します。

![](../assets/ui/monitor-destinations/select-destination.png)

既存のデータフローのリストが表示されます。 このページには、表示可能なデータフローのリストがあります。これには、宛先、ユーザー名、データフロー数、ステータスに関する情報が含まれます。

ステータスについて詳しくは、次の表を参照してください。

| ステータス | 説明 |
| ------ | ----------- |
| 有効 | この `Enabled` 「 」ステータスは、データフローがアクティブで、指定されたスケジュールに従ってデータをエクスポートしていることを示します。 |
| 無効 | この `Disabled` status は、データフローが非アクティブで、データをエクスポートしていないことを示します。 |
| 処理中 | この `Processing` 「 」ステータスは、データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローの作成直後に発生します。 |
| エラー | この `Error` ステータスは、データフローのアクティベーションプロセスが中断されたことを示します。 |

### ストリーミング宛先のデータフロー実行 {#dataflow-runs-for-streaming-destinations}

>[!CONTEXTUALHELP]
>id="platform_destinations_dataflow_identitiesactivated"
>title="アクティブ化された ID"
>abstract="選択した宛先に対して正常にアクティブ化された個々のプロファイル ID の数。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_destinations_dataflow_identitiesexcluded"
>title="除外された ID"
>abstract="属性が見つからず、同意違反に基づいて、選択した宛先のアクティベーションから除外された個々のプロファイルレコードの数。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_destinations_dataflow_identitiesfailed"
>title="失敗した ID"
>abstract="選択した宛先に対して失敗した個々のプロファイル ID の数。 詳細は、エラー診断を確認してください。"
>additional-url="https://adobe.com/go/destinations-monitor-dataflows-batch-en" text="詳しくは、ドキュメントを参照してください。"

ストリーミング先の場合、 [!UICONTROL データフローの実行] 「 」タブには、データフローの実行時に指標データが 1 時間ごとに更新されます。 ラベル付けされた最も重要な統計は、ID 用です。

ID は、プロファイルの様々なファセットを表します。 例えば、プロファイルに電話番号と電子メールアドレスの両方が含まれている場合、そのプロファイルには 2 つの ID が含まれます。

個々の実行とその特定の指標のリストが表示され、次の ID の合計が表示されます。

- **[!UICONTROL アクティブ化された ID]**:アクティベーション用に作成または更新されたプロファイル ID の合計数です。
- **[!UICONTROL 除外された ID]**:属性が見つからず、同意違反に基づいてアクティベーション用にスキップされたプロファイル ID の合計数です。
- **[!UICONTROL 失敗した ID]**:エラーが原因で宛先に対してアクティブ化されなかったプロファイル ID の合計数です。

![](../assets/ui/monitor-destinations/dataflow-runs-stream.png)

個々のデータフローの実行ごとに、次の詳細が表示されます。

- **[!UICONTROL データフローの実行開始]**:データフローの実行が開始された時刻。
- **[!UICONTROL 処理時間]**:データフローの処理に要した時間。
- **[!UICONTROL 受信したプロファイル]**:データフローで受信したプロファイルの合計数。
- **[!UICONTROL アクティブ化された ID]**:選択した宛先に対して正常にアクティブ化されたプロファイル ID の合計数です。
- **[!UICONTROL 除外された ID]**:属性が見つからず、同意違反に基づいてアクティベーションから除外されたプロファイル ID の合計数です。
- **[!UICONTROL 失敗した ID]** エラーが原因で宛先に対してアクティブ化されなかったプロファイル ID の合計数です。
- **[!UICONTROL 有効化率]**:正常にアクティブ化されたか、スキップされた受信 ID の割合。 次の式は、この値の計算方法を示しています。
   ![](../assets/ui/monitor-destinations/activation-rate-formula.png)
- **[!UICONTROL ステータス]**:データフローの状態を表します。どちらか [!UICONTROL 完了] または [!UICONTROL 処理中]. [!UICONTROL 完了] は、対応するデータフロー実行のすべての ID が 1 時間以内にエクスポートされたことを意味します。 [!UICONTROL 処理中] は、データフローの実行がまだ完了していないことを示します。

特定のデータフロー実行の詳細を表示するには、リストから実行の開始時刻を選択します。

データフロー実行の詳細ページには、受信したプロファイルの数、アクティブ化された ID の数、失敗した ID の数、除外された ID の数など、追加の情報が含まれます。

![](../assets/ui/monitor-destinations/dataflow-details-stream.png)

詳細ページには、失敗した ID と除外された ID のリストも表示されます。 失敗した ID と除外された ID の両方に関する情報（エラーコード、ID の数、説明を含む）が表示されます。 デフォルトでは、リストには失敗した ID が表示されます。 スキップされた ID を表示するには、「 **[!UICONTROL 除外された ID]** 切り替え

![](../assets/ui/monitor-destinations/dataflow-records-stream.png)

### バッチ宛先のデータフロー実行 {#dataflow-runs-for-batch-destinations}

>[!CONTEXTUALHELP]
>id="platform_monitoring_dataflow_run_details_activation"
>title="データフロー実行の詳細"
>abstract="宛先のデータフロー実行の詳細には、セグメントのアクティベーションステータスに関する情報と、一意の ID を生成するためにリアルタイム顧客プロファイルから取得された指標が含まれます。 詳しくは、指標の定義に関するガイドを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_monitoring_profiles_received"
>title="受信したプロファイル"
>abstract="データフローで受信したプロファイルの合計数。 この値は 60 分ごとに更新されます。"
>additional-url="https://adobe.com/go/destinations-monitor-dataflows-batch-en" text="詳しくは、ドキュメントを参照してください。"

バッチ保存先の場合、 [!UICONTROL データフローの実行] 「 」タブには、データフローの実行に関する指標データが表示されます。 個々の実行とその特定の指標のリストが表示され、次の ID の合計が表示されます。

- **[!UICONTROL アクティブ化された ID]**:選択した宛先に対して正常にアクティブ化された個々のプロファイル ID の数。
- **[!UICONTROL 除外された ID]**:見つからない属性や同意違反に基づいて、選択した宛先のアクティブ化から除外された個々のプロファイル ID の数。

![](../assets/ui/monitor-destinations/dataflow-runs-batch.png)

個々のデータフローの実行ごとに、次の詳細が表示されます。

- **[!UICONTROL データフローの実行開始]**:データフローの実行が開始された時刻。
- **[!UICONTROL 処理時間]**:データフローの実行が処理されるまでに要した時間。
- **[!UICONTROL 受信したプロファイル]**:データフローで受信したプロファイルの合計数。 この値は 60 分ごとに更新されます。
- **[!UICONTROL アクティブ化された ID]**:選択した宛先に対して正常にアクティブ化されたプロファイル ID の合計数です。
- **[!UICONTROL 除外された ID]**:属性が見つからず、同意違反に基づいてアクティベーションから除外されたプロファイル ID の合計数です。
- **[!UICONTROL ステータス]**:データフローの状態を表します。 次の 3 つの状態のいずれかを指定できます。 [!UICONTROL 成功], [!UICONTROL 失敗]、および [!UICONTROL 処理中]. [!UICONTROL 成功] は、データフローがアクティブで、指定されたスケジュールに従ってデータをエクスポートしていることを意味します。 [!UICONTROL 失敗] は、エラーが原因でデータのアクティベーションが中断されたことを意味します。 [!UICONTROL 処理中] は、データフローがまだアクティブではなく、通常、新しいデータフローの作成時に発生することを意味します。

特定のデータフロー実行の詳細を表示するには、リストから実行の開始時刻を選択します。

>[!NOTE]
>
>データフローの実行は、宛先データフローのスケジュール頻度に基づいて生成されます。 セグメントに適用された結合ポリシーごとに、個別のデータフローが実行されます。

データフローの詳細ページは、データフローリストに表示される詳細に加えて、データフローに関するより具体的な情報を表示します。

- **[!UICONTROL データのサイズ]**:エクスポートするデータフローのサイズ。
- **[!UICONTROL 合計ファイル数]**:データフローでエクスポートされたファイルの合計数。
- **[!UICONTROL 最終更新日]**:データフローの実行が最後に更新された時刻。

![](../assets/ui/monitor-destinations/dataflow-batch.png)

詳細ページには、失敗した ID と除外された ID のリストも表示されます。 エラーコードや説明を含む、失敗した ID と除外された ID の両方に関する情報が表示されます。 デフォルトでは、リストには失敗した ID が表示されます。 除外された ID を表示するには、 **[!UICONTROL 除外された ID]** 切り替え

![](../assets/ui/monitor-destinations/dataflow-records-batch.png)

## 「宛先の監視」ダッシュボード {#monitoring-destinations-dashboard}

>[!CONTEXTUALHELP]
>id="platform_monitoring_activation"
>title="Activation"
>abstract="宛先のアクティベーションには、セグメントのアクティベーションステータスに関する情報と、一意の ID を生成するためにリアルタイム顧客プロファイルから取得された指標が含まれます。"

次の手順で [!UICONTROL 監視] ダッシュボード、選択 **[!UICONTROL 監視]** (![監視アイコン](../assets/ui/monitor-destinations/monitoring-icon.png)) をクリックします。 1 回 [!UICONTROL 監視] ページ、選択 [!UICONTROL 宛先]. この [!UICONTROL 監視] ダッシュボードには、宛先の実行ジョブに関する指標と情報が含まれています。

ダッシュボードの中央にある「アクティベーション」パネルには、宛先に書き出されたデータのアクティベーション率に関するデータを表示する指標およびグラフが含まれています。

![](../assets/ui/monitor-destinations/dashboard-graph.png)


デフォルトでは、表示されるデータには、過去 24 時間のアクティベーション率が含まれています。 選択 **[!UICONTROL 過去 24 時間]** を使用して、表示されるレコードの時間枠を調整します。 次のオプションを使用できます。 **[!UICONTROL 過去 24 時間]**, **[!UICONTROL 過去 7 日間]**、および **[!UICONTROL 過去 30 日間]**. または、表示されるカレンダーポップアップウィンドウで日付を選択することもできます。 日付を選択したら、「 」を選択します。 **[!UICONTROL 適用]** を使用して、表示される情報の時間枠を調整します。

>[!NOTE]
>
>次のスクリーンショットは、過去 24 時間のアクティベーション率ではなく、過去 30 日間のアクティベーション率を示しています。 時間枠は、 **[!UICONTROL 過去 30 日間]**.

![](../assets/ui/monitor-destinations/dashboard-graph-change-date-range.png)

グラフはデフォルトで表示され、それを無効にして以下の宛先のリストを展開できます。 を選択します。 **[!UICONTROL 指標とグラフ]** を切り替えて、グラフを無効にします。

この **[!UICONTROL Activation]** パネルには、1 つ以上の既存のアカウントを含む宛先のリストが表示されます。 このリストには、受信したプロファイル、アクティブ化されたプロファイルレコード、失敗したプロファイルレコード、スキップされたプロファイルレコード、失敗したデータフローの合計、これらの宛先の最終更新日に関する情報も含まれます。

![](../assets/ui/monitor-destinations/dashboard-destinations.png)

また、宛先のリストをフィルタリングして、選択したカテゴリの宛先のみを表示することもできます。 を選択します。 **[!UICONTROL 宛先]** ドロップダウンをクリックし、フィルタリング対象の宛先のタイプを選択します。

![](../assets/ui/monitor-destinations/dashboard-destinations-filter-dropdown.png)

さらに、検索バーに宛先を入力して、1 つの宛先に分離することもできます。 宛先のデータフローを表示する場合は、フィルターを選択できます ![フィルター](../assets/ui/monitor-destinations/filter.png) その横に、アクティブなデータフローのリストを表示します。

![](../assets/ui/monitor-destinations/filtered-destinations.png)

すべての宛先の既存のデータフローをすべて表示する場合は、「 」を選択します。 **[!UICONTROL データフロー]**.

データフローのリストが表示され、宛先ごとにグループ化されます。 特定のデータフローの追加の詳細は、監視する宛先を見つけ、フィルターを選択することで確認できます ![フィルター](../assets/ui/monitor-destinations/filter.png) その横に表示され、その後、フィルターを選択します。 ![フィルター](../assets/ui/monitor-destinations/filter.png) をデータフローの横に表示して、詳細情報を確認します。

![](../assets/ui/monitor-destinations/dashboard-dataflows.png)


データフローの実行ページには、データフローの実行開始時間、処理時間、受信したプロファイル、アクティブ化された ID、除外された ID、失敗した ID、アクティブ化率、ステータスなど、データフローの実行に関する情報が表示されます。 特定のデータフロー実行の詳細を表示するには、フィルターを選択します ![フィルター](../assets/ui/monitor-destinations/filter.png) をクリックします。

![](../assets/ui/monitor-destinations/dashboard-dataflows-filter.png)

データフローの詳細ページには、データフローリストに表示される詳細に加えて、データフローに関するより具体的な情報が表示されます。

- **[!UICONTROL データフロー実行 ID]**:データフローの ID。
- **[!UICONTROL IMS org ID]**:データフローが属する IMS 組織。
- **[!UICONTROL 最終更新日]**:データフローの実行が最後に更新された時刻。

詳細ページには、失敗した ID と除外された ID のリストも表示されます。 失敗した ID と除外された ID の両方に関する情報（エラーコード、ID の数、説明を含む）が表示されます。 デフォルトでは、リストには失敗した ID が表示されます。 スキップされた ID を表示するには、「 **[!UICONTROL 除外された ID]** 切り替え

![](../assets/ui/monitor-destinations/identities-excluded.png)

## 次の手順

このガイドに従うことで、処理時間、アクティベーション率、ステータスなどの関連情報を含め、バッチ宛先とストリーミング宛先の両方でデータフローを監視する方法を理解できました。 Platform でのデータフローの詳細については、 [データフローの概要](../home.md). 宛先の詳細については、 [宛先の概要](../../destinations/home.md).
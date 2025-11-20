---
keywords: Experience Platform;家;人気のあるトピック;監視 ID監視データフロー。データフロー;アイデンティティ;
description: Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。 このチュートリアルでは、Experience Platform ユーザー インターフェイスを使用して ID を持つデータフロー監視方法について説明します。
title: UI での ID のデータフローの監視
type: Tutorial
exl-id: 735b0e52-74f6-47fe-98c6-e12a633b6f57
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1075'
ht-degree: 18%

---

# UI での ID のデータフローの監視

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

監視ダッシュボードでは、データの ID の状態など、ID 内のデータのアクティビティを視覚的に表現できます。 このチュートリアルでは、監視ダッシュボードを使用して、Experience Platform ユーザー インターフェイスを使用してデータの ID を監視し、ID 処理の状態を追跡できるようにする方法について説明します。

## はじめに {#getting-started}

- [データフロー](../home.md): データフローは、Experience Platform間でデータを移動するデータ ジョブの表現です。 データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   - [データフロー実行](../../sources/notifications.md)：データフロー実行は、選択したデータフローの頻度設定に基づいて繰り返しスケジュールされたジョブです。
- [ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

## 監視 ID ダッシュボード {#identity-metrics}

>[!CONTEXTUALHELP]
>id="platform_monitoring_identity_processing"
>title="ID 処理"
>abstract="ID 処理ビューには、ID サービスに取り込まれたレコードに関する情報 (追加された ID、作成されたグラフや更新されたグラフの数など) が表示されます。指標およびグラフについて詳しくは、指標定義ガイドを参照してください。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_dataflow_run_details_identity"
>title="データフロー実行の詳細"
>abstract="データフロー実行の詳細ページには、ID データフロー実行に関する詳しい情報 (組織 ID やデータフロー実行 ID など) が表示されます。"

**[!UICONTROL Identities]** ダッシュボードにアクセスするには、左側のナビゲーションで「**[!UICONTROL Monitoring]**」を選択します。 **[!UICONTROL Monitoring]** ページで、**[!UICONTROL Identities]** カードを選択します。

![ID カード。 受信したレコード数、取り込んだレコード数および成功率に関する情報が表示されます。](../assets/ui/monitor-identities/focus-card.png)

メイン **[!UICONTROL Identities]** ダッシュボードの **[!UICONTROL Identities]** カードには、受信したレコードの総数、取り込まれたレコードの数、およびレコード取得の成功率に関する情報が表示されます。

ダッシュボード自体に、IDサービス処理に関するメトリックが含まれています。 既定では、ダッシュボードにはIDサービス過去 24 時間の組織のソースの処理の詳細が表示されます。

![ID ダッシュボード。 ソースごとに受信したレコード数に関する情報が表示されます。](../assets/ui/monitor-identities/sources.png)

[!UICONTROL Identity processing] ページには、[!DNL Identity Service] に取り込まれたレコードに関する情報（追加された ID、作成されたグラフや更新されたグラフの数など）が表示されます。

このダッシュボードビューでは、次の指標を使用できます。

| IDサービス指標 | 説明 |
| ---------------- | ----------- |
| **[!UICONTROL Records received]** | データ レイクから受信したレコードの数。 |
| **[!UICONTROL Records failed]** | データのエラーが原因でExperience Platformに取り込まれなかったレコードの数。 |
| **[!UICONTROL Records skipped]** | 取り込まれたが、レコード行に識別子が 1 つしかなかったために [!DNL Identity Service] には入らなかったレコードの数。 |
| **[!UICONTROL Records ingested]** | [!DNL Identity Service] に取得されたレコードの数。 |
| **[!UICONTROL Identities added]** | [!DNL Identity Service] に追加された新しい ID の数です。 |
| **[!UICONTROL Graphs created]** | [!DNL Identity Service] で作成された新しい ID グラフの数です。 |
| **[!UICONTROL Graphs updated]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL Total failed dataflows]** | 失敗したデータフロー実行の数。 |

ソース名の横にあるフィルターアイコン ![&#x200B; フィルターアイコン &#x200B;](/help/images/icons/filter.png) を選択して、選択したソースのデータフローの ID 処理情報を確認できます。

![&#x200B; フィルターアイコンがハイライト表示されています。 このアイコンを選択すると、選択したソースのデータフローを表示できます。](../assets/ui/monitor-identities/sources-filter.png)

または、切り替えスイッチで **[!UICONTROL Dataflows]** を選択して、過去 24 時間の組織のデータフローの ID 処理の詳細を表示できます。

![Id ダッシュボード。 データフローごとに受信した ID の数に関する情報が表示されます。](../assets/ui/monitor-identities/dataflows.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL Dataflow]** | データフローの名前。 |
| **[!UICONTROL Dataset]** | データフローを挿入するデータセットの名前。 |
| **[!UICONTROL Source name]** | データフローが属するソースの名前。 |
| **[!UICONTROL Records received]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL Records failed]** | データのエラーが原因でExperience Platformに取り込まれなかったレコードの数。 |
| **[!UICONTROL Records skipped]** | 取り込まれたが、レコード行に識別子が 1 つしかなかったので [!DNL Identity Service] に取り込まれなかったレコードの数。 |
| **[!UICONTROL Records ingested]** | [!DNL Identity Service] に取得されたレコードの数。 |
| **[!UICONTROL Total records]** | 失敗したレコード、スキップされたレコード、追加された ID および重複したレコードを含むすべてのレコードの合計数。 |
| **[!UICONTROL Identities added]** | [!DNL Identity Service] に追加された新しい ID の数です。 |
| **[!UICONTROL Graphs created]** | [!DNL Identity Service] で作成された新しい ID グラフの数です。 |
| **[!UICONTROL Graphs updated]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL Total failed dataflows]** | 失敗したデータフロー実行の数。 |

データフロー実行開始時間の横にあるフィルターアイコン ![&#x200B; フィルター &#x200B;](/help/images/icons/filter.png) を選択して、[!DNL Identity] のデータフロー実行に関する詳細を表示します。

![&#x200B; フィルターアイコンがハイライト表示されています。 このアイコンを選択すると、選択したデータフローの詳細を表示できます。](../assets/ui/monitor-identities/dataflows-filter.png)

[!UICONTROL Dataflow run details]ページには、組織 ID やデータフロー実行 ID など、[!DNL Identity]データフローの実行に関する詳細情報が表示されます。このページは、取得プロセスでエラーが発生した場合に、 [!DNL Identity Service]が提供する対応するエラーコードとエラーメッセージも表示します。

![選択したデータフローに関する詳細情報を示すダッシュボードが表示されます。](../assets/ui/monitor-identities/dataflow-run-details.png)

このダッシュボード表示では次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL Records received]** | データ レイクから受信したレコードの数。 |
| **[!UICONTROL Records failed]** | データのエラーが原因でExperience Platformに取り込まれなかったレコードの数。 |
| **[!UICONTROL Records skipped]** | 取り込まれたが、レコード行に識別子が 1 つしかなかったために [!DNL Identity Service] には入らなかったレコードの数。 |
| **[!UICONTROL Records ingested]** | [!DNL Identity Service]に取り込まれたレコードの数。 |
| **[!UICONTROL Identities added]** | [!DNL Identity Service]に追加された新しい識別子の数。 |
| **[!UICONTROL Graphs created]** | [!DNL Identity Service] 年に作成された新規 ID グラフの数。 |
| **[!UICONTROL Graphs updated]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL Status]** | データフローの全体的なステータスを定義します。 ステータス値には、次のものがあります。 <ul><li>`Success`：データフローがアクティブで、指定したスケジュールに従ってデータを取り込んでいることを示します。</li><li>`Failed`: データフローのアクティブ化プロセスがエラーによって中断されたことを示します。 </li><li>`Processing`：データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローを作成した直後に発生します。</li></ul> |
| **[!UICONTROL Dataflow run start]** | データフローの実行が開始された日時。 |
| **[!UICONTROL Last updated]** | データフローが最後に更新された日時。 |
| **[!UICONTROL Error summary]** | データフローの実行に失敗した場合は、エラーコードと、データフローの実行が失敗した理由の概要が表示されます。 |
| **[!UICONTROL Dataflow run ID]** | データフロー実行の ID。 |
| **[!UICONTROL IMS org ID]** | データフロー実行が属する組織 ID。 |

さらに、トグルを選択して、失敗したレコードまたはスキップされたレコード表示できます。 エラーセクションには、エラーコードと、失敗または除外されたレコードの数に関する詳細が含まれます。

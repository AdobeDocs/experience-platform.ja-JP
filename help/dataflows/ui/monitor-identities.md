---
keywords: Experience Platform；ホーム；人気のトピック；ID の監視；データフローの監視；データフロー；ID;
description: Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタル体験をリアルタイムで提供できます。 このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して ID でデータフローを監視する方法について説明します。
title: UI での ID のデータフローの監視
type: Tutorial
exl-id: 735b0e52-74f6-47fe-98c6-e12a633b6f57
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 17%

---

# UI での ID のデータフローの監視

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

監視ダッシュボードを使用すると、データの ID のステータスなど、ID 内のデータのアクティビティを視覚的に表現できます。 このチュートリアルでは、モニタリングダッシュボードを使用してExperience Platform ユーザーインターフェイスでデータの ID を監視し、ID 処理のステータスをトラッキングできる方法について説明します。

## はじめに {#getting-started}

- [ データフロー ](../home.md)：データフローは、Experience Platform間でデータを移動するデータジョブを表します。 データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
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

**[!UICONTROL ID]** ダッシュボードにアクセスするには、左側のナビゲーションで **[!UICONTROL 監視]** を選択します。 **[!UICONTROL モニタリング]** ページで「**[!UICONTROL ID]**」カードを選択します。

![ID カード。 受信したレコード数、取り込んだレコード数および成功率に関する情報が表示されます。](../assets/ui/monitor-identities/focus-card.png)

メインの **[!UICONTROL ID]** ダッシュボードでは、**[!UICONTROL ID]** カードに、受信したレコードの合計数、取り込んだレコードの数、およびレコード取り込みの成功率に関する情報が表示されます。

ダッシュボード自体には、ID 処理に関する指標が含まれています。 デフォルトでは、ダッシュボードには、過去 24 時間の組織のソースの ID 処理の詳細が表示されます。

![Id ダッシュボード。 ソースごとに受信したレコード数に関する情報が表示されます。](../assets/ui/monitor-identities/sources.png)

[!UICONTROL ID 処理 &#x200B;] ページには、[!DNL Identity Service] に取り込まれたレコードに関する情報（追加された ID、作成されたグラフや更新されたグラフの数など）が含まれています。

このダッシュボードビューでは、次の指標を使用できます。

| ID 指標 | 説明 |
| ---------------- | ----------- |
| **[!UICONTROL 受信レコード]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL 失敗したレコード]** | データのエラーが原因でExperience Platformに取り込まれなかったレコードの数。 |
| **[!UICONTROL スキップされたレコード]** | 取り込まれたが、レコード行に識別子が 1 つしかなかったので [!DNL Identity Service] に取り込まれなかったレコードの数。 |
| **[!UICONTROL 取り込まれたレコード]** | [!DNL Identity Service] に取得されたレコードの数。 |
| **[!UICONTROL 追加された ID]** | [!DNL Identity Service] に追加された新しい ID の数です。 |
| **[!UICONTROL 作成されたグラフ]** | [!DNL Identity Service] で作成された新しい ID グラフの数です。 |
| **[!UICONTROL グラフが更新されました]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL 失敗したデータフローの合計]** | 失敗したデータフロー実行の数。 |

ソース名の横にあるフィルターアイコン ![ フィルターアイコン ](/help/images/icons/filter.png) を選択して、選択したソースのデータフローの ID 処理情報を確認できます。

![ フィルターアイコンがハイライト表示されています。 このアイコンを選択すると、選択したソースのデータフローを表示できます。](../assets/ui/monitor-identities/sources-filter.png)

または、切り替えスイッチで **[!UICONTROL データフロー]** を選択して、過去 24 時間の組織のデータフローの ID 処理の詳細を表示できます。

![Id ダッシュボード。 データフローごとに受信した ID の数に関する情報が表示されます。](../assets/ui/monitor-identities/dataflows.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL データフロー]** | データフローの名前。 |
| **[!UICONTROL データセット]** | データフローを挿入するデータセットの名前。 |
| **[!UICONTROL Source名]** | データフローが属するソースの名前。 |
| **[!UICONTROL 受信レコード]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL 失敗したレコード]** | データのエラーが原因でExperience Platformに取り込まれなかったレコードの数。 |
| **[!UICONTROL スキップされたレコード]** | 取り込まれたが、レコード行に識別子が 1 つしかなかったので [!DNL Identity Service] に取り込まれなかったレコードの数。 |
| **[!UICONTROL 取り込まれたレコード]** | [!DNL Identity Service] に取得されたレコードの数。 |
| **[!UICONTROL 合計レコード数]** | 失敗したレコード、スキップされたレコード、追加された ID および重複したレコードを含むすべてのレコードの合計数。 |
| **[!UICONTROL 追加された ID]** | [!DNL Identity Service] に追加された新しい ID の数です。 |
| **[!UICONTROL 作成されたグラフ]** | [!DNL Identity Service] で作成された新しい ID グラフの数です。 |
| **[!UICONTROL グラフが更新されました]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL 失敗したデータフローの合計]** | 失敗したデータフロー実行の数。 |

データフロー実行開始時間の横にあるフィルターアイコン ![ フィルター ](/help/images/icons/filter.png) を選択して、[!DNL Identity] のデータフロー実行に関する詳細を表示します。

![ フィルターアイコンがハイライト表示されています。 このアイコンを選択すると、選択したデータフローの詳細を表示できます。](../assets/ui/monitor-identities/dataflows-filter.png)

[!UICONTROL &#x200B; データフロー実行の詳細 &#x200B;] ページには、[!DNL Identity] データフロー実行に関する詳細（組織 ID やデータフロー実行 ID など）が表示されます。 また、このページには、取り込みプロセスでエラーが発生した場合に、[!DNL Identity Service] から提供される対応するエラーコードとエラーメッセージも表示されます。

![ 選択したデータフローに関する詳細情報を示すダッシュボードが表示されます。](../assets/ui/monitor-identities/dataflow-run-details.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL 受信レコード]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL 失敗したレコード]** | データのエラーが原因でExperience Platformに取り込まれなかったレコードの数。 |
| **[!UICONTROL スキップされたレコード]** | 取り込まれたが、レコード行に識別子が 1 つしかなかったので [!DNL Identity Service] に取り込まれなかったレコードの数。 |
| **[!UICONTROL 取り込まれたレコード]** | [!DNL Identity Service] に取得されたレコードの数。 |
| **[!UICONTROL 追加された ID]** | [!DNL Identity Service] に追加された新しい ID の数です。 |
| **[!UICONTROL 作成されたグラフ]** | [!DNL Identity Service] で作成された新しい ID グラフの数です。 |
| **[!UICONTROL グラフが更新されました]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL ステータス]** | データフローの全体的なステータスを定義します。 ステータス値には、次のものがあります。 <ul><li>`Success`：データフローがアクティブで、指定したスケジュールに従ってデータを取り込んでいることを示します。</li><li>`Failed`: データフローのアクティブ化プロセスがエラーによって中断されたことを示します。 </li><li>`Processing`：データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローを作成した直後に発生します。</li></ul> |
| **[!UICONTROL データフロー実行開始]** | データフローの実行が開始された日時。 |
| **[!UICONTROL 最終更新日]** | データフローが最後に更新された日時。 |
| **[!UICONTROL エラーの概要]** | データフローの実行に失敗した場合は、エラーコードと、データフローの実行が失敗した理由の概要が表示されます。 |
| **[!UICONTROL データフロー実行 ID]** | データフロー実行の ID。 |
| **[!UICONTROL IMS 組織 ID]** | データフロー実行が属する組織 ID。 |

さらに、切替スイッチを選択して、失敗したレコードとスキップされたレコードを表示できます。 「エラー」セクションには、エラーコードに関する詳細と、失敗または除外されたレコードの数が表示されます。

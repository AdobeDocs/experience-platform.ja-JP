---
keywords: Experience Platform、ホーム、人気の高いトピック、ID の監視、データフローの監視、データフロー、ID、
description: Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。このチュートリアルでは、Experience Platformユーザーインターフェイスを使用して ID でデータフローを監視する方法について説明します。
title: UI での ID のデータフローの監視
type: Tutorial
exl-id: 735b0e52-74f6-47fe-98c6-e12a633b6f57
source-git-commit: 1a7ba52b48460d77d0b7695aa0ab2d5be127d921
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 24%

---

# UI での ID のデータフローの監視

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

監視ダッシュボードは、データの ID のステータスなど、ID 内のデータのアクティビティを視覚的に表します。 このチュートリアルでは、Experience Platformユーザーインターフェイスを使用して監視ダッシュボードを使用してデータの ID を監視し、ID 処理のステータスを追跡する方法について説明します。

## はじめに {#getting-started}

- [データフロー](../home.md)：データフローは、Platform 間でデータを移動するデータジョブを表します。データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   - [データフロー実行](../../sources/notifications.md)：データフロー実行は、選択したデータフローの頻度設定に基づいて繰り返しスケジュールされたジョブです。
- [ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

## ID の監視ダッシュボード {#identity-metrics}

>[!CONTEXTUALHELP]
>id="platform_monitoring_identity_processing"
>title="ID 処理"
>abstract="ID 処理ビューには、ID サービスに取り込まれたレコードに関する情報 (追加された ID、作成されたグラフや更新されたグラフの数など) が表示されます。指標およびグラフについて詳しくは、指標定義ガイドを参照してください。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_dataflow_run_details_identity"
>title="データフロー実行の詳細"
>abstract="データフロー実行の詳細ページには、ID データフロー実行に関する詳しい情報 (組織 ID やデータフロー実行 ID など) が表示されます。"

次の手順で **[!UICONTROL ID]** ダッシュボード、選択 **[!UICONTROL 監視]** をクリックします。 1 回 **[!UICONTROL 監視]** ページで、 **[!UICONTROL ID]** カード。

![ID カード。 受信したレコード数、取得したレコード数、成功率に関する情報が表示されます。](../assets/ui/monitor-identities/focus-card.png)

メイン **[!UICONTROL ID]** ダッシュボード、 **[!UICONTROL ID]** 「 」カードには、受信したレコードの合計数、取り込んだレコード数、およびレコードの取り込みの成功率に関する情報が表示されます。

ダッシュボード自体には、ID 処理に関する指標が含まれています。 デフォルトでは、ダッシュボードには、過去 24 時間の組織のソースの ID 処理の詳細が表示されます。

![「 ID 」ダッシュボード。 ソースごとに受信したレコード数に関する情報が表示されます。](../assets/ui/monitor-identities/sources.png)

この [!UICONTROL ID 処理] ページには、次の場所に取り込まれたレコードに関する情報が含まれます： [!DNL Identity Service]（追加された id 数、作成されたグラフ、更新されたグラフなど）

このダッシュボードビューでは、次の指標を使用できます。

| ID 指標 | 説明 |
| ---------------- | ----------- |
| **[!UICONTROL 受信したレコード]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL 失敗したレコード]** | データのエラーが原因で Platform に取り込まれなかったレコードの数。 |
| **[!UICONTROL スキップされたレコード]** | 取り込まれたが、に取り込まれなかったレコードの数 [!DNL Identity Service] レコード行には識別子が 1 つしかなかったので |
| **[!UICONTROL 取り込まれたレコード]** | に取り込まれたレコードの数 [!DNL Identity Service]. |
| **[!UICONTROL 追加された ID]** | に追加された新しい識別子の数 [!DNL Identity Service]. |
| **[!UICONTROL 作成されたグラフ]** | で作成された新しい ID グラフの数 [!DNL Identity Service]. |
| **[!UICONTROL 更新されたグラフ]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL 失敗したデータフローの合計]** | 失敗したデータフロー実行の数。 |

フィルターアイコンを選択できます ![フィルターアイコン](../assets/ui/monitor-identities/filter.png) をクリックし、選択したソースのデータフローの ID 処理情報を確認します。

![フィルターアイコンがハイライト表示されます。 このアイコンを選択すると、選択したソースのデータフローを表示できます。](../assets/ui/monitor-identities/sources-filter.png)

または、 **[!UICONTROL データフロー]** 「 」をオンにすると、過去 24 時間の組織のデータフローの ID 処理の詳細が表示されます。

![「 ID 」ダッシュボード。 データフローごとに受信した ID の数に関する情報が表示されます。](../assets/ui/monitor-identities/dataflows.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL データフロー]** | データフローの名前。 |
| **[!UICONTROL データセット]** | データフローの挿入先のデータセットの名前。 |
| **[!UICONTROL ソース名]** | データフローが属するソースの名前。 |
| **[!UICONTROL 受信したレコード]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL 失敗したレコード]** | データのエラーが原因で Platform に取り込まれなかったレコードの数。 |
| **[!UICONTROL スキップされたレコード]** | 取り込まれたが、に取り込まれなかったレコードの数 [!DNL Identity Service] レコード行には識別子が 1 つしかなかったので |
| **[!UICONTROL 取り込まれたレコード]** | に取り込まれたレコードの数 [!DNL Identity Service]. |
| **[!UICONTROL 合計レコード数]** | 失敗したレコード、スキップされたレコード、追加された ID、重複したレコードを含む、すべてのレコードの合計数。 |
| **[!UICONTROL 追加された ID]** | に追加された新しい識別子の数 [!DNL Identity Service]. |
| **[!UICONTROL 作成されたグラフ]** | で作成された新しい ID グラフの数 [!DNL Identity Service]. |
| **[!UICONTROL 更新されたグラフ]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL 失敗したデータフローの合計]** | 失敗したデータフロー実行の数。 |

フィルターアイコンを選択します。 ![フィルター](../assets/ui/monitor-identities/filter.png) を追加し、 [!DNL Identity] データフローの実行。

![フィルターアイコンがハイライト表示されます。 このアイコンを選択すると、選択したデータフローに関する詳細を表示できます。](../assets/ui/monitor-identities/dataflows-filter.png)

この [!UICONTROL データフロー実行の詳細] ページには、 [!DNL Identity] データフローの実行（組織 ID とデータフローの実行 ID を含む）。 また、このページには、 [!DNL Identity Service]は、取得プロセスでエラーが発生した場合に使用します。

![選択したデータフローに関する詳細情報を示すダッシュボードが表示されます。](../assets/ui/monitor-identities/dataflow-run-details.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL 受信したレコード]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL 失敗したレコード]** | データのエラーが原因で Platform に取り込まれなかったレコードの数。 |
| **[!UICONTROL スキップされたレコード]** | 取り込まれたが、に取り込まれなかったレコードの数 [!DNL Identity Service] レコード行には識別子が 1 つしかなかったので |
| **[!UICONTROL 取り込まれたレコード]** | に取り込まれたレコードの数 [!DNL Identity Service]. |
| **[!UICONTROL 追加された ID]** | に追加された新しい識別子の数 [!DNL Identity Service]. |
| **[!UICONTROL 作成されたグラフ]** | で作成された新しい ID グラフの数 [!DNL Identity Service]. |
| **[!UICONTROL 更新されたグラフ]** | 新しいエッジで更新された既存の ID グラフの数。 |
| **[!UICONTROL ステータス]** | データフローの全体的なステータスを定義します。 可能なステータス値は次のとおりです。 <ul><li>`Success`:データフローがアクティブで、提供されたスケジュールに従ってデータを取り込んでいることを示します。</li><li>`Failed`:データフローのアクティベーションプロセスがエラーが原因で中断されたことを示します。 </li><li>`Processing`:データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローを作成した直後に発生します。</li></ul> |
| **[!UICONTROL データフローの実行開始]** | データフローの実行が開始された日時。 |
| **[!UICONTROL 最終更新日]** | データフローが最後に更新された日時。 |
| **[!UICONTROL エラーの概要]** | データフローの実行が失敗した場合は、エラーコードと、データフローの実行が失敗した理由の概要が表示されます。 |
| **[!UICONTROL データフロー実行 ID]** | データフロー実行の ID。 |
| **[!UICONTROL IMS org ID]** | データフローの実行が属する組織 ID。 |

また、切り替えを選択して、失敗したレコードやスキップされたレコードを表示することもできます。 「エラー」セクションには、エラーコードに関する詳細と、失敗または除外されたレコードの数が含まれます。

---
keywords: Experience Platform；ホーム；人気のトピック；プロファイルのモニター；データフローのモニター；データフロー；プロファイル；リアルタイム顧客プロファイル；
description: リアルタイム顧客プロファイルを使用すると、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。 このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用し、プロファイルでデータフローを監視する方法について説明します。
title: UI でのプロファイルのデータフローの監視
type: Tutorial
exl-id: 00b624b2-f6d1-4ef2-abf2-52cede89b684
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 16%

---

# UI でのプロファイルのデータフローの監視

リアルタイム顧客プロファイルを使用すると、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。 プロファイルを使用すると、顧客データを統合ビューに統合して、顧客インタラクションごとにアクションにつながるタイムスタンプ付きアカウントを提供できます。

監視ダッシュボードを使用すると、データのプロファイルのステータスなど、プロファイル内のデータのアクティビティを視覚的に表現できます。 このチュートリアルでは、モニタリングダッシュボードを使用してExperience Platform ユーザーインターフェイスでデータのプロファイルをモニタリングし、プロファイル処理のステータスをトラッキングする方法について説明します。

## はじめに {#getting-started}

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [&#x200B; データフロー &#x200B;](../home.md)：データフローは、Experience Platform間でデータを移動するデータジョブを表します。 データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   - [データフロー実行](../../sources/notifications.md)：データフロー実行は、選択したデータフローの頻度設定に基づいて繰り返しスケジュールされたジョブです。
- [リアルタイム顧客プロファイル](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

## プロファイル監視ダッシュボード {#profile-metrics}

>[!CONTEXTUALHELP]
>id="platform_monitoring_profile_processing"
>title="プロファイル処理"
>abstract="プロファイル処理ビューには、プロファイルサービスに取り込まれたレコードに関する情報 (作成されたプロファイルフラグメントの数、更新されたプロファイルフラグメントの数、プロファイルフラグメントの総数など) が含まれます。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_monitoring_dataflow_run_details_profile"
>title="データフロー実行の詳細"
>abstract="データフロー実行の詳細ページには、プロファイルデータフロー実行に関する詳しい情報 (組織 ID やデータフロー実行 ID など) が表示されます。"

**[!UICONTROL Profiles]** ダッシュボードにアクセスするには、左側のナビゲーションで「**[!UICONTROL Monitoring]**」を選択します。 **[!UICONTROL Monitoring]** ページで、**[!UICONTROL Profiles]** カードを選択します。

![&#x200B; プロファイルカード。 受信したレコード数、作成および更新されたプロファイルフラグメント数、成功率に関する情報が表示されます。](../assets/ui/monitor-profiles/focus-card.png)

メインの **[!UICONTROL Profiles]** ダッシュボードの **[!UICONTROL Profiles]** カードには、受信したレコードの合計数、作成および更新されたプロファイルフラグメントの数、作成および更新されたプロファイルフラグメントの成功率に関する情報が表示されます。

ダッシュボード自体には、プロファイル処理に関する指標が含まれています。 デフォルトでは、ダッシュボードには、過去 24 時間の組織のソースに関するプロファイル処理の詳細が表示されます。

![&#x200B; プロファイルダッシュボード。 ソースごとに受信したプロファイルレコード数に関する情報が表示されます。](../assets/ui/monitor-profiles/sources.png)

[!UICONTROL Profile processing] ページには、[!DNL Profile] に取り込まれたレコードに関する情報（作成されたプロファイルフラグメントの数、更新されたプロファイルフラグメントの数、プロファイルフラグメントの合計数など）が含まれています。

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL Source name]** | ソースの名前。 |
| **[!UICONTROL Records received]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL Records failed]** | 取り込まれたが、エラーが原因で [!DNL Profile] に取り込まれなかったレコードの数。 |
| **[!UICONTROL Profile fragments created]** | 追加された新しい [!DNL Profile] フラグメントの数です。 |
| **[!UICONTROL Profile fragments updated]** | 既存の [!DNL Profile] フラグメントの更新数です。 |
| **[!UICONTROL Total Profile fragments]** | 更新された既存のすべての [!DNL Profile] フラグメントと作成された新しい [!DNL Profile] フラグメントを含む、[!DNL Profile] に書き込まれたレコードの合計数です。 |
| **[!UICONTROL Total failed dataflows]** | 失敗したデータフロー実行の数。 |

ソース名の横にあるフィルターアイコン ![&#x200B; フィルターアイコン &#x200B;](/help/images/icons/filter.png) を選択して、選択したソースのデータフローのプロファイル処理情報を確認できます。

![&#x200B; フィルターアイコンがハイライト表示されています。 このアイコンを選択すると、選択したソースのデータフローを表示できます。](../assets/ui/monitor-profiles/sources-filter.png)

または、切替スイッチで **[!UICONTROL Dataflows]** を選択して、過去 24 時間の組織のデータフローのプロファイル処理の詳細を表示できます。

![&#x200B; プロファイルダッシュボード。 データフローごとに受信したプロファイルレコード数に関する情報が表示されます。](../assets/ui/monitor-profiles/dataflows.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL Dataflow]** | データフローの名前。 |
| **[!UICONTROL Dataset]** | データフローを挿入するデータセットの名前。 |
| **[!UICONTROL Source name]** | データフローが属するソースの名前。 |
| **[!UICONTROL Records received**] | データレイクから受信したレコードの数。 |
| **[!UICONTROL Records failed]** | 取り込まれたが、エラーが原因で [!DNL Profile] に取り込まれなかったレコードの数。 |
| **[!UICONTROL Profile fragments created]** | 追加された新しい [!DNL Profile] フラグメントの数です。 |
| **[!UICONTROL Profile fragments updated]** | 更新された既存の [!DNL Profile] フラグメントの数 |
| **[!UICONTROL Total Profile fragments]** | 更新された既存のすべての [!DNL Profile] フラグメントと作成された新しい [!DNL Profile] フラグメントを含む、[!DNL Profile] に書き込まれたレコードの合計数です。 |
| **[!UICONTROL Total failed flow runs]** | 失敗したデータフロー実行の数。 |
| **[!UICONTROL Last active]** | 前回データフローが実行されたタイムスタンプ。 |

データフロー実行開始時間の横にあるフィルターアイコン ![&#x200B; フィルター &#x200B;](/help/images/icons/filter.png) を選択して、[!DNL Profile] のデータフロー実行に関する詳細を表示します。

![&#x200B; フィルターアイコンがハイライト表示されています。 このアイコンを選択すると、選択したデータフローの詳細を表示できます。](../assets/ui/monitor-profiles/dataflows-filter.png)

[!UICONTROL Dataflow run details] ページには、[!DNL Profile] データフロー実行に関する詳細（組織 ID やデータフロー実行 ID など）が表示されます。 また、このページには、取り込みプロセスでエラーが発生した場合に、[!DNL Profile] から提供される対応するエラーコードとエラーメッセージも表示されます。

![&#x200B; 選択したデータフローに関する詳細情報を示すダッシュボードが表示されます。](../assets/ui/monitor-profiles/dataflow-run-details.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| -------| ----------- |
| **[!UICONTROL Records received]** | データレイクから受信したレコードの数。 |
| **[!UICONTROL Records failed]** | 取り込まれたが、エラーが原因で [!DNL Profile] に取り込まれなかったレコードの数。 |
| **[!UICONTROL Profile fragments created]** | 追加された新しい [!DNL Profile] フラグメントの数です。 |
| **[!UICONTROL Profile fragments updated]** | 既存の [!DNL Profile] フラグメントの更新数です。 |
| **[!UICONTROL Status]** | データフローの全体的なステータスを定義します。 ステータス値には、次のものがあります。 <ul><li>`Success`：データフローがアクティブで、指定したスケジュールに従ってデータを取り込んでいることを示します。</li><li>`Failed`: データフローのアクティブ化プロセスがエラーによって中断されたことを示します。 </li><li>`Processing`：データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローを作成した直後に発生します。</li></ul> |
| **[!UICONTROL Dataflow run start]** | データフローの実行が開始された日時。 |
| **[!UICONTROL Last updated]** | データフローが最後に更新された日時。 |
| **[!UICONTROL Error summary]** | データフローの実行に失敗した場合は、エラーコードと、データフローの実行が失敗した理由の概要が表示されます。 |
| **[!UICONTROL Dataflow run ID]** | データフロー実行の ID。 |
| **[!UICONTROL IMS org ID]** | データフロー実行が属する組織 ID。 |

さらに、切替スイッチを選択して、失敗したレコードとスキップされたレコードを表示できます。 「エラー」セクションには、エラーコードに関する詳細と、失敗または除外されたレコードの数が表示されます。

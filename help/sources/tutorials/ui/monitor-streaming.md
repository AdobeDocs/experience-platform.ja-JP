---
keywords: Experience Platform；ホーム；人気のトピック；アカウントの監視；データフローの監視；データフロー
description: Adobe Experience PlatformのSource コネクタには、外部ソースのデータをスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、ソース ワークスペースからのストリーミングデータフローを監視する手順を説明します。
title: UI でのストリーミングソースのデータフローの監視
exl-id: b080e398-e71f-40bd-aea1-7ea3ce86b55d
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1041'
ht-degree: 25%

---

# UI でのストリーミングソースのデータフローの監視

このチュートリアルでは、[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースを使用してストリーミングソースのデータフローを監視する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ データフロー ](../../../dataflows/home.md)：データフローは、Experience Platform間でデータを移動するデータジョブを表します。 データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   * [データフロー実行](../../notifications.md)：データフロー実行は、選択したデータフローの頻度設定に基づいて繰り返しスケジュールされたジョブです。
* [ ソース ](../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## ストリーミングソースのデータフローの監視

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

ストリーミングソースの既存のデータフローを表示するには、上部のヘッダーから **[!UICONTROL データフロー]** を選択します。

![カタログ](../../images/tutorials/monitor-streaming/catalog.png)

[!UICONTROL &#x200B; データフロー &#x200B;] ページには、ソースデータ、アカウント名、データフロー実行ステータスに関する情報など、組織内の既存のすべてのデータフローのリストが含まれています。

表示するデータフローの名前を選択します。

![ データフロー ](../../images/tutorials/monitor-streaming/dataflows.png)

データフロー実行ステータスについて詳しくは、次の表を参照してください。

| ステータス | 説明 |
| ------ | ----------- |
| 完了 | 「`Completed`」ステータスは、対応するデータフロー実行のすべてのレコードが 1 時間以内に処理されたことを示します。 データフロー実行のエラーは、`Completed` ステータスに含まれることがあります。 |
| 成功 | 「`Success`」ステータスは、対応するデータフロー実行のすべてのレコードが 1 時間以内に処理され、データフローの実行中にエラーが発生しなかったことを示します。 |
| 処理中 | 「`Processing`」ステータスは、データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローを作成した直後に発生します。 |
| エラー | 「`Error`」ステータスは、データフローのアクティブ化プロセスが中断されたことを示します。 |
| 実行されていません | 「`No runs`」ステータスは、データフローは作成されたが、データフロー実行は開始されていないことを示します。 |

[!UICONTROL &#x200B; データフローアクティビティ &#x200B;] ページには、ストリーミングデータフローに関する特定の情報が表示されます。 上部のバナーには、取り込まれたレコードと、選択した日付範囲でのすべてのストリーミングデータフロー実行に対して失敗したレコードの累積数が含まれています。

![dataflow-activity](../../images/tutorials/monitor-streaming/dataflow-activity.png)

デフォルトでは、表示されるデータには、過去 7 日間の取り込みレートが含まれています。 表示されるレコードの時間枠を調整するには、「**[!UICONTROL 過去 7 日間]**」を選択します。

カレンダーポップアップウィンドウが表示され、代替取り込み時間枠のオプションが表示されます。 データフロー実行時間枠を設定して、過去 7 日間または過去 30 日間のフロー実行を表示できます。 または、選択したカスタム時間枠を設定するようにインタラクティブカレンダーを設定します。 完了したら、「**[!UICONTROL 適用]**」を選択します。

![ カレンダー ](../../images/tutorials/monitor-streaming/calendar.png)

ページの下半分には、フロー実行ごとに受信、取り込み、失敗したレコードの数に関する情報が表示されます。 各フロー実行は、1 時間ごとの期間内に記録されます。

![ データフロー実行 ](../../images/tutorials/monitor-streaming/dataflow-run.png)

### データフロー実行指標 {#dataflow-run-metrics}

>[!CONTEXTUALHELP]
>id="platform_sources_dataflow_records_received"
>title="受信したレコード"
>abstract="受信したレコードの指標は、データフローで受信したレコードの合計数を示します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_dataflow_records_ingested"
>title="取り込まれたレコード"
>abstract="取り込まれたレコードの指標は、データレイクに取り込まれたレコードの合計数を示します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_dataflow_records_failed"
>title="失敗したレコード"
>abstract="失敗したレコードの指標は、データのエラーが原因でデータレイクに取り込まれなかったレコードの合計数を示します。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_dataflow_records_warnings"
>title="警告のあるレコード"
>abstract="警告のあるレコードの指標は、マッパー変換警告を伴って取り込まれたレコードの合計数を示します。マッパー変換エラーはすべて警告としてレポートされます。部分的に取り込まれた行は、警告を伴う成功と見なされます。"
>text="Learn more in documentation"

個々のデータフロー実行ごとに、次の詳細が表示されます。

* **[!UICONTROL データフロー実行開始]**：データフロー実行が開始された時刻です。
* **[!UICONTROL 処理時間]**：データフローが処理されるまでにかかった時間です。
* **[!UICONTROL 受信したレコード]**：ソースコネクタからデータフローで受信したレコードの合計数です。
* **[!UICONTROL 取り込まれたレコード]**:[!DNL Data Lake] に取り込まれたレコードの合計数です。
* **[!UICONTROL 警告のあるレコード]**：取り込まれた警告のあるレコードの合計数です。 マッパー変換エラーはすべて警告としてレポートされ、部分的に取り込まれた行には、警告を含む `success` というラベルが付けられます。 **メモ**：警告を含むレコードの取り込みのサポートは、ストリーミングソースでのみ使用できます。
* **[!UICONTROL 失敗したレコード]**：データのエラーが原因で [!DNL Data Lake] に取り込まれなかったレコードの数。
* **[!UICONTROL 取り込みレート]**:[!DNL Data Lake] に取り込まれたレコードの成功率です。 この指標は、[!UICONTROL &#x200B; 部分取り込み &#x200B;] が有効な場合に適用されます。
* **[!UICONTROL ステータス]**：データフローの状態（[!UICONTROL 完了]または[!UICONTROL 処理中]）を表します。[!UICONTROL &#x200B; 完了 &#x200B;] は、対応するデータフロー実行のすべてのレコードが 1 時間以内に処理されたことを意味します。 [!UICONTROL 処理中]は、データフロー実行がまだ終了していないことを意味します。

[!UICONTROL &#x200B; データフロー実行の概要 &#x200B;] ページには、対応するデータフロー実行 ID、ターゲットデータセット、組織 ID など、データフローに関する追加情報が含まれています。

エラーを含むフロー実行には、実行の失敗につながった特定のエラーと失敗したレコードの合計数を表示する [!UICONTROL &#x200B; データフロー実行エラー &#x200B;] パネルも含まれています。

![dataflow-run-overview](../../images/tutorials/monitor-streaming/dataflow-run-overview.png)

### 警告のあるレコードの表示 {#warnings}

[!UICONTROL &#x200B; 警告のあるレコード &#x200B;] フロー実行中に発生したマッパー変換警告のリストを表示します。 部分的に取り込まれた行は成功と見なされ、マッパー変換エラーが見つかった場合は警告が追加されます。

デフォルトでは、次の場合を除き、すべてのマッパー変換エラーは警告と見なされます。

* 構文エラー
* 存在しない属性への参照
* XDM データタイプの不一致

エラー診断を表示するには、「**[!UICONTROL エラー診断をプレビュー]**」を選択します。

![ 警告を含むレコード ](../../images/tutorials/monitor-streaming/records-with-warnings.png)

[!UICONTROL &#x200B; エラー診断のプレビュー &#x200B;] ウィンドウでは、データフローの実行に関する最大 100 個のエラーや警告をプレビューできます。 ここから、[!DNL Data Access] API を使用して、取り込み失敗マニフェストをダウンロードして詳細を確認することもできます。

![ 診断 ](../../images/tutorials/monitor-streaming/diagnostics.png)

## 次の手順

このチュートリアルでは、[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースを正常に使用して、ストリーミングデータフローを監視し、失敗したデータフローに至ったエラーを特定しました。 詳しくは、次のドキュメントを参照してください。

* [ソースの概要](../../home.md)
* [データフローの概要](../../../dataflows/home.md)

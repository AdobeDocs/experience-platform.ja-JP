---
keywords: Experience Platform；ホーム；人気の高いトピック；モニターアカウント；モニターデータフロー；データフロー；ソース
description: このチュートリアルでは、集計された監視表示とクロスサービス監視の両方を使用して、データフローを監視する手順を説明します。
solution: Experience Platform
title: UIでのソースのデータフローの監視
topic-legacy: overview
type: Tutorial
exl-id: 53fa4338-c5f8-4e1a-8576-3fe13d930846
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1644'
ht-degree: 6%

---

# UIのソースのデータフローの監視

Adobe Experience Platformでは、様々なソースからデータを取り込み、Experience Platform内で分析し、様々な目的地に活性化する。 プラットフォームでは、データフローに透明性を提供することで、この非線形の可能性があるデータフローの追跡プロセスを容易にします。

監視ダッシュボードは、データフローのジャーニーを視覚的に表します。 集計された監視表示を使用して、ソース・レベル、データ・フロー、データ・フロー実行に垂直に移動でき、データ・フローの成功または失敗に貢献する対応する指標を表示できます。 また、監視ダッシュボードのクロスサービス監視機能を使用して、ソースから[!DNL Identity Service]、[!DNL Profile]に至るデータフローのジャーニーを監視することもできます。

このチュートリアルでは、集計された監視表示とクロスサービス監視の両方を使用して、データフローを監視する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [データフロー](../home.md):データフローは、プラットフォーム間でデータを移動するデータ・ジョブを表します。データフローは様々なサービスで構成され、ソースコネクタからターゲットデータセット、[!DNL Identity]と[!DNL Profile]、[!DNL Destinations]にデータを移動できます。
   * [Dataflowの実行](../../sources/notifications.md):Dataflowの実行は、選択したデータフローの頻度構成に基づく定期的なスケジュールジョブです。
* [ソース](../../sources/home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [ID サービス](../../identity-service/home.md)：デバイスやシステム間で ID を関連付けることで、個々の顧客とそのビヘイビアーへの理解を深めることができます。
* [リアルタイム顧客プロファイル](../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## 集計された監視表示

[プラットフォームUI](https://platform.adobe.com)で、左側のナビゲーションから「**[!UICONTROL 監視]**」を選択して、[!UICONTROL 監視]ダッシュボードにアクセスします。 [!UICONTROL 監視]ダッシュボードには、ソースから[!DNL Identity Service]へのデータトラフィックの正常性、および[!DNL Profile]へのデータトラフィックの正常性のインサイトを含む、すべてのソースデータフローに関する指標と情報が含まれます。

ダッシュボードの中央には[!UICONTROL ソースインジェスト]パネルがあり、このパネルには、取り込まれたレコードと失敗したレコードに関するデータを表示する指標とグラフが含まれています。

![監視ダッシュボード](../assets/ui/monitor-sources/monitoring-dashboard.png)

デフォルトでは、表示されるデータには、過去24時間の取り込み率が含まれています。 「**[!UICONTROL 過去24時間]**」を選択して、表示されるレコードの時間枠を調整します。

![change-date](../assets/ui/monitor-sources/change-date.png)

カレンダーのポップアップウィンドウが表示され、代替の取り込み時間枠のオプションが表示されます。 「**[!UICONTROL 過去30日間]**」を選択し、「**[!UICONTROL 適用]**」を選択します

![adjust-time-frame](../assets/ui/monitor-sources/adjust-timeframe.png)

グラフはデフォルトで有効になっており、グラフを無効にして下のソースのリストを拡張できます。 「**[!UICONTROL 指標とグラフ]**」トグルを選択して、グラフを無効にします。

![指標とグラフ](../assets/ui/monitor-sources/metrics-graphs.png)

| ソースの取り込み | 説明 |
| ---------------- | ----------- |
| [!UICONTROL 取り込まれた記録  ] | 取り込まれたレコードの合計数です。 |
| [!UICONTROL レコードが失敗しました] | データのエラーが原因で取り込まれなかったレコードの合計数です。 |
| [!UICONTROL 失敗したデータフローの合計] | `failed`状態のデータフローの合計数です。 |

ソースインジェストリストには、少なくとも1つの既存のアカウントを含むすべてのソースが表示されます。 また、リストには、適用した時間枠に基づく各ソースの取り込み率、失敗したレコード数、失敗したデータフローの合計数に関する情報も含まれます。

![摂取源](../assets/ui/monitor-sources/source-ingestion.png)

ソースのリストを並べ替えるには、[**[!UICONTROL マイソース]**]を選択し、ドロップダウンメニューからカテゴリを選択します。 例えば、クラウドストレージに重点を置く場合は、「**[!UICONTROL クラウドストレージ]**」を選択します。

![カテゴリ順](../assets/ui/monitor-sources/sort-by-category.png)

すべてのソースの既存のデータフローを表示するには、**[!UICONTROL データフロー]**&#x200B;を選択します。

![表示全データフロー](../assets/ui/monitor-sources/view-all-dataflows.png)

または、検索バーにソースを入力して、1つのソースを分離することもできます。 ソースを識別したら、横にあるフィルタ・アイコン![フィルタ](../assets/ui/monitor-sources/filter.png)を選択して、アクティブなデータ・フローのリストを表示します。

![search](../assets/ui/monitor-sources/search.png)

データフローのリストが表示されます。 リストを絞り込み、エラーのあるデータフローに焦点を当てるには、**[!UICONTROL エラーのみを表示]**&#x200B;を選択します。

![show-failures-only](../assets/ui/monitor-sources/show-failures-only.png)

監視するデータフローを見つけ、その横にあるフィルタアイコン![filter](../assets/ui/monitor-sources/filter.png)を選択して、実行ステータスの詳細を確認します。

![dataflow](../assets/ui/monitor-sources/dataflow.png)

データフロー実行ページには、データフローの実行開始日、データのサイズ、ステータス、および処理時間に関する情報が表示されます。 データフロー実行開始時間の横にあるフィルタアイコン![filter](../assets/ui/monitor-sources/filter.png)を選択して、そのデータフロー実行の詳細を表示します。

![データフロー実行開始](../assets/ui/monitor-sources/dataflow-run-start.png)

[!UICONTROL Dataflow run details]ページには、データフローのメタデータ、部分的なインジェストの状態、およびエラー概要に関する情報が表示されます。 エラーサマリには、インジェストプロセスでエラーが発生したステップを示す、特定の最上位レベルのエラーが含まれます。

下にスクロールすると、発生したエラーに関する詳細情報が表示されます。

![dataflow-run-details](../assets/ui/monitor-sources/dataflow-run-details.png)

[!UICONTROL Dataflow run errors]パネルに、データフローのインジェストエラーを引き起こした特定のエラーおよびエラーコードが表示されます。 このシナリオでは、マッパー変換エラーが発生し、結果として24件のレコードが失敗しました。

詳しくは、「**[!UICONTROL ファイル]**」を選択してください。

![dataflow-run-errors](../assets/ui/monitor-sources/dataflow-run-errors.png)

[!UICONTROL ファイル]パネルには、ファイルの名前とパスに関する情報が含まれます。

エラーをより詳細に表すには、**[!UICONTROL プレビューエラー診断]**&#x200B;を選択します。

![files](../assets/ui/monitor-sources/files.png)

「[!UICONTROL Error diagnosticsプレビュー]」ウィンドウが開き、データフローに最大100個のエラーのプレビューが表示されます。 「**[!UICONTROL ダウンロード]**」を選択してcurlコマンドを取得し、エラー診断をダウンロードできます。

終了したら、「**[!UICONTROL 閉じる]**」を選択します。

![エラー診断](../assets/ui/monitor-sources/error-diagnostics.png)

上部のヘッダーにあるパンくずリストを使用して、[!UICONTROL 監視]ダッシュボードに戻ることができます。 **[!UICONTROL 実行開始を選択：2/14/2021, 9:47 PM]**&#x200B;で前のページに戻り、**[!UICONTROL Dataflow:忠誠度データ取り込みデモ —]**&#x200B;でデータフローページに戻れませんでした。

![パンくず](../assets/ui/monitor-sources/breadcrumbs.png)

## クロスサービスの監視

ダッシュボードの上部は、ソースレベルから[!DNL Identity Service]、[!DNL Profile]への取り込みフローを表しています。 各セルには、取り込みのその段階で発生したエラーの存在を示すドットマーカーが含まれています。 緑の点はエラーのない取り込みを意味し、赤の点は取り込みの特定の段階でエラーが発生したことを意味します。

![クロスサービス監視](../assets/ui/monitor-sources/cross-service-monitoring.png)

データフローページから、成功したデータフローを探し、その横にあるフィルタ・アイコン![filter](../assets/ui/monitor-sources/filter.png)を選択して、そのデータフロー実行情報を確認します。

![データフロー成功](../assets/ui/monitor-sources/dataflow-success.png)

[!UICONTROL ソースインジェスト]ページには、データフローの正常なインジェストを確認する情報が含まれています。 ここから、ソースレベルから[!DNL Identity Service]、[!DNL Profile]にデータフローのジャーニーを監視する開始を設定できます。

**[!UICONTROL ID]**&#x200B;を選択して、[!UICONTROL ID]ステージで取り込みを確認します。

![ソース](../assets/ui/monitor-sources/sources.png)

### [!DNL Identity] 指標

[!UICONTROL ID処理]ページには、[!DNL Identity Service]に取り込まれるレコードに関する情報（追加されたIDの数、作成されたグラフ、更新されたグラフの数など）が含まれます。

dataflow実行開始時間の横にあるフィルタアイコン![filter](../assets/ui/monitor-sources/filter.png)を選択し、[!DNL Identity]データフロー実行の詳細を表示します。

![ID](../assets/ui/monitor-sources/identities.png)

| ID指標 | 説明 |
| ---------------- | ----------- |
| [!UICONTROL 受信したレコード] | [!DNL Data Lake]から受信したレコードの数です。 |
| [!UICONTROL レコードが失敗しました] | データのエラーが原因でプラットフォームに取り込まれなかったレコードの数。 |
| [!UICONTROL スキップされたレコード] | レコード行に識別子が1つしかないため、[!DNL Identity Service]に取り込まれなかったレコードの数です。 |
| [!UICONTROL 取り込まれた記録] | [!DNL Identity Service]に取り込まれたレコードの数です。 |
| [!UICONTROL 合計レコード数] | 失敗したレコード、スキップしたレコード、[!DNL Identities]が追加されたレコード、重複したレコードを含む、すべてのレコードの合計数です。 |
| [!UICONTROL IDの追加] | [!DNL Identity Service]に追加された新しい識別子の数です。 |
| [!UICONTROL グラフの作成] | [!DNL Identity Service]で作成された新しいグラフの数。 |
| [!UICONTROL グラフの更新] | 新しいエッジで更新された既存のIDグラフの数。 |
| [!UICONTROL 失敗したデータフローの実行] | 失敗したデータフローの実行数。 |
| [!UICONTROL 処理時間] | 取り込みの開始から完了までのタイムスタンプ。 |
| [!UICONTROL ステータス] | データフローの全体的なステータスを定義します。 使用可能なステータス値は次のとおりです。 <ul><li>`Success`:データフローがアクティブで、提供されたスケジュールに従ってデータを取り込んでいることを示します。</li><li>`Failed`:データフローのアクティベーションプロセスがエラーによって中断されたことを示します。 </li><li>`Processing`:データフローがまだアクティブでないことを示します。このステータスは、多くの場合、新しいデータフローの作成直後に発生します。</li></ul> |

[!UICONTROL Dataflow run details]ページに、[!DNL Identity]データフロー実行のIMS組織IDやデータフロー実行IDなど、その他の情報が表示されます。 また、このページには、[!DNL Identity Service]が提供する対応するエラーコードとエラーメッセージも表示されます。これは、インジェストプロセスでエラーが発生した場合に表示されます。

**[!UICONTROL 実行開始を選択：2/14/2021, 9:47 PM]**&#x200B;をクリックして、前のページに戻ります。

![identitys-dataflow-run](../assets/ui/monitor-sources/identities-dataflow-run.png)

[!UICONTROL ID処理]ページで、**[!UICONTROL プロファイル]**&#x200B;を選択し、[!UICONTROL プロファイル]ステージのレコード取り込みの状態を確認します。

![select-プロファイル](../assets/ui/monitor-sources/select-profiles.png)

### [!DNL Profile] 指標

[!UICONTROL プロファイル処理]ページには、[!DNL Profile]に取り込まれるレコードに関する情報(作成されたプロファイルフラグメントの数、更新されたプロファイルフラグメント、プロファイルフラグメントの合計数など)が含まれます。

dataflow実行開始時間の横にあるフィルタアイコン![filter](../assets/ui/monitor-sources/filter.png)を選択し、[!DNL Profile]データフロー実行の詳細を表示します。

![プロファイル](../assets/ui/monitor-sources/profiles.png)

| プロファイル指標 | 説明 |
| --------------- | ----------- |
| [!UICONTROL 受信したレコード] | [!DNL Data Lake]から受信したレコードの数です。 |
| [!UICONTROL レコードが失敗しました  ] | エラーのため[!DNL Profile]に取り込まれずに取り込まれたレコードの数です。 |
| [!UICONTROL プロファイルフラグメントの追加] | 追加された新しい[!DNL Profile]フラグメントの数です。 |
| [!UICONTROL プロファイルフラグメントの更新] | 更新された既存の[!DNL Profile]フラグメントの数 |
| [!UICONTROL プロファイルフラグメントの合計] | [!DNL Profile]に書き込まれたレコードの総数です。既存の[!DNL Profile]フラグメントが更新され、新しく作成された[!DNL Profile]フラグメントも含まれます。 |
| [!UICONTROL 失敗したデータフローの実行] | 失敗したデータフローの実行数。 |
| [!UICONTROL 処理時間] | 取り込みの開始から完了までのタイムスタンプ。 |
| [!UICONTROL ステータス] | データフローの全体的なステータスを定義します。 使用可能なステータス値は次のとおりです。 <ul><li>`Success`:データフローがアクティブで、提供されたスケジュールに従ってデータを取り込んでいることを示します。</li><li>`Failed`:データフローのアクティベーションプロセスがエラーによって中断されたことを示します。 </li><li>`Processing`:データフローがまだアクティブでないことを示します。このステータスは、多くの場合、新しいデータフローの作成直後に発生します。</li></ul> |

[!UICONTROL Dataflow run details]ページに、[!DNL Profile]データフロー実行のIMS組織IDやデータフロー実行IDなど、その他の情報が表示されます。 また、このページには、[!DNL Profile]が提供する対応するエラーコードとエラーメッセージも表示されます。これは、インジェストプロセスでエラーが発生した場合に表示されます。

![プロファイル — データフロー実行](../assets/ui/monitor-sources/profiles-dataflow-run.png)

## 次の手順

このチュートリアルに従うと、**[!UICONTROL Monitoring]**&#x200B;ダッシュボードを使用して、ソースレベルから[!DNL Identity Service]、[!DNL Profile]へのインジェストデータフローを正常に監視できます。 また、インジェスト処理中にデータフローの失敗に寄与したエラーも正常に識別されました。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../profile/home.md)
* [Data Science ワークスペースの概要](../../data-science-workspace/home.md)

---
keywords: Experience Platform；ホーム；人気の高いトピック；モニターアカウント；モニターデータフロー；データフロー；ソース
description: Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ソース・ワークスペースから既存のデータ・フローを表示する手順を説明します。
solution: Experience Platform
title: UIでのソースのデータフローの監視
topic: overview
type: Tutorial
translation-type: tm+mt
source-git-commit: f8186e467dc982003c6feb01886ed16d23572955
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 4%

---


# UIのソースのデータフローの監視

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!UICONTROL ソース]ワークスペースから既存のデータフローを表示する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [ソース](../../sources/home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## データフローの監視

[Experience PlatformUI](https://platform.adobe.com)にログインし、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択して[!UICONTROL ソース]ワークスペースにアクセスします。 上部のヘッダーから「**[!UICONTROL Dataflows]**」を選択し、既存のデータフローを表示します。

![カタログ・データ・フロー](../assets/ui/monitor-sources/catalog-dataflows.png)

既存のデータフローのリストが表示されます。 このページには、ソース、ユーザー名、データフロー数およびステータスに関する情報を含む、表示可能なデータフローのリストが表示されます。

ステータスについて詳しくは、次の表を参照してください。

| ステータス | 説明 |
| ------ | ----------- |
| 有効 | `Enabled`ステータスは、データフローがアクティブであり、提供されたスケジュールに従ってデータを取り込んでいることを示します。 |
| 無効 | `Disabled`ステータスは、データフローが非アクティブで、データを取り込んでいないことを示します。 |
| Processing | `Processing`ステータスは、データフローがまだアクティブでないことを示します。 このステータスは、多くの場合、新しいデータフローの作成直後に発生します。 |
| エラー | `Error`ステータスは、データフローのアクティベーションプロセスが中断されたことを示します。 |

左上のファネルアイコンを選択して並べ替えます。

![データフローリスト](../assets/ui/monitor-sources/dataflows-list.png)

並べ替えパネルが表示されます。 スクロール・メニューからアクセスするソースを選択し、右側のリストからデータ・フローを選択します。 「三点リーダー(`...`)」ボタンを選択して、選択したデータフローで利用可能なオプションを表示することもできます。

![並べ替えデータフロー](../assets/ui/monitor-sources/dataflows-sort.png)

**[!UICONTROL Dataflowアクティビティ]**&#x200B;ページには、失敗したレコードと取り込まれたレコードの数、およびデータフローの状態と処理時間に関する情報が含まれています。 データフローの上にあるカレンダーアイコンを選択して、インジェストレコードの時間枠を調整します。

![dataflow-アクティビティ](../assets/ui/monitor-sources/dataflow-activity.png)

カレンダーを使用すると、取り込むレコードに応じて異なる時間枠を表示できます。 「[!UICONTROL 最近の7日間]」または「[!UICONTROL 最近の30日間]」の2つの事前設定されたオプションのいずれかを選択できます。 または、カレンダーを使用してカスタムの期間を設定できます。 選択した期間を選択し、「**[!UICONTROL 適用]**」を選択して続行します。

![フローカレンダー](../assets/ui/monitor-sources/flow-calendar.png)

デフォルトでは、**[!UICONTROL Dataflowアクティビティ]**&#x200B;には、データフローに関連付けられた&#x200B;**[!UICONTROL プロパティ]**&#x200B;パネルが表示されます。 リストからフロー実行を選択し、固有の実行IDに関する情報を含む、関連するメタデータを表示します。

**[!UICONTROL Dataflow run overview]**&#x200B;にアクセスするには、**[!UICONTROL Dataflow run開始]**&#x200B;を選択します。

![runs](../assets/ui/monitor-sources/run-metadata.png)

**[!UICONTROL Dataflow run overview]**&#x200B;には、メタデータ、部分的な取り込みステータス、割り当てられたエラーしきい値など、データフローに関する情報が表示されます。 上部のヘッダーには、エラーの概要も含まれます。 **[!UICONTROL エラーの概要]**&#x200B;には、インジェストプロセスでエラーが発生したステップを示す、特定の最上位レベルのエラーが含まれています。

![dataflow-run-overview](../assets/ui/monitor-sources/dataflow-run-overview.png)

**[!UICONTROL エラーの概要]**&#x200B;で確認できるエラーについては、次の表を参照してください。

| エラー | 説明 |
| ---------- | ----------- |
| `CONNECTOR-1001-500` | ソースからデータをコピー中にエラーが発生しました。 |
| `CONNECTOR-2001-500` | コピーされたデータを[!DNL Platform]に処理中にエラーが発生しました。 このエラーは、解析、検証または変換に関するものです。 |

画面の下半分には、**[!UICONTROL Dataflow run errors]**&#x200B;に関する情報が含まれます。 ここから、取り込んだファイルの表示、プレビューおよびダウンロードのエラー診断、またはファイルマニフェストのダウンロードを行うこともできます。

**[!UICONTROL Dataflow run errors]**&#x200B;セクションには、エラーコード、失敗したレコード数、エラーを説明する情報が表示されます。

**[!UICONTROL プレビューエラー診断]**&#x200B;を選択して、インジェストエラーの詳細を表示します。

![データフロー実行エラー](../assets/ui/monitor-sources/dataflow-run-errors.png)

**[!UICONTROL エラー診断プレビュー]**&#x200B;パネルが表示されます。 この画面には、ファイル名、エラーコード、エラーが発生した列の名前、エラーの説明など、インジェストエラーに関する具体的な情報が表示されます。

この節では、エラーを含む列のプレビューも説明します。

>[!IMPORTANT]
>
>**[!UICONTROL エラー診断プレビュー]**&#x200B;を有効にするには、データフローの設定時に、**[!UICONTROL 部分的なインジェスト]**&#x200B;と&#x200B;**[!UICONTROL エラー診断]**&#x200B;をアクティブにする必要があります。 これを行うと、フローの実行中に取り込まれたすべてのレコードをスキャンできます。

![プレビューエラー診断](../assets/ui/monitor-sources/preview-error-diagnostics.png)

エラーをプレビューした後、**[!UICONTROL dataflow runs overview]**&#x200B;パネル内から「**[!UICONTROL Download]**」を選択して、完全なエラー診断にアクセスし、ファイルマニフェストをダウンロードできます。 詳しくは、[エラー診断](../../ingestion/batch-ingestion/partial.md#retrieve-errors)と[メタデータ](../../ingestion/batch-ingestion/partial.md#download-metadata)のダウンロードに関するドキュメントを参照してください。

![プレビューエラー診断](../assets/ui/monitor-sources/download.png)

データフローの監視と取り込みの詳細については、[ストリーミングデータフローの監視](../../ingestion/quality/monitor-data-ingestion.md)のチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、**[!UICONTROL ソース]**&#x200B;ワークスペースから既存のアカウントとデータフローに正常にアクセスできます。 受信データは、[!DNL Real-time Customer Profile]や[!DNL Data Science Workspace]などのダウンストリーム[!DNL Platform]サービスで使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../profile/home.md)
- [Data Science ワークスペースの概要](../../data-science-workspace/home.md)

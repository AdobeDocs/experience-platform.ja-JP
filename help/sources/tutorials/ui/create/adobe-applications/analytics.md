---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での Adobe Analytics ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 41fe3e5b2a830c3182b46b3e0873b1672a1f1b03
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 15%

---


# UI での Adobe Analytics ソースコネクタの作成

このチュートリアルでは、UIで消費者データをAdobe Experience Platformに取り込むためのAdobe Analyticsソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデル（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワーク。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## Adobe Analyticsとのソース接続の作成

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) ソース **** 」を選択してソースワークスペースにアクセスします。 カ *タログ* 画面には、受信接続を作成するために利用できるソースが表示され、各ソースには、それらに関連付けられた既存のアカウント数とデータセットフローが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

[ *Adobeアプリ* ]カテゴリの下で、[ **[!UICONTROL Adobe Analytics]** ]を選択して情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ソースまたは表示のドキュメントに接続するためのオプションが表示されます。 既存のアカウントを表示するには、「 **[!UICONTROL アカウント]**」を選択します。

![](../../../../images/tutorials/create/analytics/catalog.png)

### データの選択

*Adobe Analyticsのステップが表示されます* 。 この画面には、Analyticsで事前に設定されたデータセットフローが表示されます。 「データの **[!UICONTROL 選択]**」をクリックして、新しいデータセットフローを作成できます。

>[!NOTE]
>
>異なるデータを取り込むために、1つのソースに対して複数のインバウンド接続を作成できます。

![](../../../../images/tutorials/create/analytics/dataset-flows.png)

<!---Analytics report suites can be configured for one sandbox at a time. To import the same report suite into a different sandbox, the dataset flow will have to be deleted and instantiated again via configuration for a different sandbox.--->

使用可能なレポートスイートのリストから、プラットフォームに取り込むレポートスイートを選択し、「 **[!UICONTROL 次へ]**」をクリックします。

![](../../../../images/tutorials/create/analytics/select-data.png)

### データセットフローの名前を指定する

データセットフローの詳細 *(* データセットフローの名前と説明)を入力する必要がある手順が表示されます。 「 **[UICONTROL! 終了したら次]** 。

![](../../../../images/tutorials/create/analytics/dataset-flow-detail.png)

### データセットのフローの確認

「 *レビュー* 」の手順が表示され、作成前に新しいAnalyticsのインバウンドデータセットフローを確認できます。 接続の詳細は、次のようなカテゴリ別にグループ化されます。

* *接続*:ソース接続のタイプと選択したレポートスイートが表示されます。
* *データセットとマップのフィールドの割り当て*:その他のソースコネクタを作成する場合、このコンテナには、データセットが適用するスキーマなど、ソースデータが取り込むデータセットが表示されます。 出力スキーマとデータセットは、Analyticsデータセットフローに対して自動的に設定されます。

![](../../../../images/tutorials/create/analytics/review.png)

### データセットフローの監視

データセットフローが作成されたら、データを通じて取り込まれるデータを監視できます。 カタ *ログ* 画面で、「 *データセットフロー* 」を選択して、Analyticsアカウントに関連付けられている確立済みフローのリストを表示します。

![](../../../../images/tutorials/create/analytics/catalog-dataset-flows.png)

[ *データセットフロー* ]画面が表示されます。 このページには、名前、ソースデータ、作成時間およびステータスに関する情報を含むデータセットフローのペアが表示されます。

コネクタは、2つのデータセットフローをインスタンス化します。 一方のフローはバックフィルデータを表し、もう一方のフローはライブデータを表します。 埋め戻しデータはプロファイル用に設定されていませんが、分析およびデータ科学の使用例のためにデータレークに送信されます。

バックフィル、ライブデータおよびそれぞれの待ち時間について詳しくは、 [Analytics Data Connectorの概要を参照してください](../../../../connectors/adobe-applications/analytics.md)。

リストから表示するデータセットフローを選択します。

![](../../../../images/tutorials/create/analytics/backfill.png)

「 *データセットアクティビティ* 」ページが表示されます。 このページには、グラフの形式で消費されるメッセージの割合が表示されます。 上部のヘッダーから *データ管理* (Data Governance)を選択して、ラベル付けフィールドにアクセスします。

![](../../../../images/tutorials/create/analytics/batches.png)

データセットフローに継承されたラベルは、 *Data Governance* 画面から表示できます。 特定のラベルにアクセスするには、右上の「編集」ボタンを選択します。

![](../../../../images/tutorials/create/analytics/data-gov.png)

The *Edit governance labels* panel appears. この画面では、データセットフローの契約、ID、機密ラベルにアクセスして編集できます。

Analyticsからのデータにラベルを付ける方法について詳しくは、『 [データ使用ラベルガイド](../../../../../data-governance/labels/user-guide.md)』を参照してください。

![](../../../../images/tutorials/create/analytics/labels.png)

## 次の手順とその他のリソース

接続が作成されると、ターゲットスキーマとデータセットフローが自動的に作成され、受信データが格納されます。 さらに、データのバックフィルが発生し、最大 13 か月の履歴データが取り込まれます。初回取り込みが完了すると、Analyticsデータが使用され、リアルタイム顧客プロファイルやセグメント化サービスなどの下流のプラットフォームサービスで使用されます。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
* [セグメント化サービスの概要](../../../../../segmentation/home.md)
* [Data Science ワークスペースの概要](../../../../../data-science-workspace/home.md)
* [クエリサービスの概要](../../../../../query-service/home.md)

次のビデオは、Adobe Analyticsソースコネクタを使用した取り込みデータについて理解を深めることを目的としています。

>[!WARNING]
>
> 次のビデオに示す [!DNL Platform] UIは古いです。 最新のUIのスクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/29687?quality=12&learn=on)


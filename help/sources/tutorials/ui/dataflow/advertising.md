---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでの広告コネクタのデータフローの設定
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 1%

---


# UIでの広告コネクタのデータフローの設定

データフローとは、ソースからAdobe Experience Platformデータセットにデータを取得し、取り込むスケジュール済みのタスクです。 このチュートリアルでは、広告アカウントを使用して新しいデータフローを設定する手順を説明します。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

さらに、このチュートリアルでは、既に広告アカウントを作成している必要があります。 UIで異なる支払いコネクタを作成するためのチュートリアルのリストは、 [source connectorsの概要](../../../home.md)。

## データの選択

広告アカウントを作成すると、 *[!UICONTROL データの選択]* 手順が表示され、ファイル階層を調べるためのインタラクティブインターフェイスが提供されます。

- インターフェイスの左半分はディレクトリブラウザーで、サーバーのファイルとディレクトリが表示されます。
- インターフェイスの右半分を使用すると、互換性のあるファイルから最大100行のデータをプレビューできます。

使用するディレクトリを選択し、「 **[!UICONTROL 次へ]**」を選択します。

![add-data](../../../images/tutorials/dataflow/advertising/add-data.png)

## データフィールドのXDMスキーマへのマッピング

「 *[!UICONTROL マッピング]* 」の手順が表示され、ソースデータをデータセットにマッピングするインタラクティブなインターフェイスが提供され [!DNL Platform] ます。

取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

### 既存のデータセットの使用

既存のデータセットにデータを取り込むには、「 **[!UICONTROL Use existing dataset]**」を選択し、データセットアイコンをクリックします。

![use-existing-dataset](../../../images/tutorials/dataflow/advertising/use-existing-target-dataset.png)

[ *[!UICONTROL データセットの選択]* ]ダイアログが表示されます。 使用するデータセットを見つけて選択し、「 **[!UICONTROL 続行]**」をクリックします。

![select-existing-dataset](../../../images/tutorials/dataflow/advertising/select-existing-dataset.png)

### 新しいデータセットの使用

データを新しいデータセットに取り込むには、「 **[!UICONTROL 新しいデータセットを]** 作成」を選択し、表示されるフィールドにデータセットの名前と説明を入力します。

このプロセス中に、 *[!UICONTROL 部分的な取り込み]* 、 *[!UICONTROL エラー診断も有効にできます]*。 「 *[!UICONTROL 部分的な取り込み]* 」を有効にすると、エラーを含むデータを取り込むことができ、設定可能な特定のしきい値まで取り込むことができます。 「 [!UICONTROL エラー診断」を有効にすると] 、誤ったデータに関する詳細が表示されます。 詳しくは、 [部分的なバッチインジェストの概要を参照してください](../../../../ingestion/batch-ingestion/partial.md)。

終了したら、スキーマアイコンをクリックします。

![create-new-dataset](../../../images/tutorials/dataflow/advertising/new-target-dataset.png)

[ *[!UICONTROL スキーマの]* 選択]ダイアログが表示されます。 新しいデータセットに適用するスキーマを選択し、をクリックし **[!DNL Done]**&#x200B;ます。

![select-スキーマ](../../../images/tutorials/dataflow/advertising/select-existing-schema.png)

必要に応じて、フィールドを直接マップするか、マッパー関数を使用してソースデータを変換し、計算値や計算値を抽出することができます。 データマッピングおよびマッパーの機能について詳しくは、CSVデータのXDMスキーマフィールドへの [マッピングに関するチュートリアルを参照してください](../../../../ingestion/tutorials/map-a-csv-file.md)。

[ *[!UICONTROL マッピング]* ]画面では、 *[!UICONTROL [差分]列を設定することもできます]*。 データセットフローを作成する際、タイムスタンプフィールドを基準に、スケジュールされたインクリメンタルインジェクションで取り込むレコードを決定できます。

ソースデータがマッピングされたら、「 **[!UICONTROL 次へ]**」をクリックします。

![](../../../images/tutorials/dataflow/advertising/mapping.png)

## インジェストのスケジュール設定

[ *[!UICONTROL スケジュール]* ]ステップが表示され、設定済みのマッピングを使用して選択したソースデータを自動的に取り込むように取り込みスケジュールを設定できます。 次の表に、スケジュール設定用の様々な設定可能フィールドの概要を示します。

| フィールド | 説明 |
| --- | --- |
| 頻度 | 選択可能な頻度には、分、時間、日、週があります。 |
| 間隔 | 選択した頻度の間隔を設定する整数。 |
| 開始時間 | 最初の取り込みが行われるUTCタイムスタンプ。 |
| 埋め戻し | 最初に取り込まれるデータを決定するboolean値です。 [ *[!UICONTROL バックフィル]* ]を有効にすると、指定したパスにある現在のファイルは、最初にスケジュールされた取り込み中にすべて取り込まれます。 [ *[!UICONTROL バックフィル]* ]を無効にすると[!UICONTROL 、最初の取り込み実行から*] 開始時間*の間に読み込まれたファイルだけが取り込まれます。 *[!UICONTROL 開始時間より前に読み込まれたファイルは取り込まれません]* 。 |

データフローは、スケジュールに基づいてデータを自動的に取り込むように設計されています。 このワークフローで1回だけ取り込む場合は、 **[!UICONTROL 頻度]** 「日」を設定し、 ****&#x200B;間隔に非常に大きな数値（例：10000）を適用することで、これを行うことができます。

スケジュールの値を指定し、「 **[!UICONTROL 次へ]**」をクリックします。

![スケジュール](../../../images/tutorials/dataflow/advertising/schedule.png)

## データセットフローの名前を指定する

データセットフローの詳細 *[!UICONTROL (]* データセットフローの名前と説明)を入力する必要がある手順が表示されます。 終了したら **[!UICONTROL 「次へ]** 」を選択します。

![dataset-flow-details](../../../images/tutorials/dataflow/advertising/dataset-flow-detail.png)

## データセットのフローの確認

「 *[!UICONTROL レビュー]* 」ステップが表示され、新しいデータフローを作成前に確認できます。 詳細は次のカテゴリに分類されます。

- *[!UICONTROL 接続]*: ソースの種類、選択したソースファイルの関連パス、およびそのソースファイル内の列数が表示されます。
- *[!UICONTROL データセットとマップのフィールドの割り当て]*: ソースデータが取り込まれるデータセット(データセットに従うスキーマなど)を示します。
- *[!UICONTROL スケジュール]*: 取り込みスケジュールの有効期間、頻度、間隔を表示します。

データフローをレビューしたら、 **[!UICONTROL 「Finish]** 」をクリックし、データフローを作成するまでの時間を設定します。

![レビュー](../../../images/tutorials/dataflow/advertising/review.png)

## データセットフローの監視

データセットフローが作成されたら、データを通じて取り込まれるデータを監視できます。 データセットフローの監視方法について詳しくは、 [アカウントとデータセットフローに関するチュートリアルを参照してください](../monitor.md)。

## 次の手順

このチュートリアルに従うと、マーケティング自動化システムからデータを取り込むためのデータセットフローが正しく作成され、監視データセットに関する洞察が得られます。 受信データは、やなどのダウンストリーム [!DNL Platform] サービスで使用でき [!DNL Real-time Customer Profile] るようになり [!DNL Data Science Workspace]ました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspaceの概要](../../../../data-science-workspace/home.md)

## 付録

以下の節では、ソースコネクタを使用する場合の追加情報について説明します。

### データセットフローの無効化

データセットフローが作成されると、すぐにアクティブになり、与えられたスケジュールに従ってデータを取り込みます。 アクティブなデータセットフローは、次の手順に従っていつでも無効にできます。

「 *[!UICONTROL データセットフロー]* 」画面で、無効にするデータセットフローの名前を選択します。

![browse-dataset-flow](../../../images/tutorials/dataflow/advertising/view-dataset-flows.png)

「 *[!UICONTROL プロパティ]* 」列が画面の右側に表示されます。 このパネルには、「 **[!UICONTROL 有効]** 」切り替えボタンが含まれています。 切り替えボタンをクリックして、データフローを無効にします。 同じ切り替えを使用して、データフローを無効にした後で再び有効にできます。

![disable](../../../images/tutorials/dataflow/advertising/disable.png)

### 母集団の受信データを有効にし [!DNL Profile] ます

ソースコネクタから受信するデータは、データの富化と埋め込みに使用でき [!DNL Real-time Customer Profile] ます。 データの入力について詳しくは、 [!DNL Real-time Customer Profile][プロファイルの入力に関するチュートリアルを参照してください](../profile.md)。

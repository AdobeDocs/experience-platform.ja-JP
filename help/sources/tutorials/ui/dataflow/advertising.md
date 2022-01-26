---
keywords: Experience Platform；ホーム；人気の高いトピック；データフローの設定；広告コネクタ
solution: Experience Platform
title: UI での広告ソース接続用のデータフローの設定
topic-legacy: overview
type: Tutorial
description: データフローは、ソースからAdobe Experience Platformデータセットにデータを取得して取り込むスケジュール済みタスクです。 このチュートリアルでは、広告アカウントを使用して新しいデータフローを設定する手順を説明します。
exl-id: 8dd1d809-e812-4a13-8831-189726b2430e
source-git-commit: 38f64f2ba0b40a20528aac6efff0e2fd6bc12ed2
workflow-type: tm+mt
source-wordcount: '1470'
ht-degree: 4%

---

# UI での広告接続用のデータフローの設定

データフローは、ソースからAdobe Experience Platformデータセットにデータを取得して取り込むスケジュール済みタスクです。 このチュートリアルでは、広告アカウントを使用して新しいデータフローを設定する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../../xdm/home.md):標準化されたフレームワーク [!DNL Experience Platform] は顧客体験データを整理します。
   - [スキーマ構成の基本](../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../xdm/tutorials/create-schema-ui.md):スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

さらに、このチュートリアルでは、Advertising アカウントを既に作成している必要があります。 UI で様々な支払いコネクタを作成するためのチュートリアルのリストは、 [ソースコネクタの概要](../../../home.md).

## データを選択

広告アカウントを作成した後、 **[!UICONTROL データを選択]** 手順が表示され、ファイル階層を参照できるインタラクティブインターフェイスが提供されます。

- インターフェイスの左半分はディレクトリブラウザで、サーバーのファイルとディレクトリが表示されます。
- インターフェイスの右半分を使用すると、互換性のあるファイルから最大 100 行のデータをプレビューできます。

以下を使用して、 **[!UICONTROL 検索]** オプションを使用して、使用するソースデータをすばやく識別できます。

>[!NOTE]
>
>ソースデータの検索オプションは、Analytics、分類、イベントハブ、Kinesisコネクタを除く、すべての表形式ベースのソースコネクタで使用できます。

ソースデータを見つけたら、ディレクトリを選択し、 **[!UICONTROL 次へ]**.

![select-data](../../../images/tutorials/dataflow/all-tabular/select-data.png)


## XDM スキーマへのデータフィールドのマッピング

The **[!UICONTROL Mapping]** step appears, providing an interactive interface to map the source data to a [!DNL Platform] dataset.

取り込むインバウンドデータのデータセットを選択します。 既存のデータセットを使用するか、新しいデータセットを作成できます。

### 既存のデータセットを使用する

既存のデータセットにデータを取り込むには、「 」を選択します。 **[!UICONTROL 既存のデータセットを使用]**&#x200B;をクリックし、データセットアイコンをクリックします。

![use-existing-dataset](../../../images/tutorials/dataflow/advertising/use-existing-target-dataset.png)

この **[!UICONTROL データセットを選択]** ダイアログが表示されます。 使用するデータセットを見つけ、選択して、 **[!UICONTROL 続行]**.

![select-existing-dataset](../../../images/tutorials/dataflow/advertising/select-existing-dataset.png)

### 新しいデータセットを使用

データを新しいデータセットに取り込むには、「 **[!UICONTROL 新しいデータセットを作成]** をクリックし、提供されたフィールドにデータセットの名前と説明を入力します。

You can attach a schema field by entering a schema name in the **[!UICONTROL Select schema]** search bar. また、ドロップダウンアイコンを選択して、既存のスキーマのリストを表示することもできます。 または、 **[!UICONTROL 詳細検索]** 既存のスキーマの画面（それぞれの詳細を含む）にアクセスする。

この手順の間に、 [!DNL Real-time Customer Profile] エンティティの属性と行動を総合的に把握できます。 すべての有効なデータセットのデータは、 [!DNL Profile] および変更は、データフローを保存する際に適用されます。

切り替え **[!UICONTROL プロファイルデータセット]** ボタンを使用して [!DNL Profile].

![create-new-dataset](../../../images/tutorials/dataflow/advertising/target-dataset.png)

この **[!UICONTROL スキーマを選択]** ダイアログが表示されます。 新しいデータセットに適用するスキーマを選択し、 **[!DNL Done]**.

![select-schema](../../../images/tutorials/dataflow/advertising/select-existing-schema.png)

Based on your needs, you can choose to map fields directly, or use data prep functions to transform source data to derive computed or calculated values. マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../data-prep/ui/mapping.md).

>[!TIP]
>
>Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対するインテリジェントなレコメンデーションを提供します。 マッピングルールは、使用例に合わせて手動で調整できます。

![](../../../images/tutorials/dataflow/all-tabular/mapping.png)

選択 **[!UICONTROL データをプレビュー]** ：選択したデータセットから最大 100 行のサンプルデータのマッピング結果を確認します。

プレビュー中、ID 列は、マッピング結果を検証する際に必要な重要な情報なので、最初のフィールドとして優先付けされます。

![](../../../images/tutorials/dataflow/all-tabular/mapping-preview.png)

ソースデータをマッピングしたら、 **[!UICONTROL 閉じる]**.

## 取り込み実行のスケジュール設定

この **[!UICONTROL スケジュール]** 手順が表示され、設定済みのマッピングを使用して選択したソースデータを自動的に取り込むように取り込むように、取り込みスケジュールを設定できます。 次の表に、スケジュール設定用の様々な設定可能フィールドの概要を示します。

| フィールド | 説明 |
| --- | --- |
| 頻度 | 選択可能な頻度には次のものが含まれます `Once`, `Minute`, `Hour`, `Day`、および `Week`. |
| 間隔 | 選択した頻度の間隔を設定する整数。 |
| Start time | 最初の取り込みがいつ行われるかを示す UTC タイムスタンプ。 |
| Backfill | A boolean value that determines what data is initially ingested. If **[!UICONTROL バックフィル]** が有効になっている場合、指定されたパス内の現在のすべてのファイルが、最初にスケジュールされた取り込み中に取り込まれます。 If **[!UICONTROL Backfill]** is disabled, only the files that are loaded in between the first run of ingestion and the start time will be ingested. 開始時より前に読み込まれたファイルは取り込まれません。 |
| 差分列 | フィルターされた一連のソーススキーマフィールド（タイプ、日付、時間）を含むオプション。 このフィールドは、新しいデータと既存のデータを区別するために使用されます。 増分データは、選択した列のタイムスタンプに基づいて取り込まれます。 |

データフローは、スケジュールに従ってデータを自動的に取り込むように設計されています。 まず、取り込み頻度を選択します。 次に、2 つのフロー実行の間隔を指定する間隔を設定します。 間隔の値は、ゼロ以外の整数で、15 以上に設定する必要があります。

取り込みの開始時間を設定するには、開始時間ボックスに表示される日時を調整します。 または、カレンダーアイコンを選択して開始時間の値を編集できます。 開始時間は、現在の UTC 時間以上である必要があります。

選択 **[!UICONTROL 増分データの読み込み基準]** 」をクリックしてデルタ列を割り当てます。 このフィールドには、新しいデータと既存のデータの違いが表示されます。

![schedule-interval](../../../images/tutorials/dataflow/databases/schedule-interval-on.png)

### 1 回限りの取り込みデータフローの設定

1 回限りの取り込みを設定するには、「頻度」ドロップダウン矢印を選択し、「 **[!UICONTROL 1 回]**.

>[!TIP]
>
>**[!UICONTROL 間隔]** および **[!UICONTROL バックフィル]** は、1 回限りの取り込みでは表示されません。

スケジュールに適切な値を指定したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![schedule-once](../../../images/tutorials/dataflow/databases/schedule-once.png)

## データフローの詳細を入力

この **[!UICONTROL データフローの詳細]** 手順が表示され、新しいデータフローに名前を付け、簡単な説明を入力できます。

このプロセスの間に、 **[!UICONTROL 部分取り込み]** および **[!UICONTROL エラー診断]**. 有効化 **[!UICONTROL 部分取り込み]** は、エラーを含むデータを特定のしきい値まで取り込む機能を提供します。 1 回 **[!UICONTROL 部分取り込み]** が有効な場合、 **[!UICONTROL エラーしきい値%]** バッチのエラーしきい値を調整するためにダイヤルします。 または、入力ボックスを選択して手動でしきい値を調整することもできます。 詳しくは、 [部分バッチ取得の概要](../../../../ingestion/batch-ingestion/partial.md).
データフローの値を指定し、「 」を選択します。 **[!UICONTROL 次へ]**.

![データフローの詳細](../../../images/tutorials/dataflow/all-tabular/dataflow-detail.png)

## データフローの確認

この **[!UICONTROL レビュー]** 手順が表示され、新しいデータフローを作成する前に確認できます。 詳細は、次のカテゴリに分類されます。

- **[!UICONTROL Connection]**: Shows the source type, the relevant path of the chosen source file, and the amount of columns within that source file.
- **[!UICONTROL データセットの割り当てとフィールドのマッピング]**:データセットが準拠するスキーマを含め、ソースデータの取り込み先のデータセットを示します。
- **[!UICONTROL スケジュール]**:取り込みスケジュールのアクティブな期間、頻度、間隔を表示します。

データフローをレビューしたら、「 」をクリックします。 **[!UICONTROL 完了]** とは、データフローが作成されるまでしばらく時間をかけます。

![review](../../../images/tutorials/dataflow/advertising/review.png)

## データフローの監視

Once your dataflow has been created, you can monitor the data that is being ingested through it to see information on ingestion rates, success, and errors. データフローの監視方法の詳細については、 [UI でのアカウントとデータフローの監視](../monitor.md).

## データフローを削除

You can delete dataflows that are no longer necessary or were incorrectly created using the **[!UICONTROL Delete]** function available in the **[!UICONTROL Dataflows]** workspace. データフローの削除方法の詳細については、 [UI でのデータフローの削除](../delete.md).

## 次の手順

このチュートリアルに従うことで、マーケティング自動化システムからデータを取り込むためのデータフローを正常に作成し、監視データセットに関するインサイトを得ることができました。 受信データをダウンストリームで使用できるようになりました [!DNL Platform] 次のようなサービス： [!DNL Real-time Customer Profile] および [!DNL Data Science Workspace]. 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../profile/home.md)
- [Data Science Workspace の概要](../../../../data-science-workspace/home.md)

## 付録

次の節では、ソースコネクタの操作に関する追加情報を示します。

### データフローの無効化

データフローを作成すると、そのデータフローは直ちにアクティブになり、指定されたスケジュールに従ってデータを取り込みます。 You can disable an active dataflow at any time by following the instructions below.

内 **[!UICONTROL データフロー]** 画面で、無効にするデータフローの名前を選択します。

![browse-dataset-flow](../../../images/tutorials/dataflow/advertising/view-dataset-flows.png)

The **[!UICONTROL Properties]** column appears on the right-hand side of the screen. This panel contains an **[!UICONTROL Enabled]** toggle button. 切り替えボタンをクリックして、データフローを無効にします。 同じ切り替えを使用して、無効にした後でデータフローを再度有効にすることができます。

![disable](../../../images/tutorials/dataflow/advertising/disable.png)

### のインバウンドデータをアクティブ化 [!DNL Profile] 母集団

ソースコネクタからの受信データは、 [!DNL Real-time Customer Profile] データ。 の [!DNL Real-time Customer Profile] データについては、次のチュートリアルを参照してください： [プロファイル母集団](../profile.md).

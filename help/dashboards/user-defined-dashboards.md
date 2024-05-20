---
title: カスタムダッシュボード
description: カスタムダッシュボードを作成および管理する方法を説明します。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して主要指標を視覚化できます。
exl-id: a9ab83f7-b68d-4dbf-9dc6-ef253df5c82c
source-git-commit: 17ad52864bbca09844c0241b6451e6811bd8f413
workflow-type: tm+mt
source-wordcount: '1624'
ht-degree: 3%

---

# カスタムダッシュボード

Adobe Experience Platform ダッシュボードを使用すると、ダッシュボード機能を通じて、インサイトを迅速に獲得し、ビジュアライゼーションをカスタマイズできます。 この機能を使用してカスタムダッシュボードを作成および管理します。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して、組織に関連する主要指標を視覚化できます。


<!-- Getting started / permissions section commented out for Beta. This will be necessary after GA only

## Getting started

To view dashboards in Adobe Experience Platform you must have the appropriate permissions enabled. Please read the [dashboards permissions documentation](./permissions.md#available-permissions) to learn how to grant users the ability to view, edit, and update Experience Platform dashboards using Adobe Admin Console. If you do not have administrator privileges for your organization, contact your product administrator to obtain the required permissions. -->

## カスタムダッシュボードの作成

カスタムダッシュボードを作成するには、まずダッシュボードインベントリに移動します。 を選択 **[!UICONTROL ダッシュボード]** Platform UI の左側のナビゲーションからに続いて **[!UICONTROL ダッシュボードを作成]**.

![左側のナビゲーションのダッシュボードと「ダッシュボードを作成」がハイライト表示されたダッシュボードインベントリ。](./images/user-defined-dashboards/create-dashboard.png)

カスタムダッシュボードを追加する前、ダッシュボードインベントリには何も表示されず、「ダッシュボードが見つかりません」と表示されます。 メッセージ。 作成すると、すべてのダッシュボードがダッシュボードインベントリに表示されます。

<!-- >[!NOTE]
>
>To edit an existing dashboard, select the dashboard name from the inventory list followed by the pencil icon (![A pencil icon.](./images/user-defined-dashboards/edit-icon.png))
>![A custom inventory listed in the dashboard inventory.](./images/user-defined-dashboards/dashbaord-inventory.png "A custom inventory listed in the dashboard inventory."){width="100" zoomable="yes"} -->

この [!UICONTROL ダッシュボードを作成] ダイアログが表示されます。 作成するウィジェットのコレクションにわかりやすい名前を入力し、選択します **[!UICONTROL 保存]**.

![ダッシュボードを作成ダイアログ。](./images/user-defined-dashboards/create-dashboard-dialog.png)

Data Distiller SKU を購入したユーザーは、カスタム SQL クエリを使用してインサイトを作成できます。 を参照してください。 [カスタマイズ可能なインサイト作成ガイド](./data-distiller/customizable-insights/overview.md) このワークフローの手順について説明します。

新しく作成された空のダッシュボードが表示され、ビューの左上隅に選択した名前が表示されます。

## ウィジェットを作成 {#create-widget}

>[!CONTEXTUALHELP]
>id="platform_dashboards_udd_maxwidgets"
>title="ウィジェットの最大数"
>abstract="ダッシュボードサービスでは、最大 10 個のウィジェットをサポートします。ダッシュボードに 10 個のウィジェットを追加すると、「[!UICONTROL 新しいウィジェットを追加]」オプションが無効になり、グレーで表示されます。"

新しいダッシュボードの表示で、 **[!UICONTROL 新しいウィジェットを追加]** でウィジェット作成プロセスを開始します。

>[!IMPORTANT]
>
>各ダッシュボードは、最大 10 個のウィジェットをサポートします。 ダッシュボードに 10 個のウィジェットを追加すると、「[!UICONTROL 新しいウィジェットを追加]」オプションが無効になり、グレーで表示されます。

![「新しいウィジェットを追加」が強調表示された新しい空のダッシュボード。](./images/user-defined-dashboards/add-new-widget.png)

### ウィジェットコンポーザー

ウィジェットコンポーザーワークスペースが表示されます。 次に、を選択します **[!UICONTROL データを選択]** をクリックして、ウィジェットに属性を追加するデータモデルを選択します。

![ウィジェットコンポーザーワークスペース。](./images/user-defined-dashboards/widget-composer.png)

#### データモデルを選択 {#select-data-model}

この [!UICONTROL データモデルを選択] ダイアログが表示されます。 左側の列からデータモデルを選択し、使用可能なすべてのテーブルのプレビューリストを表示します。 Real-time Customer Data Platformの事前設定済みデータモデルには、という名前が付けられます [!UICONTROL CDPInsights].

>[!TIP]
>
>情報アイコン（![情報アイコン。](./images/user-defined-dashboards/info-icon.png)）に設定し、データパネルに表示するには長すぎる場合に、完全なデータモデル名を表示します。

![データを選択ダイアログ。](./images/user-defined-dashboards/select-data-model-dialog.png)

プレビューリストには、データモデルに含まれるテーブルに関する詳細が表示されます。 次の表に、列フィールドとその潜在的な値の説明を示します。

| 列フィールド | 説明 |
|---|---|
| [!UICONTROL タイトル] | テーブルの名前。 |
| [!UICONTROL テーブルタイプ] | テーブルのタイプ。 使用できるタイプは次のとおりです。 `fact`, `dimension`、および `none`. |
| [!UICONTROL レコード] | 選択したテーブルに関連付けられているレコードの数。 |
| [!UICONTROL 参照] | 選択したテーブルに結合されるテーブルの数。 |
| [!UICONTROL 属性] | 選択したテーブルの属性の数。 |

を選択 **[!UICONTROL 次]** をクリックして、データモデルの選択を確認します。 次の表示では、使用可能なテーブルのリストが左パネルに表示されます。 テーブルを選択して、選択したテーブルに含まれるデータの包括的な分類を表示します。

### ウィジェットを入力 {#populate-widget}

この [!UICONTROL プレビュー] パネルには、のタブが含まれています [!UICONTROL サンプルレコード] および [!UICONTROL 属性]. この [!UICONTROL サンプルレコード] タブは、選択したテーブルのレコードのサブセットを表形式ビューで表示します。 この [!UICONTROL 属性] タブには、選択したテーブルに関連付けられたすべての属性の属性名、データタイプ、ソーステーブルが表示されます。

左側のパネルにあるリストからテーブルを選択してウィジェットのデータを指定し、を選択します。 **[!UICONTROL を選択]** をクリックしてウィジェットコンポーザーに戻ります。

![「選択」がハイライト表示されたデータを選択ダイアログ](./images/user-defined-dashboards/select-a-table.png)

これで、選択したテーブルのデータがウィジェットコンポーザーに入力されます。

データモデルと現在選択されているテーブルが左側のパネルの上部に表示され、ウィジェットの作成に使用できる属性がに一覧表示されます [!UICONTROL 属性] 列。 検索バーを使用して、リストをスクロールする代わりに属性を検索したり、鉛筆アイコン（![鉛筆アイコン。](./images/user-defined-dashboards/edit-icon.png)）を選択します。

![ウィジェットコンポーザー内でデータが入力されるウィジェット。](./images/user-defined-dashboards/populated-widget-composer.png)

#### 属性の追加とフィルター {#add-and-filter-attributes}

追加アイコン（![追加アイコン。](./images/user-defined-dashboards/add-icon.png)）を選択して、ウィジェットに属性を追加します。 表示されるドロップダウンメニューを使用すると、X 軸、Y 軸、色、ウィジェットのフィルターのいずれかとして属性を追加できます。 この [!UICONTROL カラー] 属性を使用すると、X 軸マークと Y 軸マークの結果を色に基づいて区別できます。 これを行うには、3 番目の属性の構成に基づいて、結果を異なる色に分割します。

>[!TIP]
>
>X 軸と Y 軸の配置を反転する場合は、上向き矢印アイコンと下向き矢印アイコン（![上下の矢印アイコン。](./images/user-defined-dashboards/switch-axis-icon.png)）を選択して、配置を切り替えます。

![アドアイコンドロップダウンがハイライト表示されたウィジェットコンポーザー。](./images/user-defined-dashboards/attributes-dropdown.png)

ウィジェットのグラフのタイプを変更するには、 [!UICONTROL マーク] ドロップダウンから利用可能なオプションを選択します。 オプションには、バー、点、ティック、線分、または面積があります。 選択すると、ウィジェットの現在の設定のプレビュービジュアライゼーションが生成されます。

![マークドロップダウンがハイライト表示されたウィジェットコンポーザー。](./images/user-defined-dashboards/marks-dropdown.png)

属性をフィルターとして追加すると、ウィジェットに含める値と除外する値を選択できます。 属性リストからフィルターを追加すると、 [!UICONTROL フィルター] ダイアログが表示されるので、チェックボックスを使用して値を選択または選択解除できます。

![ウィジェットから値をフィルタリングするためのフィルターダイアログ。](./images/user-defined-dashboards/filter-dialog.png)

#### 履歴データの除外 {#filter-historical-data}

ウィジェットで生成されたインサイトから履歴データを除外するには、 `date_key` フィルターとしての属性と選択 **[!UICONTROL 最近の日付]** 続いて **[!UICONTROL 適用]**. このフィルターにより、インサイトの取得に使用されるデータが最新のシステムスナップショットから取得されます。

![この [!UICONTROL フィルター：date_key] ～との対話 [!UICONTROL 最近の日付] および [!UICONTROL 適用] ハイライト表示](./images/user-defined-dashboards/recent-date.png)

または、カスタム期間を作成して、データをフィルタリングすることもできます。 を選択 **[!UICONTROL 日付を選択]** 使用可能な日付のリストでダイアログを拡張します。 の使用 **[!UICONTROL すべて選択]** チェックボックス：使用可能なすべてのオプションを有効または無効にするか、各日のチェックボックスを個別に選択します。 最後に、を選択します **[!UICONTROL 適用]** をクリックして選択を確定します。

>[!NOTE]
>
>次の場合 `date_key` 属性は既にフィルターとして追加されています。その後に続く「。..」を選択します **[!UICONTROL 編集]** ドロップダウンオプションから、フィルター期間を変更します。

![この [!UICONTROL フィルター：date_key] 個々の「日」チェックボックスがオンとオフの両方になっているダイアログ。](./images/user-defined-dashboards/select-dates.png)

### ウィジェットのプロパティ

プロパティアイコン（![プロパティアイコン。](./images/user-defined-dashboards/properties-icon.png)）を選択し、プロパティ パネルを開きます。 が含まれる [!UICONTROL プロパティ] パネルで、ウィジェットの名前を [!UICONTROL ウィジェットのタイトル] テキストフィールド。

![プロパティアイコンとウィジェットタイトルフィールドがハイライト表示されたプロパティパネル。](./images/user-defined-dashboards/properties-panel.png)

ウィジェットのプロパティパネルから、ウィジェットの複数の要素を編集できます。 ウィジェットの凡例の場所を編集するための完全なコントロールがあります。 凡例を移動するには、 [!UICONTROL 凡例の配置] 使用可能なオプションのリストから目的の場所をドロップダウンで選択します。 凡例に関連付けられているラベルや X 軸または Y 軸の名前を変更するには、に新しい名前を入力します [!UICONTROL 凡例のタイトル] テキストフィールド、または [!UICONTROL 軸ラベル] テキストフィールドに入力します。

#### ウィジェットを保存 {#save-widget}

ウィジェットコンポーザーで保存すると、ウィジェットがダッシュボードにローカルに保存されます。 作業内容を保存し、後で再開する場合は、「」を選択します **[!UICONTROL 保存]**. ウィジェット名の下のチェックマークアイコンは、ウィジェットが保存されたことを示します。 または、ウィジェットの設定が完了したら、を選択します。 **[!UICONTROL 保存して閉じる]** をクリックして、ダッシュボードにアクセスできるその他すべてのユーザーがウィジェットを使用できるようにします。 を選択 **[!UICONTROL キャンセル]** 作業を中断してカスタムダッシュボードに戻る場合。

![新規ウィジェットの保存の確認。](./images/user-defined-dashboards/save-confirmation.png)

>[!TIP]
>
>プロパティアイコン（![プロパティアイコン。](./images/user-defined-dashboards/properties-icon.png)）を選択し、ダッシュボードの作成に関する詳細を確認します。 表示されるダイアログでダッシュボードの名前を変更できます。

このワークスペース内でウィジェットの並べ替えとサイズ変更を行うことができます。 を選択 **[!UICONTROL 保存]** ダッシュボード名と設定済みレイアウトを保持します。

![カスタムウィジェットと「保存」ボタンがハイライト表示されたユーザー定義ダッシュボード。](./images/user-defined-dashboards/user-defined-dashboard.png)

Adobe Real-time Customer Data Platform インサイトダッシュボードの各クエリに効率的に実行するのに十分なリソースがあることを確認するために、API は、各クエリに同時実行スロットを割り当てることで、リソースの使用状況を追跡します。 システムでは最大 4 つの同時クエリを処理できるので、同時に 4 つのクエリスロットをいつでも使用できます。 クエリは同時実行スロットに基づいてキューに入れられ、同時実行スロットが十分に使用可能になるまでキュー内で待機されます。

### ウィジェットの編集、複製、削除 {#duplicate}

ウィジェットを作成したら、カスタムダッシュボードからウィジェット全体を編集、複製または削除できます。

>[!TIP]
>
>既存のカスタムダッシュボードを切り替えるには、左側のナビゲーションバーのダッシュボードを選択し、インベントリリストからダッシュボード名を選択します。

鉛筆アイコン（![鉛筆アイコン。](./images/user-defined-dashboards/edit-icon.png)）を選択し、編集モードに入ります。

![鉛筆アイコンがハイライトされたカスタムダッシュボード。](./images/user-defined-dashboards/edit-mode.png)

次に、編集、コピー、削除するウィジェットの右上にある省略記号を選択します。 ドロップダウンメニューから適切なアクションを選択します。

![カスタムダッシュボード内のウィジェット。省略記号と「複製」ウィジェットがハイライト表示されています。](./images/user-defined-dashboards/duplicate.png)

>[!NOTE]
>
>複製を使用すると、インサイトの属性をカスタマイズして、ゼロから始めなくても一意のウィジェットを作成できます。 ウィジェットを複製すると、カスタムダッシュボードに表示されます。 新しいウィジェットの省略記号を選択し、続けて省略記号を選択できます **[!UICONTROL 編集]**&#x200B;をクリックし、インサイトをカスタマイズします。

## 次の手順とその他のリソース

このドキュメントでは、カスタムダッシュボードの作成方法と、そのダッシュボードのカスタムウィジェットを作成、編集、更新する方法について、より深く理解しました。

で使用可能な事前設定済みの指標とビジュアライゼーションを見つけるには [プロファイル](./guides/profiles.md#standard-widgets), [セグメント](./guides/audiences.md#standard-widgets)、および [宛先](./guides/destinations.md#standard-widgets) ダッシュボードについては、それぞれのドキュメントで標準ウィジェットのリストを参照してください。

Experience Platformのダッシュボードの理解を深めるために、次のビデオを視聴してください。

>[!VIDEO](https://video.tv.adobe.com/v/3409637?quality=12&learn=on)

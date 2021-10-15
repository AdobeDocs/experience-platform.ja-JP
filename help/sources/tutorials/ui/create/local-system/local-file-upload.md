---
keywords: Experience Platform；ホーム；人気のあるトピック；ローカルシステム；ファイルのアップロード；csv のマッピング；csv ファイルのマッピング；xdm への csv ファイルのマッピング；xdm への csv のマッピング；ui ガイド；
solution: Experience Platform
title: UI でのローカルファイルアップロードソースコネクタの作成
topic-legacy: overview
type: Tutorial
description: ローカルシステムのソース接続を作成して、ローカルファイルを Platform に取り込む方法を説明します。
exl-id: 9ce15362-c30d-40cc-9d9c-caa650579390
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '1271'
ht-degree: 18%

---

# UI でのローカルファイルアップロードソースコネクタの作成

このチュートリアルでは、ユーザーインターフェイスを使用してローカルファイルを Platform に取り込むためのローカルファイルアップロードソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] System](../../../../../xdm/home.md): The standardized framework by which Platform organizes customer experience data.
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## Platform へのローカルファイルのアップロード

プラットフォーム UI で、左のナビゲーションバーから「 **[!UICONTROL ソース]** 」を選択して、「 [!UICONTROL  ソース ] 」ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「[!UICONTROL  ローカルシステム ]」カテゴリで、「**[!UICONTROL ローカルファイルのアップロード]**」を選択し、「**[!UICONTROL 設定]**」を選択します。

![カタログ](../../../../images/tutorials/create/local/catalog.png)

### 既存のデータセットを使用する

[!UICONTROL  データフローの詳細 ] ページでは、CSV データを既存のデータセットに取り込むか、新しいデータセットに取り込むかを選択できます。

CSV データを既存のデータセットに取り込むには、「**[!UICONTROL 既存のデータセット]**」を選択します。 [!UICONTROL  詳細検索 ] オプションを使用するか、ドロップダウンメニューで既存のデータセットのリストをスクロールして、既存のデータセットを取得できます。

![select-existing-dataset](../../../../images/tutorials/create/local/select-existing-dataset.png)

With a dataset selected, provide a name for your dataflow and an optional description.

このプロセスの間に、[!UICONTROL  エラー診断 ] および [!UICONTROL  部分取得 ] を有効にすることもできます。 [!UICONTROL エラー] 診断では、データフローで発生したエラーレコードの詳細なエラーメッセージ生成が可能で [!UICONTROL す。部分的な取り込みで] は、エラーを含むデータを、手動で定義した特定のしきい値まで取り込むことができます。詳しくは、[ バッチの部分取得の概要 ](../../../../../ingestion/batch-ingestion/partial.md) を参照してください。

![dataflow-detail-existing](../../../../images/tutorials/create/local/dataflow-detail-existing.png)

### 新しいデータセットの使用

CSV データを新しいデータセットに取り込むには、「**[!UICONTROL 新しいデータセット]**」を選択し、出力データセット名と説明（オプション）を入力します。 次に、「[!UICONTROL  詳細検索 ]」オプションを使用するか、ドロップダウンメニューで既存のスキーマのリストをスクロールして、マッピングするスキーマを選択します。

![select-new-dataset](../../../../images/tutorials/create/local/select-new-dataset.png)

With a schema selected, provide a name for your dataflow and an optional description, and then apply the [!UICONTROL Error diagnostics] and [!UICONTROL Partial ingestion] settings you want for your dataflow. 終了したら、「**[!UICONTROL 次へ]**」を選択します。

### Select data

The [!UICONTROL Select data] step appears, providing you an interface to upload your local files and preview their structure and contents. 「**[!UICONTROL ファイル]** を選択」を選択して、ローカルシステムから CSV ファイルをアップロードします。 または、アップロードする CSV ファイルを [!UICONTROL  ファイルをドラッグ&amp;ドロップ ] パネルにドラッグ&amp;ドロップできます。

>[!TIP]
>
>現在、ローカルファイルのアップロードでは CSV ファイルのみがサポートされています。 各ファイルの最大ファイルサイズは 1 GB です。

![choose-files](../../../../images/tutorials/create/local/choose-files.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、ファイルの内容と構造が表示されます。

![preview-sample-data](../../../../images/tutorials/create/local/preview-sample-data.png)

ファイルに応じて、ソースデータの列区切り文字（タブ、コンマ、パイプ、カスタム列区切り文字など）を選択できます。 **[!UICONTROL 区切り文字]** ドロップダウン矢印を選択し、メニューから適切な区切り文字を選択します。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![delimiter](../../../../images/tutorials/create/local/delimiter.png)

### マッピング

[!UICONTROL  マッピング ] 手順が表示され、ソーススキーマのソースフィールドをターゲットスキーマの適切なターゲット XDM フィールドにマッピングするインターフェイスが提供されます。

![mapping-interface](../../../../images/tutorials/create/local/mapping-interface.png)

#### データのプレビュー

「**[!UICONTROL データのプレビュー]**」を選択すると、選択したデータセットから最大 100 行のサンプルデータのマッピング結果が表示されます。

![プレビューとマッピング](../../../../images/tutorials/create/local/preview-mapping.png)

プレビュー時、ID 列は最初のフィールドとして優先付けされます。マッピング結果を検証する際に必要な重要な情報です。 終了したら、「**[!UICONTROL 閉じる]**」を選択します。

![プレビューパネル](../../../../images/tutorials/create/local/preview-panel.png)

#### 計算フィールドの追加

計算フィールドでは、入力スキーマの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して参照を容易にできます。

「**[!UICONTROL 計算済みフィールドを追加]**」ボタンを選択して次に進みます。

![add-calculated-field](../../../../images/tutorials/create/local/add-calculated-field.png)

[!UICONTROL 計算フィールドの作成] パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。タブの 1 つを選択して、式エディターに関数、フィールドまたは演算子を追加します。

![create-calculated-field](../../../../images/tutorials/create/local/create-calculated-field.png)

| タブ | 説明 |
| --------- | ----------- |
| 関数 | 「関数」タブには、データの変換に使用できる関数が一覧表示されます。計算フィールド内で使用できる関数の詳細については、 [データ準備（マッパー）関数の使用](../../../../../data-prep/functions.md) に関するガイドを参照してください。 |
| フィールド | 「フィールド」タブには、ソーススキーマで使用できるフィールドと属性が表示されます。 |
| 演算子 | 「演算子」タブには、データの変換に使用できる演算子が一覧表示されます。 |

式エディターを選択して、フィールド、関数、演算子を手動で追加します。 Once you have created a calculated field, select **[!UICONTROL Save]** to proceed.

![式エディター](../../../../images/tutorials/create/local/expression-editor.png)

#### Filter source schema mapping tree

ソーススキーマをフィルタリングするには、「**[!UICONTROL すべてのソースフィールド]**」を選択し、ドロップダウンメニューからマッピングする特定のフィールドを選択します。

次の表に、ソーススキーマツリーの並べ替えオプションを示します。

| ソースフィールド | 説明 |
| --- | --- |
| [!UICONTROL すべてのソースフィールド] | このオプションは、ソーススキーマのすべてのソースフィールドを表示します。 このオプションはデフォルトで表示されます。 |
| [!UICONTROL 必須フィールド] | このオプションは、ソーススキーマをフィルタリングして、マッピングの完了に必要なフィールドのみを表示します。 |
| [!UICONTROL ID フィールド] | このオプションでは、ソーススキーマをフィルタリングして、ID としてマークされたフィールドのみを表示します。 |
| [!UICONTROL マッピングされたフィールド] | このオプションは、ソーススキーマをフィルタリングして、既にマッピングされているフィールドのみを表示します。 |
| [!UICONTROL マッピングされていないフィールド] | このオプションでは、ソーススキーマをフィルタリングして、まだマッピングされていないフィールドのみを表示します。 |
| [!UICONTROL レコメンデーションを含むフィールド] | このオプションでは、ソーススキーマをフィルタリングして、マッピングレコメンデーションを含むフィールドのみを表示します。 |

![all-source-fields](../../../../images/tutorials/create/local/all-source-fields.png)

#### Intelligent recommendations

Platform automatically provides intelligent recommendations for auto-mapped fields based on the target schema or dataset that you selected. You can manually adjust mapping rules to suit your use cases.

![source-schema-tree](../../../../images/tutorials/create/local/source-schema-tree.png)

すべての自動生成マッピング値を受け入れるには、「**[!UICONTROL すべてのターゲットフィールドを受け入れる]**」を選択します。

![all-target-fields](../../../../images/tutorials/create/local/all-target-fields.png)

ソーススキーマに複数のレコメンデーションが使用できる場合があります。 この場合、マッピングカードには最も目立つレコメンデーションが表示され、その後に使用可能なレコメンデーションの数を示す青い円が表示されます。 電球アイコンを選択すると、追加の推奨事項のリストが表示されます。 代わりに、マッピング先のレコメンデーションの横にあるチェックボックスを選択して、代替レコメンデーションの 1 つを選択できます。

![手動マッピング](../../../../images/tutorials/create/local/manual-mapping.png)

または、ソーススキーマを手動でターゲットスキーマにマッピングすることもできます。 それには、マッピングするソーススキーマの上にマウスポインターを置き、プラス (`+`) アイコンを選択します。

![select-plus-icon](../../../../images/tutorials/create/local/select-plus-icon.png)

「**[!UICONTROL ソースをターゲットフィールドにマッピング]**」ポップオーバーが表示されます。 ここから、マッピングするフィールドを選択し、「**[!UICONTROL 保存]**」を選択して新しいマッピングを追加できます。

![map-source-to-target-field](../../../../images/tutorials/create/local/map-source-to-target-field.png)

終了したら、「**[!UICONTROL 終了]**」を選択します。

![終了](../../../../images/tutorials/create/local/finish.png)

## データ取得の監視

CSV ファイルがマッピングされ、作成されたら、監視ダッシュボードを使用して、CSV ファイルを通じて取り込まれるデータを監視できます。 For more information, see the tutorial on [monitoring sources dataflows in the UI](../../../../../dataflows/ui/monitor-sources.md).

## 次の手順

このチュートリアルに従うと、フラットな CSV ファイルを XDM スキーマにマッピングし、Platform に取り込むことができます。このデータは、[!DNL Real-time Customer Profile] などのダウンストリームの [!DNL Platform] サービスで使用できるようになりました。 詳しくは、[[!DNL Real-time Customer Profile]](../../../../../profile/home.md) の概要を参照してください。

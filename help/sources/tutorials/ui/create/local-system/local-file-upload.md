---
keywords: Experience Platform；ホーム；人気のあるトピック；ローカルシステム；ファイルのアップロード；csvのマップ；csvファイルのマップ；xdmへのcsvファイルのマップ；xdmへのcsvのマップ；uiガイド；
solution: Experience Platform
title: UIでのローカルファイルアップロードソースコネクタの作成
topic-legacy: overview
type: Tutorial
description: ローカルシステムのソース接続を作成して、ローカルファイルをPlatformに取り込む方法を説明します
source-git-commit: 1bf112db27b534e2ec977be7b47e3becf75ee066
workflow-type: tm+mt
source-wordcount: '1271'
ht-degree: 7%

---

# UIでのローカルファイルアップロードソースコネクタの作成

このチュートリアルでは、ユーザーインターフェイスを使用してローカルファイルをPlatformに取り込むためのローカルファイルアップロードソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## Platformへのローカルファイルのアップロード

プラットフォームUIで、左側のナビゲーションバーから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL ソース]」ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、目的の特定のソースを見つけることもできます。

「[!UICONTROL ローカルシステム]」カテゴリで、「**[!UICONTROL ローカルファイルのアップロード]**」を選択し、「**[!UICONTROL 設定]**」を選択します。

![カタログ](../../../../images/tutorials/create/local/catalog.png)

### 既存のデータセットを使用する

[!UICONTROL データフローの詳細]ページでは、CSVデータを既存のデータセットに取り込むか、新しいデータセットに取り込むかを選択できます。

CSVデータを既存のデータセットに取り込むには、「**[!UICONTROL 既存のデータセット]**」を選択します。 [!UICONTROL 詳細検索]オプションを使用するか、ドロップダウンメニューで既存のデータセットのリストをスクロールして、既存のデータセットを取得できます。

![select-existing-dataset](../../../../images/tutorials/create/local/select-existing-dataset.png)

データセットを選択し、データフローの名前と説明（オプション）を入力します。

このプロセスの間に、[!UICONTROL エラー診断]および[!UICONTROL 部分取得]を有効にすることもできます。 [!UICONTROL エラー] 診断は、データフローで発生するエラーレコードの詳細なエラーメッセージ生成を可能にしま [!UICONTROL す。「部分取り込] み」では、エラーを含むデータを、手動で定義した特定のしきい値まで取り込むことができます。詳しくは、「[バッチ取得の部分の概要](../../../../../ingestion/batch-ingestion/partial.md)」を参照してください。

![dataflow-detail-existing](../../../../images/tutorials/create/local/dataflow-detail-existing.png)

### 新しいデータセットの使用

CSVデータを新しいデータセットに取り込むには、「**[!UICONTROL 新しいデータセット]**」を選択し、出力データセット名と説明（オプション）を入力します。 次に、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニュー内の既存のスキーマのリストをスクロールして、マッピングするスキーマを選択します。

![select-new-dataset](../../../../images/tutorials/create/local/select-new-dataset.png)

スキーマを選択し、データフローの名前とオプションの説明を入力し、データフローに必要な[!UICONTROL エラー診断]および[!UICONTROL 部分取得]設定を適用します。 終了したら、「**[!UICONTROL 次へ]**」を選択します。

### データの選択

[!UICONTROL データの選択]手順が表示され、ローカルファイルをアップロードし、構造とコンテンツをプレビューするためのインターフェイスが提供されます。 「**[!UICONTROL ファイル]**&#x200B;を選択」を選択して、ローカルシステムからCSVファイルをアップロードします。 または、アップロードするCSVファイルを[!UICONTROL ファイルをドラッグ&amp;ドロップ]パネルにドラッグ&amp;ドロップします。

>[!TIP]
>
>ローカルファイルのアップロードでは、現在、CSVファイルのみがサポートされています。 各ファイルの最大ファイルサイズは1 GBです。

![choose-files](../../../../images/tutorials/create/local/choose-files.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、ファイルの内容と構造が表示されます。

![preview-sample-data](../../../../images/tutorials/create/local/preview-sample-data.png)

ファイルに応じて、ソースデータの列区切り文字（タブ、コンマ、パイプ、カスタム列区切り文字など）を選択できます。 **[!UICONTROL 区切り文字]**&#x200B;ドロップダウン矢印を選択し、メニューから適切な区切り文字を選択します。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![delimiter](../../../../images/tutorials/create/local/delimiter.png)

### マッピング

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドをターゲットスキーマ内の適切なターゲットXDMフィールドにマッピングするインターフェイスが提供されます。

![mapping-interface](../../../../images/tutorials/create/local/mapping-interface.png)

#### データのプレビュー

「**[!UICONTROL データのプレビュー]**」を選択すると、選択したデータセットから最大100行のサンプルデータのマッピング結果が表示されます。

![preview-mapping](../../../../images/tutorials/create/local/preview-mapping.png)

プレビュー時、ID列は、マッピング結果を検証する際に必要な重要な情報なので、最初のフィールドとして優先順位付けされます。 終了したら、「**[!UICONTROL 閉じる]**」を選択します。

![preview-panel](../../../../images/tutorials/create/local/preview-panel.png)

#### 計算フィールドの追加

計算フィールドでは、入力スキーマの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して参照を容易にできます。

「**[!UICONTROL 計算済みフィールドを追加]**」ボタンを選択して次に進みます。

![add-calculated-field](../../../../images/tutorials/create/local/add-calculated-field.png)

[!UICONTROL 計算済みフィールドを作成]パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。 タブの1つを選択して、式エディターに関数、フィールドまたは演算子を追加します。

![create-calculated-field](../../../../images/tutorials/create/local/create-calculated-field.png)

| タブ | 説明 |
| --------- | ----------- |
| 関数 | 「関数」タブには、データの変換に使用できる関数が一覧表示されます。 計算フィールド内で使用できる関数の詳細については、[データ準備（マッパー）関数](../../../../../data-prep/functions.md)の使用に関するガイドを参照してください。 |
| フィールド | 「フィールド」タブには、ソーススキーマで使用できるフィールドと属性が表示されます。 |
| 演算子 | 「オペレーター」タブには、データの変換に使用できる演算子が一覧表示されます。 |

式エディターを選択して、フィールド、関数、演算子を手動で追加します。 計算フィールドを作成したら、「**[!UICONTROL 保存]**」を選択して次に進みます。

![式エディター](../../../../images/tutorials/create/local/expression-editor.png)

#### ソーススキーママッピングツリーのフィルタリング

ソーススキーマをフィルタリングするには、「**[!UICONTROL すべてのソースフィールド]**」を選択し、ドロップダウンメニューからマッピングする特定のフィールドを選択します。

次の表に、ソーススキーマツリーの並べ替えオプションを示します。

| ソースフィールド | 説明 |
| --- | --- |
| [!UICONTROL すべてのソースフィールド] | このオプションは、ソーススキーマのすべてのソースフィールドを表示します。 このオプションはデフォルトで表示されます。 |
| [!UICONTROL 必須フィールド] | このオプションは、ソーススキーマをフィルタリングして、マッピングの完了に必要なフィールドのみを表示します。 |
| [!UICONTROL ID フィールド] | このオプションでは、ソーススキーマをフィルタリングして、IDとしてマークされたフィールドのみを表示します。 |
| [!UICONTROL マッピングされたフィールド] | このオプションは、ソーススキーマをフィルタリングして、既にマッピングされているフィールドのみを表示します。 |
| [!UICONTROL マッピングされていないフィールド] | このオプションでは、ソーススキーマをフィルタリングして、まだマッピングされていないフィールドのみを表示します。 |
| [!UICONTROL レコメンデーションを含むフィールド] | このオプションでは、ソーススキーマをフィルタリングして、マッピングレコメンデーションを含むフィールドのみを表示します。 |

![all-source-fields](../../../../images/tutorials/create/local/all-source-fields.png)

#### インテリジェントな推奨事項

Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを自動的に提供します。 使用例に合わせてマッピングルールを手動で調整できます。

![source-schema-tree](../../../../images/tutorials/create/local/source-schema-tree.png)

すべての自動生成マッピング値を受け入れるには、「**[!UICONTROL すべてのターゲットフィールドを受け入れる]**」を選択します。

![all-target-fields](../../../../images/tutorials/create/local/all-target-fields.png)

ソーススキーマに複数のレコメンデーションが使用できる場合があります。 この場合、マッピングカードには最も目立つレコメンデーションが表示され、その後に使用可能なレコメンデーションの数を示す青い円が表示されます。 電球アイコンを選択すると、追加の推奨事項のリストが表示されます。 代わりに、マッピング先のレコメンデーションの横にあるチェックボックスを選択して、代替レコメンデーションの1つを選択できます。

![手動マッピング](../../../../images/tutorials/create/local/manual-mapping.png)

または、ソーススキーマを手動でターゲットスキーマにマッピングすることもできます。 それには、マッピングするソーススキーマの上にマウスポインターを置き、プラス(`+`)アイコンを選択します。

![select-plus-icon](../../../../images/tutorials/create/local/select-plus-icon.png)

「**[!UICONTROL ソースをターゲットフィールドにマッピング]**」ポップオーバーが表示されます。 ここから、マッピングするフィールドを選択し、「**[!UICONTROL 保存]**」を選択して新しいマッピングを追加できます。

![map-source-to-target-field](../../../../images/tutorials/create/local/map-source-to-target-field.png)

終了したら、「**[!UICONTROL 終了]**」を選択します。

![終了](../../../../images/tutorials/create/local/finish.png)

## データ取得の監視

CSVファイルがマッピングされて作成されたら、監視ダッシュボードを使用して、CSVファイルを使用して取得されるデータを監視できます。 詳しくは、UI](../../../../../dataflows/ui/monitor-sources.md)での[ソースのデータフローの監視に関するチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、フラットな CSV ファイルを XDM スキーマにマッピングし、Platform に取り込むことができます。このデータは、[!DNL Real-time Customer Profile]などのダウンストリームの[!DNL Platform]サービスで使用できるようになりました。 詳しくは、[[!DNL Real-time Customer Profile]](../../../../../profile/home.md)の概要を参照してください。

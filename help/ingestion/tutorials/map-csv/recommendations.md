---
title: AI で生成されたレコメンデーション（ベータ版）を使用して、CSV ファイルを XDM スキーマにマッピングする
description: このチュートリアルでは、AI で生成されたレコメンデーションを使用して、CSV ファイルを XDM スキーマにマッピングする方法について説明します。
exl-id: 1daedf0b-5a25-4ca5-ae5d-e9ee1eae9e4d
source-git-commit: a9887535b12b8c4aeb39bb5a6646da88db4f0308
workflow-type: ht
source-wordcount: '1043'
ht-degree: 100%

---

# AI で生成されたレコメンデーション（ベータ版）を使用して、CSV ファイルを XDM スキーマにマッピングする

>[!IMPORTANT]
>
>この機能は現在ベータ版で利用可能で、お客様の組織はまだアクセスできない可能性があります。ドキュメントと機能は変更される場合があります。
>
>Platform で一般に利用可能な CSV マッピング機能について詳しくは、[既存のスキーマへの CSV ファイルのマッピング](./existing-schema.md)に関するドキュメントを参照してください。

CSV データを [!DNL Adobe Experience Platform] に取り込むには、データを [!DNL Experience Data Model]（XDM）スキーマにマッピングする必要があります。マッピング先を[既存のスキーマ](./existing-schema.md)に選択できます。ただし、使用するスキーマや構造が不明な場合は、代わりに、Platform UI 内の機械学習（ML）モデルに基づく動的なレコメンデーションを使用できます。

## はじめに

このチュートリアルでは、[!DNL Platform] の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM System)]](../../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
   * 少なくとも、[XDM での動作](../../../xdm/home.md#data-behaviors)の概念を理解して、データを[!UICONTROL プロファイル]クラス（レコードの動作）または [!UICONTROL ExperienceEvent] クラス（時系列の動作）にマッピングするかどうかを決定できるようになる必要があります。
* [バッチ取得](../../batch-ingestion/overview.md)： [!DNL Platform] がユーザー指定のデータファイルからデータを取り込む方法。
* [Adobe Experience Platform データ準備](../../batch-ingestion/overview.md)：取り込んだデータを XDM スキーマに準拠するようにマッピングおよび変換できる一連の機能。[データ準備機能](../../../data-prep/functions.md)に関するドキュメントは、スキーママッピングに特に関連します。

## データフローの詳細を入力

Experience Platform UI で、左側のナビゲーションの「**[!UICONTROL ソース]**」を選択します。**[!UICONTROL カタログ]**&#x200B;ビューで、**[!UICONTROL ローカルシステム]**&#x200B;カテゴリに移動します。**[!UICONTROL ローカルファイルをアップロード]**&#x200B;で、「**[!UICONTROL データを追加]**」を選択します。

![Platform UI の[!UICONTROL ソース]カタログで、[!UICONTROL ローカルファイルのアップロード]にある「[!UICONTROL データを追加]」が選択された状態](../../images/tutorials/map-csv-recommendations/local-file-upload.png)

**[!UICONTROL XDM スキーマに CSV をマッピング]**&#x200B;のワークフローが表示されるので、**[!UICONTROL データフローの詳細]**&#x200B;手順を開始します。

「**[!UICONTROL ML レコメンデーションを使用して新しいスキーマを作成する]**」を選択し、新しいコントロールを表示します。マッピングする CSV データに適したクラス（[!UICONTROL Profile] または [!UICONTROL ExperienceEvent]）を選択します。 必要に応じて、ドロップダウンメニューを使用して、ご自分のビジネスに関連する業界を選択するか、提供されたカテゴリが該当しない場合は、空白のままにすることもできます。 組織が [B2B](../../../xdm/tutorials/relationship-b2b.md) モデルで運営する場合、「**[!UICONTROL B2B データ]**」チェックボックスを選択します。

![ML レコメンデーションオプションが選択された場合の[!UICONTROL データフローの詳細]手順。 クラスに[!UICONTROL プロファイル]、業界に[!UICONTROL 通信業]が選択されている場合](../../images/tutorials/map-csv-recommendations/select-class-and-industry.png)

ここから、CSV データから作成されるスキーマの名前と、そのスキーマで取り込まれるデータを含む出力データセットの名前を指定します。

続行する前に、オプションで、データフローに次の追加機能を設定できます。

| 入力名 | 説明 |
| --- | --- |
| [!UICONTROL 説明] | データフローに関する説明。 |
| [!UICONTROL エラー診断] | 有効にすると、新しく取り込んだバッチに対してエラーメッセージが生成され、[API](../../batch-ingestion/api-overview.md) で対応するバッチを取得する際に表示できます。 |
| [!UICONTROL 部分取り込み] | 有効にすると、新しいバッチデータの有効なレコードは、指定したエラーしきい値内で取り込まれます。このしきい値を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 |
| [!UICONTROL データフローの詳細] | CSV データを Platform に取り込むデータフローの名前と説明（オプション）を入力します。ワークフローを開始すると、データフローにデフォルト名が自動的に割り当てられます。名前の変更はオプションです。 |
| [!UICONTROL アラート] | データフローが開始された後に、データフローのステータスに関して受け取りを希望する[製品内アラート](../../../observability/alerts/overview.md)をリストから選択します。 |

{style=&quot;table-layout:auto&quot;}

データフローの設定が終了したら、「**[!UICONTROL 次へ]**」を選択します。

![[!UICONTROL データフローの詳細]セクションが完了しました](../../images/tutorials/map-csv-recommendations/dataflow-detail-complete.png)

## データの選択

**[!UICONTROL データを選択]**&#x200B;の手順で、左の列を使用して CSV ファイルをアップロードします。**[!UICONTROL ファイルを選択]**&#x200B;を選択して、ファイルを開くエクスプローラーダイアログを開いてファイルを選択するか、直接ファイルを列にドラッグ＆ドロップします。

![[!UICONTROL データを選択]する手順でハイライト表示された「[!UICONTROL ファイルを選択]」ボタンおよびドラッグ＆ドロップ](../../images/tutorials/map-csv-recommendations/upload-files.png)

ファイルをアップロードすると、サンプルデータセクションが表示され、受信したデータの最初の 10 行が表示され、正しくアップロードされたことを確認できます。「**[!UICONTROL 次へ]**」をクリックして続行します。

![ワークスペース内で入力されるサンプルデータ行](../../images/tutorials/map-csv-recommendations/data-uploaded.png)

## スキーママッピングの設定

データフロー設定とアップロードした CSV ファイルに基づいて、ML モデルが実行され、新しいスキーマを生成します。処理が完了すると、[!UICONTROL マッピング]手順が表示され、生成されたスキーマ構造の完全にナビゲーション可能なビューと共に、個々のフィールドのマッピングが表示されます。

![UI の[!UICONTROL マッピング]手順で、マッピングされたすべての CSV フィールドと結果のスキーマ構造を表示する](../../images/tutorials/map-csv-recommendations/schema-generated.png)

ここから、必要に応じてオプションで「[フィールドマッピングを編集](#edit-mappings)」または「[関連付けられているフィールドグループを変更](#edit-schema)」を選択できます。十分な設定ができた時点で、「**[!UICONTROL 終了]**」を選択してマッピングを完了し、事前に設定したデータフローを開始します。CSV データはシステムに取り込まれ、生成されたスキーマ構造に基づいてデータセットが設定され、ダウンストリームの Platform サービスで利用できる状態になります。

![「[!UICONTROL 終了]」ボタンを選択している状態で、CSV マッピングプロセスを完了する](../../images/tutorials/map-csv-recommendations/finish-mapping.png)

### フィールドマッピングの編集 {#edit-mappings}

フィールドマッピングプレビューを使用すると、既存のマッピングを編集したり、完全に削除したりできます。 UI でマッピングセットを管理する方法について詳しくは、[データ準備マッピング用 UI ガイド](../../../data-prep/ui/mapping.md#mapping-interface)を参照してください。

### フィールドグループの編集 {#edit-field-groups}

ML モデルを使用すると、CSV フィールドは既存の XDM フィールドグループに自動的にマッピングされます。特定の CSV フィールドのフィールドグループを変更する場合は、スキーマツリーの横にある「**[!UICONTROL 編集]**」を選択します。

![スキーマツリーの横で選択されている「[!UICONTROL 編集]」ボタン](../../images/tutorials/map-csv-recommendations/edit-schema-structure.png)

ダイアログが表示され、マッピング内の任意のフィールドの表示名、データタイプ、フィールドグループを編集できます。ソースフィールドの横にある「編集」アイコン（![編集アイコン](../../images/tutorials/map-csv-recommendations/edit-icon.png)）を選択して、右側の列の詳細を編集してから、「**[!UICONTROL 適用]**」を選択します。

![変更されているソースフィールドの推奨フィールドグループ](../../images/tutorials/map-csv-recommendations/select-schema-field.png)

ソースフィールドのスキーマレコメンデーションの調整が終了したら、「**[!UICONTROL 保存]**」を選択して変更を適用します。

## 次の手順

このガイドでは、AI によって生成されたレコメンデーションを使用して CSV ファイルを XDM スキーマにマッピングし、バッチ取得を使用してそのデータを Platform に取り込む方法について説明しました。

CSV ファイルを既存のスキーマにマッピングする手順については、[既存のスキーママッピングワークフロー](./existing-schema.md)を参照してください。事前に作成されたソース接続を使用した、リアルタイムでの Platform へのデータストリーミングについて詳しくは、[ソースの概要](../../../sources/home.md)を参照してください。

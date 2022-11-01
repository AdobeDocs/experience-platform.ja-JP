---
title: AI で生成されたRecommendations（ベータ版）を使用して CSV ファイルを XDM スキーマにマッピングする
description: このチュートリアルでは、AI で生成されたレコメンデーションを使用して、CSV ファイルを XDM スキーマにマッピングする方法について説明します。
source-git-commit: a8a7523c5b7f696ecc0ae89cb4e0474b44a222e7
workflow-type: tm+mt
source-wordcount: '1021'
ht-degree: 4%

---

# AI で生成されたレコメンデーション（ベータ版）を使用して、CSV ファイルを XDM スキーマにマッピングします

>[!IMPORTANT]
>
>この機能は現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。 ドキュメントと機能は変更される場合があります。
>
>Platform で一般に利用可能な CSV マッピング機能について詳しくは、 [既存のスキーマへの CSV ファイルのマッピング](./existing-schema.md).

CSV データをに取り込むため [!DNL Adobe Experience Platform]の場合、データは [!DNL Experience Data Model] (XDM) スキーマ。 マッピング先を選択できます。 [既存のスキーマ](./existing-schema.md)ただし、使用するスキーマや構造が不明な場合は、代わりに、Platform UI 内の機械学習 (ML) モデルに基づく動的なレコメンデーションを使用できます。

## はじめに

このチュートリアルでは、次のコンポーネントに関する十分な知識が必要です。 [!DNL Platform]:

* [[!DNL Experience Data Model (XDM System)]](../../../xdm/home.md)：[!DNL Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。
   * 少なくとも、 [XDM での動作](../../../xdm/home.md#data-behaviors)を使用すると、データをにマッピングするかどうかを決定できます [!UICONTROL プロファイル] クラス（レコードの動作）または [!UICONTROL ExperienceEvent] クラス（時系列の動作）を使用します。
* [バッチ取得](../../batch-ingestion/overview.md)[!DNL Platform]： がユーザー指定のデータファイルからデータを取り込む方法。
* [Adobe Experience Platform Data Prep](../../batch-ingestion/overview.md):取り込んだデータを XDM スキーマに準拠するようにマッピングおよび変換できる一連の機能です。 に関するドキュメント [データ準備関数](../../../data-prep/functions.md) は、スキーママッピングに特に関連します。

## データフローの詳細を入力

Experience PlatformUI で、 **[!UICONTROL ソース]** をクリックします。 の **[!UICONTROL カタログ]** 表示するには、 **[!UICONTROL ローカルシステム]** カテゴリ。 の下 **[!UICONTROL ローカルファイルのアップロード]**&#x200B;を選択します。 **[!UICONTROL データを追加]**.

![この [!UICONTROL ソース] Platform UI のカタログ、を使用 [!UICONTROL データを追加] under [!UICONTROL ローカルファイルのアップロード] 選択されています](../../images/tutorials/map-csv-recommendations/local-file-upload.png)

この **[!UICONTROL CSV XDM スキーマのマッピング]** ワークフローが表示され、 **[!UICONTROL データフローの詳細]** 手順

選択 **[!UICONTROL ML レコメンデーションを使用して新しいスキーマを作成する]**&#x200B;を呼び出し、新しいコントロールを表示します。 マッピングする CSV データに適したクラスを選択します ([!UICONTROL プロファイル] または [!UICONTROL ExperienceEvent]) をクリックし、ドロップダウンメニューを使用して、ビジネスに関連する業界を選択します。 組織が [B2B(B2B)](../../../xdm/tutorials/relationship-b2b.md) モデルを選択するには、 **[!UICONTROL B2B データ]** チェックボックス。

![この [!UICONTROL データフローの詳細] 「 ML レコメンデーション」オプションを選択して手順を進めます。 [!UICONTROL プロファイル] がクラスに選択され、 [!UICONTROL 通信業] 業界に選ばれる](../../images/tutorials/map-csv-recommendations/select-class-and-industry.png)

ここから、CSV データから作成されるスキーマの名前と、そのスキーマで取り込まれるデータを含む出力データセットの名前を指定します。

オプションで、データフローに次の追加機能を設定できます。

| 入力名 | 説明 |
| --- | --- |
| [!UICONTROL 説明] | データフローの説明。 |
| [!UICONTROL エラー診断] | 有効にすると、新しく取り込んだバッチに対してエラーメッセージが生成され、 [API](../../batch-ingestion/api-overview.md). |
| [!UICONTROL 部分取り込み] | 有効にすると、新しいバッチデータの有効なレコードは、指定したエラーしきい値内に取り込まれます。 このしきい値を使用すると、バッチ全体が失敗する前に許容可能なエラーの割合を設定できます。 |
| [!UICONTROL データフローの詳細] | CSV データを Platform に取り込むデータフローの名前と説明（オプション）を入力します。 このワークフローを開始すると、データフローにデフォルト名が自動的に割り当てられます。 名前の変更はオプションです。 |
| [!UICONTROL アラート] | リストから選択 [製品内アラート](../../../observability/alerts/overview.md) データフローが開始された後に、データフローのステータスに関して受け取る必要がある情報です。 |

データフローの設定が完了したら、「 **[!UICONTROL 次へ]**.

![この [!UICONTROL データフローの詳細] セクションが完了しました](../../images/tutorials/map-csv-recommendations/dataflow-detail-complete.png)

## データの選択

の **[!UICONTROL データを選択]** 手順の左の列を使用して、CSV ファイルをアップロードします。 次を選択できます。 **[!UICONTROL ファイルを選択]** をクリックしてエクスプローラーダイアログを開き、ファイルを選択するか、ファイルを列に直接ドラッグ&amp;ドロップします。

![この [!UICONTROL ファイルを選択] ボタンと、 [!UICONTROL データを選択] 手順](../../images/tutorials/map-csv-recommendations/upload-files.png)

ファイルをアップロードすると、サンプルデータセクションが表示され、受信したデータの最初の 10 行が示されます。これにより、正しくアップロードされたことを確認できます。 「**[!UICONTROL 次へ]**」をクリックして続行します。

![サンプルのデータ行がワークスペース内に入力されている](../../images/tutorials/map-csv-recommendations/data-uploaded.png)

## スキーママッピングの設定

ML モデルを実行して、データフロー設定とアップロードした CSV ファイルに基づいて新しいスキーマを生成します。 処理が完了すると、 [!UICONTROL マッピング] step は入力され、生成されたスキーマ構造の完全にナビゲーション可能なビューと共に、個々のフィールドのマッピングが表示されます。

![この [!UICONTROL マッピング] UI の手順で、マッピングされたすべての CSV フィールドと結果のスキーマ構造を表示します。](../../images/tutorials/map-csv-recommendations/schema-generated.png)

ここから、オプションでを選択できます [フィールドマッピングの編集](#edit-mappings) または [関連付けられているフィールドグループを変更する](#edit-schema) 必要に応じて。 満足したら、「 」を選択します。 **[!UICONTROL 完了]** マッピングを完了し、前に設定したデータフローを開始する場合。 この CSV データはシステムに取り込まれ、生成されたスキーマ構造に基づいてデータセットが設定され、ダウンストリームの Platform サービスで利用できる状態になります。

![この [!UICONTROL 完了] ボタンを選択し、CSV マッピングプロセスを完了しています](../../images/tutorials/map-csv-recommendations/finish-mapping.png)

### フィールドマッピングの編集 {#edit-mappings}

フィールドマッピングプレビューを使用して、既存のマッピングを編集するか、完全に削除します。 UI でマッピングセットを管理する方法について詳しくは、 [データ準備マッピング用 UI ガイド](../../../data-prep/ui/mapping.md#mapping-interface).

### フィールドグループを編集 {#edit-field-groups}

CSV フィールドは、ML モデルを使用して既存のフィールドグループに自動的にマッピングされます。 特定の CSV フィールドのフィールドグループを変更する場合は、 **[!UICONTROL 編集]** スキーマツリーの横に表示されます。

![この [!UICONTROL 編集] スキーマツリーの横で選択されているボタン](../../images/tutorials/map-csv-recommendations/edit-schema-structure.png)

ダイアログが表示され、マッピング内の任意のフィールドの表示名、データタイプ、フィールドグループを編集できます。 編集アイコン (![編集アイコン](../../images/tutorials/map-csv-recommendations/edit-icon.png)) をクリックし、選択する前に右列で詳細を編集するために、ソースフィールドの横にある **[!UICONTROL 適用]**.

![変更するソースフィールドの推奨フィールドグループ](../../images/tutorials/map-csv-recommendations/select-schema-field.png)

ソースフィールドのスキーマレコメンデーションの調整が完了したら、「 **[!UICONTROL 保存]** 変更を適用します。

## 次の手順

このガイドでは、AI 生成のレコメンデーションを使用して CSV ファイルを XDM スキーマにマッピングし、バッチ取得を使用してそのデータを Platform に取り込む方法について説明しました。

CSV ファイルを既存のスキーマにマッピングする手順については、 [既存のスキーママッピングワークフロー](./existing-schema.md). 事前に作成されたソース接続を使用した、リアルタイムでの Platform へのデータストリーミングについて詳しくは、 [ソースの概要](../../../sources/home.md).

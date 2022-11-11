---
keywords: Experience Platform;ホーム;人気のトピック;CSV のマッピング;CSV ファイルのマッピング;xdm への CSV ファイルのマッピング;xdm への CSV のマッピング;ui ガイド;
solution: Experience Platform
title: 既存の XDM スキーマへの CSV ファイルのマッピング
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して、既存の XDM スキーマに CSV ファイルをマッピングする方法について説明します。
exl-id: 15f55562-269d-421d-ad3a-5c10fb8f109c
source-git-commit: 160a75a1811afa002717e167887550715eee5471
workflow-type: ht
source-wordcount: '945'
ht-degree: 100%

---

# 既存の XDM スキーマへの CSV ファイルのマッピング

>[!NOTE]
>
>このドキュメントでは、既存の XDM スキーマに CSV ファイルをマッピングする方法について説明します。AI 生成のスキーマレコメンデーションツール（現在はベータ版）の使用方法について詳しくは、[機械学習のレコメンデーションを使用した CSV ファイルのマッピング](./recommendations.md)を参照してください。

CSV データを [!DNL Adobe Experience Platform] に取り込むには、データを [!DNL Experience Data Model]（XDM）スキーマにマッピングする必要があります。このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して、CSV ファイルを XDM スキーマにマッピングする方法について説明します。

## はじめに

このチュートリアルでは、[!DNL Platform] の次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Experience Data Model (XDM System)]](../../../xdm/home.md)：[!DNL Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。
- [バッチ取得](../../batch-ingestion/overview.md)：[!DNL Platform] がユーザー指定のデータファイルからデータを取り込む方法。
- [Adobe Experience Platform データ準備](../../batch-ingestion/overview.md)：取り込んだデータを XDM スキーマに準拠するようにマッピングおよび変換できる一連の機能。[データ準備関数](../../../data-prep/functions.md)に関するドキュメントは、特にスキーママッピングに関連しています。

また、このチュートリアルでは、CSV データの取り込み先のデータセットを既に作成している必要があります。UI でデータセットを作成する手順については、[データ取得のチュートリアル](../ingest-batch-data.md)を参照してください。

## 宛先の選択

[[!DNL Adobe Experience Platform]](https://platform.adobe.com) にログインし、左側のナビゲーション バーから「**[!UICONTROL ワークフロー]**」を選択して、**[!UICONTROL ワークフロー]**&#x200B;ワークスペースにアクセスします。

**[!UICONTROL ワークフロー]**&#x200B;画面から、「**[!UICONTROL データ取り込み]**」セクションで「**[!UICONTROL XDM スキーマに CSV をマッピング]**」を選択し、「**[!UICONTROL 起動]**」を選択します。

![](../../images/tutorials/map-a-csv-file/workflows.png)

「**[!UICONTROL XDM スキーマに CSV をマッピング]**」ワークフローが表示されるので、**[!UICONTROL 宛先]**&#x200B;手順を開始します。取り込むインバウンドデータのデータセットを選択します。既存のデータセットを使用することも、新しいデータセットを作成することもできます。

**既存のデータセットを使用する**

CSV データを既存のデータセットに取り込むには、「**[!UICONTROL 既存のデータセットを使用]**」を選択します。検索機能を使用するか、パネル内の既存のデータセットのリストをスクロールすると、既存のデータセットを取得できます。

![](../../images/tutorials/map-a-csv-file/use-existing-dataset.png)

CSV データを新しいデータセットに取り込むには、「**[!UICONTROL 新しいデータセットを作成]**」を選択し、表示されたフィールドにデータセットの名前と説明を入力します。検索機能を使用するか、提供されたスキーマのリストをスクロールして、スキーマを選択します。「**[!UICONTROL 次へ]**」を選択して次に進みます。

![](../../images/tutorials/map-a-csv-file/create-new-dataset.png)

## データの追加

**[!UICONTROL データを追加]**&#x200B;手順が表示されます。CSV ファイルを指定されたスペースにドラッグ＆ドロップするか、「**[!UICONTROL ファイルを選択]**」を選択して CSV ファイルを手動で入力します。

![](../../images/tutorials/map-a-csv-file/add-data.png)

ファイルがアップロードされると、「**[!UICONTROL サンプルデータ]**」セクションが表示され、データの最初の 10 行が表示されます。データが期待どおりにアップロードされたことを確認したら、「**[!UICONTROL 次へ]**」を選択します。

![](../../images/tutorials/map-a-csv-file/sample-data.png)

## XDM スキーマフィールドへの CSV フィールドのマッピング

「**[!UICONTROL マッピング]**」手順が表示されます。CSV ファイルの列は「**[!UICONTROL ソースフィールド]**」の下にリストされ、対応する XDM スキーマフィールドが「**[!UICONTROL ターゲットフィールド]**」の下にリストされます。

[!DNL Platform] は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを自動的に提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。

![](../../images/tutorials/map-a-csv-file/mapping-with-suggestions.png)

すべての自動生成マッピング値を承認するには、「[!UICONTROL すべてのターゲットフィールドを承認]」というラベルの付いたチェックボックスを選択します。

![](../../images/tutorials/map-a-csv-file/filled-mapping-with-suggestions.png)

ソーススキーマに複数のレコメンデーションが使用できる場合があります。 これが発生すると、マッピングカードに最も目立つレコメンデーションが表示され、その後に使用可能なレコメンデーションの数を含む青い円が表示されます。電球アイコンを選択すると、追加のレコメンデーションのリストが表示されます。代わりに、マッピング先のレコメンデーションの横にあるチェックボックスをオンにして、代替レコメンデーションの 1 つを選択できます。

![](../../images/tutorials/map-a-csv-file/multiple-recommendations.png)

または、ソーススキーマをターゲットスキーマに手動でマッピングすることもできます。マッピングするソーススキーマにポインタを合わせて、プラスアイコンを選択します。

![](../../images/tutorials/map-a-csv-file/mapping-with-suggestions-and-buttons.png)

**[!UICONTROL ソースをターゲットフィールドにマッピング]**&#x200B;ポップオーバーが表示されます。ここから、マッピングするフィールドを選択し、「**[!UICONTROL 保存]**」を選択して新しいマッピングを追加します。

![](../../images/tutorials/map-a-csv-file/manual-mapping.png)

マッピングの 1 つを削除する場合は、そのマッピングにポインタを合わせて、マイナスアイコンを選択します。

### 計算フィールドの追加 {#add-calculated-field}

計算フィールドでは、入力スキーマの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して参照を容易にできます。

「**[!UICONTROL 計算フィールドを追加]**」ボタンを選択して続行します。

![](../../images/tutorials/map-a-csv-file/add-calculated-field.png)

**[!UICONTROL 計算フィールドの作成]** パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。タブの 1 つを選択して、式エディターに関数、フィールドまたは演算子を追加します。

![](../../images/tutorials/map-a-csv-file/create-calculated-fields.png)

| タブ | 説明 |
| --------- | ----------- |
| フィールド | 「フィールド」タブには、ソーススキーマで使用できるフィールドと属性が表示されます。 |
| 関数 | 「関数」タブには、データの変換に使用できる関数が一覧表示されます。計算フィールド内で使用できる関数の詳細については、 [データ準備（マッパー）関数の使用](../../../data-prep/functions.md) に関するガイドを参照してください。 |
| 演算子 | 「演算子」タブには、データの変換に使用できる演算子が一覧表示されます。 |

中央にある式エディターを使用して、フィールド、関数、演算子を手動で追加できます。 式の作成を開始するには、エディターを選択します。

![](../../images/tutorials/map-a-csv-file/create-calculated-field.png)

「**[!UICONTROL 保存]**」を選択して次に進みます。

マッピング画面が再表示され、新しく作成したソースフィールドが表示されます。 対応するターゲットフィールドを適用し、「**[!UICONTROL 完了]**」を選択してマッピングを完了します。

![](../../images/tutorials/map-a-csv-file/new-calculated-field.png)

## データ取得の監視

CSV ファイルがマッピングされ、作成されたら、CSV ファイルを通じて取り込まれるデータを監視できます。データ取り込みの監視について詳しくは、[データ取り込みの監視](../../../ingestion/quality/monitor-data-ingestion.md)に関するチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、フラットな CSV ファイルを XDM スキーマにマッピングし、[!DNL Platform] に取り込むことができます。[!DNL Real-time Customer Profile] などのダウンストリームの [!DNL Platform] サービスで、このデータを使用できるようになりました。詳しくは、[[!DNL Real-time Customer Profile]](../../../profile/home.md) の概要を参照してください。

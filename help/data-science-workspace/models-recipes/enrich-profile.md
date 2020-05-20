---
keywords: Experience Platform;machine learning model;Data Science Workspace;Real-time Customer Profile;popular topics
solution: Experience Platform
title: 機械学習インサイトによるリアルタイムの顧客プロファイルの強化
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34
workflow-type: tm+mt
source-wordcount: '1226'
ht-degree: 0%

---


# 機械学習インサイトによるリアルタイムの顧客プロファイルの強化

Adobe Experience Platform Data Science Workspaceは、機械学習モデルを作成、評価、利用してデータ予測とインサイトを生成するためのツールとリソースを提供します。 機械学習インサイトがプロファイル対応データセットに取り込まれると、同じデータがプロファイルレコードとして取り込まれ、Experience Platform Segmentation Serviceを使用して関連要素のサブセットにセグメント化できます。

このドキュメントでは、リアルタイム顧客プロファイルを機械学習のインサイトに強化するためのチュートリアルを順を追って説明します。手順は次のセクションに分かれています。

1. [出力スキーマとデータセットの作成](#create-an-output-schema-and-dataset)
2. [出力スキーマとデータセットの設定](#configure-an-output-schema-and-dataset)
3. [セグメントビルダーを使用したセグメントの作成](#create-segments-using-the-segment-builder)

## はじめに

このチュートリアルでは、プロファイルデータの取り込みとセグメントの作成に関わるAdobe Experience Platformの様々な側面について、十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

* [リアルタイム顧客プロファイル](../../rtcdp/overview.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [IDサービス](../../identity-service/home.md): プラットフォームに取り込まれる個別のデータソースからIDをブリッジすることで、リアルタイムの顧客プロファイルを有効にします。
* [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも参照することを強くお勧めします。

* [スキーマ構成の基本](../../xdm/schema/composition.md): XDMスキーマ、構築ブロック、原則、Experience Platformで使用するスキーマを構成するためのベストプラクティスについて説明します。
* [スキーマエディタのチュートリアル](../../xdm/tutorials/create-schema-ui.md): Experience Platform内のスキーマエディターを使用してスキーマを作成する手順について詳しく説明します。

## 出力スキーマとデータセットの作成 {#create-an-output-schema-and-dataset}

リアルタイム顧客プロファイルをスコアリングインサイトで豊かにするための最初の手順は、データが定義する現実世界のオブジェクト（人物など）を把握することです。 データに関する理解を得ることで、リレーショナルデータベースの設計と同様、データに意味を持つ構造を記述し、設計できます。

スキーマの構成は、クラスの割り当てから始まります。 スキーマに格納されるデータ（レコードまたは時系列）の行動面を定義するクラスです。 この節では、スキーマビルダーを使用してスキーマを作成する基本的な手順を説明します。 詳細なチュートリアルについては、スキーマエディタを使用したスキーマの [作成に関するチュートリアルを参照してください](../../xdm/tutorials/create-schema-ui.md)。

1. Adobe Experience Platformで、「 **[!UICONTROL スキーマ]** 」タブをクリックしてスキーマブラウザーにアクセスします。 「 **[!UICONTROL スキーマを]** 作成 *」をクリックして*スキーマエディタにアクセスし、スキーマをインタラクティブに作成できます。
   ![](../images/models-recipes/enrich-rtcdp/schema_browser.png)

2. 「 *組版* 」ウィンドウで「 **[!UICONTROL 割り当て]** 」をクリックし、使用可能なクラスを参照します。
   * 既存のクラスを割り当てるには、をクリックして目的のクラスを選択し、「 **[!UICONTROL Assign Class]**」をクリックします。
      ![](../images/models-recipes/enrich-rtcdp/existing_class.png)

   * カスタムクラスを作成するには、ブラウザーウィンドウの中央上部近くにある **[!UICONTROL 新しいクラスを作成]** (Create New Class)をクリックします。 クラス名と説明を入力し、クラスの動作を選択します。 終了したら、「 **[!UICONTROL クラスを割り当て]** 」をクリックします。
      ![](../images/models-recipes/enrich-rtcdp/create_new_class.png)
   この時点で、スキーマの構造にはいくつかのクラスフィールドが含まれている必要があり、ミックスインを割り当てる準備が整います。 ミックスインは、特定の概念を説明する1つ以上のフィールドのグループです。

3. 「 *コンポジション* 」ウィンドウで、「 **[!UICONTROL Mixins]**** 」サブセクションのをクリックします。
   * 既存のMixinを割り当てるには、目的のMixinをクリックしてハイライト表示し、「 **[!UICONTROL 追加Mixin]**」をクリックします。 クラスとは異なり、複数のミックスインを1つのスキーマに割り当てるのは、割り当てる必要がある限り可能です。
      ![](../images/models-recipes/enrich-rtcdp/existing_mixin.png)

   * 新しいMixinを作成するには、ブラウザウィンドウの中央上部近くにある **** 「新しいMixinを作成」をクリックします。 ミックスインの名前と説明を入力し、完了したら「Mixinを **[!UICONTROL 割り当て]** 」をクリックします。
      ![](../images/models-recipes/enrich-rtcdp/create_new_mixin.png)

   * Mixinフィールドを追加するには、 *コンポジション* (Composition)ウィンドウ内でMixinの名前をクリックします。 次に、「構造 **[!UICONTROL 」(]** Structure *)ウィンドウ内の「* 追加フィールド」(Field)をクリックして、Mixinフィールドを追加するオプションを提供します。 それに応じて、mixinプロパティを必ず指定してください。
      ![](../images/models-recipes/enrich-rtcdp/mixin_properties.png)

4. スキーマの構築が完了したら、「 *構造* 」ウィンドウ内でスキーマの最上位フィールドをクリックし、スキーマのプロパティを右側の「プロパティー」ウィンドウに表示します。 名前と説明を入力し、「 **[!UICONTROL 保存]** 」をクリックしてスキーマを作成します。
   ![](../images/models-recipes/enrich-rtcdp/save_schema.png)

5. 新しく作成したスキーマを使用して出力データセットを作成するには、左のナビゲーション列で **[!UICONTROL 「Datasets]** 」をクリックし、「データセットを **[!UICONTROL 作成]**」をクリックします。 次の画面で、「スキーマからデータセットを **[!UICONTROL 作成]**」を選択します。
   ![](../images/models-recipes/enrich-rtcdp/dataset_overview.png)

6. スキーマブラウザーを使用して、新しく作成したスキーマを探して選択し、「 **[!UICONTROL 次へ]**」をクリックします。
   ![](../images/models-recipes/enrich-rtcdp/choose_schema.png)

7. 名前と説明（オプション）を入力し、「 **[!UICONTROL 完了]** 」をクリックしてデータセットを作成します。
   ![](../images/models-recipes/enrich-rtcdp/configure_dataset.png)

出力スキーマデータセットを作成したら、次のセクションに進み、プロファイルエンリッチメントの設定と有効化を行う準備が整いました。

## 出力スキーマとデータセットの設定 {#configure-an-output-schema-and-dataset}

データセットのプロファイルを有効にする前に、データセットのスキーマにプライマリIDフィールドを設定し、スキーマのプロファイルを有効にする必要があります。 新しいスキーマを作成して有効にする場合は、スキーマエディタを使用したスキーマの [作成に関するチュートリアルを参照してください](../../xdm/tutorials/create-schema-ui.md)。 それ以外の場合は、次の手順に従って既存のスキーマとデータセットを有効にします。

1. Adobe Experience Platformで、プロファイルを有効にする出力スキーマをスキーマブラウザーで探し、その名前をクリックして構成を表示します。
   ![](../images/models-recipes/enrich-rtcdp/schemas.png)

2. スキーマ構造を展開し、主識別子として設定する適切なフィールドを探します。 目的のフィールドをクリックして、そのプロパティを表示します。
   ![](../images/models-recipes/enrich-rtcdp/schema_structure.png)

3. フィールドの **[!UICONTROL Identity]** プロパティと **[!UICONTROL Primary Identity]** プロパティを有効にし、適切な **[!UICONTROL Identity名前空間を選択して、フィールドを主IDとして設定します]**。 変更を行ったら、 **[!UICONTROL 「適用]** 」をクリックします。
   ![](../images/models-recipes/enrich-rtcdp/set_identity.png)

4. スキーマ構造の最上位のオブジェクトをクリックしてスキーマのプロパティを表示し、 **[!UICONTROL プロファイル]** スイッチを切り替えてスキーマのプロファイルを有効にします。 「 **[!UICONTROL 保存]** 」をクリックして変更を確定すると、このスキーマを使用して作成されたデータセットをプロファイル可能にできます。
   ![](../images/models-recipes/enrich-rtcdp/enable_schema.png)

5. プロファイルを有効にするデータセットを見つけるには、データセットブラウザを使用し、その名前をクリックして詳細を表示します。
   ![](../images/models-recipes/enrich-rtcdp/datasets.png)

6. 右側のプロファイル列にある **[!UICONTROL プロファイル]** ・スイッチを切り替えて、情報用のデータセットを有効にします。
   ![](../images/models-recipes/enrich-rtcdp/enable_dataset.png)

データがプロファイル対応データセットに取り込まれると、同じデータがプロファイルレコードとして取り込まれます。 スキーマとデータセットを準備したら、適切なモデルを使用してスコアリング実行を実行し、データセットに含まれるデータを生成し、このチュートリアルでセグメントビルダーを使用してインサイトセグメントを作成します。

## セグメントビルダーを使用したセグメントの作成 {#create-segments-using-the-segment-builder}

インサイトを生成し、プロファイル対応データセットに取り込んだので、セグメントビルダーを使用して関連要素のサブセットを識別し、データを管理できます。 独自のセグメントを作成するには、次の手順に従います。

1. Adobe Experience Platformで、「 **[!UICONTROL セグメント]** 」タブをクリックし、「セグメントを **[!UICONTROL 作成]** 」をクリックしてセグメントビルダーにアクセスします。
   ![](../images/models-recipes/enrich-rtcdp/segments_overview.png)

2. セグメントビルダー内で、左側のレールから、セグメントの主要な構成要素にアクセスできます。 属性、イベントおよび既存のセグメント。 各文書パーツは、それぞれのタブに表示されます。 プロファイルが有効なスキーマを拡張するクラスを選択し、セグメントの構築ブロックを参照して探します。
   ![](../images/models-recipes/enrich-rtcdp/segment_builder.png)

3. 構築ブロックをルールビルダーのキャンバスにドラッグ&amp;ドロップし、比較文を指定して作成を完了します。
   ![](../images/models-recipes/enrich-rtcdp/drag_fill.gif)

4. セグメントを作成する際に、 *セグメントのプロパティ* パネルを観察することで、セグメントの結果の予測をプレビューできます。
   ![](../images/models-recipes/enrich-rtcdp/preview_segment.gif)

5. 適切な「 **[!UICONTROL 結合ポリシー]**」を選択し、名前とオプションの説明を入力し、「 **[!UICONTROL 保存]** 」をクリックして新しいセグメントを完成させます。
   ![](../images/models-recipes/enrich-rtcdp/save_segment.png)


## 次の手順 {#next-steps}

このドキュメントでは、プロファイル用のスキーマとデータセットを有効にするために必要な手順を順を追って説明し、セグメントビルダーを使用したインサイトセグメントの作成ワークフローを簡単に説明しました。 セグメントとセグメントビルダーについて詳しくは、 [Segmentationサービスの概要を参照してください](../../segmentation/home.md)。
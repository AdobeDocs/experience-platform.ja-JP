---
keywords: Experience Platform;machine learning model;Data Science Workspace;Real-time Customer Profile;popular topics
solution: Experience Platform
title: 機械学習インサイトによるリアルタイムの顧客プロファイルの強化
topic: Tutorial
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

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

1. Adobe Experience Platformで、タブをクリックしてスキーマブラウザーにアクセスし **[!UICONTROL Schema]** ます。 をクリックして、 **[!UICONTROL Create Schema]** スキーマエディタにアクセスします **。このエディタでは、スキーマをインタラクティブに作成できます。
   ![](../images/models-recipes/enrich-rtcdp/schema_browser.png)

2. 「 *Composition* 」ウィンドウでをクリックし、使用可能なクラス **[!UICONTROL Assign]** を参照します。
   * 既存のクラスを割り当てるには、をクリックして目的のクラスを選択し、をクリックし **[!UICONTROL Assign Class]**ます。
      ![](../images/models-recipes/enrich-rtcdp/existing_class.png)

   * カスタムクラスを作成するには、ブラウザーウィンドウの中央上部付近にある「 **[!UICONTROL Create New Class]** 見つかった」をクリックします。 クラス名と説明を入力し、クラスの動作を選択します。 終了 **[!UICONTROL Assign Class]** したら、をクリックします。
      ![](../images/models-recipes/enrich-rtcdp/create_new_class.png)
   この時点で、スキーマの構造にはいくつかのクラスフィールドが含まれている必要があり、ミックスインを割り当てる準備が整います。 ミックスインは、特定の概念を説明する1つ以上のフィールドのグループです。

3. 「 *コンポジション* 」ウィンドウで、「 **[!UICONTROL Add]** ミックスイン ** 」サブセクションのをクリックします。
   * 既存のMixinを割り当てるには、をクリックし、目的のMixinをハイライト表示して、をクリックし **[!UICONTROL Add Mixin]**ます。 クラスとは異なり、複数のミックスインを1つのスキーマに割り当てるのは、割り当てる必要がある限り可能です。
      ![](../images/models-recipes/enrich-rtcdp/existing_mixin.png)

   * 新しいミックスインを作成するには、ブラウザーウィンドウの中央上部付近にある「 **[!UICONTROL Create New Mixin]** 見つかった」をクリックします。 ミックスインの名前と説明を入力し、完了したら、をクリック **[!UICONTROL Assign Mixin]** します。
      ![](../images/models-recipes/enrich-rtcdp/create_new_mixin.png)

   * Mixinフィールドを追加するには、 *コンポジション* (Composition)ウィンドウ内でMixinの名前をクリックします。 次に、「 **[!UICONTROL Add Field]** 構造 ** 」ウィンドウ内のをクリックして、Mixinフィールドを追加するオプションを提供します。 それに応じて、mixinプロパティを必ず指定してください。
      ![](../images/models-recipes/enrich-rtcdp/mixin_properties.png)

4. スキーマの構築が完了したら、「 *構造* 」ウィンドウ内でスキーマの最上位フィールドをクリックし、スキーマのプロパティを右側の「プロパティー」ウィンドウに表示します。 名前と説明を入力し、をクリックしてスキーマ **[!UICONTROL Save]** を作成します。
   ![](../images/models-recipes/enrich-rtcdp/save_schema.png)

5. 新しく作成したスキーマを使用して出力データセットを作成するには、左側のナビゲーション列 **[!UICONTROL Datasets]** をクリックし、をクリックし **[!UICONTROL Create dataset]**&#x200B;ます。 次の画面で、を選択し **[!UICONTROL Create dataset from schema]**ます。
   ![](../images/models-recipes/enrich-rtcdp/dataset_overview.png)

6. スキーマブラウザーを使用して、新しく作成したスキーマを探して選択し、をクリックし **[!UICONTROL Next]**ます。
   ![](../images/models-recipes/enrich-rtcdp/choose_schema.png)

7. 名前と説明（オプション）を入力し、をクリックしてデータセット **[!UICONTROL Finish]** を作成します。
   ![](../images/models-recipes/enrich-rtcdp/configure_dataset.png)

出力スキーマデータセットを作成したら、次のセクションに進み、プロファイルエンリッチメントの設定と有効化を行う準備が整いました。

## 出力スキーマとデータセットの設定 {#configure-an-output-schema-and-dataset}

データセットのプロファイルを有効にする前に、データセットのスキーマにプライマリIDフィールドを設定し、スキーマのプロファイルを有効にする必要があります。 新しいスキーマを作成して有効にする場合は、スキーマエディタを使用したスキーマの [作成に関するチュートリアルを参照してください](../../xdm/tutorials/create-schema-ui.md)。 それ以外の場合は、次の手順に従って既存のスキーマとデータセットを有効にします。

1. Adobe Experience Platformで、プロファイルを有効にする出力スキーマをスキーマブラウザーで探し、その名前をクリックして構成を表示します。
   ![](../images/models-recipes/enrich-rtcdp/schemas.png)

2. スキーマ構造を展開し、主識別子として設定する適切なフィールドを探します。 目的のフィールドをクリックして、そのプロパティを表示します。
   ![](../images/models-recipes/enrich-rtcdp/schema_structure.png)

3. フィールドのプロパティと **[!UICONTROL Identity]** プロパティを有効にし、適切なプロパティを選択して、フィールドを主IDとして設定し **[!UICONTROL Primary Identity]****[!UICONTROL Identity Namespace]**&#x200B;ます。 変更 **[!UICONTROL Apply]** を行ったら、をクリックします。
   ![](../images/models-recipes/enrich-rtcdp/set_identity.png)

4. スキーマ構造の最上位のオブジェクトをクリックしてスキーマのプロパティを表示し、スイッチを切り替えてスキーマのプロファイルを有効にし **[!UICONTROL Profile]** ます。 クリック **[!UICONTROL Save]** して変更を終了します。このスキーマを使用して作成されたデータセットは、プロファイルを有効にできます。
   ![](../images/models-recipes/enrich-rtcdp/enable_schema.png)

5. プロファイルを有効にするデータセットを見つけるには、データセットブラウザを使用し、その名前をクリックして詳細を表示します。
   ![](../images/models-recipes/enrich-rtcdp/datasets.png)

6. 右側のプロファイル列にあるスイッチを切り替えて、情報用のデータセットを有効にし **[!UICONTROL Profile]** ます。
   ![](../images/models-recipes/enrich-rtcdp/enable_dataset.png)

データがプロファイル対応データセットに取り込まれると、同じデータがプロファイルレコードとして取り込まれます。 スキーマとデータセットを準備したら、適切なモデルを使用してスコアリング実行を実行し、データセットに含まれるデータを生成し、このチュートリアルでセグメントビルダーを使用してインサイトセグメントを作成します。

## セグメントビルダーを使用したセグメントの作成 {#create-segments-using-the-segment-builder}

インサイトを生成し、プロファイル対応データセットに取り込んだので、セグメントビルダーを使用して関連要素のサブセットを識別し、データを管理できます。 独自のセグメントを作成するには、次の手順に従います。

1. Adobe Experience Platformで、タブをクリックし、続いてセグメントビルダー **[!UICONTROL Segments]****[!UICONTROL Create Segment]** にアクセスします。
   ![](../images/models-recipes/enrich-rtcdp/segments_overview.png)

2. セグメントビルダー内で、左側のレールから、セグメントの主要な構成要素にアクセスできます。 属性、イベントおよび既存のセグメント。 各文書パーツは、それぞれのタブに表示されます。 プロファイルが有効なスキーマを拡張するクラスを選択し、セグメントの構築ブロックを参照して探します。
   ![](../images/models-recipes/enrich-rtcdp/segment_builder.png)

3. 構築ブロックをルールビルダーのキャンバスにドラッグ&amp;ドロップし、比較文を指定して作成を完了します。
   ![](../images/models-recipes/enrich-rtcdp/drag_fill.gif)

4. セグメントを作成する際に、 *セグメントのプロパティ* パネルを観察することで、セグメントの結果の予測をプレビューできます。
   ![](../images/models-recipes/enrich-rtcdp/preview_segment.gif)

5. 適切なセグメントを選択 **[!UICONTROL Merge Policy]**&#x200B;し、名前と説明（オプション）を入力してから、をクリックして新しいセグメント **[!UICONTROL Save]** を完成させます。
   ![](../images/models-recipes/enrich-rtcdp/save_segment.png)


## 次の手順 {#next-steps}

このドキュメントでは、プロファイル用のスキーマとデータセットを有効にするために必要な手順を順を追って説明し、セグメントビルダーを使用したインサイトセグメントの作成ワークフローを簡単に説明しました。 セグメントとセグメントビルダーについて詳しくは、 [Segmentationサービスの概要を参照してください](../../segmentation/home.md)。
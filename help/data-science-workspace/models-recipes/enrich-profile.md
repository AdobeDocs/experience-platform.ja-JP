---
keywords: Experience Platform;machine learning model;Data Science Workspace;Real-time Customer Profile;popular topics
solution: Experience Platform
title: リアルタイムの顧客プロファイルと機械学習の洞察を強化
topic: Tutorial
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# リアルタイムの顧客プロファイルと機械学習の洞察を強化

Adobe Experience Platform Data Science Workspaceは、機械学習モデルを作成、評価、利用してデータ予測とインサイトを生成するためのツールとリソースを提供します。 機械学習インサイトがプロファイル対応のデータセットに取り込まれると、同じデータがプロファイルレコードとして取り込まれ、Experience Platform Segmentation Serviceを使用して関連する要素のサブセットにセグメント化できます。

このドキュメントでは、リアルタイム顧客プロファイルを機械学習の洞察に結び付けるための手順を順を追って説明します。手順は次の節に分かれています。

1. [出力スキーマとデータセット](#create-an-output-schema-and-dataset)
2. [出力データセットとスキーマセットの設定](#configure-an-output-schema-and-dataset)
3. [セグメントビルダーを使用したセグメントの作成](#create-segments-using-the-segment-builder)

## はじめに

このチュートリアルでは、プロファイルデータの取り込みとセグメントの作成に関わるAdobe Experience Platformの様々な側面を理解しておく必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

* [リアルタイム顧客プロファイル](../../rtcdp/overview.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [IDサービス](../../identity-service/home.md):異なるデータソースのIDをPlatformに取り込むことで、リアルタイムの顧客プロファイルを有効にします。
* [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

上記のドキュメントに加え、スキーマおよびスキーマエディターに関する次のガイドも確認することを強くお勧めします。

* [スキーマ構成の基本](../../xdm/schema/composition.md):XDMスキーマ、構築ブロック、原則およびExperience Platformで使用するスキーマを構成するためのベストプラクティスについて説明します。
* [スキーマエディタのチュートリアル](../../xdm/tutorials/create-schema-ui.md):Experience Platform内のプラットフォームエディターを使用してスキーマを作成するための詳細な手順を説明します。

## 出力スキーマとデータセット {#create-an-output-schema-and-dataset}

リアルタイム顧客のプロファイルをスコアリングインサイトで強化するための最初のステップは、データが定義する現実世界のオブジェクト（人物など）を把握することです。 データを理解することで、リレーショナルデータベースの設計と同様に、データに意味を持つ構造を説明し、設計できます。

クラスを割り当てることで、スキーマの構成が開始されます。 クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。 ここでは、スキーマビルダを使用してスキーマを作成する基本手順を説明します。 詳細なチュートリアルについては、チュートリアルエディタを使用した [スキーマの作成を参照してください](../../xdm/tutorials/create-schema-ui.md)。

1. Adobe Experience Platformで、「 **スキーマ** 」タブをクリックしてスキーマブラウザー 「作成」 **スキーマをクリックする** と、 *スキーマエディターにアクセスします*。このエディターで、インタラクティブにスキーマを作成できます。
   ![](../images/models-recipes/enrich-rtcdp/schema_browser.png)

2. 「構成」ウィン *ドウで* 、「割り当て」をク **リックし** 、使用可能なクラスを参照します。
   * 既存のクラスを割り当てるには、目的のクラスをクリックして強調表示し、「クラスを割り当て」をク **リックしま**す。
      ![](../images/models-recipes/enrich-rtcdp/existing_class.png)

   * カスタムクラスを作成するには、ブラウ **ザーウィンドウの中央上** 部近くにある「新しいクラスを作成」をクリックします。 クラス名と説明を入力し、クラスの動作を選択します。 完了したら **、「クラスの割り当て** 」をクリックします。
      ![](../images/models-recipes/enrich-rtcdp/create_new_class.png)
   この時点で、スキーマの構造にいくつかのクラスフィールドが含まれ、ミックスインを割り当てる準備が整いました。 ミックスインは、特定の概念を説明する1つ以上のフィールドのグループです。

3. コンポジ *ション* ウィンドウで、 *****Mixins* サブセクションのをクリックします。
   * 既存のMixinを割り当てるには、目的のMixinをクリックしてハイライトし、「 **追加Mixin」をクリックします**。 クラスとは異なり、複数のミックスインを1つのスキーマに割り当てるのは、適切な場合に限られます。
      ![](../images/models-recipes/enrich-rtcdp/existing_mixin.png)

   * 新しいミックスインを作成するには、ブ **ラウザウィンドウの中央** 上付近にある[新しいミックスインを作成]をクリックします。 Mixinの名前と説明を入力し、完了したら「 **Assign Mixin** 」をクリックします。
      ![](../images/models-recipes/enrich-rtcdp/create_new_mixin.png)

   * Mixinフィールドを追加するには、コンポジションウィンドウ内でMixinの名前をクリ *ックし* ます。 次に、構造ウィンドウ内の「フィールド」( **Field** )をクリックして、ミックスインフィールドを追加す *追加るオプションを* 提供します。 それに応じて、mixinプロパティを指定します。
      ![](../images/models-recipes/enrich-rtcdp/mixin_properties.png)

4. スキーマの作成が完了したら、「構造」( *Structure* )ウィンドウ内のスキーマの最上位フィールドをクリックし、スキーマのプロパティを右側のプロパティウィンドウに表示します。 名前と説明を入力し、「保存」をクリックし **て** 、スキーマを作成
   ![](../images/models-recipes/enrich-rtcdp/save_schema.png)

5. 新しく作成したスキーマを使用して出力データセットを作成します。作成するには、左のナビゲ **ーション列で** 「 **Datasets**」をクリックし、「Create dataset」をクリックします。 次の画面で、「データセットを **スキーマ**」を
   ![](../images/models-recipes/enrich-rtcdp/dataset_overview.png)

6. スキーマブラウザーを使用して、新しく作成したスキーマを探して選択し、「 **Next**」をクリックします。
   ![](../images/models-recipes/enrich-rtcdp/choose_schema.png)

7. 名前と説明（オプション）を入力し、「完了」をクリックし **て** 、データセットを作成します。
   ![](../images/models-recipes/enrich-rtcdp/configure_dataset.png)

これで、出力スキーマデータセットを作成し、次のセクションに進んで設定し、プロファイルエンリッチメントを有効にする準備が整いました。

## 出力データセットとスキーマセットの設定 {#configure-an-output-schema-and-dataset}

プロファイル用のデータセットを有効にする前に、データセットのスキーマにプライマリIDフィールドを設定し、プロファイル用のスキーマを有効にする必要があります。 新しいスキーマを作成して有効にする場合は、スキーマエディタを使用したスキーマの作成に関するチ [ュートリアルを参照してください](../../xdm/tutorials/create-schema-ui.md)。 それ以外の場合は、次の手順に従って既存のスキーマとデータセットを有効にします。

1. Adobe Experience Platformで、プロファイルを有効にする出力スキーマをスキーマブラウザーで探し、名前をクリックして構成を表示します。
   ![](../images/models-recipes/enrich-rtcdp/schemas.png)

2. スキーマ構造を展開し、主識別子として設定する適切なフィールドを探します。 目的のフィールドをクリックして、そのプロパティを表示します。
   ![](../images/models-recipes/enrich-rtcdp/schema_structure.png)

3. フィールドの **Identity** プロパティと **Primary Identityプロパティを有効にし、適切な** Identity **名前空間を選択して、フィールドをプライマリIDとして設定**&#x200B;します。 変更を **行ったら** 、[適用]をクリックします。
   ![](../images/models-recipes/enrich-rtcdp/set_identity.png)

4. スキーマ構造の最上位オブジェクトをクリックしてスキーマのプロパティを表示し、プロファイルスイッチを切り替えてプロファイルのスキーマを有効に **し** ます。 「保存 **** 」をクリックして変更を確定し、このスキーマを使用して作成されたデータセットをプロファイルできます。
   ![](../images/models-recipes/enrich-rtcdp/enable_schema.png)

5. データセットブラウザーを使用して、プロファイルを有効にするデータセットを探し、その名前をクリックして詳細を表示します。
   ![](../images/models-recipes/enrich-rtcdp/datasets.png)

6. 右側の情報列にある **プロファイルスイッチを切り替え** 、プロファイルのデータセットを有効にします。
   ![](../images/models-recipes/enrich-rtcdp/enable_dataset.png)

データがプロファイル対応のデータセットに取り込まれると、同じデータがデータレコードとして取り込まれるプロファイルとなります。 スキーマとデータセットの準備が整ったら、適切なモデルを使用してスコアリング実行を実行し、データセットに含まれるデータを生成し、このチュートリアルを続けて、セグメントビルダーを使用してインサイトセグメントを作成します。

## セグメントビルダーを使用したセグメントの作成 {#create-segments-using-the-segment-builder}

これで、プロファイル対応データセットにインサイトを生成し、取り込んだので、セグメントビルダーを使用して関連する要素のサブセットを識別し、データを管理できます。 次の手順に従って、独自のセグメントを作成します。

1. Adobe Experience Platformで、「セグメント」タブをクリックし **、** 「セグメントを作成」 **をクリックして** 、セグメントビルダーにアクセスします。
   ![](../images/models-recipes/enrich-rtcdp/segments_overview.png)

2. セグメントビルダー内で、左のレールから、セグメントの主要な構成要素にアクセスできます。属性、イベント、既存のセグメント。 各文書パーツはそれぞれのタブに表示されます。 プロファイルが有効なスキーマが拡張するクラスを選択し、セグメントの構築ブロックを参照して探します。
   ![](../images/models-recipes/enrich-rtcdp/segment_builder.png)

3. 構築ブロックをルールビルダーキャンバスにドラッグ&amp;ドロップし、比較文を指定して完成させます。
   ![](../images/models-recipes/enrich-rtcdp/drag_fill.gif)

4. セグメントを作成する際に、 *Segment Propertiesパネルを見て、セグメントの結果の予測* をプレビューできます。
   ![](../images/models-recipes/enrich-rtcdp/preview_segment.gif)

5. 適切な「マージ **ポリシー**」を選択し、名前とオプションの説明を入力し、「保存」をクリ **ックして** 、新しいセグメントを完了します。
   ![](../images/models-recipes/enrich-rtcdp/save_segment.png)


## 次の手順 {#next-steps}

このドキュメントでは、プロファイルのスキーマとデータセットを有効にするために必要な手順を順を追って説明し、セグメントビルダーを使用してインサイトセグメントを作成するワークフローを簡単に説明しました。 セグメントとセグメントビルダーについて詳しくは、 [Segmentation serviceの概要を参照してください](../../segmentation/home.md)。
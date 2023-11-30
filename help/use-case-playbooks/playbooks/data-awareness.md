---
solution: Experience Platform
title: ユースケースプレイブックにおけるデータ認識の概要
description: エンドインスピレーションのサンドボックスで生成されたアセットを他のサンドボックスにコピーして、価値の創出に要する時間を短縮する方法を説明します。
badgeBeta: label="ベータ版" type="Informative"
source-git-commit: cbf5f2aaf9bb8113ad5eadac888e9b4f85b199b8
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---


# ユースケースプレイブックにおけるデータ認識の概要

使用例プレイブックは、一般的なマーケティングの使用例で使用するオーディエンス、スキーマ、ジャーニーなどのアセットを生成するように設計されたマーケティングテンプレートです。 Adobe Experience Platform内では、これらのテンプレートは、複数の標準フィールドおよびフィールドグループを参照します。 ただし、場合によっては、既に独自のスキーマ、フィールド、フィールドグループを設定していることもあります。 これにより、ジャーニーなどのユースケーステンプレートで生成されるアセットの一部に、データとの互換性がなくなる場合があります。 データ認識機能を使用して、生成されたアセットを既存のアセットと適切に並べ、補完する方法を理解するには、このチュートリアルをお読みください。

## 前提条件 {#prerequisites}

このチュートリアルを読む前に、 [使用例プレイブックテンプレート](/help/use-case-playbooks/playbooks/discover.md#search-and-filter) および [インスタンスの作成](/help/use-case-playbooks/playbooks/create-share-reuse.md) 好みのプレイブックの

インスタンスを作成すると、インスピレーションが得られるサンドボックス内に、ジャーニー、セグメント、スキーマ、メッセージなどのアセットのセットが生成されます。 これらのアセットを他のサンドボックスにコピーする方法について説明します。

### パッケージの作成と公開 {#create-publish-package}

1. インスピレーションを得たサンドボックスから別のサンドボックスにオブジェクトをインポートするには、ユースケースプレイブックの目的のインスタンスを参照し、「 」を選択します。 **[!UICONTROL 別のサンドボックスに公開]** アーティファクトをパッケージとして書き出す場合。

   ![様々なユースケースインスタンスを示すGIF](/help/use-case-playbooks/assets/playbooks/data-awareness/browse-to-existing-instances-of-playbook.gif)

2. 次に、 **[!UICONTROL 別のサンドボックスに公開]** ボタンをクリックすると、モーダルが表示されます。 名前と説明（オプション）を入力し、「 」を選択します。 **[!UICONTROL 作成]**. この手順では、生成されたアセットを、別のサンドボックスに読み込むことができるパッケージにバンドルします。

   ![パッケージを作成するためのモーダル](/help/use-case-playbooks/assets/playbooks/data-awareness/create-package-modal.png)

3. 次に移動： **サンドボックス** ページを左側のナビゲーションに移動し、 **パッケージ** 「 」タブで、パッケージを検索してパブリッシュします。 ドラフト状態のパッケージを公開するには、 [サンドボックスツール](/help/sandboxes/ui/sandbox-tooling.md#add-an-object-to-an-existing-package-and-publish) 文書。

   ![パッケージはドラフト状態または非公開状態です](/help/use-case-playbooks/assets/playbooks/data-awareness/draft-mode.png)

   ![パッケージのパブリッシュ](/help/use-case-playbooks/assets/playbooks/data-awareness/publish-draft.png)

4. 公開が成功したら、パッケージの参照ページで、 **+** 「 」ボタンが有効になっています。

   ![「サンドボックス」ページの「パッケージ」タブ](/help/use-case-playbooks/assets/playbooks/data-awareness/packages.png)

   >[!NOTE]
   >
   > ドラフトモードの間はパッケージをインポートできないので、パッケージの詳細ページを開き、パッケージを公開します。

5. を選択します。 **+** を制御し、ワークフローを開始して、ユースケースプレイブックで生成されたアセットを **[!UICONTROL ターゲットサンドボックス]**. ターゲットサンドボックスを選択し、ドロップダウンを使用して、インポートするパッケージ名を確認します。 次の手順に進む前に、ジョブ名やジョブの説明などのジョブの詳細を追加します。

   ![インポートワークフローの開始、ターゲットの選択、パッケージの確認、ジョブの詳細の追加を行います。](/help/use-case-playbooks/assets/playbooks/data-awareness/import-package-import-settings.png)

   >[!NOTE]
   >
   > パッケージは他の開発用サンドボックスにのみ読み込むことができます。 実稼動サンドボックスは、このようなインポートに対して無効になっています。

6. Adobe Analytics の **[!UICONTROL 依存関係を表示]** 手順では、スキーマをマッピングし、インスピレーションを得たサンドボックスから target サンドボックスに他のアセットをコピーできます。 The **[!UICONTROL 完了]** 各スキーマをマッピングするまで、「 」ボタンは無効です。

   ![「依存関係を表示」ステップでスキーマをマッピングし、「完了」ボタンを有効にします。](/help/use-case-playbooks/assets/playbooks/data-awareness/import-package-view-dependencies.png)

### スキーマをマッピング {#map-schemas}

1. 最初のスキーマをマッピングします。 スキーママッピングダイアログに、ターゲットスキーマを選択するためのドロップダウンが表示されます。 ソーススキーマがプロファイルスキーマの場合、 [個々の和集合プロファイルスキーマ](/help/xdm/classes/individual-profile.md). ページが最初に表示されたときに、ソースデータとターゲットフィールドの間で自動生成されたマッピングレコメンデーションを確認できます。 マッピングを編集するには、ターゲットフィールドを選択し、新しいフィールドを選択します。 提案されたマッピングを変更する場合は、 **検証** ボタンを使用して新しいマッピングを検証し、新しいマッピングにリンクされている可能性のあるエラーを表示します。 選択 **保存** マッピングが完了したら、

   ![ターゲットスキーマを選択するためのドロップダウン付きのスキーママッピングダイアログ。](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-existing-fields.png)

2. 引き続きスキーマ内のすべてのフィールドのマッピングを行います。 スキーマが [イベントスキーマ](/help/xdm/classes/experienceevent.md)のダイアログには、ターゲットサンドボックス内のすべてのイベントスキーマを表示できるドロップダウンが表示されます。

   ![ドロップダウンからターゲットスキーマを選択](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-event-schema.png)

3. で使用可能なスキーマからスキーマを選択 **ターゲットサンドボックス**.

   ![スキーマを選択](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-available-schemas.png)

4. マッピングを完了し、「 」を選択します。 **保存**.

   ![マッピングを保存](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-existing-modal.png)

5. スキーマ内のすべてのフィールドのマッピングを完了したら、「 」を選択します。 **完了** をクリックして、インポートワークフローを完了します。

   ![フローを終了](/help/use-case-playbooks/assets/playbooks/data-awareness/complete-flow.png)

   >[!NOTE]
   >
   > スキーマはインスピレーションに富んだサンドボックスなので、アセットを変更することはできませんが、パッケージの依存関係であるので、スキーマ以外のアセットは変更できません。

### インポートステータス {#import-status}

1. 自動的に [**インポート**](/help/sandboxes/ui/sandbox-tooling.md#view-import-details) ページで、読み込みの進行状況を確認できます。

   ![読み込みの進行状況を示すページ](/help/use-case-playbooks/assets/playbooks/data-awareness/import-progress.png)

2. パッケージの読み込み中に、パッケージのアセットがターゲットサンドボックスに作成されます。 完了したら、インポートプロセス中にマッピングしたフィールドを参照します。 これでプロセスが完了し、インスピレーションが得られたサンドボックスのアセットも、テスト用のターゲットサンドボックスに存在するようになりました。

   ![ターゲットサンドボックスで生成されたアセット](/help/use-case-playbooks/assets/playbooks/data-awareness/packages.png)

## 次の手順

このガイドを読むと、使用例のプレイブックを、 [サンドボックスツール](/help/sandboxes/ui/sandbox-tooling.md#monitor-import-jobs-and-view-import-objects-details) を使用して、スキーマを参照する実行可能なジャーニーを作成します。 共通の [Real-Time CDPの使用例](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md).
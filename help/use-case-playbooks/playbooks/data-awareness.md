---
solution: Experience Platform
title: ユースケースプレイブックでのデータ認識の概要
description: エンドインスピレーショナルサンドボックスで生成されたアセットを他のサンドボックスにコピーして、価値創出までの時間を短縮する方法を説明します。
role: Developer
exl-id: 537eff13-f5fe-4cc9-9769-ab47b3cecda7
source-git-commit: 54b3d2ef22f7afb47fa8c9430c5c1645c94c837d
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 0%

---

# Publish プレイブックで生成されたアセットの他のサンドボックスへの転送 {#publish-to-other-sandboxes}

ユースケースプレイブックは、一般的なマーケティングユースケース用にオーディエンス、スキーマ、ジャーニーなどのアセットを生成するように設計されたマーケティングテンプレートです。 プレイブックで作成されたアセットを感動的なサンドボックスでテストできます。準備が整ったら、アセットを他の開発用サンドボックスに読み込んで、それらのサンドボックスで使用可能なデータをさらにテストできます。 テストに問題がなければ、開発用サンドボックスから実稼動用サンドボックスにアセットを移動できます。

ただし、場合によっては、他の開発用サンドボックスで独自のスキーマ、フィールド、フィールドグループを既に設定していることがあります。 これにより、ジャーニーなどのユースケーステンプレートで生成されたアセットの一部が、データと互換性を持たなくなる可能性があります。 データ認識機能を使用して、生成されたアセットと既存のアセットをより適切に連携および補完する方法を理解するには、このチュートリアルを参照してください。

## 前提条件 {#prerequisites}

このチュートリアルを読む前に、 [使用可能なユースケースプレイブックテンプレート](/help/use-case-playbooks/playbooks/choose.md#search-and-filter) および [インスタンスの作成](/help/use-case-playbooks/playbooks/create-share-reuse.md) 好みのプレイブックの。

インスタンスを作成すると、インスピレーションの得られるサンドボックス内に、ジャーニー、セグメント、スキーマ、メッセージなどの一連のアセットが生成されます。 これらのアセットを他のサンドボックスにコピーする方法については、こちらを参照してください。

### パッケージの作成と公開 {#create-publish-package}

>[!NOTE]
>
> パッケージは、他の開発用サンドボックスにのみ読み込むことができます。 必要な変更または更新をすべて行ったら、それらの開発用サンドボックスから実稼動環境にアセットまたはパッケージを読み込むことができます。 ユースケースプレイブックサンドボックスから実稼動環境に直接読み込むことはできません。

1. 感動的なサンドボックスから別のサンドボックスにオブジェクトを読み込むには、ユースケースプレイブックの目的のインスタンスを参照し、次のいずれかを選択します **[!UICONTROL 別のサンドボックスへのPublish]** アーティファクトをパッケージとして書き出します。

   ![様々なユースケースインスタンスを示すGIF](/help/use-case-playbooks/assets/playbooks/data-awareness/browse-to-existing-instances-of-playbook.gif)

2. を選択したら、 **[!UICONTROL 別のサンドボックスへのPublish]** ボタンをクリックすると、モーダルが表示されます。 名前と説明（オプション）を入力して、 **[!UICONTROL 作成]**. この手順では、生成されたアセットを別のサンドボックスに読み込むことができるパッケージにバンドルします。

   ![パッケージを作成するためのモーダル](/help/use-case-playbooks/assets/playbooks/data-awareness/create-package-modal.png)

3. に移動します。 **サンドボックス** 左側のナビゲーションの「」ページで、 **パッケージ** タブをクリックし、パッケージを見つけて公開します。 ドラフト状態のパッケージを公開するには、 [サンドボックスツール](/help/sandboxes/ui/sandbox-tooling.md#add-an-object-to-an-existing-package-and-publish) ドキュメント。

   ![ドラフト状態または未公開状態のパッケージ](/help/use-case-playbooks/assets/playbooks/data-awareness/draft-mode.png)

   ![パッケージの公開](/help/use-case-playbooks/assets/playbooks/data-awareness/publish-draft.png)

4. 公開が成功すると、パッケージの参照ページに次の項目が表示されます。 **+** ボタンが名前の横で有効になっています。

   ![サンドボックスページの「パッケージ」タブ](/help/use-case-playbooks/assets/playbooks/data-awareness/packages.png)

   >[!NOTE]
   >
   > ドラフトモードの間はパッケージを読み込むことができません。パッケージの詳細ページを開いてパッケージを公開してください。

5. 「」を選択します **+** ワークフローを制御して開始し、ユースケースプレイブックによって生成されたアセットをに読み込みます **[!UICONTROL ターゲットサンドボックス]**. ターゲットサンドボックスを選択し、ドロップダウンを使用して読み込むパッケージ名を確認します。 次の手順に進む前に、ジョブ名やジョブの説明などのジョブの詳細を追加します。

   ![インポートワークフローの開始、ターゲットの選択、パッケージの確認、ジョブの詳細の追加を行います。](/help/use-case-playbooks/assets/playbooks/data-awareness/import-package-import-settings.png)

6. が含まれる **[!UICONTROL 依存関係の表示]** 手順では、スキーマをマッピングし、他のアセットを感動的なサンドボックスからターゲットサンドボックスにコピーできます。 この **[!UICONTROL 終了]** 各スキーマをマッピングするまで、ボタンは無効になります。

   ![「依存関係を表示」手順でスキーマをマッピングし、「完了」ボタンを有効にします。](/help/use-case-playbooks/assets/playbooks/data-awareness/import-package-view-dependencies.png)

### スキーマのマッピング {#map-schemas}

1. 最初のスキーマのマッピング スキーママッピングダイアログに、ターゲットスキーマを選択するためのドロップダウンが表示されます。 ソーススキーマがプロファイルスキーマの場合、以外のターゲットスキーマオプションはありません [個別和集合プロファイルスキーマ](/help/xdm/classes/individual-profile.md). ページが最初に表示されたときに、ソースデータフィールドとターゲットフィールドの間に自動生成されたマッピングレコメンデーションを確認できます。 ターゲットフィールドを選択してから新しいフィールドを選択すると、マッピングを編集できます。 提案されたマッピングを変更する場合は、 **Validate** ボタン：新しいマッピングを検証し、新しいマッピングにリンクしている可能性のあるエラーを表示します。 を選択 **保存** マッピングが完了したら、

   ![ターゲットスキーマを選択するためのドロップダウンを含むスキーママッピングダイアログ。](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-existing-fields.png)

2. スキーマ内のすべてのフィールドのマッピングを続行します。 スキーマがの場合 [イベントスキーマ](/help/xdm/classes/experienceevent.md)このダイアログには、ターゲットサンドボックス内のすべてのイベントスキーマを表示できるドロップダウンが表示されます。

   ![ドロップダウンからのターゲットスキーマの選択](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-event-schema.png)

3. で使用可能なスキーマからスキーマを選択 **ターゲットサンドボックス**.

   ![スキーマを選択](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-available-schemas.png)

4. マッピングを完了し、を選択します **保存**.

   ![マッピングを保存](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-existing-modal.png)

5. スキーマ内のすべてのフィールドのマッピングが完了したら、以下を選択します。 **終了** 読み込みワークフローを完了します。

   ![フローの完了](/help/use-case-playbooks/assets/playbooks/data-awareness/complete-flow.png)

   >[!NOTE]
   >
   > これは感動的なサンドボックスなので、スキーマ以外のアセットは変更できませんが、パッケージの依存関係なので表示されます。

### インポートステータス {#import-status}

1. 自動的ににリダイレクトされます [**インポート**](/help/sandboxes/ui/sandbox-tooling.md#view-import-details) インポートの進行状況を確認できるページ。

   ![読み込みの進行状況を示すページ](/help/use-case-playbooks/assets/playbooks/data-awareness/import-progress.png)

2. パッケージの読み込み中、パッケージのアセットは、ターゲットサンドボックスに作成されます。 入力が完了すると、読み込みプロセス中にマッピングされたフィールドが参照されます。 これでプロセスが完了し、感動的なサンドボックスのアセットがターゲットサンドボックスにも表示され、テストできるようになりました。

   ![ターゲットサンドボックスに生成されたアセット](/help/use-case-playbooks/assets/playbooks/data-awareness/packages.png)

## 次の手順

このガイドを読むことで、ユースケースプレイブックをと共に活用する方法について、理解が深まりました [サンドボックスツール](/help/sandboxes/ui/sandbox-tooling.md#monitor-import-jobs-and-view-import-objects-details) スキーマを参照する実行可能なジャーニーを作成します。 一般的な機能の詳細情報 [Real-Time CDPのユースケース](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md).

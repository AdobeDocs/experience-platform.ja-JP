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

このチュートリアルを読む前に、[ 使用可能なユースケースプレイブックテンプレート ](/help/use-case-playbooks/playbooks/choose.md#search-and-filter) および [ インスタンスの作成 ](/help/use-case-playbooks/playbooks/create-share-reuse.md) を参照してください。

インスタンスを作成すると、インスピレーションの得られるサンドボックス内に、ジャーニー、セグメント、スキーマ、メッセージなどの一連のアセットが生成されます。 これらのアセットを他のサンドボックスにコピーする方法については、こちらを参照してください。

### パッケージの作成と公開 {#create-publish-package}

>[!NOTE]
>
> パッケージは、他の開発用サンドボックスにのみ読み込むことができます。 必要な変更または更新をすべて行ったら、それらの開発用サンドボックスから実稼動環境にアセットまたはパッケージを読み込むことができます。 ユースケースプレイブックサンドボックスから実稼動環境に直接読み込むことはできません。

1. 感動的なサンドボックスから別のサンドボックスにオブジェクトを読み込むには、ユースケースプレイブックの目的のインスタンスを参照し、**[!UICONTROL 別のサンドボックスにPublish]** を選択して、アーティファクトをパッケージとして書き出します。

   ![ 様々なユースケースインスタンスを示すGIF](/help/use-case-playbooks/assets/playbooks/data-awareness/browse-to-existing-instances-of-playbook.gif)

2. 「**[!UICONTROL 別のサンドボックスにPublish]**」ボタンを選択すると、モーダルが表示されます。 名前とオプションの説明を入力し、「**[!UICONTROL 作成]**」を選択します。 この手順では、生成されたアセットを別のサンドボックスに読み込むことができるパッケージにバンドルします。

   ![ パッケージを作成するためのモーダル ](/help/use-case-playbooks/assets/playbooks/data-awareness/create-package-modal.png)

3. 左側のナビゲーションの **サンドボックス** ページに移動し、「**パッケージ**」タブを選択し、パッケージを見つけて公開します。 ドラフト状態のパッケージを公開するには、[ サンドボックスツール ](/help/sandboxes/ui/sandbox-tooling.md#add-an-object-to-an-existing-package-and-publish) ドキュメントの手順に従います。

   ![ パッケージがドラフトまたは未公開の状態です ](/help/use-case-playbooks/assets/playbooks/data-awareness/draft-mode.png)

   ![ パッケージの公開 ](/help/use-case-playbooks/assets/playbooks/data-awareness/publish-draft.png)

4. 公開が正常に完了すると、パッケージの参照ページで、名前の横に「**+**」ボタンが有効になっているはずです。

   ![ サンドボックスページの「パッケージ」タブ ](/help/use-case-playbooks/assets/playbooks/data-awareness/packages.png)

   >[!NOTE]
   >
   > ドラフトモードの間はパッケージを読み込むことができません。パッケージの詳細ページを開いてパッケージを公開してください。

5. **+** コントロールを選択してワークフローを開始し、ユースケースプレイブックによって生成されたアセットを **[!UICONTROL ターゲットサンドボックス]** に読み込みます。 ターゲットサンドボックスを選択し、ドロップダウンを使用して読み込むパッケージ名を確認します。 次の手順に進む前に、ジョブ名やジョブの説明などのジョブの詳細を追加します。

   ![ インポートワークフローの開始、ターゲットの選択、パッケージの確認、ジョブの詳細の追加 ](/help/use-case-playbooks/assets/playbooks/data-awareness/import-package-import-settings.png)

6. **[!UICONTROL 依存関係の表示]** 手順では、スキーマをマッピングし、他のアセットを感動的なサンドボックスからターゲットサンドボックスにコピーできます。 各スキーマをマッピングするまで、「**[!UICONTROL 完了]**」ボタンは無効になります。

   ![ 「依存関係を表示」手順でスキーマをマッピングし、「終了」ボタンを有効にします。](/help/use-case-playbooks/assets/playbooks/data-awareness/import-package-view-dependencies.png)

### スキーマのマッピング {#map-schemas}

1. 最初のスキーマのマッピング スキーママッピングダイアログに、ターゲットスキーマを選択するためのドロップダウンが表示されます。 ソーススキーマがプロファイルスキーマの場合、[ 個々の結合プロファイルスキーマ ](/help/xdm/classes/individual-profile.md) 以外にターゲットスキーマオプションはありません。 このページが最初に表示されると、Source データとターゲットフィールドの間に、自動生成されたマッピングレコメンデーションを確認できます。 ターゲットフィールドを選択してから新しいフィールドを選択すると、マッピングを編集できます。 提示されたマッピングを変更する場合は、「**検証**」ボタンを使用して新しいマッピングを検証し、新しいマッピングにリンクされている可能性のあるエラーを表示します。 マッピングが完了したら、「**保存**」を選択します。

   ![ ターゲットスキーマを選択するためのドロップダウンを含むスキーマママッピングダイアログ。](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-existing-fields.png)

2. スキーマ内のすべてのフィールドのマッピングを続行します。 スキーマが [ イベントスキーマ ](/help/xdm/classes/experienceevent.md) の場合、ダイアログにドロップダウンが表示され、ターゲットサンドボックス内のすべてのイベントスキーマを表示できます。

   ![ ドロップダウンからのターゲットスキーマの選択 ](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-event-schema.png)

3. **ターゲットサンドボックス** で使用可能なスキーマからスキーマを選択します。

   ![ スキーマを選択 ](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-available-schemas.png)

4. マッピングを完了し、「**保存**」を選択します。

   ![ マッピングを保存 ](/help/use-case-playbooks/assets/playbooks/data-awareness/map-to-existing-modal.png)

5. スキーマ内のすべてのフィールドのマッピングが完了したら、「**完了**」を選択してインポートワークフローを完了します。

   ![ フローの完了 ](/help/use-case-playbooks/assets/playbooks/data-awareness/complete-flow.png)

   >[!NOTE]
   >
   > これは感動的なサンドボックスなので、スキーマ以外のアセットは変更できませんが、パッケージの依存関係なので表示されます。

### インポートステータス {#import-status}

1. 読み込みの進行状況を確認できる [**読み込み**](/help/sandboxes/ui/sandbox-tooling.md#view-import-details) ページに自動的にリダイレクトされます。

   ![ 読み込みの進行状況を示すページ ](/help/use-case-playbooks/assets/playbooks/data-awareness/import-progress.png)

2. パッケージの読み込み中、パッケージのアセットは、ターゲットサンドボックスに作成されます。 入力が完了すると、読み込みプロセス中にマッピングされたフィールドが参照されます。 これでプロセスが完了し、感動的なサンドボックスのアセットがターゲットサンドボックスにも表示され、テストできるようになりました。

   ![ ターゲットサンドボックスで生成されたアセット ](/help/use-case-playbooks/assets/playbooks/data-awareness/packages.png)

## 次の手順

このガイドを読むことで、ユースケースプレイブックを [ サンドボックスツール ](/help/sandboxes/ui/sandbox-tooling.md#monitor-import-jobs-and-view-import-objects-details) と共に活用して、スキーマを参照する実行可能なジャーニーを作成する方法について、理解が深まりました。 一般的な [Real-Time CDPのユースケースについて説明 ](/help/rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) ます。

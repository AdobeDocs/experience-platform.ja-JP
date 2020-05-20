---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オーディエンスセグメントの書き出し用データセットの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 6d24637dc6cc282f98288b6416e4a3b7cebe42ea
workflow-type: tm+mt
source-wordcount: '766'
ht-degree: 0%

---


# オーディエンスセグメントの書き出し用データセットの作成

Adobe Experience Platformを使用すると、顧客のプロファイルを特定の属性に基づいてオーディエンスに簡単にセグメント化できます。 セグメントを作成したら、そのオーディエンスをデータセットにエクスポートし、データセットにアクセスして処理することができます。 エクスポートが正常に完了するには、データセットを正しく設定する必要があります。

Experience Platform UIを使用したオーディエンスセグメントの書き出しに使用できるデータセットを作成する手順を説明します。

このチュートリアルは、セグメント結果の [評価とアクセスのチュートリアルで概要を説明している手順に直接関連しています](./evaluate-a-segment.md)。 セグメントの評価のチュートリアルでは、Catalog APIを使用してデータセットを作成する手順を説明します。このチュートリアルでは、Experience Platform UIを使用してデータセットを作成する手順について概説します。

## はじめに

セグメントをエクスポートするには、XDM個別プロファイル和集合スキーマに基づくデータセットが必要です。 和集合スキーマは、システム生成の読み取り専用スキーマで、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するすべてのスキーマのフィールドを集計します。 和集合表示のスキーマの詳細については、『スキーマレジストリ開発ガイド』の「 [リアルタイム顧客プロファイル」の節を参照してください](../../xdm/schema/composition.md#union)。

UIで和集合スキーマを表示するには、左側のナビゲーションで **プロファイル** (「 ** 」)をクリックし、次に示す和集合スキーマ(「」)タブをクリックします。

![Experience Platform UIの「和集合スキーマ」タブ](../images/tutorials/segment-export-dataset/union-schema-ui.png)


## Datasetsワークスペース

Experience Platform UI内のデータセットワークスペースを使用すると、IMS組織が作成したすべてのデータセットの表示と管理を行えるほか、新しいデータセットを作成できます。

データセットワークスペースを表示するには、左側のナビゲーションで **「Datasets** 」をクリックし、「 *Browse* （参照）」タブをクリックします。 The datasets workspace contains a list of datasets, including columns showing *Name*, *Created* (date and time), *Source*, *Schema*, and *Last Batch Status*, as well as the date and time the dataset was *Last Updated*. 各列の幅に応じて、すべての列を表示するために左右にスクロールする必要がある場合があります。

>[!NOTE] 検索バーの横にあるフィルターアイコンをクリックして、フィルター機能を使用し、リアルタイム顧客プロファイルに対して有効になっているデータセットのみを表示します。

![すべてのデータセットの表示](../images/tutorials/segment-export-dataset/datasets-workspace.png)

## データセットの作成

データセットを作成するには、Datasetsワークスペースの右上隅にある **「データセットを作成** 」をクリックします。

![「データセットを作成」をクリックします](../images/tutorials/segment-export-dataset/dataset-click-create.png)

データセットの *作成画面で、「スキーマからデータセットを* 作成 **」をクリックして続行します** 。

![データソースの選択](../images/tutorials/segment-export-dataset/create-dataset.png)

## 「XDM Individualプロファイル和集合」スキーマの選択

データセットに使用するXDM個別プロファイル和集合スキーマを選択するには、 *スキーマを選択画面で「和集合」タイプの「XDM個別プロファイル」スキーマを探します* 。

「 **XDM個々のプロファイル**」の横のラジオボタンを選択し、右上隅の「 **次へ** 」をクリックします。

![スキーマの選択](../images/tutorials/segment-export-dataset/select-schema.png)

## データセットの設定

データセットを **設定画面では、データセットに** 名前を付ける必要があり *、* 説明 ** (Description)も表示されます。

**データセット名に関する注意事項：**
- データセット名は短くてわかりやすい名前にし、後でライブラリ内で簡単に見つけられるようにしてください。
- データセット名は一意にする必要があります。つまり、将来再利用できなくなるように固有の名前を付ける必要もあります。
- 説明フィールドを使用して、データセットに関する追加情報を提供することをお勧めします。これは、今後、他のユーザーがデータセットを区別する際に役立つ場合があるためです。

データセットに名前と説明が付いたら、「 **完了**」をクリックします。

![データセットの設定](../images/tutorials/segment-export-dataset/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、Datasetsワークスペースの「 *データセットアクティビティ* 」タブに戻りました。 ワークスペースの左上隅に、データセットの名前と、「バッチが追加されていません」という通知が表示されます。 これは、まだこのデータセットにバッチを追加していないので、予想されることです。

Workspaceの右側には、新しいデータセットに関連する情報を含む「 **情報** 」 *タブが表示されます。例えば、名前名、名前、説明、説明、、*&#x200B;スキーマ名、説明、ストリーミング、元データセットなど ************&#x200B;です。 「情報」タブには、データセットがいつ *作成されたかと* 、 *最終変更日に関する情報も表示されます* 。

オーディエンスセグメントの書き出しワークフローを完了するにはこの値が必要なので、 **データセットID**&#x200B;を控えておいてください。

![データセットアクティビティ](../images/tutorials/segment-export-dataset/dataset-activity.png)

## 次の手順

XDM個々のプロファイル和集合スキーマに基づいてデータセットを作成したら、 **データセットID** を使用して、 [評価とセグメント結果](./evaluate-a-segment.md) のチュートリアルを続行できます。

この時点で、評価セグメントの結果のチュートリアルに戻り、セグメントの [書き出しのオーディエンスメンバー用の](./evaluate-a-segment.md#generate-profiles-for-audience-members) XDM個別プロファイルの [生成](./evaluate-a-segment.md#export-a-segment) (Generate XDM Individual Workflow)の手順に進んでください。

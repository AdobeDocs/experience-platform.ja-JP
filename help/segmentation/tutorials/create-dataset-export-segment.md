---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データセグメントを書き出すためのオーディエンスセットの作成
topic: tutorial
translation-type: tm+mt
source-git-commit: 6d24637dc6cc282f98288b6416e4a3b7cebe42ea

---


# データセグメントを書き出すためのオーディエンスセットの作成

Adobe Experience Platformを使用すると、特定の属性に基づいて顧客のプロファイルをオーディエンスに簡単にセグメント化できます。 セグメントを作成したら、そのオーディエンスをデータセットに書き出し、そこからアクセスして処理できます。 エクスポートを正常に行うには、データセットを正しく設定する必要があります。

このチュートリアルでは、Experience Platform UIを使用してオーディエンスセグメントを書き出すために使用できるデータセットを作成する手順を説明します。

このチュートリアルは、セグメント結果の評価とアクセスに関するチュートリアルで概要を説 [明している手順に直接関連していま](./evaluate-a-segment.md)す。 セグメントの評価のチュートリアルでは、Catalog APIを使用してデータセットを作成する手順を説明します。このチュートリアルでは、Experience Platform UIを使用してデータセットを作成する手順について説明します。

## はじめに

セグメントを書き出すには、データセットがXDM個別プロファイル和集合スキーマに基づいている必要があります。 和集合スキーマは、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するすべてのスキーマのフィールドを集計する、システム生成の読み取り専用のスキーマです。 和集合表示スキーマの詳細については、『スキーマレジストリ開発 [者ガイド』の「リアルタイム顧客プロファイル」の節を参照してください](../../xdm/schema/composition.md#union)。

UIで表示和集合スキーマを行うには、左側のナビゲーションで **「** プロファイル」をクリックし、次に下に示すように *和集合スキーマ* 」タブをクリックします。

![和集合スキーマタブ(Experience Platform UI)](../images/tutorials/segment-export-dataset/union-schema-ui.png)


## データセットワークスペース

Experience Platform UI内のデータセットワークスペースを使用すると、IMS組織が作成したすべてのデータセットを表示および管理し、新しいデータセットを作成できます。

データセットの表示を行うには、左側のナ **ビゲーションで** 「Datasets」をクリックし、「 *Browse* 」タブをクリックします。 The datasets workspace contains a list of datasets, including columns showing *Name*, *Created* (date and time), *Source*, *Schema*, and *Last Batch Status*, as well as the date and time the dataset was *Last Updated*. 各列の幅に応じて、すべての列を表示するには左または右にスクロールする必要があります。

>[!NOTE] 検索バーの横にあるフィルターアイコンをクリックして、フィルタリング機能を使用し、リアルタイム顧客プロファイル用に有効なデータセットのみを表示します。

![表示すべてのデータセット](../images/tutorials/segment-export-dataset/datasets-workspace.png)

## データセットの作成

データセットを作成するには、Datasetsワー **クスペースの** 右上隅にある「データセットを作成」をクリックします。

![「データセットを作成」をクリックします](../images/tutorials/segment-export-dataset/dataset-click-create.png)

データセットを *作成画面で* 、「スキーマか **らデータセットを作成** 」をクリックして続行します。

![データソースの選択](../images/tutorials/segment-export-dataset/create-dataset.png)

## 「Select XDM Individual」プロファイル和集合スキーマ

データセットで使用するXDM個別プロファイル和集合スキーマを選択するには、スキーマを選択画面で、「和集合」タイプの「XDM個別プロファイル」スキーマを探 *します* 。

「 **XDM Individualプロファイル**」の横のラジオボタンを選択し、右上 **隅の「Next** 」をクリックします。

![選択スキーマ](../images/tutorials/segment-export-dataset/select-schema.png)

## データセットの設定

データセ **ットの設定画面で** 、データセットに *Name* （名前）を指定し、データセットの ** 説明も入力できます。

**データセット名に関する注意事項：**
- データセット名は短く、わかりやすい名前にする必要があります。これにより、後でライブラリ内で簡単に見つけられるようになります。
- データセット名は一意である必要があります。つまり、今後再利用されないように十分な固有の名前を付ける必要があります。
- 説明フィールドを使用して、データセットに関する追加情報を提供することをお勧めします。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「完了」をクリッ **クします**。

![データセットの設定](../images/tutorials/segment-export-dataset/configure-dataset.png)

## データセットアクティビティ

これで空のデータセットが作成され、データセットワークスペースの「デ *ータセット* アクティビティ」タブに戻りました。 ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。 このデータセットにバッチをまだ追加していないので、この問題は発生しません。

Workspaceの右側に、 **NameNameNameNameDescriptionDatasetID** ,DescriptionDestateDatasetName,DestreingName,StreamingName, *StreamingName,StreamingSource,Name,StreamingSource,S,S,* Soure,スキーマ,Name,Name,Name,Name,Sourceなど、新しいデータセットに関連する情報を含む *Info* Info *Inforを含む* Info *Infoタブが、* Info *Info* Info *Info* Info *,Info*,InfoIngInfoinfoInfoInforIngIngInforIngInfoIngInfoIngInfoInfo また、「情報」タブには、データセットがいつ作成されたかと *最終変更* 日に関する情 *報も含まれます* 。

データセットIDは、オーディエンスセグメ **ントの書き出しワークフローを完了するために**、この値が必要なので、注意してください。

![データセットアクティビティ](../images/tutorials/segment-export-dataset/dataset-activity.png)

## 次の手順

これで、XDM個々のプロファイル和集合スキーマに基づいてデータセットを作成したので、 **Dataset ID** （データセットID）を使用して、セグメント結果の評価とア [クセスのチュートリアルを続行できます](./evaluate-a-segment.md) 。

この時点で、評価セグメントの結果のチュートリアルに戻り、セグメントの書き出しワークフローの「オーディエンスメンバー用の [XDM個々のプロファイルを生成](./evaluate-a-segment.md#generate-profiles-for-audience-members) 」の手順 [を参照してください](./evaluate-a-segment.md#export-a-segment) 。

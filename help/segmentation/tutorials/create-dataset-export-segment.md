---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント化サービス；セグメント化；セグメント化；データセットの作成；オーディエンスセグメントのエクスポート；セグメントのエクスポート；
solution: Experience Platform
title: オーディエンスセグメントを書き出すデータセットの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Experience Platform UI で、オーディエンスセグメントのエクスポートに使用できるデータセットを作成する手順を説明します。
exl-id: 1cd16e43-b050-42ba-a894-d7ea477b65f3
source-git-commit: 44d7e11e79ed0e6041ff2e4438ddb7141ae3532d
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 29%

---

# オーディエンスセグメントをエクスポートするためのデータセットの作成

[!DNL Adobe Experience Platform] では、特定の属性に基づいて顧客プロファイルをオーディエンスにセグメント化できます。セグメントを作成したら、そのオーディエンスをデータセットにエクスポートして、オーディエンスにアクセスしたり、オーディエンスの操作をおこなうことができます。 エクスポートを正常におこなうには、データセットを正しく設定する必要があります。

このチュートリアルでは、[!DNL Experience Platform] UI を使用してオーディエンスセグメントを書き出すために使用できるデータセットを作成する手順について説明します。

このチュートリアルは、[ セグメント結果の評価とアクセス ](./evaluate-a-segment.md) に関するチュートリアルで説明した手順に直接関連しています。 セグメント評価のチュートリアルでは、[!DNL Catalog Service] API を使用してデータセットを作成する手順を説明します。このチュートリアルでは、[!DNL Experience Platform] UI を使用してデータセットを作成する手順について説明します。

## はじめに

セグメントをエクスポートするには、データセットが [!DNL XDM Individual Profile Union Schema] に基づいている必要があります。 和集合スキーマは、同じクラスを共有するすべてのスキーマのフィールドを集計する、システムで生成される読み取り専用のスキーマです。 和集合スキーマの詳細については、[ スキーマ構成の基本 ](../../xdm/schema/composition.md#union) に関するガイドを参照してください。

UI で和集合スキーマを表示するには、左側のナビゲーションで「**[!UICONTROL Profiles]**」を選択し、次に示すように「**[!UICONTROL Union Schema]**」を選択します。

![Experience Platform UI の和集合スキーマタブ](../images/tutorials/segment-export-dataset/union.png)


## データセットワークスペース

「[!UICONTROL  データセット ]」ワークスペースでは、組織のすべてのデータセットを表示および管理できます。

左側のナビゲーションで「**[!UICONTROL データセット]**」を選択してワークスペースにアクセスし、「**[!UICONTROL 参照]**」を選択します。 このタブには、データセットとその詳細のリストが表示されます。 各列の幅によっては、すべての列を表示するには、左または右にスクロールする必要があります。

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンを選択して、フィルタリング機能を使用し、[!DNL Real-time Customer Profile] に対して有効なデータセットのみを表示します。

![データセットの表示](../images/tutorials/segment-export-dataset/browse.png)

## データセットの作成

データセットを作成するには、「**[!UICONTROL データセットを作成]**」を選択します。

![「データセットを作成」を選択します。](../images/tutorials/segment-export-dataset/create-dataset.png)

次の画面で、「**[!UICONTROL Create Dataset from Schema]**」を選択します。

![データソースの選択](../images/tutorials/segment-export-dataset/create-from-schema.png)

## XDM 個別プロファイル和集合スキーマの選択

データセットで使用する [!DNL XDM Individual Profile Union Schema] を選択するには、**[!UICONTROL Select Schema]** 画面で「[!UICONTROL XDM Individual Profile]」スキーマを探します。 スキーマを選択したら、右側のレールの「**[!UICONTROL API の使用]**」の下にある和集合スキーマかどうかを確認できます。 [!UICONTROL  スキーマ ] のパスが `_union` で終わる場合は、和集合スキーマです。

>[!NOTE]
>
>和集合スキーマは定義によりリアルタイム顧客プロファイルに参加しているにもかかわらず、従来のスキーマと同じ方法でプロファイルに対して有効になっていないので、「無効」と表示されます。

「**[!UICONTROL XDM Individual Profile]**」の横のラジオボタンを選択し、「**[!UICONTROL Next]**」を選択します。

![スキーマの選択](../images/tutorials/segment-export-dataset/select-schema.png)

## データセットの設定

次の画面で、データセットに名前を付ける必要があります。 オプションで説明を追加することもできます。

**データセット名に関する注意事項：**
* 後でライブラリ内で簡単に見つけられるように、データセット名は短く、わかりやすい名前にする必要があります。
* データセット名は一意である必要があります。つまり、将来再利用されないように十分に具体的である必要があります。
* ベストプラクティスは、説明フィールドを使用して、データセットに関する追加情報を提供することです。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「**[!UICONTROL 完了]**」を選択します。

![データセットの設定](../images/tutorials/segment-export-dataset/configure-dataset.png)

## データセットアクティビティ

データセットを作成すると、そのデータセットのアクティビティページが表示されます。 ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。このデータセットにバッチをまだ追加していないので、これは期待通りです。

右側のレールには、データセット ID、名前、説明、スキーマなど、新しいデータセットに関する情報が含まれています。 **[!UICONTROL データセット ID]** は、オーディエンスセグメントのエクスポートワークフローを完了するために必要になるので、メモしておいてください。

![データセットアクティビティ](../images/tutorials/segment-export-dataset/activity.png)

## 次の手順

これで、[!DNL XDM Individual Profile Union Schema] に基づいてデータセットを作成したので、データセット ID を使用して、[ セグメント結果の評価とアクセス ](./evaluate-a-segment.md) のチュートリアルを続行できます。

この時点で、セグメント結果の評価チュートリアルに戻り、セグメントワークフローの書き出しの [ オーディエンスメンバーのプロファイルの生成 ](./evaluate-a-segment.md#generate-profiles) 手順を参照してください。

---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: 結合ポリシーユーザーガイド
topic: guide
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 83%

---


# 結合ポリシーユーザーガイド

Adobe Experience Platform では、複数のソースからデータを組み合わせて、個々の顧客の全体像を把握できます。When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view.

RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。このガイドでは、Adobe Experience Platform ユーザーインターフェイスを使用して結合ポリシーを操作する手順を説明します。

If you would prefer to work with merge policies using the [!DNL Real-time Customer Profile] API, please follow the instructions outlined in the [merge policies API tutorial](../api/merge-policies.md).

## はじめに

This guide requires a working understanding of the various [!DNL Experience Platform] services involved with merge policies. このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

* [!DNL Real-time Customer Profile](../home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [!DNL Identity Service](../../identity-service/home.md): 取り込ま [!DNL Real-time Customer Profile] れる異なるデータソースのIDをブリッジ化することで有効に [!DNL Platform]します。
* [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

## 結合ポリシーの表示

Within the [!DNL Experience Platform] user interface, you can begin to work with merge policies and see a list of your organization&#39;s existing merge policies by clicking **[!UICONTROL Profile]** in the left-rail and then selecting the **[!UICONTROL Merge policies]** tab.

![ポリシーの結合ランディングページ](../images/merge-policies/landing.png)

組織で使用可能な各結合ポリシーの詳細は、「*[!UICONTROL ポリシー名]*」、「*[!UICONTROL デフォルトの結合ポリシー]*」、「*[!UICONTROL スキーマ]*」などのランディングページに表示されます。

表示する詳細を選択するか、表示する列を追加するには、右側の列選択アイコンを選択し、列名をクリックして、列をビューに追加またはビューから削除します。

![](../images/merge-policies/adjust-view.png)

## 結合ポリシーの作成

新しい結合ポリシーを作成するには、「**[!UICONTROL 結合ポリシー]**」タブの右上付近にある「**[!UICONTROL 結合ポリシーを作成]**」をクリックします。

![ポリシーの結合ランディングページ](../images/merge-policies/create-new.png)

「**[!UICONTROL 結合ポリシーを作成]**」画面が表示され、新しい結合ポリシーに関する重要な情報を指定できます。

![](../images/merge-policies/create.png)

* **[!UICONTROL 名前]**：結合ポリシーの名前は、説明的かつ簡潔にする必要があります。
* **[!UICONTROL スキーマ]**：結合ポリシーと関連付けられたスキーマです。この結合ポリシーを作成する XDM ポリシーを指定します。組織は、スキーマごとに複数の結合ポリシーを作成できます。
* **[!UICONTROL ID ステッチ]**：このフィールドでは、顧客の関連 ID を特定する方法を定義します。次の 2 つの値を使用できます。
   * **[!UICONTROL なし]**：ID ステッチを実行しない。
   * **[!UICONTROL 非公開グラフ]**：非公開の ID グラフに基づいて ID ステッチを実行します。
* **[!UICONTROL 属性の結合]**：プロファイルフラグメントとは、1 人の顧客に存在する ID のリストのうち、1 つの ID のみに関するプロファイル情報を表します。使用される ID グラフタイプによって複数の ID が生成される場合、プロファイルプロパティの値が競合する可能性があり、優先度を指定する必要があります。*属性の結合*&#x200B;を使用すると、結合の競合が発生した場合に優先順位を付けるデータセットプロファイルの値を指定できます。次の 2 つの値を使用できます。
   * **[!UICONTROL 順序付きタイムスタンプ]**：競合が発生した場合は、最近更新されたプロファイルを優先します。
   * **[!UICONTROL データセットの優先順位]**：訪問者のデータセットに基づいて、プロファイルフラグメントの優先順位を指定します。このオプションを選択する場合は、関連するデータセットと優先順位を選択する必要があります。詳しくは、以下に示す[データセットの優先順位](#dataset-precedence)を参照してください。
* **[!UICONTROL 既定の結合ポリシー]**：組織のデフォルトとしてこの結合ポリシーを使用するかどうかを選択するトグルボタン。セレクターをオンに切り替えて新しいポリシーを保存すると、以前のデフォルトのポリシーが自動的に更新され、デフォルトのポリシーが無効になります。

### データセットの優先順位 {#dataset-precedence}

*[!UICONTROL 属性の結合]*&#x200B;値を選択する際に、「*[!UICONTROL データセットの優先順位]*」を選択すると、プロファイルフラグメントをその値の元のデータセットに基づいて優先させることができます。

例えば、あるデータセットに情報が存在し、他のデータセットのデータよりも優先度や信頼度が高い場合などに使用できます。

「*[!UICONTROL データセットの優先順位]*」を選択すると、別のパネルが開き、データセットが含まれる「*[!UICONTROL 使用可能なデータセット]*」から選択する（またはチェックボックスを使用してすべて選択する）必要があります。その後、これらのデータセットを「*[!UICONTROL 選択したデータセット]*」パネルにドラッグ＆ドロップし、正しい優先順位にドラッグできます。最上位のデータセットには最も高い優先順位が付けられ、2 番目のデータセットには 2 番目の優先順位が付けられます。

![](../images/merge-policies/dataset-precedence.png)

結合ポリシーの作成が完了したら、「**[!UICONTROL 保存]**」をクリックして「*[!UICONTROL 結合ポリシー]*」タブに戻り、新しい結合ポリシーがポリシーのリストに表示されます。

## 結合ポリシーの編集

既存のマージポリシーを変更するには、「*[!UICONTROL 結合ポリシー]*」タブを使用し、編集するマージポリシーの「*[!UICONTROL ポリシー名]*」をクリックします。

![ポリシーの結合ランディングページ](../images/merge-policies/select-edit.png)

「*[!UICONTROL 結合ポリシーの編集]*」画面が表示されたら、「*[!UICONTROL 名前]*」、「*[!UICONTROL スキーマ]*」、「*[!UICONTROL ID ステッチ]*」タイプおよび「*[!UICONTROL 属性マージ]*」タイプに変更を加え、このポリシーを組織の&#x200B;*[!UICONTROL デフォルトの結合ポリシー]*&#x200B;にするかどうかを選択できます。

>[!N注意]
>結合ポリシー ID は編集できません。この ID は編集画面の上部に表示されます。これはシステムによって生成された読み取り専用の ID で、変更できません。

![](../images/merge-policies/edit-screen.png)

必要な変更を加えたら、「**[!UICONTROL 保存]**」をクリックして「*[!UICONTROL 結合ポリシー]*」タブに戻り、更新されたマージポリシー情報が表示されるようにします。

![](../images/merge-policies/edited.png)

## データガバナンスポリシー違反

結合ポリシーを作成または更新すると、結合ポリシーが組織で定義されたデータ使用ポリシーに違反しているかどうかを確認するチェックが実行されます。Data usage policies are part of Adobe Experience Platform [!DNL Data Governance] and are rules that describe the kinds of marketing actions that you are allowed to, or restricted from, performing on specific [!DNL Platform] data. 例えば、結合ポリシーを使用して、サードパーティの宛先に対してアクティブ化されたセグメントを作成し、組織のデータ使用ポリシーによって特定のデータをサードパーティに書き出すことを防止している場合、結合ポリシーを保存しようとすると、「データガバナンスポリシーの違反が検出されました」という通知が表示されます。

この通知には、違反したデータ使用ポリシーのリストが含まれ、リストから選択すると、違反の詳細を表示できます。アクティベーションポリシーを選択すると、「*データ系列*」タブに「*違反理由*」と「*影響を受けるアクティベーション*」が表示され、データ使用ポリシーの違反の詳細を確認できます。

Adobe Experience Platform 内でのデータガバナンスの実行方法について詳しくは、まず[データガバナンスの概要](../../data-governance/home.md)を読んでください。

![](../images/merge-policies/policy-violation.png)

## 次の手順

これで、IMS　組織の結合ポリシーを作成して設定しました。これらのポリシーを使用して、プロファイルデータからオーディエンスセグメントを作成できます。See the [Segmentation overview](../../segmentation/home.md) for more information on how to create and work with segments using [!DNL Experience Platform].
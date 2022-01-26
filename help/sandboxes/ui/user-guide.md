---
keywords: Experience Platform；ホーム；人気の高いトピック；サンドボックスユーザーガイド；サンドボックスガイド
solution: Experience Platform
title: サンドボックス UI ガイド
topic-legacy: user guide
description: このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。
exl-id: b258c822-5182-4217-9d1b-8196d889740f
source-git-commit: 59b608ba06028f7f345f6a59dab333a4fdf2d225
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 16%

---

# サンドボックス UI ガイド

このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。

## サンドボックスの表示

Platform UI で、「 **[!UICONTROL サンドボックス]** 左側のナビゲーションで「 」を選択し、 **[!UICONTROL 参照]** 開く [!UICONTROL サンドボックス] ダッシュボード。 ダッシュボードには、組織で使用可能なすべてのサンドボックスが一覧表示されます。それぞれのタイプ（実稼動または開発）も含まれます。

![ビューサンドボックス](../images/ui/view-sandboxes.png)

## サンドボックス間の切り替え

サンドボックスインジケーターは、Platform UI の上部のヘッダーにあり、現在存在するサンドボックスのタイトル、その地域およびそのタイプを表示します。

![sandbox-indicator](../images/ui/sandbox-indicator.png)

サンドボックスを切り替えるには、サンドボックスのインジケーターを選択し、ドロップダウンリストから目的のサンドボックスを選択します。

![switcher-interface](../images/ui/switcher-interface.png)

サンドボックスを選択すると、画面が更新され、選択したサンドボックスに対して更新がおこなわれます。

![sandbox-switched](../images/ui/sandbox-switched.png)

## 新しいサンドボックスの作成

>[!NOTE]
>
>新しいサンドボックスを作成する際は、まず、その新しいサンドボックスを [Adobe Admin Console](https://adminconsole.adobe.com/) 新しいサンドボックスの使用を開始する前に。 詳しくは、 [製品プロファイルの権限の管理](../../access-control/ui/permissions.md) サンドボックスを製品プロファイルにプロビジョニングする方法に関する情報を参照してください。

次のビデオでは、Experience Platformでのサンドボックスの使用方法の概要を簡単に説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29838/?quality=12&learn=on)

新しいサンドボックスを作成するには、「 **[!UICONTROL サンドボックスを作成]** をクリックします。

![create-sandbox](../images/ui/create-sandbox.png)

この **[!UICONTROL サンドボックスを作成]** ダイアログボックスが表示されます。 開発用サンドボックスを作成する場合は、「 **[!UICONTROL 開発]** （ドロップダウンパネル内）を選択します。 新しい実稼動サンドボックスを作成するには、「 **[!UICONTROL 実稼動]**.

![sandbox-type](../images/ui/sandbox-type.png)

タイプを選択した後、サンドボックスに名前とタイトルを指定します。 タイトルは、人間が読み取り可能なもので、簡単に識別できる説明的なタイトルにする必要があります。 サンドボックス名は、API 呼び出しで使用する、すべて小文字の識別子なので、一意で簡潔な名前にする必要があります。 サンドボックス名は、文字で始まり、最大 256 文字で、英数字とハイフン (-) のみで構成する必要があります。

完了したら、「**[!UICONTROL 作成]**」をクリックします。

![sandbox-info](../images/ui/sandbox-info.png)

サンドボックスの作成が完了したら、ページを更新すると、 **[!UICONTROL サンドボックス]** ステータスが「[!UICONTROL 作成中]&quot;. 新しいサンドボックスのプロビジョニングには約 30 秒かかり、その後、ステータスが「[!UICONTROL アクティブ]&quot;.

![new-sandbox](../images/ui/new-sandbox.png)

## サンドボックスのリセット

>[!WARNING]
>
>次に、デフォルトの実稼動サンドボックスまたはユーザーが作成した実稼動サンドボックスをリセットできない可能性のある例外のリストを示します。 <ul><li>サンドボックスでホストされる ID グラフがAdobe Analyticsで [クロスデバイス分析 (CDA)](https://experienceleague.adobe.com/docs/analytics/components/cda/overview.html?lang=ja) 機能。</li><li>サンドボックスでホストされる ID グラフがAdobe Audience Managerで [People Based Destinations (PBD)](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html?lang=ja).</li><li>デフォルトの実稼動サンドボックスに CDA 機能と PBD 機能の両方のデータが含まれている場合、リセットすることはできません。</li><li>Adobe Audience Managerまたは Audience Core Service での双方向のセグメント共有に使用される、ユーザーが作成した実稼動用サンドボックスは、警告メッセージの後でリセットできます。</li></ul>

実稼動または開発用サンドボックスをリセットすると、サンドボックスの名前と関連する権限を維持しながら、そのサンドボックスに関連付けられているすべてのリソース（スキーマ、データセットなど）が削除されます。 この「クリーンな」サンドボックスは、引き続き、アクセス権を持つユーザーと同じ名前で使用できます。

リセットするサンドボックスをサンドボックスのリストから選択します。 表示される右側のナビゲーションパネルで、「 」を選択します。 **[!UICONTROL サンドボックスのリセット]**.

![reset](../images/ui/reset.png)

選択を確認するダイアログボックスが表示されます。 選択 **[!UICONTROL 続行]** をクリックして続行します。

![reset-warning](../images/ui/reset-warning.png)

最終確認ウィンドウで、ダイアログボックスにサンドボックスの名前を入力し、「 」を選択します。 **[!UICONTROL リセット]**.

![reset-confirm](../images/ui/reset-confirm.png)

## サンドボックスの削除

>[!WARNING]
>
>デフォルトの実稼動サンドボックスは削除できません。 ただし、との双方向のセグメント共有に使用される、ユーザーが作成した実稼動用サンドボックス [!DNL Audience Manager] または [!DNL Audience Core Service] は、警告メッセージの後に削除できます。

実稼動または開発用サンドボックスを削除すると、権限を含め、そのサンドボックスに関連付けられているすべてのリソースが完全に削除されます。

サンドボックスのリストから削除するサンドボックスを選択します。 表示される右側のナビゲーションパネルで、「 」を選択します。 **[!UICONTROL 削除]**.

![次を削除します。](../images/ui/delete.png)

選択を確認するダイアログボックスが表示されます。 選択 **[!UICONTROL 続行]** をクリックして続行します。

![delete-warning](../images/ui/delete-warning.png)

最終確認ウィンドウで、ダイアログボックスにサンドボックスの名前を入力し、「 」を選択します。  **[!UICONTROL 続行]**.

![delete-confirm](../images/ui/delete-confirm.png)

## 次の手順

このドキュメントでは、Experience Platform UI 内でサンドボックスを管理する方法について説明しました。サンドボックス API を使用したサンドボックスの管理方法について詳しくは、『[サンドボックス開発者ガイド](../api/getting-started.md)』を参照してください。

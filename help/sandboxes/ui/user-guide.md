---
keywords: Experience Platform;ホーム;人気の高いトピック;サンドボックスユーザーガイド;サンドボックスガイド
solution: Experience Platform
title: サンドボックス UI ガイド
description: このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。
exl-id: b258c822-5182-4217-9d1b-8196d889740f
source-git-commit: 0db23e475d6546ebb886a56d5915d023ea215125
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 80%

---

# サンドボックス UI ガイド

このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。

## サンドボックスの表示

Platform UI で、左側のナビゲーションの「**[!UICONTROL サンドボックス]**」を選択し、「**[!UICONTROL 参照]**」を選択して[!UICONTROL サンドボックス]ダッシュボードを開きます。 ダッシュボードには、組織で使用可能なすべてのサンドボックスが一覧表示されます。それぞれのタイプ（実稼動または開発）も含まれます。

![view-sandboxes](../images/ui/view-sandboxes.png)

## サンドボックス間の切り替え

サンドボックスインジケーターは、Platform UI の上部ヘッダーにあり、現在使用しているサンドボックスのタイトル、その地域およびそのタイプを表示します。

![sandbox-indicator](../images/ui/sandbox-indicator.png)

サンドボックスを切り替えるには、サンドボックスインジケーターを選択し、ドロップダウンリストから目的のサンドボックスを選択します。

![switcher-interface](../images/ui/switcher-interface.png)

サンドボックスを選択すると、画面が更新され、選択したサンドボックスに対して更新が行われます。

![sandbox-switched](../images/ui/sandbox-switched.png)

## 新しいサンドボックスの作成 {#create}

>[!CONTEXTUALHELP]
>id="platform_sandboxes_sandboxname"
>title="サンドボックス名"
>abstract="サンドボックス名は、このサンドボックスの一意の ID の作成にバックエンドで使用されるテキストです。"

>[!CONTEXTUALHELP]
>id="platform_sandboxes_sandboxtitle"
>title="サンドボックスのタイトル"
>abstract="サンドボックスのタイトルは、Experience Platform UI 全体でのメニューおよびドロップダウンのサンドボックスを表す表示名です。"

>[!NOTE]
>
>新しいサンドボックスを作成するには、そのサンドボックスを使用する前に、[[!UICONTROL  権限 ]](../../access-control/abac/ui/permissions.md) の役割に追加する必要があります。 サンドボックスを役割にプロビジョニングする方法については、[ 役割のサンドボックスの管理 ](../../access-control/abac/ui/permissions.md#managing-sandboxes-for-role) ドキュメントを参照してください。

次のビデオでは、Experience Platform でのサンドボックスの使用方法の概要を簡単に説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29838/?quality=12&learn=on)

新しいサンドボックスを作成するには、画面の右上隅にある「**[!UICONTROL サンドボックスを作成]**」を選択します。

![create-sandbox](../images/ui/create-sandbox.png)

**[!UICONTROL サンドボックスを作成]**&#x200B;ダイアログボックスが表示されます。 開発用サンドボックスを作成している場合は、ドロップダウンパネルの「**[!UICONTROL 開発]**」を選択します。 新しい実稼動サンドボックスを作成するには、「**[!UICONTROL 実稼動]**」を選択します。

![sandbox-type](../images/ui/sandbox-type.png)

タイプを選択した後、サンドボックスに名前とタイトルを指定します。 タイトルは、人間が読み取り可能で、かつ簡単に識別できる説明的なタイトルにする必要があります。サンドボックスの名前は、API 呼び出しで使用する、すべて小文字の識別子なので、一意かつ簡潔なものにする必要があります。サンドボックス名は、文字で始まり、最大 256 文字で、英数字とハイフン (-) のみで構成する必要があります。

完了したら、「**[!UICONTROL 作成]**」をクリックします。

![sandbox-info](../images/ui/sandbox-info.png)

サンドボックスの作成が完了したら、ページを更新すると、**[!UICONTROL サンドボックス]**&#x200B;ダッシュボードに新しいサンドボックスがステータスは「[!UICONTROL 作成中]」になります。新しいサンドボックスのプロビジョニングには約 30 秒かかり、その後、ステータスが「[!UICONTROL アクティブ]」に変わります。

![new-sandbox](../images/ui/new-sandbox.png)

## サンドボックスのリセット

>[!WARNING]
>
>次に、デフォルトの実稼動サンドボックスまたはユーザー作成の実稼動サンドボックスをリセットできない可能性のある例外のリストを示します。
>* サンドボックスでホストされている ID グラフが[クロスデバイス分析（CDA）](https://experienceleague.adobe.com/docs/analytics/components/cda/overview.html?lang=ja)機能向けに Adobe Analytics でも使用されている場合、デフォルトの実稼働サンドボックスをリセットすることはできません。
>* サンドボックスでホストされている ID グラフが [People Based Destinations（PBD）](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html?lang=ja)の Adobe Audience Manager でも使用されている場合、デフォルトの実稼動サンドボックスをリセットすることはできません。
>* CDA 機能と PBD 機能の両方のデータが含まれている場合、デフォルトの実稼動サンドボックスをリセットすることはできません。
>* Adobe Audience Manager または Audience Core Service での双方向のセグメント共有に使用される、ユーザー作成の実稼動用サンドボックスは、警告メッセージの後でリセットできます。
>* サンドボックスのリセットを開始する前に、コンポジションを手動で削除して、関連するオーディエンスデータが適切にクリーンアップされるようにする必要があります。

### オーディエンス構成の削除

オーディエンス構成は、現在、サンドボックスリセット機能と統合されていないので、サンドボックスリセットを実行する前に、オーディエンスを手動で削除する必要があります。

左側のナビゲーションから **[!UICONTROL オーディエンス]** を選択し、次に **[!UICONTROL コンポジション]** を選択します。

![[!UICONTROL  オーディエンス ] ワークスペースの「[!UICONTROL  コンポジション ]」タブ ](../images/ui/audiences.png)

次に、最初のオーディエンスの横にある省略記号（`...`）を選択し、「**[!UICONTROL 削除]**」を選択します。

![ 「[!UICONTROL  削除 ] オプションがハイライト表示されたオーディエンスメニュー ](../images/ui/delete-composition.png)

削除が成功したことを示す確認メッセージが表示され、「**[!UICONTROL コンポジション]**」タブに戻ります。

すべてのコンポジションで上記の手順を繰り返します。 これにより、オーディエンスインベントリからすべてのオーディエンスが削除されます。 すべてのオーディエンスを削除したら、引き続きサンドボックスをリセットできます。

### サンドボックスのリセット

実稼動または開発のサンドボックスをリセットすると、サンドボックスの名前と関連付けられた権限は保持されたまま、そのサンドボックスに関連付けられているすべてのスキーマ（リソース、データセットなど）が削除されます。この「クリーンな」サンドボックスは、引き続き、アクセス権を持つユーザーと同じ名前で使用できます。

リセットするサンドボックスをサンドボックスのリストから選択します。 表示される右側のナビゲーションパネルで、「**[!UICONTROL サンドボックスのリセット]**」を選択します。 

![reset](../images/ui/reset.png)

選択内容を確認するダイアログボックスが表示されます。「**[!UICONTROL 続行]**」を選択して次に進みます。

![reset-warning](../images/ui/reset-warning.png)

最終確認ウィンドウで、ダイアログボックスにサンドボックスの名前を入力し、「**[!UICONTROL リセット]**」を選択します。 

![reset-confirm](../images/ui/reset-confirm.png)

## サンドボックスの削除

>[!WARNING]
>
>デフォルトの実稼動サンドボックスは削除できません。ただし、[!DNL Audience Manager] との双方向のセグメント共有に使用される、ユーザーが作成した実稼動用サンドボックス または [!DNL Audience Core Service] は、警告メッセージの後に削除できます。

実稼動用もしくは開発用サンドボックスを削除すると、権限を含め、そのサンドボックスに関連付けられているすべてのリソースが完全に削除されます。

サンドボックスのリストから削除するサンドボックスを選択します。表示される右側のナビゲーションパネルで、「**[!UICONTROL 削除]**」を選択します。

![削除](../images/ui/delete.png)

選択内容を確認するダイアログボックスが表示されます。「**[!UICONTROL 続行]**」を選択して次に進みます。

![delete-warning](../images/ui/delete-warning.png)

最終確認ウィンドウで、ダイアログボックスにサンドボックスの名前を入力し、「**[!UICONTROL 続行]**」を選択します。

![delete-confirm](../images/ui/delete-confirm.png)

## 次の手順

このドキュメントでは、Experience Platform UI 内でサンドボックスを管理する方法について説明しました。サンドボックス API を使用したサンドボックスの管理方法について詳しくは、『[サンドボックス開発者ガイド](../api/getting-started.md)』を参照してください。

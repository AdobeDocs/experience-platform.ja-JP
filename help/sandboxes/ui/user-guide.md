---
keywords: Experience Platform;home;popular topics;sandbox user guide;sandbox guide
solution: Experience Platform
title: サンドボックスユーザーガイド
topic: user guide
description: このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。
translation-type: tm+mt
source-git-commit: 2d1a9699866bd39de7251731e9f0cd2f753a5083
workflow-type: tm+mt
source-wordcount: '623'
ht-degree: 50%

---


# サンドボックスユーザーガイド

このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。

## サンドボックスの表示

In the Experience Platform UI, select **[!UICONTROL Sandboxes]** in the left-navigation to open the **[!UICONTROL Sandboxes]** dashboard. ダッシュボードには、組織で使用可能なすべてのサンドボックスがリストされます。これには、サンドボックスのタイプ（実稼動または開発）や状態（アクティブ、作成、削除、失敗）が含まれます。

![](../images/ui/view-sandboxes.png)

## サンドボックス間の切り替え

画面の左上にある&#x200B;**サンドボックス切り替えボタン**&#x200B;コントロールには、現在アクティブなサンドボックスが表示されます。

![](../images/ui/sandbox-switcher.png)

サンドボックスを切り替えるには、サンドボックス切り替えボタンを選択し、ドロップダウンリストから目的のサンドボックスを選択します。

![](../images/ui/switcher-menu.png)

サンドボックスを選択すると、画面は、選択したサンドボックスで更新され、サンドボックス切り替えコントロールが搭載されるようになります。

![](../images/ui/switched.png)

## サンドボックス用の検索

サンドボックス切り替えメニューの検索機能を使用して、利用可能なサンドボックスのリスト間を移動できます。 組織が使用できるすべてのサンドボックスをフィルタリングするためにアクセスするサンドボックスの名前を入力します。

![](../images/ui/sandbox-search.png)

## 新しいサンドボックスの作成

Experience Platformでのサンドボックスの使用方法の概要を簡単に確認するには、次のビデオを使用します。

>[!VIDEO](https://video.tv.adobe.com/v/29838/?quality=12&learn=on)

UIで新しいサンドボックスを作成するには、画面の右上にある「サンドボックスを **[!UICONTROL 作成]** 」ボタンを選択します。

![](../images/ui/create-sandbox.png)

**[!UICONTROL サンドボックスを作成]**&#x200B;ダイアログが表示され、サンドボックスの表示タイトルと名前を指定するよう求められます。**表示タイトル**&#x200B;は、人間が読み取り可能なもので、簡単に識別できる説明的なタイトルにする必要があります。The sandbox **[!UICONTROL Name]** is an all-lowercase identifier for use in API calls, and should therefore be unique and concise. サンドボックス **[!UICONTROL 名]** (Sandbox **Name**)は、英数字とハイフン(-)で構成する必要があります。また、最大256文字まで使用できます。

When finished, select **[!UICONTROL Create]**.

![](../images/ui/create-dialog.png)

>[!NOTE]
>
> あなたが作成できるのは非実稼動のサンドボックスタイプのみ制限されているので、**[!UICONTROL タイプ]**&#x200B;オプションは「非実稼動」でロックされ、操作できません。

Once you have finished creating the sandbox, refresh the page and the new sandbox appears in the **[!UICONTROL Sandboxes]** dashboard with a status of &quot;[!UICONTROL Creating]&quot;. New sandboxes take approximately 15 minutes to be provisioned by the system, after which their status changes to &quot;[!UICONTROL Active]&quot;.

![](../images/ui/creating.png)

## サンドボックスのリセット

>[!NOTE]
>
> この機能は、実稼動以外のサンドボックスでのみ使用できます。実稼動用サンドボックスはリセットできません。

実稼動以外のサンドボックスをリセットすると、サンドボックスの名前と関連付けられた権限は保持されたまま、そのサンドボックスに関連付けられているすべてのスキーマ（リソース、データセットなど）が削除されます。この「クリーンな」サンドボックスは、引き続き、アクセス権を持つユーザーと同じ名前で使用できます。

To reset a sandbox in the UI, select **[!UICONTROL Sandboxes]** in the left-nav, then select the sandbox you want to reset. In the dialog that appears on the right-hand side of the screen, select **[!UICONTROL Reset Sandbox]**.

![](../images/ui/reset-sandbox.png)

選択内容を確認するダイアログが表示されます。Select **[!UICONTROL Reset]** to continue.

![](../images/ui/reset-confirm.png)

A confirmation message appears and the sandbox&#39;s state changes to &quot;**[!UICONTROL Resetting]&quot;**. Once it has been provisioned by the system, its state will update to **&quot;[!UICONTROL Active]&quot;** or **&quot;[!UICONTROL Failed]&quot;**.

![](../images/ui/resetting.png)

## サンドボックスの削除

>[!NOTE]
>
> この機能は、実稼動以外のサンドボックスでのみ使用できます。実稼動用サンドボックスは削除できません。

非実稼動用サンドボックスを削除すると、権限を含め、そのサンドボックスに関連付けられているすべてのリソースが完全に削除されます。

To delete a sandbox in the UI, select **[!UICONTROL Sandboxes]** in the left-nav, then select the sandbox you want to delete. In the dialog that appears on the right-hand side of the screen, select **[!UICONTROL Delete Sandbox]**.

![](../images/ui/delete-sandbox.png)

選択内容を確認するダイアログが表示されます。Select **[!UICONTROL Delete]** to continue.

![](../images/ui/delete-confirm.png)

確認メッセージが表示され、**[!UICONTROL サンドボックス]**&#x200B;ワークスペースからサンドボックスが削除されます。

## 次の手順

このドキュメントでは、Experience Platform UI 内でサンドボックスを管理する方法について説明しました。サンドボックス API を使用したサンドボックスの管理方法について詳しくは、『[サンドボックス開発者ガイド](../api/getting-started.md)』を参照してください。
---
keywords: Experience Platform；ホーム；人気のあるトピック；サンドボックスユーザーガイド；サンドボックスガイド
solution: Experience Platform
title: サンドボックスUIガイド
topic-legacy: user guide
description: このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。
exl-id: b258c822-5182-4217-9d1b-8196d889740f
source-git-commit: 3b0f156d3d6a13fbad45a153749b81a0d6244283
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 21%

---

# サンドボックスUIガイド

このドキュメントでは、Adobe Experience Platform ユーザーインターフェイスのサンドボックスに関連する様々な操作を実行する手順について説明します。

## サンドボックスの表示

Platform UIで、左側のナビゲーションで「**[!UICONTROL サンドボックス]**」を選択し、「[!UICONTROL サンドボックス]」ダッシュボードを開きます。 ダッシュボードには、組織で使用可能なすべてのサンドボックスがリストされます。これには、サンドボックスのタイプ（実稼動または開発）や状態（アクティブ、作成、削除、失敗）が含まれます。

![](../images/ui/view-sandboxes.png)

## サンドボックス間の切り替え

画面の左上にある&#x200B;**サンドボックス切り替えボタン**&#x200B;コントロールには、現在アクティブなサンドボックスが表示されます。

![](../images/ui/sandbox-switcher.png)

サンドボックスを切り替えるには、サンドボックス切り替えボタンを選択し、ドロップダウンリストから目的のサンドボックスを選択します。

![](../images/ui/switcher-menu.png)

サンドボックスを選択すると、画面は、選択したサンドボックスで更新され、サンドボックス切り替えコントロールが搭載されるようになります。

![](../images/ui/switched.png)

## サンドボックスの検索

サンドボックス切り替えメニューの検索機能を使用して、使用可能なサンドボックスのリスト間を移動できます。 組織で使用可能なすべてのサンドボックスをフィルタリングするためにアクセスするサンドボックスの名前を入力します。

![](../images/ui/sandbox-search.png)

## 新しいサンドボックスの作成

Experience Platformでのサンドボックスの使用方法の概要については、次のビデオを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/29838/?quality=12&learn=on)

新しいサンドボックスを作成するには、画面の右上隅にある「**[!UICONTROL サンドボックス]**&#x200B;を作成」を選択します。

![作成](../images/ui/create.png)

**[!UICONTROL サンドボックス]**&#x200B;を作成ダイアログボックスが表示されます。 開発用サンドボックスを作成する場合は、ドロップダウンパネルで「**[!UICONTROL Development]**」を選択します。 新しい実稼動用サンドボックスを作成するには、「**[!UICONTROL 実稼動]**」を選択します。

![type](../images/ui/type.png)

タイプを選択した後、サンドボックスに名前とタイトルを指定します。 タイトルは、人間が読み取り可能なもので、簡単に識別できる説明的なものにする必要があります。 サンドボックス名は、API呼び出しで使用する小文字の識別子なので、一意で簡潔な名前にする必要があります。 サンドボックス名は、文字で始まり、最大256文字で、英数字とハイフン(-)のみで構成する必要があります。

完了したら、「**[!UICONTROL 作成]**」をクリックします。

![info](../images/ui/info.png)

サンドボックスの作成が完了したら、ページを更新すると、**[!UICONTROL サンドボックス]**&#x200B;ダッシュボードに新しいサンドボックスが表示され、ステータスは「[!UICONTROL 作成]」になります。 新しいサンドボックスのプロビジョニングには約30秒かかり、その後、ステータスが「[!UICONTROL アクティブ]」に変わります。

## サンドボックスのリセット

>[!IMPORTANT]
>
>デフォルトの実稼動用サンドボックスは、ホストされているIDグラフがAdobe Analyticsで[クロスデバイス分析(CDA)](https://experienceleague.adobe.com/docs/analytics/components/cda/overview.html)機能にも使用されている場合、またはAdobe Audience ManagerでホストされているIDグラフが[People Based Destinations(PBD)](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview.html)機能にも使用されている場合は、リセットできません。

実稼動用サンドボックスまたは開発用サンドボックスをリセットすると、そのサンドボックスに関連付けられたすべてのリソース（スキーマ、データセットなど）が削除され、サンドボックスの名前と関連する権限が維持されます。 この「クリーンな」サンドボックスは、引き続き、アクセス権を持つユーザーと同じ名前で使用できます。

サンドボックスのリストからリセットするサンドボックスを選択します。 表示される右側のナビゲーションパネルで、「**[!UICONTROL サンドボックスリセット]**」を選択します。

![reset](../images/ui/reset.png)

選択を確認するダイアログボックスが表示されます。 **[!UICONTROL 続行]**&#x200B;を選択して続行します。

![reset-warning](../images/ui/reset-warning.png)

最後の確認ウィンドウで、ダイアログボックスにサンドボックスの名前を入力し、「**[!UICONTROL リセット]**」を選択します。

![reset-confirm](../images/ui/reset-confirm.png)

しばらくすると、リセットの成功を確認する確認ボックスが画面の下部に表示されます。

![成功](../images/ui/success.png)

### 警告

CDAデータを含むデフォルトの実稼動サンドボックスはリセットできず、次の警告が返されます。

![cda](../images/ui/cda.png)

PBDデータを含むデフォルトの実稼動サンドボックスもリセットできず、次の警告が返されます。

![pbd](../images/ui/pbd.png)

CDAとPBDの両方のデータを含むデフォルトの実稼動用サンドボックスもリセットできず、次の警告が返されます。

![both](../images/ui/both.png)

[!DNL Audience Manager]または[!DNL Audience Core Service]との双方向セグメント共有に使用する実稼動用サンドボックスをリセットできます。 「[!UICONTROL 続行]」を選択して、リセットを続行します。

![both](../images/ui/seg.png)

## サンドボックスの削除

>[!IMPORTANT]
>
>デフォルトの実稼動用サンドボックスは削除できません。

実稼動用サンドボックスまたは開発用サンドボックスを削除すると、権限を含め、そのサンドボックスに関連付けられているすべてのリソースが完全に削除されます。

サンドボックスのリストから削除するサンドボックスを選択します。 表示される右側のナビゲーションパネルで、「**[!UICONTROL 削除]**」を選択します。

![次を削除します。](../images/ui/delete.png)

選択を確認するダイアログボックスが表示されます。 **[!UICONTROL 続行]**&#x200B;を選択して続行します。

![delete-warning](../images/ui/delete-warning.png)

最後の確認ウィンドウで、ダイアログボックスにサンドボックスの名前を入力し、「**[!UICONTROL 続行]**」を選択します。

![delete-confirm](../images/ui/delete-confirm.png)

[!DNL Audience Manager]または[!DNL Audience Core Service]との双方向セグメント共有に使用される、ユーザーが作成した実稼動用サンドボックスは、次の警告の後でも削除できます。

![seg](../images/ui/delete-seg.png)

## 次の手順

このドキュメントでは、Experience Platform UI 内でサンドボックスを管理する方法について説明しました。サンドボックス API を使用したサンドボックスの管理方法について詳しくは、『[サンドボックス開発者ガイド](../api/getting-started.md)』を参照してください。

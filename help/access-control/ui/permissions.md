---
keywords: Experience Platform;ホーム;人気のトピック;製品プロファイル;権限の管理
solution: Experience Platform
title: 製品プロファイルに対する権限の管理
description: Adobe Experience Platform のアクセス制御では、Adobe Admin Console を使用して、様々な Platform 機能のロールと権限を管理できます。このドキュメントでは、Platform の製品プロファイルの権限を管理する方法について説明します。
exl-id: ca403bef-6d62-4ca9-bba6-d1280ac63171
source-git-commit: 1812af74e82f3071963177356b3cd4b23ea567f5
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 92%

---

# 製品プロファイルに対する権限の管理

[新しい製品プロファイルを作成](#create-a-new-product-profile)すると、すぐにプロファイルに対する権限を設定するよう求められます。既存のプロファイルに対する権限を編集する場合は、「**[!UICONTROL 製品プロファイル]**」タブからプロファイルを選択してプロファイルの詳細ページを開き、「**[!UICONTROL 権限]**」を選択します。

![permissions](../images/permissions.png)

権限はカテゴリに分けられ、このページに表示されます。リストには、カテゴリ名、含まれる権限の数（およびアクティブな権限の数）、説明が表示されます。詳しくは、 [リソース権限](/help/access-control/home.md#permissions) ：各ロールで使用可能な権限の分類。

リストの任意のカテゴリをクリックして、**[!UICONTROL 権限の編集]**&#x200B;ページを開きます。

![権限の編集](../images/edit-permissions.png)

**[!UICONTROL 権限の編集]**&#x200B;ページには、選択した製品プロファイルの権限を追加および削除するためのワークスペースが表示されます。画面の左側には、権限カテゴリのリストが表示されます。カテゴリをクリックすると、**[!UICONTROL 使用可能な権限項目]**&#x200B;に表示される権限が変更されます。

例えば、データモデリングの権限を更新するには、「 」を選択します。 **[!UICONTROL データモデリング]**.

![profile-management](../images/profile-management.png)

権限を追加するには、権限名の横にあるプラス&#x200B;**（+）**&#x200B;アイコンを選択します。または、「**[!UICONTROL すべて追加]**」をクリックして、現在のカテゴリのすべての権限をプロファイルに追加します。追加された権限は、「**[!UICONTROL 含まれる権限項目]**」の下に表示されます。

![add-permission](../images/add-permission.png)

>[!NOTE]
>
>「**[!UICONTROL 含まれる権限項目]**」リストには、現在選択されているカテゴリから追加された権限のみが表示されます。

権限を削除するには、権限の名前の横にある「**X**」アイコンをクリックするか、**[!UICONTROL すべて削除]**&#x200B;を選択して、現在のカテゴリの下にあるすべての権限を削除します。削除された権限は、「**[!UICONTROL 使用可能な権限項目]**」の下に再表示されます。

引き続き使用可能なカテゴリを確認し、必要な権限を追加します。終了したら「**[!UICONTROL 保存]**」をクリックします。

![remove-permisson](../images/remove-permission.png)

製品プロファイルの 「**[!UICONTROL 権限]**」タブが再び表示され、選択した権限がアクティブになったことが示されます。

![permissions-updated](../images/permissions-updated.png)

## 次の手順

権限が設定されている場合は、次の手順に進み、[製品プロファイルの詳細とサービスを管理](details-and-services.md)できます。

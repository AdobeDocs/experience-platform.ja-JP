---
solution: Experience Platform
title: Experience Platform ダッシュボードのアクセス権限の取得方法と付与方法
type: Documentation
description: Adobe Admin Console を使用して Experience Platform ダッシュボードの表示、編集、更新の機能をユーザーに付与します。
exl-id: 2e50790f-b3ab-4851-a9a5-7cb98bf98ce3
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 100%

---

# ダッシュボードのアクセス権限

ダッシュボードの表示、編集、更新の機能をユーザーに付与するには、まず権限を有効にする必要があります。Adobe Experience Platform では、アクセス制御は Adobe Admin Console を通じて提供されます。この機能は、[!DNL Admin Console] の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。

このドキュメントでは、Admin Console 内の特定のダッシュボード権限へのアクセスを提供する方法の概要を説明します。アクセス権限の取得と割り当てについて詳しくは[アクセス制御の概要](../access-control/home.md)を参照してください。

>[!NOTE]
>
>[!DNL Experience Platform] 用にアクセス制御を設定するには、[!DNL Experience Platform] 製品統合を持つ組織の管理者権限が必要です。詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)に関する Adobe Help Center の記事を参照してください。

## 使用可能な権限 {#available-permissions}

Experience Platform 内でダッシュボードにアクセスするために必要な権限は主に 2 つあります。その権限は次の通りです。

* **ライセンス使用状況ダッシュボードの表示**：この権限を持つユーザーは Experience Platform UI 内のライセンス使用状況ダッシュボードに読み取り専用でアクセスできます。
* **標準ダッシュボードの管理**：この権限を持つユーザーは、Data Warehouse にないカスタム属性を追加できます。

以下の手順は、Admin Console を使用してこれらの権限を追加する方法を示しています。

## 製品プロファイルの選択

Experience Platform でダッシュボードへのアクセス権をユーザーに付与するには、[Adobe Admin Console](https://adminconsole.adobe.com) にログインし、上部のナビゲーションから「**製品**」を選択します。

![](images/admin-console/admin-console-overview.png)

左側のナビゲーションにある「Experience Cloud」ドロップダウンから、または「*すべての製品とサービス*」の下に表示されるカードから、**Adobe Experience Platform**&#x200B;を選択します。Adobe Experience Platform 製品ページで、ダッシュボードの権限を追加する製品プロファイルを選択するか、「**新しいプロファイル**」を選択して新しい製品プロファイルを作成します。

![](images/admin-console/products.png)

選択した製品プロファイルが開き、その製品プロファイルに関連付けられているユーザーが表示されます。製品プロファイルの権限を管理するには、「**権限**」を選択します。

![](images/admin-console/product-users.png)

## 権限の追加/編集

「**権限**」タブには、製品プロファイルに使用可能なすべての権限が表示されます。「**ダッシュボード**」の行を特定し、現在「0/2 が含まれています」と表示されていることか確認してください。これは製品プロファイルに対して有効なダッシュボード権限がないことを意味します。

ダッシュボードの権限を編集するには、ダッシュボードの行で「**編集**」を選択します。

![](images/admin-console/product-permissions.png)

**権限を編集**&#x200B;ダイアログが開き、使用可能な権限項目と含まれる権限項目が表示されます。権限の横にあるプラス記号（`+`）を選択して追加するか、「**+すべてを追加**」を選択してすべての権限を一度に追加します。

権限の説明については、このドキュメントの前の節[使用可能な権限](#available-permissions)を参照してください。

>[!NOTE]
>
>すべてのユーザーに対してすべての権限を有効にする必要はありません。組織の構造に応じて、特定のユーザーに対して個別の製品プロファイルを作成し、制限付きアクセス（読み取り専用など）を許可することができます。

権限が追加されたら、「**保存**」を選択して製品プロファイルに戻ります。

![](images/admin-console/dashboard-permissions.png)

製品プロファイルに戻ると、**ダッシュボード**&#x200B;の行に「2/2 が含まれています」と表示されていることを確認し、権限が追加されたことを確認できます。

![](images/admin-console/product-permissions-included.png)

## 次の手順

ダッシュボードへのアクセス権限を追加したので、組織内のユーザーは Experience Platform UI 内でダッシュボードの表示を開始し、割り当てた権限に基づいて他の操作を実行できます。

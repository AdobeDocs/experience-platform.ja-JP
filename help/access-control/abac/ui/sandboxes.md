---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御管理サンドボックス
description: Adobe Experience Cloudの権限インターフェイスを使用したサンドボックスの管理。
exl-id: c21eb319-fc0d-442a-b778-bbfa2d6bb22d
source-git-commit: 8c9503c9923372ef919d485d4ec0e3ebda5a2413
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 26%

---

# サンドボックスの管理 {#mange-sandboxes}

>[!CONTEXTUALHELP]
>id="platform_permissions_sandboxes_about"
>title="サンドボックスとは"
>abstract="サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションです。サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスに限定され、他のサンドボックスには影響しません。サンドボックスへのアクセスは、役割を通じて管理されます。"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/sandbox/home" text="サンドボックスの概要"

サンドボックスは、デジタルエクスペリエンスアプリケーションの開発プロセスをシームレスに統合する、Adobe Experience Platformの 1 つのインスタンス内の仮想パーティションです。 サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスのみに限定され、他のサンドボックスには影響しません。サンドボックスについて詳しくは、「[ サンドボックスの概要 ](../../../sandboxes/home.md)」を参照してください。

## サンドボックスを探索 {#explore-sandboxes}

サンドボックスの詳細および関連する役割を表示するには、**[!UICONTROL Permissions]** Adobe Experience Cloud[ の ](https://experience.adobe.com/){target="_blank"} に移動します。 左パネルの「**[!UICONTROL Sandboxes]**」セクションから「**[!UICONTROL Resources]**」を選択します。

組織のサンドボックスのリストが表示されます。 表示するサンドボックスをリストから選択します。 または、検索バーにサンドボックスの名前を入力してサンドボックスを検索するか、フィルターアイコン（![ フィルターアイコン ](../../../images/icons/filter.png)）を選択して **[!UICONTROL Sandbox Type]** ドロップダウンメニューを使用してタイプでサンドボックスをフィルタリングします。

![ 権限内のサンドボックスワークスペース。](../../images/ui/sandboxes/sandboxes-overview.png){zoomable="yes"}

>[!NOTE]
>
>権限のサンドボックスワークスペースでは、サンドボックス管理アクションは許可されていません。 サンドボックスを管理するには、ワークスペースの右上にある「**[!UICONTROL Open sandbox manager]**」オプションを選択します。

「**[!UICONTROL Details]**」タブには、サンドボックスの概要が表示されます。 概要には、サンドボックスの **[!UICONTROL Title]**、**[!UICONTROL Sandbox Name]**、**[!UICONTROL Type]**、**[!UICONTROL Region]**、**[!UICONTROL Modified]** 日、**[!UICONTROL Modified by]** および **[!UICONTROL Status]** が表示されます。

![ サンドボックスの詳細ワークスペース。](../../images/ui/sandboxes/sandbox-details.png){zoomable="yes"}

「**[!UICONTROL Roles]**」タブを選択して、サンドボックスが割り当てられている役割を表示します。 役割を選択すると、その役割のワークスペースに移動します。

<!-- To manage the role's sandboxes, follow the [](./roles.md) guide. -->

![ サンドボックスの役割ワークスペース。](../../images/ui/sandboxes/sandbox-roles.png){zoomable="yes"}


## 次の手順

これで、サンドボックスの詳細と役割を表示する方法を理解できました。 属性ベースのアクセス制御の詳細については、[ 属性ベースのアクセス制御の概要 ](../overview.md) を参照してください。

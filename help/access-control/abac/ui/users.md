---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御管理ユーザー
description: Adobe Experience Cloudの権限インターフェイスで、ユーザーとユーザーグループを管理できます。
exl-id: 16450867-040a-4be1-a6c0-f03d0a1b90ba
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 9%

---

# ユーザーを管理し、ユーザーグループを追加 {#manage-users}

>[!CONTEXTUALHELP]
>id="platform_permissions_users_about"
>title="ユーザーとは"
>abstract="ユーザーは、Experience Platform へのアクセス権が付与された個人です。 個々のユーザーの組織のリソースへのアクセスは、役割を通じて管理されます。"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/abac/permissions-ui/roles" text="役割の管理"

ユーザーは、Adobe Experience Platformにアクセスできる個人です。 組織のリソースに対する個々のユーザーのアクセスは、[役割](./roles.md){target="_blank"}を通じて管理されます。 組織は[&#x200B; ユーザーグループ &#x200B;](#user-groups)を作成して、複数のユーザーに同時にシームレスにアクセスできるようにすることもできます。 ユーザーはAdmin Consoleで管理され、Adobe Experience Platform製品カードに関連付けられているユーザーは、Experience Platformのユーザーリストの一部として表示されます。

## ユーザーの管理

<!-- 
ADD LINKS INTO IMPORTANT NOTE BELOW
>[!IMPORTANT]
>
>[!UICONTROL Permissions] manages access control for existing Experience Platform users. To add users to Experience Platform, navigate to Adobe Admin Console through the **[!UICONTROL Edit in admin console]** option. To learn how to add users through the Admin Console, follow the [adding users to Experience Platform](...){#target="_blank"} guide.
-->

組織のユーザーを表示するには、[Adobe Experience Cloud](https://experience.adobe.com/){target="_blank"}の&#x200B;**[!UICONTROL Permissions]**&#x200B;に移動します。 左側のパネルで「**[!UICONTROL Users]**」を選択します。

![権限内のユーザーワークスペース。](../../images/ui/users/users-overview.png){zoomable="yes"}

ユーザーのリストが表示されます。 表示するユーザーをリストから選択します。 または、検索バーを使用して、ユーザーの名前またはメールアドレスを入力してユーザーを検索します。

「**[!UICONTROL Details]**」タブには、ユーザーの概要が表示されます。 概要には、ユーザーの&#x200B;**[!UICONTROL Name]**、**[!UICONTROL Preferred languages]**、**[!UICONTROL Account Type]**、**[!UICONTROL Authentication ID]**、**[!UICONTROL Email]**、**[!UICONTROL Email verified]** ステータス、**[!UICONTROL Country code]**、および&#x200B;**[!UICONTROL Phone number]**&#x200B;が表示されます。

![&#x200B; ユーザーの詳細ワークスペース。](../../images/ui/users/user-details.png){zoomable="yes"}

「**[!UICONTROL Roles]**」タブを選択して、ユーザーが割り当てられている役割を表示します。

![&#x200B; ユーザーの役割ワークスペース。](../../images/ui/users/user-roles.png){zoomable="yes"}

### ユーザーへの役割の追加 {#add-user-role}

ユーザーに役割を追加するには、**[!UICONTROL Add Roles]**&#x200B;を選択します。

![役割を追加オプションがハイライト表示されたユーザーの役割のワークスペース。](../../images/ui/users/user-add-roles.png){zoomable="yes"}

**[!UICONTROL Add Roles]** ダイアログが表示されます。 ユーザーに追加する役割を選択し、**[!UICONTROL Save]**&#x200B;を選択します。

![役割を選択し、保存オプションを強調表示した役割を追加ダイアログ。](../../images/ui/users/user-roles-add-roles-confirm.png){zoomable="yes"}

### ユーザーからの役割の削除 {#remove-user-role}

ユーザーから役割を削除するには、役割の名前の横にある&#x200B;**X**&#x200B;を選択します。

<!-- 
ADD LINKS INTO IMPORTANT NOTE BELOW

>[!NOTE]
>
>Role's that have been added to a user through a user group cannot be removed through the user's role workspace. Role's that have been added through a user group will have an [!Info icon](/help/images/icons/info.png) beside the **X** containing information about the associated user group. To remove the role, the role would need to be [removed from the user group](#remove-user-group-role).
-->

![役割の削除オプションがハイライト表示されたユーザーの役割ワークスペース。](../../images/ui/users/user-roles-remove.png){zoomable="yes"}

確認ダイアログが表示されます。 役割の削除を完了するには、**[!UICONTROL Confirm]**&#x200B;を選択してください。

![確認オプションがハイライト表示された役割を削除するための確認ダイアログ。](../../images/ui/users/user-roles-remove.png){zoomable="yes"}

## ユーザーグループの管理 {#user-groups}

ユーザーグループとは、同じ機能を実行するためのアクセス権を持つ、グループ化された複数のユーザーのことです。

<!-- 
ADD LINKS INTO IMPORTANT NOTE BELOW
>[!IMPORTANT]
>
>[!UICONTROL Permissions] manages access control for existing Experience Platform user groups. To add user groups to Experience Platform, navigate to Admin Console through the **[!UICONTROL Edit in admin console]** option. To learn how to add user groups in the Admin Console, follow the [adding user groups to Experience Platform](...){#target="_blank"} guide.
-->

組織のユーザーを表示するには、[Adobe Experience Cloud](https://experience.adobe.com/){target="_blank"}の&#x200B;**[!UICONTROL Permissions]**&#x200B;に移動します。左側のパネルの&#x200B;**[!UICONTROL Users]** セクションから&#x200B;**[!UICONTROL Groups]**&#x200B;を選択します。

![&#x200B; ユーザーが権限内のワークスペースをグループ化します。](../../images/ui/users/user-groups-overview.png){zoomable="yes"}

ユーザーグループのリストが表示されます。 リストから表示するグループを選択します。

「**[!UICONTROL Details]**」タブには、ユーザーグループの概要が表示されます。 概要には、グループの&#x200B;**[!UICONTROL Name]**、**[!UICONTROL Description]**、**[!UICONTROL User Count]**&#x200B;および&#x200B;**[!UICONTROL Admin count]**&#x200B;が表示されます。

![&#x200B; ユーザーグループの詳細ワークスペース。](../../images/ui/users/user-group-details.png){zoomable="yes"}

「**[!UICONTROL Users]**」タブを選択して、グループに割り当てられたユーザーのリストを表示します。

![&#x200B; ユーザーグループのユーザーワークスペース。](../../images/ui/users/user-group-users.png){zoomable="yes"}

「**[!UICONTROL Roles]**」タブを選択して、現在グループに割り当てられている役割のリストを表示します。

![&#x200B; ユーザーグループの役割ワークスペース。](../../images/ui/users/user-group-roles.png){zoomable="yes"}

### ユーザーグループへの役割の追加 {#add-user-group-role}

グループに新しい役割を追加するには、**[!UICONTROL Add Roles]**&#x200B;を選択します。

![役割を追加オプションがハイライト表示されたユーザーグループの役割ワークスペース。](../../images/ui/users/user-group-add-roles.png){zoomable="yes"}

**[!UICONTROL Add Roles]** ダイアログが表示されます。 追加する役割を選択し、**[!UICONTROL Save]**&#x200B;を選択します。 役割は、ユーザーグループに属するすべてのユーザーに追加されます。

![役割を選択し、保存オプションがハイライト表示された役割を追加ダイアログ。](../../images/ui/users/user-group-add-roles-select.png){zoomable="yes"}

### ユーザーグループからの役割の削除 {#remove-user-group-role}

ユーザーグループから役割を削除するには、役割の名前の横にある&#x200B;**X**&#x200B;を選択します。

![役割の削除オプションがハイライト表示されたユーザーグループの役割ワークスペース。](../../images/ui/users/user-group-remove-role.png){zoomable="yes"}

確認ダイアログが表示されます。 役割の削除を完了するには、**[!UICONTROL Confirm]**&#x200B;を選択してください。

![確認オプションがハイライト表示された役割を削除するための確認ダイアログ。](../../images/ui/users/user-group-remove-role-confirm.png){zoomable="yes"}

## API資格情報

>[!IMPORTANT]
>
>権限でAPI資格情報を表示および管理できるのは、システム管理者のみです。

Experience Platform APIをユーザーまたは開発者として使用するには、役割の特定の権限に加えて、API資格情報を追加する必要があります。 権限を使用すると、以前に作成したExperience Platform製品に割り当てられたAPI資格情報を役割に割り当てることができます。 API資格情報の作成と割り当て、および必要な権限に関する完全なガイドについては、[Experience Platform APIの認証とアクセス &#x200B;](/help/landing/api-authentication.md){target="_blank"}のステップバイステップチュートリアルを参照してください。

Experience Platformに関連付けられている組織のAPI資格情報を表示するには、[Adobe Experience Cloud](https://experience.adobe.com/){target="_blank"}の&#x200B;**[!UICONTROL Permissions]**&#x200B;に移動します。 左側のパネルの「**[!UICONTROL Users]**」セクションから「**[!UICONTROL API Credentials]**」を選択します。

![権限内のAPI資格情報ワークスペース。](../../images/ui/users/api-credentials-overview.png){zoomable="yes"}

>[!NOTE]
>
> 組織のすべての製品に対する組織のAPI資格情報を表示するか、資格情報の詳細を確認するには、**[!UICONTROL Edit in admin console]**&#x200B;を選択します。

API認証情報のリストが表示されます。 表示するAPI資格情報をリストから選択します。

「**[!UICONTROL Details]**」タブには、API資格情報の概要が表示されます。 概要には、資格情報の&#x200B;**[!UICONTROL Name]**、**[!UICONTROL Modified]**&#x200B;日付、**[!UICONTROL Modified By]**&#x200B;属性、**[!UICONTROL Created]**&#x200B;日付、**[!UICONTROL Created by]**&#x200B;属性、**[!UICONTROL API key]**、**[!UICONTROL Technical ID]**&#x200B;および&#x200B;**[!UICONTROL Email]**&#x200B;が表示されます。

![API資格情報の詳細ワークスペース。](../../images/ui/users/api-credential-details.png){zoomable="yes"}

「**[!UICONTROL Roles]**」タブを選択します。 API資格情報に関連付けられている役割のリストが表示されます。

![API資格情報の役割ワークスペース。](../../images/ui/users/api-credential-roles.png){zoomable="yes"}

### API資格情報への役割の追加 {#add-api-credential-role}

API資格情報に役割を追加するには、**[!UICONTROL Add Roles]**&#x200B;を選択します。

![役割を追加オプションがハイライト表示されたAPI資格情報のワークスペース。](../../images/ui/users/api-credential-add-roles.png){zoomable="yes"}

**[!UICONTROL Add Roles]** ダイアログが表示されます。 ユーザーに追加する役割を選択し、**[!UICONTROL Save]**&#x200B;を選択します。

![役割を選択し、保存オプションを強調表示した役割を追加ダイアログ。](../../images/ui/users/api-credential-add-roles-select.png){zoomable="yes"}

### API資格情報からの役割の削除 {#remove-api-credential-role}

API資格情報から役割を削除するには、API資格情報の名前の横にある&#x200B;**X**&#x200B;を選択します。

![役割の削除オプションがハイライト表示されたAPI資格情報の役割ワークスペース。](../../images/ui/users/api-credential-remove-role.png){zoomable="yes"}

確認ダイアログが表示されます。 役割の削除を完了するには、**[!UICONTROL Confirm]**&#x200B;を選択してください。

![確認オプションがハイライト表示された役割を削除するための確認ダイアログ。](../../images/ui/users/api-credential-remove-role-confirm.png){zoomable="yes"}

## 次の手順

ユーザー、ユーザーグループ、およびAPI資格情報の詳細と役割を表示する方法を理解しました。 属性ベースのアクセス制御について詳しくは、[属性ベースのアクセス制御の概要](../overview.md)を参照してください。

<!--
The following video is intended to support your understanding of developer and API credentials.

>[!VIDEO](https://video.tv.adobe.com/v/3446399/?captions=jpn&learn=on)
-->
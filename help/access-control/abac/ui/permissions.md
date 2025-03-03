---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御管理役割の権限
description: このドキュメントでは、Adobe Experience Cloud の権限インターフェイスを使用して、役割の権限を設定する方法について説明します
exl-id: 8acd2bb6-eef8-4b23-8fd8-3566c7508fe7
source-git-commit: afd883c530ab1b335888e79b5f4075e774fced4b
workflow-type: tm+mt
source-wordcount: '1699'
ht-degree: 39%

---

# 役割の権限の管理 {#manage-role-permissions}

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_about"
>title="役割とは？"
>abstract="役割は、管理者、スペシャリストまたはエンドユーザーが組織内のリソースに対して持つアクセス権を定義します。これらは、Platform インスタンスとやり取りするユーザーを分類し、アクセス制御ポリシーの構成要素になります。役割には特定の権限セットがあり、必要な表示または書き込みアクセスの範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/roles.html?lang=ja" text="役割の管理"

>[!IMPORTANT]
>
>アクセス制御は、権限の付与にユーザー ID（ユーザーに割り当てられた内部の一意の ID）を使用します。組織を Adobe ID から Business ID に移行すると、ユーザー ID が変更され、新しく生成されたユーザー ID がアクセス制御で使用されるので、そのユーザーに設定されたすべての権限は失われます。組織が Business ID に移行されている場合、アドビ担当者に連絡して、Adobe IDから Business ID にユーザー ID を移行してください。

権限は、管理者がユーザーの役割およびアクセスポリシーを定義し、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できる、Experience Cloud の領域です。

権限を通じて、役割を作成および管理し、それらの役割に対して必要なリソース権限を割り当てることができます。また、権限では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。

[新しい役割の作成](#create-a-new-role)が完了するとすぐに、「**[!UICONTROL 役割]**」タブに戻ります。既存の役割の権限を編集する場合は、「**[!UICONTROL 役割]**」タブをクリックします。または、「フィルター」オプションを使用して結果をフィルターし、役割を見つけます。

## 役割のフィルタリング

ファネルアイコン（![フィルターアイコン](/help/images/icons/filter.png)）を選択し、フィルターコントロールのリストを表示して、結果を絞り込みます。

![ ロールのフィルターセクションがハイライト表示された権限 UI のロールダッシュボード。](../../images/flac-ui/flac-filters.png)

役割には次のフィルターを UI で使用できます。

| フィルター | 説明 |
| --- | --- |
| [!UICONTROL 作成期間] | 結果をフィルターする日付範囲を定義する開始日および／または終了日を選択します。 |
| [!UICONTROL 作成者] | ドロップダウンからユーザーを選択し、役割の作成者でフィルタリングします。 |
| [!UICONTROL 次の期間で変更された] | 結果をフィルターする日付範囲を定義する開始日および／または終了日を選択します。 |
| [!UICONTROL 変更者] | 役割の変更者でフィルタリングします。そのためには、ドロップダウンからユーザーを選択します。 |

フィルターを削除するには、該当するフィルターのピルアイコンの「X」を選択するか、「**[!UICONTROL すべてクリア]**」をクリックして、すべてのフィルターを削除します。

![ 選択したフィルターで「X」と「すべての選択をクリア」がハイライト表示された権限 UI の役割ダッシュボード ](../../images/flac-ui/flac-clear-filters.png)

## 役割の詳細 {#role-details}

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_details"
>title="役割の概要"
>abstract="役割の概要ダイアログには、特定の役割がアクセスできるリソースやサンドボックスなど、役割の詳細が表示されます。役割のワークスペース内の対応するタブに移動すると、役割のラベル、ユーザー、ユーザーグループ、API 資格情報を管理できます。"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/abac/permissions-ui/permissions#manage-labels-for-a-role" text="役割のラベルの管理"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/abac/permissions-ui/permissions#manage-users-for-a-role" text="役割のユーザーの管理"

「**[!UICONTROL 役割]**」タブから役割を選択すると、役割の [!UICONTROL  詳細 ] ダッシュボードが開きます。

![ 選択した役割の詳細ワークスペースが表示され、概要情報がハイライト表示されます。](../../images/flac-ui/flac-details.png)

[!UICONTROL  詳細 ] ダッシュボードには、役割の概要が表示されます。 概要には、役割名、説明、作成者、最終変更者が、作成日と変更日と共に表示されます。 また、役割に関連付けられた権限と、割り当てられたサンドボックスのリストも表示されます。 必要に応じて、役割の名前と説明を変更できます。

## 役割のラベルの管理

**[!UICONTROL ラベル]** タブを選択して役割ラベルワークスペースを開き、「**[!UICONTROL ラベルを追加]**」を選択して役割にラベルを割り当てます。

![ 役割のラベル ワークスペースは、「ラベル」タブと「ラベルを追加」ボタンがハイライト表示されて表示されます。](../../images/flac-ui/flac-labels.png)

**[!UICONTROL アクセスラベルとデータガバナンスラベルを適用]** ダイアログが表示され、ラベルのリストが表示されます。 リストには、ラベル名、わかりやすい名前、カテゴリ、説明が表示されます。

役割に追加するラベルをリストから選択し、「**[!UICONTROL 保存]**」を選択します。

![ ラベルが選択されたアクセスラベルとデータガバナンスラベルを適用ダイアログ。](../../images/flac-ui/flac-add-labels.png)

追加されたラベルは、「**[!UICONTROL ラベル]**」タブに表示されます。

![ 追加されたラベルがハイライト表示された役割のラベルワークスペース。](../../images/flac-ui/flac-added-labels.png)

役割からラベルを削除するには、ラベルを選択して「**[!UICONTROL ラベルを削除]**」を選択します。

![ 役割が選択され、「ラベルを削除」オプションがハイライト表示されている役割のラベルワークスペース。](../../images/flac-ui/flac-delete-labels.png)

## 役割のサンドボックスの管理

「**[!UICONTROL 詳細]**」タブを選択し、「**[!UICONTROL サンドボックス]**」セクションに移動します。 **[!UICONTROL すべて表示]** を選択して、役割に追加されたサンドボックスの完全なリストを表示します。

![ サンドボックスセクションがハイライト表示された役割の詳細ワークスペース。](../../images/flac-ui/flac-sandboxes.png)

役割にさらにサンドボックスを追加するには、UI の右上から **[!UICONTROL 編集]** を選択します。

![ 「編集」オプションがハイライト表示された役割の詳細ワークスペース。](../../images/flac-ui/flac-add-sandboxes.png)

次の画面では、ドロップダウンを使用して、役割に含めるサンドボックスリソースを選択するように求められます。 終了したら、「**[!UICONTROL 保存]**」、「**[!UICONTROL 閉じる]** の順に選択します。

![ サンドボックスリソース ドロップダウンメニューがハイライト表示されている役割のリソースダッシュボード。](../../images/flac-ui/flac-add-role-permission.png)

## 役割のユーザーの管理

「**[!UICONTROL ユーザー]**」タブを選択して役割 [!UICONTROL  ユーザー ] ワークスペースを開きます。次に、「**[!UICONTROL ユーザーを追加]**」を選択して、ユーザーを役割に割り当てます。

![ 役割のユーザーワークスペースは、「ユーザー」タブと「ユーザーを追加」オプションがハイライト表示された状態で表示されます。](../../images/flac-ui/flac-users.png)

**[!UICONTROL ユーザーを追加]** ダイアログが表示されます。 役割に追加するユーザーをリストから選択します。 または、検索バーを使用して名前またはメールアドレスを入力してユーザーを検索し、「**[!UICONTROL 保存]**」を選択します。

![ ユーザーが選択され、検索バーと「保存」オプションがハイライト表示されたユーザーを追加ダイアログ ](../../images/flac-ui/flac-add-users.png)

追加されたユーザーは「**[!UICONTROL ユーザー]**」タブの下に表示されます。

![ 役割に追加されたユーザーを示す役割のユーザーワークスペース。](../../images/flac-ui/flac-added-users.png)

役割からユーザーを削除するには、ユーザー名の横にある「**X**」アイコンを選択します。

![ 「X」オプションがハイライト表示されたユーザーを示す役割のユーザーワークスペース。](../../images/flac-ui/flac-remove-users.png)

次のビデオは、新しい役割の作成とその役割のユーザーの管理に関する理解を深めることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/336081/?learn=on)

## 役割の API 資格情報の管理 {#manage-api-credentials-for-role}

>[!CONTEXTUALHELP]
>id="platform_permissions_apicredentials_about"
>title="API 資格情報とは"
>abstract="API 資格情報は役割に割り当てられ、ユーザーと開発者に Platform API へのアクセスを許可します。 Platform API を使用すると、計算属性の設定、データやエンティティへのアクセス、データのエクスポート、不要なデータやバッチの削除など、データに対する基本的な CRUD （作成、読み取り、更新、削除）操作をプログラムで実行できます。"
>additional-url="https://experienceleague.adobe.com/en/docs/experience-platform/landing/platform-apis/api-guide" text="Platform API ガイド"

>[!IMPORTANT]
>
> [!UICONTROL  権限 ] で API 資格情報を使用および管理するには、ユーザーにシステム管理者権限が必要です。

Experience Platform API をユーザーまたは開発者として使用するには、役割の特定の権限セットに加えて API 資格情報を追加する必要があります。 API 資格情報の作成と割り当て、および必要な権限について詳しくは、[Experience Platform API の認証とアクセス ](../../../landing/api-authentication.md#generate-credentials) のステップバイステップのチュートリアルを参照してください。

**[!UICONTROL API 資格情報]** タブを選択して役割 API 資格情報ワークスペースを開き、「**[!UICONTROL API 資格情報を追加]**」を選択して、API 資格情報を役割に割り当てます。

![ 「資格情報を追加」オプションがハイライト表示された役割の API 資格情報ワークスペース。](../../images/flac-ui/flac-api-credentials.png)

**[!UICONTROL API 資格情報を追加]** ダイアログが表示されます。 役割に追加するリストから API 資格情報を選択し、「**[!UICONTROL 保存]**」を選択します。

![ 認証情報が選択され、「保存」オプションがハイライト表示された API 認証情報を追加ダイアログ ](../../images/flac-ui/flac-add-api-credentials.png)

追加された API 資格情報は、「**[!UICONTROL API 資格情報]**」タブの下に表示されます。

![ 追加された資格情報が表示された役割の API 資格情報ワークスペース。](../../images/flac-ui/flac-added-api-credentials.png)

API 資格情報を役割から削除するには、API 資格情報名の横にある「**X**」アイコンを選択します。

![ 資格情報を削除する X オプションがハイライト表示されている役割の API 資格情報ワークスペース。](../../images/flac-ui/flac-remove-api-credentials.png)

**[!UICONTROL API 資格情報を削除]** ダイアログが表示され、削除を確認するよう求められます。 「**[!UICONTROL 確認]**」を選択して、選択した秘密鍵証明書の削除を完了します。

![ 資格情報の削除を確認するように促す「資格情報を削除」ポップオーバーがハイライト表示されます。](../../images/flac-ui/flac-confirm-api-credentials-delete.png)

「**[!UICONTROL API 資格情報]**」タブに戻されます。

## 役割のユーザーグループの管理 {#manage-user-groups}

>[!CONTEXTUALHELP]
>id="platform_permissions_usergroups_about"
>title="ユーザーグループとは"
>abstract="ユーザーグループは、同じ機能へのアクセス権を共有する複数のユーザーの集まりです。 組織内のリソースへのアクセスは、ユーザーグループに割り当てられた役割を通じて管理されます。"
>additional-url="https://experienceleague.adobe.com/en/docs/experience-platform/access-control/abac/permissions-ui/roles" text="役割の管理"

ユーザーグループとは、同じ機能を実行するためのアクセス権を持つ、グループ化された複数のユーザーのことです。

「**[!UICONTROL ユーザーグループ]**」タブを選択して役割のユーザーグループワークスペースを開きます。次に、「**[!UICONTROL グループを追加]**」を選択して、ユーザーグループを役割に割り当てます。

![ 「グループを追加」オプションを使用した役割のユーザーグループワークスペース ](../../images/flac-ui/flac-user-groups.png)

**[!UICONTROL グループを追加]** ダイアログが表示されます。 役割に追加するユーザーグループをリストから選択します。 または、検索バーを使用してグループ名を入力してユーザーグループを検索し、「**[!UICONTROL 保存]**」を選択します。

![ ユーザーグループが選択され、「検索と保存」オプションがハイライト表示されたグループを追加ダイアログ ](../../images/flac-ui/flac-add-user-groups.png)

追加されたユーザーグループは「**[!UICONTROL ユーザーグループ]**」タブの下に表示されます。

![ 追加されたユーザーグループのリストを表示する役割のユーザーグループワークスペース。](../../images/flac-ui/flac-added-user-groups.png)

役割からユーザーグループを削除するには、ユーザーグループ名の横にある「**X**」アイコンを選択します。

![ 特定のユーザーグループを削除する X オプションがハイライト表示されている役割のユーザーグループのワークスペース。](../../images/flac-ui/flac-remove-user-groups.png)

**[!UICONTROL ユーザーグループを削除]** ダイアログが表示され、削除を確認するよう求められます。 「**[!UICONTROL 確認]**」を選択して、選択したユーザーグループを削除します。

![ ユーザーグループを削除するためのポップオーバーが表示され、ハイライト表示されます。](../../images/flac-ui/flac-confirm-user-groups-delete.png)

「**[!UICONTROL ユーザーグループ]**」タブに戻されます。

## Experience Platformへのユーザーの追加

システム管理者は、ユーザーがAdobe Developer Consoleで [ 統合を作成 ](../../../landing/api-authentication.md#generate-credentials) できるように、開発者にアクセス権を付与できます。

ユーザーExperience Platformを追加するには、[Admin Consoleにログインし ](https://adminconsole.adobe.com) 「**[!UICONTROL ユーザーを追加]**」を選択します。

![ 「ユーザーを追加」オプションがハイライト表示されたAdobe Admin Console ダッシュボード。](../../images/flac-ui/product-profile-add-users.png)

**[!UICONTROL チームにユーザーを追加]**&#x200B;ダイアログが表示されます。ユーザーのメールアドレス、名（オプション）および姓（オプション）を入力します。 次に、「**[!UICONTROL 製品]**」を選択します。

![ 「ユーザーフィールドと製品」オプションがハイライト表示されたチームにユーザーを追加ダイアログ ](../../images/flac-ui/product-profile-add-users-to-your-team.png)

**[!UICONTROL 製品を選択]** ダイアログが表示されます。 「**[!UICONTROL Adobe Experience Platform]**」を選択します。

![Adobe Experience Platformがハイライト表示された製品を選択ダイアログ ](../../images/flac-ui/product-profile-select-product.png)

**[!UICONTROL 製品プロファイルを選択]** ダイアログが表示されます。 **[!UICONTROL AEP-Default-All-Users]** を選択してから、「**[!UICONTROL 保存]**」を選択します。

![AEP-Default-All-Users が選択され、「適用」がハイライト表示された製品プロファイルを選択ダイアログ ](../../images/flac-ui/product-profile-select-product-profiles.png)

情報を確認し、「**[!UICONTROL 保存]**」を選択してユーザーを追加します。

![ ユーザー情報と選択した項目および「保存」オプションがハイライト表示されたチームにユーザーを追加ダイアログ ](../../images/flac-ui/product-profile-save-user.png)

## 次の手順

権限が設定されたら、次の手順に進んで、[ユーザーを管理](users.md)することができます。

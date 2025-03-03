---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: 属性ベースのアクセス制御役割の作成
description: このドキュメントでは、Adobe Experience Cloudの権限インターフェイスを使用した役割の管理について説明します
exl-id: 85699716-339d-4992-8390-95563c7ea7fe
source-git-commit: 74980c6108a32ec6736ab5892d89590e04e8a500
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 30%

---

# 役割の管理

役割は、管理者、スペシャリストまたはエンドユーザーが組織内のリソースに対して持つアクセス権を定義します。役割ベースのアクセス制御環境では、ユーザーアクセスプロビジョニングは、共通の責任とニーズによってグループ化されます。役割には特定の権限セットがあり、必要な表示または書き込みアクセスの範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。

## 新しい役割の作成 {#create-new-role}

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_about_create"
>title="新しい役割の作成"
>abstract="新しい役割を作成すると、Platform インスタンスとやり取りするユーザーをより適切に分類できます。 例えば、社内マーケティングチームの役割を作成し、その役割に規制医療データ（RHD）ラベルを適用して、社内マーケティングチームが保護された医療情報（PHI）にアクセスできるようにすることができます。または、外部エージェンシーの役割を作成したうえで、その役割に RHD ラベルを適用しないことにより、その役割の PHI データへのアクセスを拒否することもできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/roles.html?lang=ja" text="役割の作成"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/abac/end-to-end-guide#label-roles" text="役割へのラベルの適用"

新しい役割を作成するには、サイドバーの「**[!UICONTROL 役割]**」タブを選択し、「**[!UICONTROL 役割を作成]**」を選択します。

![flac-new-role](../../images/flac-ui/flac-new-role.png)

**[!UICONTROL 新しい役割の作成]** ダイアログが表示され、名前とオプションの説明を入力するよう求められます。

終了したら、「**[!UICONTROL 確認]**」を選択します。

![flac-create-new-role](../../images/flac-ui/flac-create-new-role.png)

次に、ドロップダウンメニューを使用して、役割に含めるリソース権限を選択します。

![flac-add-role-permission](../../images/flac-ui/flac-add-role-permission.png)

さらにリソースを追加するには、左側のナビゲーションパネルから ]**0}Adobe Experience Platform} を選択して、リソースのリストを表示します。**[!UICONTROL &#x200B;または、左側のナビゲーションパネルの検索バーにリソース名を入力します。

![flac-add-additional-resources](../../images/flac-ui/flac-add-additional-resources.png)

関連するリソースをクリックしてメインパネルにドラッグ&amp;ドロップします。

![flac-additional-resources-added](../../images/flac-ui/flac-additional-resources-added.png)

ドロップダウンメニューを使用して、役割に含めるリソース権限を選択します。 役割に含めるすべてのリソースに対して、この手順を繰り返します。 終了したら、「**[!UICONTROL 保存して終了]**」を選択します。

![flac-save-resources](../../images/flac-ui/flac-save-resources.png)

新しい役割が正常に作成されると、ユーザーは **[!UICONTROL 役割]** ページにリダイレクトされ、新しく作成された役割がリストに表示されます。

![flac-role-saved](../../images/flac-ui/flac-role-saved.png)

役割の権限を作成してから管理する方法について詳しくは、[ 役割の権限の管理 ](#manage-permissions-for-a-role) の節を参照してください。

次のビデオは、新しい役割の作成とその役割のユーザーの管理に関する理解を深めることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/336081/?learn=on)

## 役割の複製

既存の役割を複製するには、「**[!UICONTROL 役割]** タブから役割を選択します。 または、「フィルター」オプションを使用して結果をフィルターし、複製する役割を見つけます。

![flac-duplicate-role](../../images/flac-ui/flac-duplicate-role.png)

次に、画面の右上から「**[!UICONTROL 複製]**」を選択します。

![flac-duplicate](../../images/flac-ui/flac-duplicate.png)

**[!UICONTROL 役割を複製]** ダイアログが表示され、複製操作の確認を求められます。

![flac-duplicate-confirm](../../images/flac-ui/flac-duplicate-confirm.png)

次に、役割の詳細ページに移動して、役割の名前と権限を変更できます。 詳細、ラベル、サンドボックスが以前の役割から複製されます。 ユーザーは、「ユーザー」タブを使用して追加する必要があります。 [ 役割の権限の管理 ](permissions.md) ドキュメントを表示して、役割への詳細、ラベル、サンドボックス、ユーザーの追加について確認できます。

左矢印をクリックして「**[!UICONTROL 役割]**」タブに戻ります。

![flac-return-to-roles](../../images/flac-ui/flac-return-to-roles.png)

新しい役割が **[!UICONTROL 役割]** ページのリストに表示されます。

![flac-role-duplicate-saved](../../images/flac-ui/flac-role-duplicate-saved.png)

## 役割の削除

役割名の横にある省略記号（`…`）を選択すると、役割を編集、削除または複製するためのコントロールがドロップダウンに表示されます。 ドロップダウンから「プロジェクト」を選択します。

![flac-role-delete](../../images/flac-ui/flac-role-delete.png)

**[!UICONTROL ユーザーロールを削除]** ダイアログが表示され、削除を確認するよう求められます。

![flac-confirm-role-delete](../../images/flac-ui/flac-confirm-role-delete.png)

「**[!UICONTROL 役割]** タブに戻されます。

## 次の手順

新しい役割を作成したら、次の手順 [ 役割の権限を管理 ](permissions.md) に進むことができます。

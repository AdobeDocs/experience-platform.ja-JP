---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: アクセス制御ポリシーの管理
description: ここでは、Adobe Experience Cloudの権限インターフェイスを使用したアクセス制御ポリシーの管理について説明します。
exl-id: 66820711-2db0-4621-908d-01187771de14
source-git-commit: afd883c530ab1b335888e79b5f4075e774fced4b
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 12%

---

# アクセス制御ポリシーの管理

アクセス制御ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを規定するステートメントです。 アクセスポリシーは、ローカルまたはグローバルのいずれかであり、他のポリシーを上書きできます。 Adobeには、すぐにアクティブ化できる、または組織がラベルに基づいて特定のオブジェクトへのアクセスの制御を開始する準備が整った場合に常にアクティブ化できるデフォルトポリシーが用意されています。 デフォルトのポリシーでは、リソースに適用されるラベルを活用して、ユーザーが一致するラベルの役割にない限り、アクセスを拒否します。

>[!IMPORTANT]
>
>アクセスポリシーをデータ使用ポリシーと混同しないでください。データ使用ポリシーは、組織内のどのユーザーがアクセス権を持つかではなく、Adobe Experience Platformでのデータの使用方法を制御します。 詳しくは、[ データ使用ポリシー ](../../../data-governance/policies/create.md) の作成に関するガイドを参照してください。

<!-- ## Create a new policy

To create a new policy, select the **[!UICONTROL Policies]** tab in the sidebar and select **[!UICONTROL Create Policy]**.

![flac-new-policy](../../images/flac-ui/flac-new-policy.png)

The **[!UICONTROL Create a new policy]** dialog appears, prompting you to enter a name, and an optional description. When finished, select **[!UICONTROL Confirm]**.

![flac-create-new-policy](../../images/flac-ui/flac-create-new-policy.png)

Using the dropdown arrow select if you would like to **Permit access to** (![flac-permit-access-to](../../images/flac-ui/flac-permit-access-to.png)) a resource or **Deny access to** (![flac-deny-access-to](../../images/flac-ui/flac-deny-access-to.png)) a resource.

Next, select the resource that you would like to include in the policy using the dropdown menu and search access type, read or write.

![flac-flac-policy-resource-dropdown](../../images/flac-ui/flac-policy-resource-dropdown.png)

Next, using the dropdown arrow select the condition you would like to apply to this policy, **The following being true** (![flac-policy-true](../../images/flac-ui/flac-policy-true.png)) or **The following being false** (![flac-policy-false](../../images/flac-ui/flac-policy-false.png)).

Select the plus icon to **Add matches expression** or **Add expression group** for the resource. 

![flac-policy-expression](../../images/flac-ui/flac-policy-expression.png)

Using the dropdown, select the **Resource**.

![flac-policy-resource-dropdown](../../images/flac-ui/flac-policy-resource-dropdown-1.png)

Next, using the dropdown select the **Matches**.

![flac-policy-matches-dropdown](../../images/flac-ui/flac-policy-matches-dropdown.png)

Next, using the dropdown, select the type of label (**[!UICONTROL Core label]** or **[!UICONTROL Custom label]**) to match the label assigned to the User in roles.

![flac-policy-user-dropdown](../../images/flac-ui/flac-policy-user-dropdown.png)

Finally, select the **Sandbox** that you would like the policy conditions to apply to, using the dropdown menu.

![flac-policy-sandboxes-dropdown](../../images/flac-ui/flac-policy-sandboxes-dropdown.png)

Select **Add resource** to add more resources. Once finished, select **[!UICONTROL Save and exit]**.

![flac-policy-save-and-exit](../../images/flac-ui/flac-policy-save-and-exit.png)

The new policy is successfully created, and you are redirected to the **[!UICONTROL Policies]** tab, where you will see the newly created policy appear in the list. 

![flac-policy-saved](../../images/flac-ui/flac-policy-saved.png)

## Edit a policy

To edit an existing policy, select the policy from the **[!UICONTROL Policies]** tab. Alternatively, use the filter option to filter the results to find the policy you want to edit.

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

Next, select the ellipsis (`…`) next to the policies name, and a dropdown displays controls to edit, deactivate, delete, or duplicate the role. Select edit from the dropdown.

![flac-policy-edit](../../images/flac-ui/flac-policy-edit.png)

The policy permissions screen appears. Make the updates then select **[!UICONTROL Save and exit]**.

![flac-policy-save-and-exit](../../images/flac-ui/flac-policy-save-and-exit.png)

The policy is successfully updated, and you are redirected to the **[!UICONTROL Policies]** tab.

## Duplicate a policy

To duplicate an existing policy, select the policy from the **[!UICONTROL Policies]** tab. Alternatively, use the filter option to filter the results to find the policy you want to edit.

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

Next, select the ellipsis (`…`) next to a policies name, and a dropdown displays controls to edit, deactivate, delete, or duplicate the role. Select duplicate from the dropdown.

![flac-policy-duplicate](../../images/flac-ui/flac-policy-duplicate.png)

The **[!UICONTROL Duplicate policy]** dialog appears, prompting you to confirm the duplication. 

![flac-policy-duplicate-confirm](../../images/flac-ui/flac-duplicate-confirm.png)

The new policy appears in the list as a copy of the original on the **[!UICONTROL Policies]** tab.

![flac-role-duplicate-saved](../../images/flac-ui/flac-role-duplicate-saved.png)

## Delete a policy

To delete an existing policy, select the policy from the **[!UICONTROL Policies]** tab. Alternatively, use the filter option to filter the results to find the policy you want to delete.

![flac-policy-select](../../images/flac-ui/flac-policy-select.png)

Next, select the ellipsis (`…`) next to a policies name, and a dropdown displays controls to edit, deactivate, delete, or duplicate the role. Select delete from the dropdown.

![flac-policy-delete](../../images/flac-ui/flac-policy-delete.png)

The **[!UICONTROL Delete user policy]** dialog appears, prompting you to confirm the deletion. 

![flac-policy-delete-confirm](../../images/flac-ui/flac-policy-delete-confirm.png)

You are returned to the **[!UICONTROL policies]** tab and a confirmation of deletion pop over appears.

![flac-policy-delete-confirmation](../../images/flac-ui/flac-policy-delete-confirmation.png) -->

## サンドボックスのポリシーの設定

>[!IMPORTANT]
>
>デフォルトでは、すべての顧客に対して [!UICONTROL  自動インクルード ] 機能がオンになっています。つまり、すべてのサンドボックスがポリシーに追加されます。

>[!NOTE]
>
>**[!UICONTROL Default-Label-Based-Access-Control-Policy]** ポリシーは現在、設定に使用できる唯一のポリシーです。

ポリシーに関連付けられたサンドボックスを表示するには、「**[!UICONTROL ポリシー]** タブをクリックします。

![ 使用可能な既存のポリシーのリストを表示するポリシーページ ](../../images/abac-end-to-end-user-guide/abac-policies-page.png)

次に、ポリシーを選択し、「**[!UICONTROL サンドボックス]**」タブを選択します。 ポリシーに関連付けられているサンドボックスのリストが表示されます。

![ 使用可能な既存のポリシーのリストを表示するポリシーページ ](../../images/flac-ui/abac-policies-sandboxes-tab.png)

### すべてのサンドボックスにポリシーを追加

「**[!UICONTROL サンドボックス]**」タブの「自動インクルード **[!UICONTROL 切り替えを使用して、すべてのサンドボックスのポリシーをアクティベート]** ます。

![ 「[!UICONTROL  自動インクルード ] 切り替えを表示する [!UICONTROL  サンドボックス ] タブ ](../../images/flac-ui/abac-policies-auto-include.png)

**[!UICONTROL 自動インクルードを有効にする]** ダイアログが表示され、選択を確認するように求められます。 「**[!UICONTROL 有効]**」を選択して、設定を完了します。

![[!UICONTROL  自動インクルードを有効にする ] ダイアログハイライト [!UICONTROL  有効にする ]](../../images/flac-ui/abac-policies-auto-include-enable.png)

>[!SUCCESS]
>
>このポリシーは、既存のすべてのサンドボックスに対して有効になっており、新しいサンドボックスが使用可能になると自動的に追加されます。

### サンドボックスを選択するためのポリシーの追加

>[!IMPORTANT]
>
>[!UICONTROL  自動インクルード ] 切り替えがオフになっている場合、今後のサンドボックスはデフォルトではポリシーに含まれません。 サンドボックスを管理し、ポリシーに手動で追加する必要があります。

「**[!UICONTROL サンドボックス]**」タブの **[!UICONTROL 自動インクルード]** 切り替えを使用して、すべてのサンドボックスのポリシーを無効にします。

![ 「[!UICONTROL  自動インクルード ] 切り替えを表示する [!UICONTROL  サンドボックス ] タブ ](../../images/flac-ui/abac-policies-auto-include.png)

「**[!UICONTROL サンドボックス]**」タブで「**[!UICONTROL サンドボックスを追加]**」を選択し、このポリシーを適用するサンドボックスを選択します。

![ ポリシーに追加されたサンドボックスのリストを表示する [!UICONTROL  サンドボックス ] タブ。](../../images/flac-ui/abac-policies-sandboxes-tab-add.png)

サンドボックスのリストが表示されます。 追加するサンドボックスをリストから選択します。 または、検索バーを使用してサンドボックスを検索します。 「**[!UICONTROL 保存]**」を選択します。

![ ポリシーに追加できる既存のサンドボックスのリストを表示する [!UICONTROL  サンドボックスを追加 ] ページ ](../../images/flac-ui/abac-policies-sandboxes-list.png)

>[!SUCCESS]
>
>選択したサンドボックスは、正常にポリシーに追加されました。

### ポリシーからのサンドボックスの削除

サンドボックスを削除するには、サンドボックス名の横にある **X** アイコンを選択します。

![ サンドボックスのリストを表示する「[!UICONTROL  サンドボックス ]」タブ。削除する [!UICONTROL X] がハイライト表示されます。](../../images/flac-ui/abac-policies-remove-sandbox-x.png)

**[!UICONTROL 削除]** ダイアログが表示され、選択を確認するように求められます。 「**[!UICONTROL 確認]**」を選択して削除を完了します。

![[!UICONTROL  確認 ] をハイライト表示した [!UICONTROL  削除 ] ダイアログ ](../../images/flac-ui/abac-policies-remove-sandbox.png)

>[!SUCCESS]
>
>選択したサンドボックスはポリシーから正常に削除されました。

## ポリシーのアクティベート {#activate-policy}

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about"
>title="ポリシーとは"
>abstract="ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを確立するステートメントです。すべての組織には、ラベルに基づいて特定のオブジェクトへのアクセスの制御を開始するためにアクティブ化する必要がある、デフォルトのポリシーがあります。 リソースに適用されたラベルは、ラベルが一致する役割にユーザーが割り当てられていない限り、アクセスを拒否します。 デフォルトのポリシーは、編集または削除できませんが、アクティブ化または非アクティブ化は可能です。"
>additional-url="https://experienceleague.adobe.com/en/docs/experience-platform/access-control/abac/permissions-ui/labels" text="ラベルの管理"

既存のポリシーをアクティブにするには、「**[!UICONTROL ポリシー]**」タブをクリックします。

![flac-policy-select](../../images/abac-end-to-end-user-guide/abac-policies-page.png)

次に、省略記号（`…`）をクリックします。ドロップダウンに、役割を編集、アクティブ化、削除または複製するためのコントロールが表示されます。 ドロップダウンから「アクティブ化」を選択します。

![flac-policy-activate](../../images/abac-end-to-end-user-guide/abac-policies-activate.png)

**[!UICONTROL ポリシーをアクティブ化]** ダイアログが表示され、アクティベーションを確認するプロンプトが表示されます。

![flac-policy-activate-confirm](../../images/abac-end-to-end-user-guide/abac-activate-policies-dialog.png)


「 **[!UICONTROL ポリシー]** 」タブが開き、アクティベーションを確認するポップアップが表示されます。ポリシーのステータスはアクティブと表示されます。

![flac-policy-activated](../../images/abac-end-to-end-user-guide/abac-policies-confirm-activate.png)

## 次の手順

ポリシーをアクティブにすると、次の手順 [ 役割の権限の管理 ](permissions.md) に進むことができます。

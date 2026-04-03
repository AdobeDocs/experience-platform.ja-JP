---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: アクセス制御ポリシーの管理
description: Adobe Experience Cloudの権限インターフェイスを使用して、アクセス制御ポリシーを管理します。
exl-id: 66820711-2db0-4621-908d-01187771de14
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 14%

---

# アクセス制御ポリシーの管理

アクセス制御ポリシーとは、属性をまとめて、許可されるアクションと許可されないアクションを確立するステートメントです。 Adobeには、すぐにアクティブ化できるデフォルトのポリシーが用意されています。また、[ ラベル ](./labels.md){target="_blank"}に基づいて特定のオブジェクトへのアクセス制御を開始する準備が整った場合にもアクティブ化できます。 既定のポリシー&#x200B;**[!UICONTROL Default-Label-Based-Access-Control-Policy]**&#x200B;では、ユーザーが一致するラベルを持つ役割に属していない限り、リソースに適用されたラベルを利用してアクセスを拒否します。

>[!IMPORTANT]
>
>アクセス制御ポリシーは、Adobe Experience Platformでのデータの使用方法を制御するデータ使用ポリシーと混同しないでください。 詳しくは、[ データ使用ポリシー](../../../data-governance/policies/create.md){target="_blank"}の作成に関するガイドを参照してください。

## サンドボックスのポリシーの設定 {#configure-policy}

>[!NOTE]
>
>**[!UICONTROL Default-Label-Based-Access-Control-Policy]** ポリシーは現在、構成に使用できる唯一のポリシーです。

ポリシーの設定を開始するには、**[!UICONTROL Permissions]** Adobe Experience Cloud[の](https://experience.adobe.com/){target="_blank"}に移動します。 左側のパネルから「**[!UICONTROL Policies]**」を選択します。 リストから&#x200B;**[!UICONTROL Default-Label-Based-Access-Control-Policy]**&#x200B;を選択します。

![既存のポリシーのリストを表示するポリシーワークスペース。](../../images/ui/policies/policies-home.png){zoomable="yes"}

ポリシーの詳細ワークスペースが表示されます。 「**[!UICONTROL Sandboxes]**」を選択します。ポリシーに関連付けられているサンドボックスのリストが表示されます。

![関連するサンドボックスのリストを表示するポリシーのサンドボックスワークスペース。](../../images/ui/policies/policy-sandbox.png){zoomable="yes"}

### すべてのサンドボックスにポリシーを追加 {#add-policy-to-all}

>[!IMPORTANT]
>
>デフォルトでは、**[!UICONTROL Auto-include]**&#x200B;はオンになっています。つまり、現在および将来のすべてのサンドボックスが自動的にポリシーに追加されます。

**[!UICONTROL Auto-include]**&#x200B;機能をオフにして、今後のサンドボックスがポリシーに自動的に追加されないようにします。 機能&#x200B;**をオフに切り替えても、ポリシーからサンドボックスは削除されません**。

![自動含めるトグルが強調表示され、「オフ」状態になっているポリシーの「サンドボックス」タブ。](../../images/ui/policies/policy-auto-include.png){zoomable="yes"}

**[!UICONTROL Auto-include]**&#x200B;がポリシーでアクティブでない場合は、トグルを使用して再度オンにすることができます。 **[!UICONTROL Enable Auto-include]** ダイアログが表示され、選択を確認するメッセージが表示されます。 **[!UICONTROL Enable]**&#x200B;を選択して、構成設定を完了します。

>[!NOTE]
>
>**[!UICONTROL Auto-include]**&#x200B;がオフになっている間にポリシーから削除したサンドボックスは、再度追加されます。

![有効化オプションがハイライト表示された自動含めるダイアログ。](../../images/ui/policies/policy-enable-auto-include.png){zoomable="yes"}

### ポリシーのサンドボックスを手動で選択 {#manually-select-sandboxes}

ポリシーにサンドボックスを手動で追加または削除するには、**[!UICONTROL Auto-include]** トグル **をオフにする必要があります。**

#### サンドボックスを追加

ポリシーにサンドボックスを追加するには、**[!UICONTROL Add Sandboxes]**&#x200B;を選択します。

![ サンドボックスを追加オプションがハイライト表示されたポリシーのワークスペース。](../../images/ui/policies/policy-add-sandboxes.png){zoomable="yes"}

**[!UICONTROL Add Sandboxes]** ダイアログが表示されます。 ポリシーに追加するサンドボックスを選択し、**[!UICONTROL Save]**&#x200B;を選択します。

![ サンドボックスを選択し、保存オプションを強調表示したサンドボックスを追加ダイアログ。](../../images/ui/policies/policy-add-sandboxes-select.png){zoomable="yes"}

>[!NOTE]
>
>使用可能なすべてのサンドボックスが既にポリシーに追加されている場合は、ダイアログ内に「ライブラリに何も含まれていません」というメッセージが表示されます。

#### サンドボックスの削除

ポリシーからサンドボックスを削除するには、リストから削除するサンドボックスを見つけて、**X** アイコンを選択します。

![ サンドボックスを削除するために、「x」がハイライト表示されたポリシーのサンドボックスリスト。](../../images/ui/policies/policy-remove-sandbox.png){zoomable="yes"}

確認ダイアログが表示されます。 ポリシーからサンドボックスの削除を完了するには、**[!UICONTROL Confirm]**&#x200B;を選択します。

![確認オプションがハイライト表示されたサンドボックスの確認ダイアログ。](../../images/ui/policies/policy-remove-sandbox-confirmation.png){zoomable="yes"}

## ポリシーのアクティベート {#activate-policy}

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about"
>title="ポリシーとは"
>abstract="ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを確立するステートメントです。どの組織にもデフォルトのポリシーがあります。このポリシーをアクティブ化して、ラベルに基づいて特定のオブジェクトへのアクセスの制御を開始する必要があります。リソースに適用されたラベルは、ラベルが一致する役割にユーザーが割り当てられていない限り、アクセスを拒否します。ポリシーを編集または削除できませんが、アクティブ化または非アクティブ化することはできます。"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/abac/permissions-ui/labels" text="ラベルの管理"

既存のポリシーをアクティブにするには、**[!UICONTROL Policies]**&#x200B;の「**[!UICONTROL Permissions]**」タブからポリシーを選択します。 ポリシーのアクティベーションステータスは、**[!UICONTROL Status]** セクションの下に表示されます。

![ ポリシーのステータスがハイライト表示されたポリシーワークスペース。](../../images/ui/policies/policy-status.png){zoomable="yes"}

ポリシーの詳細ワークスペースが表示されます。 **[!UICONTROL Activate]** を選択します。

![ アクティブ化オプションがハイライト表示されたポリシーの詳細ワークスペース。](../../images/ui/policies/policy-activate.png){zoomable="yes"}

**[!UICONTROL Activate Policy]** ダイアログが表示されます。 ポリシーのアクティブ化を完了するには、**[!UICONTROL Confirm]**&#x200B;を選択します。

![確認オプションがハイライト表示されたポリシーの有効化ダイアログ。](../../images/ui/policies/policy-activate-confirm.png){zoomable="yes"}

## 次の手順

ポリシーがアクティブ化されると、次の手順に進んで、役割[の権限を](permissions.md)管理できます。

<!--
Policies are applied at the sandbox level to control which sandboxes enforce label-based access control. By default, the **[!UICONTROL Auto-include]** feature is turned on, which means all current and future sandboxes are automatically added to the policy. When **[!UICONTROL Auto-include]** is turned off, only the sandboxes you manually add will be subject to the policy's access control rules.

To begin configuring a policy's sandboxes, navigate to **[!UICONTROL Permissions]** in [Adobe Experience Cloud](https://experience.adobe.com/){target="_blank"}. Select **[!UICONTROL Policies]** from the left panel, then select the **[!UICONTROL Default-Label-Based-Access-Control-Policy]** from the list.

The policy's details workspace appears. Select the **[!UICONTROL Sandboxes]** tab to view the list of sandboxes associated with the policy and access the sandbox configuration options.

### Manage Auto-include {#manage-auto-include}

To control which sandboxes are included in a policy, you can toggle the **[!UICONTROL Auto-include]** feature on or off. When you toggle off **[!UICONTROL Auto-include]**, future sandboxes will not be automatically added to the policy. However, toggling off the feature **will not** remove any sandboxes that are already included in the policy.

To re-enable **[!UICONTROL Auto-include]**, use the toggle to turn it back on. The **[!UICONTROL Enable Auto-include]** dialog appears prompting you to confirm your selection. Select **[!UICONTROL Enable]** to complete the configuration setting.

>[!NOTE]
>
>When you re-enable **[!UICONTROL Auto-include]**, any sandboxes you previously removed from the policy will be re-added.

### Manually manage sandboxes {#manually-manage-sandboxes}

When **[!UICONTROL Auto-include]**is turned off, you can manually add or remove specific sandboxes from the policy. This gives you precise control over which sandboxes enforce the policy's access control rules.

>[!NOTE]
>
>To manually add or remove sandboxes, the **[!UICONTROL Auto-include]** toggle **must** be off.

**To add sandboxes:**

Select **[!UICONTROL Add Sandboxes]** from the policy's sandbox workspace.

The **[!UICONTROL Add Sandboxes]** dialog appears, displaying your library of available sandboxes. Select the sandbox(es) you wish to add to the policy and then select **[!UICONTROL Save]**.

>[!NOTE]
>
>If all available sandboxes are already included in the policy, you will see a "You have nothing in your library" message within the dialog.

**To remove sandboxes:**

Find the sandbox you wish to remove from the list and select the **X** icon next to its name.

A confirmation dialog will appear. Select **[!UICONTROL Confirm]** to finish removing the sandbox from the policy.

-->
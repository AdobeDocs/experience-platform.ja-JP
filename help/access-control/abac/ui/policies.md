---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;ABAC
title: アクセス制御ポリシーの管理
description: Adobe Experience Cloudの権限インターフェイスを使用してアクセス制御ポリシーを管理します。
exl-id: 66820711-2db0-4621-908d-01187771de14
source-git-commit: 2a26c8786adc412dc643c8a0c94b966e439e034b
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 10%

---

# アクセス制御ポリシーの管理

アクセス制御ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを規定するステートメントです。 Adobeには、すぐにアクティブ化できる、または組織が [ ラベル ](./labels.md){target="_blank"} に基づいて特定のオブジェクトへのアクセスの制御を開始する準備ができたときにアクティブ化できるデフォルトのポリシーが用意されています。 デフォルトのポリシー **[!UICONTROL Default-Label-Based-Access-Control-Policy]** は、リソースに適用されるラベルを活用して、ラベルが一致する役割にユーザーがない限りアクセスを拒否します。

>[!IMPORTANT]
>
>アクセス制御ポリシーを、Adobe Experience Platformでのデータの使用方法を制御するデータ使用ポリシーと混同しないでください。 詳しくは、[ データ使用ポリシー ](../../../data-governance/policies/create.md){target="_blank"} の作成に関するガイドを参照してください。

## ポリシー用サンドボックスの設定 {#configure-policy}

ポリシーはサンドボックスレベルで適用され、ラベルベースのアクセス制御を適用するサンドボックスを制御します。 デフォルトでは、**[!UICONTROL Auto-include]** 機能がオンになっています。つまり、現在と将来のすべてのサンドボックスが自動的にポリシーに追加されます。 **[!UICONTROL Auto-include]** がオフの場合、手動で追加したサンドボックスのみが、ポリシーのアクセス制御規則の対象となります。

>[!NOTE]
>
>**[!UICONTROL Default-Label-Based-Access-Control-Policy]** ポリシーは現在、設定に使用できる唯一のポリシーです。

ポリシーのサンドボックスの設定を開始するには、**[!UICONTROL Permissions]** Adobe Experience Cloud[ の ](https://experience.adobe.com/){target="_blank"} に移動します。 左側のパネルから「**[!UICONTROL Policies]**」を選択し、リストから **[!UICONTROL Default-Label-Based-Access-Control-Policy]** を選択します。

![ 既存のポリシーのリストが表示されているポリシーワークスペース。](../../images/ui/policies/policies-home.png){zoomable="yes"}

ポリシーの詳細ワークスペースが表示されます。 「**[!UICONTROL Sandboxes]**」タブを選択して、ポリシーに関連付けられたサンドボックスのリストを表示し、サンドボックス設定オプションにアクセスします。

![ 関連付けられたサンドボックスのリストを表示する、ポリシーのサンドボックスワークスペース。](../../images/ui/policies/policy-sandbox.png){zoomable="yes"}

### 自動インクルードを管理 {#manage-auto-include}

>[!IMPORTANT]
>
>デフォルトでは、**[!UICONTROL Auto-include]** がオンになっています。つまり、現在と将来のすべてのサンドボックスが自動的にポリシーに追加されます。

ポリシーに含めるサンドボックスを制御するには、**[!UICONTROL Auto-include]** 機能のオンとオフを切り替えます。 **[!UICONTROL Auto-include]** をオフにすると、以降のサンドボックスは自動的にはポリシーに追加されません。 ただし、この機能をオフに切り替えると **ポリシーに既に含まれているサンドボックスは削除されません**。

![ 自動インクルードの切り替えがハイライト表示され、「オフ」状態のポリシーの「サンドボックス」タブ ](../../images/ui/policies/policy-auto-include.png){zoomable="yes"}

**[!UICONTROL Auto-include]** を再度有効にするには、切替スイッチを使用してオンに戻します。 選択を確認するように求める **[!UICONTROL Enable Auto-include]** ダイアログが表示されます。 「**[!UICONTROL Enable]**」を選択して、設定を完了します。

>[!NOTE]
>
>**[!UICONTROL Auto-include]** を再度有効にすると、以前にポリシーから削除したサンドボックスが再度追加されます。

![ 「有効」オプションがハイライト表示された自動インクルードダイアログを有効にする ](../../images/ui/policies/policy-enable-auto-include.png){zoomable="yes"}

### サンドボックスの手動管理 {#manually-manage-sandboxes}

**[!UICONTROL Auto-include]**がオフになっている場合、特定のサンドボックスをポリシーに手動で追加またはポリシーから削除できます。 これにより、ポリシーのアクセス制御規則を適用するサンドボックスを正確に制御できます。

>[!NOTE]
>
>サンドボックスを手動で追加または削除するには、「**[!UICONTROL Auto-include]**」切替スイッチ **必須** をオフにします。

**サンドボックスを追加するには：**

ポリシーのサンドボックスワークスペースから「**[!UICONTROL Add Sandboxes]**」を選択します。

![ 「サンドボックスを追加」オプションがハイライト表示されたポリシーのワークスペース。](../../images/ui/policies/policy-add-sandboxes.png){zoomable="yes"}

**[!UICONTROL Add Sandboxes]** ダイアログが表示され、使用可能なサンドボックスのライブラリが表示されます。 ポリシーに追加するサンドボックスを選択して、「**[!UICONTROL Save]**」を選択します。

![ サンドボックスが選択され、「保存」オプションがハイライト表示されたサンドボックスを追加ダイアログ ](../../images/ui/policies/policy-add-sandboxes-select.png){zoomable="yes"}

>[!NOTE]
>
>使用可能なすべてのサンドボックスが既にポリシーに含まれている場合は、ダイアログ内に「ライブラリに何もありません」というメッセージが表示されます。

**サンドボックスを削除するには：**

リストから削除するサンドボックスを見つけ、その名前の横にある **X** アイコンを選択します。

![ サンドボックスを削除するために「x」がハイライト表示されたポリシーのサンドボックスリスト ](../../images/ui/policies/policy-remove-sandbox.png){zoomable="yes"}

確認ダイアログが表示されます。 「**[!UICONTROL Confirm]**」を選択して、ポリシーからのサンドボックスの削除を完了します。

![ 「確認」オプションがハイライト表示されたサンドボックスの確認ダイアログ ](../../images/ui/policies/policy-remove-sandbox-confirmation.png){zoomable="yes"}

## ポリシーのアクティベート {#activate-policy}

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about"
>title="ポリシーとは"
>abstract="ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを確立するステートメントです。どの組織にもデフォルトのポリシーがあります。このポリシーをアクティブ化して、ラベルに基づいて特定のオブジェクトへのアクセスの制御を開始する必要があります。リソースに適用されたラベルは、ラベルが一致する役割にユーザーが割り当てられていない限り、アクセスを拒否します。ポリシーの編集や削除はできませんが、アクティブ化や非アクティブ化は可能です。"
>additional-url="https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/abac/permissions-ui/labels" text="ラベルの管理"

既存のポリシーをアクティブにするには、**[!UICONTROL Policies]** の「**[!UICONTROL Permissions]**」タブからポリシーを選択します。 ポリシーのアクティベーションステータスは、「アクティベー **[!UICONTROL Status]**」セクションに表示されます。

![ ポリシーのステータスがハイライト表示されたポリシーワークスペース。](../../images/ui/policies/policy-status.png){zoomable="yes"}

ポリシーの詳細ワークスペースが表示されます。 **[!UICONTROL Activate]** を選択します。

![ 「アクティブ化」オプションがハイライト表示されたポリシーの詳細ワークスペース。](../../images/ui/policies/policy-activate.png){zoomable="yes"}

**[!UICONTROL Activate Policy]** ダイアログが表示されます。 「**[!UICONTROL Confirm]**」を選択して、ポリシーのアクティブ化を完了します。

![ 「確認」オプションがハイライト表示されたポリシーをアクティブ化ダイアログ ](../../images/ui/policies/policy-activate-confirm.png){zoomable="yes"}

## 次の手順

ポリシーをアクティブにすると、次の手順 [ 役割の権限の管理 ](permissions.md) に進むことができます。

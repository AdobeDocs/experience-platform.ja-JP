---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;
title: 属性ベースのアクセス制御エンドツーエンドガイド
description: このドキュメントでは、Adobe Experience Platformの属性ベースのアクセス制御に関するエンドツーエンドガイドを提供します
role: Developer
exl-id: 7e363adc-628c-4a66-a3bd-b5b898292394
source-git-commit: c89ae9befa3befbffab9d6468f3c207ab8e7b74f
workflow-type: tm+mt
source-wordcount: '1736'
ht-degree: 24%

---

# 属性ベースのアクセス制御エンドツーエンドガイド

Adobe Experience Platformの属性ベースのアクセス制御を使用すると、自分自身や他のマルチブランドのプライバシーを重視するお客様に、ユーザーアクセスをより柔軟に管理できます。 スキーマフィールドやセグメントなどの個々のオブジェクトへのアクセスは、オブジェクトの属性と役割に基づくポリシーで許可できます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

この機能を使用すると、スキーマフィールドやセグメントなどを、組織またはデータの使用範囲を定義するラベルで分類できます。 Adobe Journey Optimizerのジャーニー、オファーおよびその他のオブジェクトに同じラベルを適用できます。 同時に、管理者は、エクスペリエンスデータモデル（XDM）スキーマフィールドに関するアクセスポリシーを定義し、それらのフィールドにアクセスできるユーザーまたはグループ（内部、外部、またはサードパーティのユーザー）をより適切に管理できます。

>[!NOTE]
>
>このドキュメントでは、アクセス制御ポリシーのユースケースに焦点を当てています。 を管理するポリシーを設定しようとしている場合 **use** どの Platform ユーザーがアクセスするかではなく、データの詳細については、に関するエンドツーエンドガイドを参照してください。 [データガバナンス](../../data-governance/e2e.md) その代わり。

## はじめに

このチュートリアルでは、次の Platform コンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)] システム](../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)：[!DNL Platform] 内のセグメント化エンジンで、顧客の行動と属性に基づいて顧客プロファイルからオーディエンスセグメントを作成するのに使用されます。

### ユースケースの概要

属性ベースのアクセス制御ワークフローの例について説明します。このワークフローでは、役割、ラベル、ポリシーを作成して割り当て、ユーザーが組織内の特定のリソースにアクセスできるかどうかを設定します。 このガイドでは、機密データへのアクセスを制限する例を使用して、ワークフローを示します。 このユースケースの概要を次に示します。

医療提供者で、組織内のリソースへのアクセスを設定したい場合。

* 社内マーケティングチームから、 **[!UICONTROL PHI/規制医療データ]** データ。
* 外部エージェンシーはにアクセスできません **[!UICONTROL PHI/規制医療データ]** データ。

そのためには、役割、リソース、ポリシーを設定する必要があります。

以下を行います。

* [ユーザーの役割のラベル付け](#label-roles)：マーケティンググループが外部の代理店と連携するヘルスケアプロバイダー（ACME ビジネスグループ）の例を使用します。
* [リソースのラベル付け（スキーマフィールドとセグメント）](#label-resources)：を割り当てます **[!UICONTROL PHI/規制医療データ]** スキーマのリソースとセグメントにラベルを付けます。
* 
   * [相互にリンクするポリシーをアクティベートします。](#policy)：デフォルトのポリシーを有効にして、リソースのラベルを役割のラベルに接続することでスキーマフィールドおよびセグメントへのアクセスを防ぎます。 ラベルが一致するユーザーには、すべてのサンドボックスでスキーマフィールドとセグメントにアクセスできます。

## 権限

[!UICONTROL 権限] は、管理者がユーザーロールとポリシーを定義して、プロダクトアプリケーション内の機能とオブジェクトのパーミッションを管理できるExperience Cloud領域です。

から [!UICONTROL 権限]を使用すると、役割を作成および管理し、それらの役割に対して必要なリソース権限を割り当てることができます。 [!UICONTROL 権限] また、では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。

管理者権限がない場合は、システム管理者に連絡してアクセス権を取得してください。

管理者権限があれば、に移動します。 [Adobe Experience Cloud](https://experience.adobe.com/) Adobe資格情報を使用してログインします。 ログイン後、 **[!UICONTROL 概要]** ページが、管理者権限を持つ組織に対して表示されます。 このページには、組織が購読している製品と、組織にユーザーと管理者を追加するための他のコントロールが表示されます。 を選択 **[!UICONTROL 権限]** をクリックして、Platform 統合用のワークスペースを開きます。

![Adobe Experience Cloudで「権限」製品が選択されている様子を示す画像](../images/flac-ui/flac-select-product.png)

Platform UI の権限ワークスペースが表示され、で開きます **[!UICONTROL 役割]** ページ。

## 役割にラベルを適用 {#label-roles}

>[!CONTEXTUALHELP]
>id="platform_permissions_labels_about"
>title="ラベルとは"
>abstract="ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。Platform には、アドビ定義の「コア」データ使用状況ラベルがいくつか用意されています。これらは、データガバナンスに適用できる様々な一般的制限に対応しています。例えば、RHD (規制医療データ) などの、機密を意味する「S」ラベルを使用すると、保護された医療情報 (PHI) を参照するデータを分類できます。また、組織のニーズに合わせて独自のカスタムラベルを定義することもできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-governance/labels/overview.html?lang=ja#understanding-data-usage-labels" text="データ使用ラベルの概要"

>[!CONTEXTUALHELP]
>id="platform_permissions_labels_about_create"
>title="新しいラベルの作成"
>abstract="組織のニーズに合わせて独自のカスタムラベルを作成できます。カスタムラベルを使用して、データガバナンスとアクセス制御の両方の設定をデータに適用できます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-governance/labels/overview.html?lang=ja#manage-labels" text="カスタムラベルの管理"

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_about"
>title="役割とは"
>abstract="役割は、Platform インスタンスとやり取りするユーザーのタイプを分類する方法で、アクセス制御ポリシーの構成要素です。 役割には特定の権限セットがあり、必要な表示または書き込みアクセスの範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/roles.html?lang=ja" text="役割の管理"

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_about_create"
>title="新しい役割の作成"
>abstract="新しい役割を作成するいことで、Platform インスタンスにアクセスするユーザーをより適切に分類することができます。例えば、社内マーケティングチーム用の役割を作成して、その役割に RHD ラベルを適用することで、社内マーケティングチームが保護された医療情報 (PHI) にアクセスできるようになります。また、外部エージェンシー用の役割を作成して、その役割に RHD ラベルを適用しないことで、医療情報 (PHI) データにアクセスするのを拒否することもできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/roles.html?lang=ja#create-a-new-role" text="新しい役割の作成"

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_details"
>title="役割の概要"
>abstract="役割の概要ダイアログには、その役割へのアクセスが許可されているリソースとサンドボックスが表示されます。"

役割は、Platform インスタンスとやり取りするユーザーのタイプを分類する方法で、アクセス制御ポリシーの構成要素です。 役割には特定の権限セットが付与され、必要なアクセス範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。

開始するには、を選択します **[!UICONTROL ACME ビジネス グループ]** から **[!UICONTROL 役割]** ページ。

![役割で「ACME ビジネスロール」が選択されている様子を示す画像](../images/abac-end-to-end-user-guide/abac-select-role.png)

次に、を選択します **[!UICONTROL ラベル]** を選択してから、 **[!UICONTROL ラベルを追加]**.

![「ラベル」タブで「ラベルを追加」が選択されている様子を示す画像](../images/abac-end-to-end-user-guide/abac-select-add-labels.png)

組織のすべてのラベルのリストが表示されます。 を選択 **[!UICONTROL RHD]** ラベルを追加： **[!UICONTROL PHI/規制医療データ]**. ラベルの横に青いチェックマークが表示されるまでしばらく待ってから、 **[!UICONTROL 保存]**.

![RHD ラベルが選択および保存されていることを示す画像](../images/abac-end-to-end-user-guide/abac-select-role-label.png)

>[!NOTE]
>
>組織グループを役割に追加すると、そのグループ内のすべてのユーザーが役割に追加されます。 組織グループに対する変更（削除または追加されたユーザー）は、役割内で自動的に更新されます。

## スキーマフィールドへのラベルの適用 {#label-resources}

これで、のユーザーロールが設定されました [!UICONTROL RHD] ラベル。次の手順では、その役割で制御するリソースに同じラベルを追加します。

を選択 **[!UICONTROL スキーマ]** 左側のナビゲーションからを選択し、 **[!UICONTROL ACME ヘルスケア]** 表示されるスキーマのリストから。

![「スキーマ」タブから ACME Healthcare スキーマが選択されている様子を示す画像](../images/abac-end-to-end-user-guide/abac-select-schema.png)

次に、を選択します **[!UICONTROL ラベル]** をクリックして、スキーマに関連付けられているフィールドを表示するリストを表示します。 ここから、1 つまたは複数のフィールドに一度にラベルを割り当てることができます。 「」を選択します **[!UICONTROL 血糖]** および **[!UICONTROL InsulinLevel]** フィールドを選択してから、 **[!UICONTROL アクセスラベルとデータガバナンスラベルを適用]**.

![血糖値とインスリンレベルが選択され、アクセスラベルとデータガバナンスラベルが選択されていることを示す画像](../images/abac-end-to-end-user-guide/abac-select-schema-labels-tab.png)

この **[!UICONTROL ラベルを編集]** ダイアログが表示され、スキーマフィールドに適用するラベルを選択できます。 このユースケースでは、 **[!UICONTROL PHI/規制医療データ]** ラベルを付け、次を選択します **[!UICONTROL 保存]**.

![RHD ラベルが選択および保存されていることを示す画像](../images/abac-end-to-end-user-guide/abac-select-schema-labels.png)

>[!NOTE]
>
>ラベルがフィールドに追加されると、そのラベルは、そのフィールドの親リソース（クラスまたはフィールドグループ）に適用されます。 親クラスまたはフィールドグループが他のスキーマで使用されている場合、それらのスキーマは同じラベルを継承します。

## セグメントへのラベルの適用

>[!NOTE]
>
>同じアクセス制限を適用する場合、ラベル付き属性を利用するセグメントにも同じようにラベルを付ける必要があります。

スキーマフィールドのラベル設定が完了したら、セグメントのラベル設定を開始できます。

を選択 **[!UICONTROL セグメント]** 左側のナビゲーションから。 組織で使用可能なセグメントのリストが表示されます。 この例では、次の 2 つのセグメントに機密ヘルスデータが含まれているため、ラベルが付けられます。

* 血糖値が 100 を超える
* インスリン 50 未満

を選択 **[!UICONTROL 血糖値が 100 を超える]** をクリックして、セグメントのラベル付けを開始します。

![「セグメント」タブから血糖値が 100 を超えて選択されている様子を示す画像](../images/abac-end-to-end-user-guide/abac-select-segment.png)

セグメント **[!UICONTROL 詳細]** 画面が表示されます。 を選択 **[!UICONTROL アクセスを管理]**.

![管理アクセスの選択を示す画像](../images/abac-end-to-end-user-guide/abac-segment-fields-manage-access.png)

この **[!UICONTROL ラベルを編集]** ダイアログが表示され、セグメントに適用するラベルを選択できます。 このユースケースでは、 **[!UICONTROL PHI/規制医療データ]** ラベルを付け、次を選択します **[!UICONTROL 保存]**.

![RHD ラベルの選択と保存が選択されていることを示す画像](../images/abac-end-to-end-user-guide/abac-select-segment-labels.png)

で上記の手順を繰り返します **[!UICONTROL インスリン 50 未満]**.

## アクセス制御ポリシーのアクティブ化 {#policy}

デフォルトのアクセス制御ポリシーは、ラベルを活用して、特定の Platform リソースにアクセスできるユーザーの役割を定義します。 この例では、スキーマフィールドに対応するラベルを持つ役割に属していないユーザーに対しては、すべてのサンドボックスでスキーマフィールドおよびセグメントへのアクセスが拒否されます。

アクセス制御ポリシーをアクティブにするには、以下を選択します [!UICONTROL 権限] 左側のナビゲーションからを選択し、 **[!UICONTROL ポリシー]**.

![表示されたポリシーのリスト](../images/abac-end-to-end-user-guide/abac-policies-page.png)

次に、省略記号（`...`）、ポリシー名の横にあるドロップダウンに、役割を編集、アクティブ化、削除または複製するためのコントロールが表示されます。 を選択 **[!UICONTROL Activate]** ドロップダウンから。

![ポリシーをアクティブ化するドロップダウン](../images/abac-end-to-end-user-guide/abac-policies-activate.png)

ポリシーのアクティブ化ダイアログが表示され、アクティベーションを確認するプロンプトが表示されます。 を選択 **[!UICONTROL 確認]**.

![ポリシーダイアログをアクティベート](../images/abac-end-to-end-user-guide/abac-activate-policies-dialog.png)

ポリシーのアクティベーションの確認を受信し、に戻ります [!UICONTROL ポリシー] ページ。

![ポリシーの有効化の確認](../images/abac-end-to-end-user-guide/abac-policies-confirm-activate.png)

<!-- ## Create an access control policy {#policy}

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about"
>title="What are policies?"
>abstract="Policies are statements that bring attributes together to establish permissible and impermissible actions. Every organization comes with a default policy that you must activate to define rules for resources like segments and schema fields. Default policies can neither be edited nor deleted. However, default policies can be activated or deactivated."
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/policies.html" text="Manage policies"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about_create"
>title="Create a policy"
>abstract="Create a policy to define the actions that your users can and cannot take against your segments and schema fields."
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/policies.html#create-a-new-policy" text="Create a policy"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_edit_permitdeny"
>title="Configure permissible and impermissible actions for a policy"
>abstract="A <b>deny access to</b> policy will deny users access when the criteria is met. Combined with <b>The following being false</b> - all users will be denied access unless they meet the matching criteria set. This type of policy allows you to protect a sensitive resource and only allow access to users with matching labels. <br>A <b>permit access to</b> policy will permit users access when the criteria are met. When combined with <b>The following being true</b> - users will be given access if they meet the matching criteria set. This does not explicitly deny access to users, but adds a permit access. This type of policy allows you to give additional access to resource and in addition to those users who might already have access through role permissions."
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/policies.html#edit-a-policy" text="Edit a policy"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_edit_resource"
>title="Configure permissions for a resource"
>abstract="A resource is the asset or object that a user can or cannot access. Resources can be segments or schemas fields. You can configure write, read, or delete permissions for segments and schema fields."

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_edit_condition"
>title="Edit conditions"
>abstract="Apply conditional statements to your policy to configure user access to certain resources. Select match all to require users to have roles with the same labels as a resource to be permitted access. Select match any to require users to have a role with just one label matching a label on a resource. Labels can either be defined as core or custom labels, with core labels representing labels created and provided by Adobe and custom labels representing labels that you created for your organization."

Access control policies leverage labels to define which user roles have access to specific Platform resources. Policies can either be local or global and can override other policies. In this example, access to schema fields and segments will be denied in all sandboxes for users who don't have the corresponding labels in the schema field.

>[!NOTE]
>
>A "deny policy" is created to grant access to sensitive resources because the role grants permission to the subjects. The written policy in this example **denies** you access if you are missing the required labels.

To create an access control policy, select **[!UICONTROL Permissions]** from the left navigation and then select **[!UICONTROL Policies]**. Next, select **[!UICONTROL Create policy]**.

![Image showing Create policy being selected in the Permissions](../images/abac-end-to-end-user-guide/abac-create-policy.png)

The **[!UICONTROL Create new policy]** dialog appears, prompting you to enter a name and an optional description. Select **[!UICONTROL Confirm]** when finished.

![Image showing the Create new policy dialog and selecting Confirm](../images/abac-end-to-end-user-guide/abac-create-policy-details.png)

To deny access to the schema fields, use the dropdown arrow and select **[!UICONTROL Deny access to]** and then select **[!UICONTROL No resource selected]**. Next, select **[!UICONTROL Schema Field]** and then select **[!UICONTROL All]**.

![Image showing Deny access and resources selected](../images/abac-end-to-end-user-guide/abac-create-policy-deny-access-schema.png)

The table below shows the conditions available when creating a policy:

| Conditions | Description |
| --- | --- |
| The following being false| When 'Deny access to' is set, access will be restricted if the user does not meet the criteria selected. |
| The following being true| When 'Permit access to' is set, access will be permitted if the user meets the selected criteria. |
| Matches any| The user has a label that matches any label applied to a resource. |
| Matches all| The user has all labels that matches all labels applied to a resource. |
| Core label| A core label is an Adobe-defined label that is available in all Platform instances.|
| Custom label| A custom label is a label that has been created by your organization.|

Select **[!UICONTROL The following being false]** and then select **[!UICONTROL No attribute selected]**. Next, select the user **[!UICONTROL Core label]**, then select **[!UICONTROL Matches all]**. Select the resource **[!UICONTROL Core label]** and finally select **[!UICONTROL Add resource]**.

![Image showing the conditions being selected and Add resource being selected](../images/abac-end-to-end-user-guide/abac-create-policy-deny-access-schema-expression.png)

>[!TIP]
>
>A resource is the asset or object that a subject can or cannot access. Resources can be segments or schemas.

To deny access to the segments, use the dropdown arrow and select **[!UICONTROL Deny access to]** and then select **[!UICONTROL No resource selected]**. Next, select **[!UICONTROL Segment]** and then select **[!UICONTROL All]**.

Select **[!UICONTROL The following being false]** and then select **[!UICONTROL No attribute selected]**. Next, select the user **[!UICONTROL Core label]**, then select **[!UICONTROL Matches all]**. Select the resource **[!UICONTROL Core label]** and finally select **[!UICONTROL Save]**.

![Image showing conditions selected and Save being selected](../images/abac-end-to-end-user-guide/abac-create-policy-deny-access-segment.png)

Select **[!UICONTROL Activate]** to activate the policy, and a dialog appears which prompts you to confirm activation. Select **[!UICONTROL Confirm]** and then select **[!UICONTROL Close]**.

![Image showing the Policy being activated ](../images/abac-end-to-end-user-guide/abac-create-policy-activation.png) -->

## 次の手順

役割、スキーマフィールド、セグメントへのラベルの適用が完了しました。 これらの役割に割り当てられる外部エージェンシーは、これらのラベルとその値をスキーマ、データセットおよびプロファイルビューで表示することが制限されています。 また、これらのフィールドは、セグメントビルダーを使用する場合、セグメント定義で使用することも制限されています。

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](./overview.md)を参照してください。

次のビデオは、属性ベースのアクセス制御についての理解を深めるために、また、役割、リソース、ポリシーを設定する方法の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/345641?learn=on)

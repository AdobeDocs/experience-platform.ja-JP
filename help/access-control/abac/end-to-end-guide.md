---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;
title: 属性ベースのアクセス制御エンドツーエンドガイド
description: このドキュメントでは、Adobe Experience Platformの属性ベースのアクセス制御に関するエンドツーエンドガイドを提供します
role: Developer
exl-id: 7e363adc-628c-4a66-a3bd-b5b898292394
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 12%

---

# 属性ベースのアクセス制御エンドツーエンドガイド

Adobe Experience Platformの属性ベースのアクセス制御を使用すると、自分自身や他のマルチブランドのプライバシーを重視するお客様に、ユーザーアクセスをより柔軟に管理できます。 スキーマフィールドやオーディエンスなどの個々のオブジェクトへのアクセスは、オブジェクトの属性と役割に基づくポリシーで許可できます。 この機能を使用すると、組織内の特定のExperience Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

この機能を使用すると、スキーマフィールドやオーディエンスなどを、組織またはデータの使用範囲を定義するラベルで分類できます。 Adobe Journey Optimizerのジャーニー、オファーおよびその他のオブジェクトに同じラベルを適用できます。 同時に、管理者は、エクスペリエンスデータモデル（XDM）スキーマフィールドに関するアクセスポリシーを定義し、それらのフィールドにアクセスできるユーザーまたはグループ（内部、外部、またはサードパーティのユーザー）をより適切に管理できます。

>[!NOTE]
>
>このドキュメントでは、アクセス制御ポリシーのユースケースに焦点を当てています。 データへのアクセス権を持つExperience Platform ユーザーではなく、データの **使用** を管理するポリシーを設定しようとしている場合は、代わりに [ データガバナンス ](../../data-governance/e2e.md) に関するエンドツーエンドガイドを参照してください。

## はじめに

このチュートリアルでは、次のExperience Platform コンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)] システム](../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)：[!DNL Experience Platform] 内のセグメント化エンジンで、顧客の行動と属性に基づいて顧客プロファイルからオーディエンスセグメントを作成するのに使用されます。

### ユースケースの概要

属性ベースのアクセス制御ワークフローの例について説明します。このワークフローでは、役割、ラベル、ポリシーを作成して割り当て、ユーザーが組織内の特定のリソースにアクセスできるかどうかを設定します。 このガイドでは、機密データへのアクセスを制限する例を使用して、ワークフローを示します。 このユースケースの概要を次に示します。

医療提供者で、組織内のリソースへのアクセスを設定したい場合。

* 社内のマーケティングチームは、**[!UICONTROL PHI/ Regulated Health Data]** のデータにアクセスできる必要があります。
* 外部エージェンシーは、**[!UICONTROL PHI/ Regulated Health Data]** のデータにアクセスできません。

そのためには、役割、リソース、ポリシーを設定する必要があります。

以下を行います。

* [ ユーザーの役割のラベル付け ](#label-roles)：マーケティンググループが外部の代理店と連携するヘルスケアプロバイダー（ACME ビジネスグループ）の例を使用します。
* [ リソースにラベルを付ける（スキーマフィールドとオーディエンス） ](#label-resources)：スキーマリソースとオーディエンスに **[!UICONTROL PHI/ Regulated Health Data]** ラベルを割り当てます。
* [ それらをリンクするポリシーをアクティブ化 ](#policy): リソースのラベルを役割のラベルに接続することで、スキーマフィールドやオーディエンスにアクセスできないようにするデフォルトポリシーを有効にします。 ラベルが一致するユーザーには、すべてのサンドボックスでスキーマフィールドとセグメントにアクセスできます。

## 権限

[!UICONTROL Permissions] は、管理者がユーザーロールとポリシーを定義して、製品アプリケーション内の機能とオブジェクトの権限を管理できる、Experience Cloudの領域です。

[!UICONTROL Permissions] を使用すると、役割を作成および管理し、それらの役割に対して必要なリソース権限を割り当てることができます。 [!UICONTROL Permissions] た、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。

管理者権限がない場合は、システム管理者に連絡してアクセス権を取得してください。

管理者権限があれば、[Adobe Experience Cloud](https://experience.adobe.com/) に移動し、Adobeの資格情報を使用してログインします。 ログインすると、管理者権限を持つ組織の **[!UICONTROL Overview]** ページが表示されます。 このページには、組織が購読している製品と、組織にユーザーと管理者を追加するための他のコントロールが表示されます。 「**[!UICONTROL Permissions]**」を選択して、Experience Platform統合用のワークスペースを開きます。

![Adobe Experience Cloudで「権限」製品が選択されている様子を示す画像 ](../images/flac-ui/flac-select-product.png)

Experience Platform UI の権限ワークスペースが表示され、**[!UICONTROL Overview]** ページが開きます。

## 役割へのラベルの適用 {#label-roles}

>[!CONTEXTUALHELP]
>id="platform_permissions_labels_about"
>title="ラベルとは？"
>abstract="ラベルを使用すると、そのデータに適用される使用状況ポリシーおよびアクセスポリシーに従ってデータセットとフィールドを分類できます。Adobe Experience Platform には、アドビ定義の<strong>コア</strong>データ使用状況ラベルがいくつか用意されています。これらは、データガバナンスに適用できる様々な一般的制限に対応しています。例えば、RHD（規制医療データ）などの機密ラベルを意味する <strong>S</strong> ラベルを使用すると、保護された医療情報（PHI）を参照するデータを分類できます。また、組織のニーズに合わせて独自のカスタムラベルを定義することもできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-governance/labels/overview.html?lang=ja#understanding-data-usage-labels" text="データ使用ラベルの概要"

ロールは、Experience Platform インスタンスとやり取りするユーザーのタイプを分類する手段であり、アクセス制御ポリシーの構成要素です。 役割には特定の権限セットが付与され、必要なアクセス範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。

開始するには、左側のナビゲーションから「**[!UICONTROL Roles]**」を選択し、「**[!UICONTROL ACME Business Group]**」を選択します。

![ 役割で ACME ビジネスグループが選択されている様子を示す画像 ](../images/abac-end-to-end-user-guide/abac-select-role.png)

次に、「**[!UICONTROL Labels]**」を選択し、次に「**[!UICONTROL Add Labels]**」を選択します。

![ 「ラベル」タブで「ラベルを追加」が選択されている様子を示す画像 ](../images/abac-end-to-end-user-guide/abac-select-add-labels.png)

組織のすべてのラベルのリストが表示されます。 **[!UICONTROL RHD]** を選択して **[!UICONTROL PHI/Regulated Health Data]** のラベルを追加し、**[!UICONTROL Save]** を選択します。

![RHD ラベルが選択され、保存されていることを示す画像 ](../images/abac-end-to-end-user-guide/abac-select-role-label.png)

>[!NOTE]
>
>組織グループを役割に追加すると、そのグループ内のすべてのユーザーが役割に追加されます。 組織グループに対する変更（削除または追加されたユーザー）は、役割内で自動的に更新されます。

## スキーマフィールドへのラベルの適用 {#label-resources}

[!UICONTROL RHD] ラベルを使用してユーザーの役割を設定したので、次の手順は、その役割を制御するリソースに同じラベルを追加することです。

上部ナビゲーションから、「**アプリケーションスイッチャー**」アイコンで表された「![ アプリケーションスイッチャー ](/help/images/icons/apps.png)」を選択し、「**[!UICONTROL Experience Platform]**」を選択します。

![ アプリケーションスイッチャーのドロップダウンメニューからExperience Platformが選択されている様子を示す画像 ](../images/abac-end-to-end-user-guide/abac-select-experience-platform.png)

左側のナビゲーションから「**[!UICONTROL Schemas]**」を選択し、表示されるスキーマのリストから「**[!UICONTROL ACME Healthcare]**」を選択します。

![ 「スキーマ」タブから ACME Healthcare スキーマが選択されている様子を示す画像 ](../images/abac-end-to-end-user-guide/abac-select-schema.png)

次に、「**[!UICONTROL Labels]**」を選択して、スキーマに関連付けられているフィールドを表示するリストを表示します。 ここから、1 つまたは複数のフィールドに一度にラベルを割り当てることができます。 「**[!UICONTROL BloodGlucose]**」フィールドと「**[!UICONTROL InsulinLevel]**」フィールドを選択し、「**[!UICONTROL Apply access and data governance labels]**」を選択します。

![ 血糖値とインスリンレベルが選択され、アクセスラベルとデータガバナンスラベルを適用が選択されていることを示す画像 ](../images/abac-end-to-end-user-guide/abac-select-schema-labels-tab.png)

**[!UICONTROL Edit labels]** ダイアログが表示され、スキーマフィールドに適用するラベルを選択できます。 このユースケースでは、**[!UICONTROL PHI/ Regulated Health Data]** ラベルを選択してから「**[!UICONTROL Save]**」を選択します。

![RHD ラベルが選択され、保存されていることを示す画像 ](../images/abac-end-to-end-user-guide/abac-select-schema-labels.png)

>[!NOTE]
>
>ラベルがフィールドに追加されると、そのラベルは、そのフィールドの親リソース（クラスまたはフィールドグループ）に適用されます。 親クラスまたはフィールドグループが他のスキーマで使用されている場合、それらのスキーマは同じラベルを継承します。

## オーディエンスへのラベルの適用

>[!NOTE]
>
>同じアクセス制限を適用する場合、ラベル付き属性を利用するオーディエンスにも同じようにラベルを付ける必要があります。

スキーマフィールドのラベル設定が完了したら、オーディエンスのラベル設定を開始できます。

「**[!UICONTROL Audiences]**」セクションの下にある左側のナビゲーションから「**[!UICONTROL Customers]**」を選択します。 組織で使用可能なオーディエンスのリストが表示されます。 この例では、次の 2 つのオーディエンスに機密ヘルスデータが含まれているので、ラベルが付けられます。

* 血糖値が 100 を超える
* インスリン 50 未満

（チェックボックスではなく、オーディエンス名に基づいて） **[!UICONTROL Blood Glucose >100]** を選択し、オーディエンスのラベル付けを開始します。

![ 「オーディエンス」タブから血糖値が 100 を超えて選択されている様子を示す画像 ](../images/abac-end-to-end-user-guide/abac-select-audience.png)

セグメント **[!UICONTROL Details]** 画面が表示されます。 **[!UICONTROL Manage Access]** を選択します。

![ 管理アクセスの選択を示す画像 ](../images/abac-end-to-end-user-guide/abac-audience-fields-manage-access.png)

**[!UICONTROL Apply access and data governance labels]** ダイアログが表示され、オーディエンスに適用するラベルを選択できます。 このユースケースでは、**[!UICONTROL PHI/ Regulated Health Data]** ラベルを選択してから「**[!UICONTROL Save]**」を選択します。

![RHD ラベルの選択と保存が選択されていることを示す画像 ](../images/abac-end-to-end-user-guide/abac-select-audience-labels.png)

**[!UICONTROL Insulin <50]** で上記の手順を繰り返します。

>[!NOTE]
>
> [!UICONTROL Permissions] オブジェクトレベルのアクセス制御 [」を使用して、](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/access-control/object-based-access) ワークスペースで作成したラベル（上記のセグメントラベルなど）をAdobe Journey Optimizerの様々なオブジェクトに割り当てます。

## アクセス制御ポリシーのアクティブ化 {#policy}

デフォルトのアクセス制御ポリシーでは、ラベルを活用して、特定のExperience Platform リソースにアクセスできるユーザーの役割を定義します。 この例では、スキーマフィールドに対応するラベルを持つ役割に属していないユーザーに対しては、すべてのサンドボックスでスキーマフィールドおよびオーディエンスへのアクセスが拒否されます。

アクセス制御ポリシーをアクティブにするには、左側のナビゲーションから「[!UICONTROL Permissions]」を選択し、次に「**[!UICONTROL Policies]**」を選択します。

![ 表示されたポリシーのリスト ](../images/abac-end-to-end-user-guide/abac-policies-page.png)

次に、`...` ールの横にある省略記号（**[!UICONTROL Default-Field-Level-Access-Control-Policy]**）を選択すると、役割を編集、アクティブ化、削除または複製するためのコントロールがドロップダウンに表示されます。 ドロップダウンから「**[!UICONTROL Activate]**」を選択します。

![ ポリシーをアクティブ化するためのドロップダウン ](../images/abac-end-to-end-user-guide/abac-policies-activate.png)

ポリシーのアクティブ化ダイアログが表示され、アクティベーションを確認するプロンプトが表示されます。 **[!UICONTROL Confirm]** を選択します。

![ ポリシーをアクティベートダイアログ ](../images/abac-end-to-end-user-guide/abac-activate-policies-dialog.png)

ポリシーのアクティベーションの確認を受け取り、[!UICONTROL Policies] のページに戻ります。

![ ポリシーのアクティブ化の確認 ](../images/abac-end-to-end-user-guide/abac-policies-confirm-activate.png)

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

Access control policies leverage labels to define which user roles have access to specific Experience Platform resources. Policies can either be local or global and can override other policies. In this example, access to schema fields and segments will be denied in all sandboxes for users who don't have the corresponding labels in the schema field.

>[!NOTE]
>
>A "deny policy" is created to grant access to sensitive resources because the role grants permission to the subjects. The written policy in this example **denies** you access if you are missing the required labels.
a
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
| Core label| A core label is an Adobe-defined label that is available in all Experience Platform instances.|
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

![Image showing the Policy being activated](../images/abac-end-to-end-user-guide/abac-create-policy-activation.png) -->

## 次の手順

これで、役割、スキーマフィールド、オーディエンスに対するラベルの適用が完了しました。 これらの役割に割り当てられる外部エージェンシーは、これらのラベルとその値をスキーマ、データセットおよびプロファイルビューで表示することが制限されています。 また、これらのフィールドは、セグメントビルダーを使用する場合、セグメント定義で使用することも制限されています。

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](./overview.md)を参照してください。

次のビデオは、属性ベースのアクセス制御についての理解を深めるために、また、役割、リソース、ポリシーを設定する方法の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/345641?learn=on)

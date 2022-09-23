---
keywords: Experience Platform；ホーム；人気のトピック；アクセス制御；属性ベースのアクセス制御；
title: 属性ベースのアクセス制御エンドツーエンドガイド
description: このドキュメントでは、Adobe Experience Platformの属性ベースのアクセス制御に関するエンドツーエンドのガイドを提供します
hide: true
hidefromtoc: true
source-git-commit: 230bcfdb92c3fbacf2e24e7210d61e2dbe0beb86
workflow-type: tm+mt
source-wordcount: '2315'
ht-degree: 7%

---

# 属性ベースのアクセス制御エンドツーエンドガイド

属性ベースのアクセス制御は、プライバシーを意識するブランドがユーザーアクセスを柔軟に管理できるAdobe Experience Platformの機能です。 スキーマフィールドやセグメントなどの個々のオブジェクトを、ユーザーの役割に割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

この機能を使用すると、スキーマフィールドやセグメントなどを、組織やデータの使用範囲を定義するラベルで分類できます。 Adobe Journey Optimizerでは、ジャーニーとオファーに同じラベルを適用できます。 同時に、管理者は、XDM スキーマフィールドに関するアクセスポリシーを定義し、それらのフィールドにアクセスできるユーザーやグループ（内部、外部、サードパーティのユーザー）をより適切に管理できます。

## はじめに

このチュートリアルでは、次の Platform コンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM)] システム](../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [Adobe Experience Platform セグメント化サービス](../../segmentation/home.md)：[!DNL Platform] 内のセグメントエンジンは、顧客の行動と属性に基づいて、顧客プロファイルからオーディエンスセグメントを作成するのに使用されます。

### 使用例の概要

このガイドでは、機密データへのアクセスを制限してワークフローを示す使用例を使用します。 属性ベースのアクセス制御ワークフローの例を使用して、ロール、ラベル、ポリシーを作成および割り当て、ユーザーが組織内の特定のリソースにアクセスできるかどうかを設定します。 この使用例を次に示します。

医療機関であり、組織内のリソースへのアクセスを設定する場合。

* 社内のマーケティングチームが **[!UICONTROL PHI/規制対象の健康データ]** データ。
* 外部の代理店がにアクセスできない **[!UICONTROL PHI/規制対象の健康データ]** データ。

これをおこなうには、役割、リソース、ポリシーを設定する必要があります。

以下をおこないます。

* [ユーザーの役割のラベル付け](#label-roles):外部の代理店と連携するマーケティンググループを持つ医療プロバイダー（ACME ビジネスグループ）の例を使用します。
* [リソース（スキーマフィールドとセグメント）のラベル付け](#label-resources):を **[!UICONTROL PHI/規制対象の健康データ]** スキーマのリソースおよびセグメントに対するラベル
* [両者をリンクさせるポリシーを作成する](#policy):ポリシーを作成して、リソースのラベルを、スキーマフィールドおよびセグメントへのアクセスを拒否する役割のラベルにリンクします。 これにより、一致するラベルを持たないユーザーのすべてのサンドボックスのスキーマフィールドおよびセグメントへのアクセスが拒否されます。

## 権限

[!UICONTROL 権限は、管理者がユーザーの役割およびアクセスポリシーを定義し、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できる、Experience Cloud の領域です。]

～ [!UICONTROL 権限]では、役割を作成および管理し、それらの役割に対する必要なリソース権限を割り当てることができます。 [!UICONTROL また、権限では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。]

管理者権限がない場合は、システム管理者に問い合わせてアクセス権を取得してください。

管理者権限を取得したら、に移動します。 [Adobe Experience Cloud](https://experience.adobe.com/) にログインし、Adobeの資格情報を使用してログインします。 ログイン後、 **[!UICONTROL 概要]** ページが表示されます。 このページには、組織が購読している製品と、組織全体にユーザーおよび管理者を追加するためのその他のコントロールが表示されます。 選択 **[!UICONTROL 権限]** をクリックして、Platform 統合用のワークスペースを開きます。

![Adobe Experience Cloudで選択されている権限製品を示す画像](../images/flac-ui/flac-select-product.png)

Platform UI の権限ワークスペースが表示され、 **[!UICONTROL 役割]** ページ。

## ロールへのラベルの適用 {#label-roles}

>[!CONTEXTUALHELP]
>id="platform_permissions_labels_about"
>title="ラベルとは"
>abstract="ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 Platform には、Adobeが定義する「コア」データ使用ラベルがいくつか用意されています。これは、データガバナンスに適用できる様々な一般的な制限に対応しています。 例えば、RHD(Regulated Health Data) などの機密性の高い「S」ラベルを使用すると、PHI(Protected Health Information) を参照するデータを分類できます。 また、組織のニーズに合わせて独自のカスタムラベルを定義することもできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-governance/labels/overview.html?lang=en#understanding-data-usage-labels" text="データ使用ラベルの概要"

>[!CONTEXTUALHELP]
>id="platform_permissions_labels_about_create"
>title="新しいラベルを作成"
>abstract="組織のニーズに合わせて独自のカスタムラベルを作成できます。 カスタムラベルは、データガバナンスとアクセス制御の両方の設定をデータに適用するために使用できます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/data-governance/labels/overview.html?lang=en#manage-labels" text="カスタムラベルの管理"

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_about"
>title="役割とは"
>abstract="役割は、Platform インスタンスとやり取りするユーザーのタイプを分類する方法で、アクセス制御ポリシーの構成要素になります。 ロールには、特定の権限のセットがあり、組織のメンバーは、必要な表示または書き込みアクセスの範囲に応じて、1 つ以上のロールに割り当てることができます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/roles.html?lang=en" text="役割の管理"

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_about_create"
>title="新しいロールを作成"
>abstract="Platform インスタンスにアクセスするユーザーをより分類しやすくするための新しい役割を作成できます。 例えば、社内マーケティングチームの役割を作成し、その役割に RHD ラベルを適用すると、社内マーケティングチームが Protected Health Information(PHI) にアクセスできるようになります。 または、外部エージェンシーのロールを作成し、そのロールに RHD ラベルを適用しないことで、そのロールの PHI データへのアクセスを拒否することもできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/roles.html?lang=en#create-a-new-role" text="新しいロールを作成"

>[!CONTEXTUALHELP]
>id="platform_permissions_roles_details"
>title="ロールの概要"
>abstract="役割の概要ダイアログには、特定の役割がアクセスできるリソースとサンドボックスが表示されます。"

役割は、Platform インスタンスとやり取りするユーザーのタイプを分類する方法で、アクセス制御ポリシーの構成要素です。 ロールには特定の権限のセットがあり、組織のメンバーは、必要なアクセスの範囲に応じて、1 つ以上のロールに割り当てることができます。

利用を開始するには、「 **[!UICONTROL ACME ビジネスグループ]** から **[!UICONTROL 役割]** ページ。

![「ロール」で選択されている ACME ビジネスロールを示す画像](../images/abac-end-to-end-user-guide/abac-select-role.png)

次に、 **[!UICONTROL ラベル]** 次に、 **[!UICONTROL ラベルを追加]**.

![「ラベル」タブで「ラベルを追加」が選択されていることを示す画像](../images/abac-end-to-end-user-guide/abac-select-add-labels.png)

組織内のすべてのラベルのリストが表示されます。 選択 **[!UICONTROL RHD]** ラベルを追加するには **[!UICONTROL PHI/規制対象の健康データ]**. ラベルの横に青いチェックマークが表示されるまでしばらく待ってから、を選択します。 **[!UICONTROL 保存]**.

![選択して保存される RHD ラベルを示す画像](../images/abac-end-to-end-user-guide/abac-select-role-label.png)

## スキーマフィールドへのラベルの適用 {#label-resources}

これで、 [!UICONTROL RHD] ラベルを設定する場合は、次に、そのロールに対して制御するリソースに同じラベルを追加します。

選択 **[!UICONTROL スキーマ]** 左のナビゲーションから、 **[!UICONTROL ACME Healthcare]** 表示されるスキーマのリストから。

![「スキーマ」タブから選択された ACME Healthcare スキーマを示す画像](../images/abac-end-to-end-user-guide/abac-select-schema.png)

次に、 **[!UICONTROL ラベル]** をクリックすると、スキーマに関連付けられたフィールドを表示するリストが表示されます。 ここから、1 つまたは複数のフィールドに一度にラベルを割り当てることができます。 を選択します。 **[!UICONTROL 血糖]** および **[!UICONTROL InsulinLevel]** フィールドを選択し、 **[!UICONTROL ガバナンスラベルを編集]**.

![BloodGucode と InsulneLevel が選択されていることを示す画像と、選択されているガバナンスラベルを編集します](../images/abac-end-to-end-user-guide/abac-select-schema-labels-tab.png)

この **[!UICONTROL ラベルを編集]** ダイアログが表示され、スキーマフィールドに適用するラベルを選択できます。 この使用例では、 **[!UICONTROL PHI/規制対象の健康データ]** ラベルを選択し、「 **[!UICONTROL 保存]**.

![選択して保存される RHD ラベルを示す画像](../images/abac-end-to-end-user-guide/abac-select-schema-labels.png)

>[!NOTE]
>
>フィールドにラベルを追加すると、そのラベルがそのフィールドの親リソース（クラスまたはフィールドグループ）に適用されます。 親クラスまたはフィールドグループが他のスキーマで使用されている場合、これらのスキーマは同じラベルを継承します。

## セグメントへのラベルの適用

スキーマフィールドのラベル付けが完了したら、セグメントのラベル付けを開始できます。

選択 **[!UICONTROL セグメント]** をクリックします。 組織で使用可能なセグメントのリストが表示されます。 この例では、次の 2 つのセグメントに、機密性の高いヘルスデータが含まれるので、ラベルが付けられます。

* 血糖 > 100
* インスリン &lt;50

選択 **[!UICONTROL 血糖 > 100]** をクリックして、セグメントのラベル付けを開始します。

![[ セグメント ] タブから選択された血糖値が 100 を超えている画像](../images/abac-end-to-end-user-guide/abac-select-segment.png)

セグメント **[!UICONTROL 詳細]** 画面が表示されます。 選択 **[!UICONTROL アクセスを管理]**.

![「アクセスを管理」の選択を示す画像](../images/abac-end-to-end-user-guide/abac-segment-fields-manage-access.png)

この **[!UICONTROL ラベルを編集]** ダイアログが表示され、セグメントに適用するラベルを選択できます。 この使用例では、 **[!UICONTROL PHI/規制対象の健康データ]** ラベルを選択し、「 **[!UICONTROL 保存]**.

![RHD ラベルの選択と選択中の保存を示す画像](../images/abac-end-to-end-user-guide/abac-select-segment-labels.png)

上記の手順を **[!UICONTROL インスリン &lt;50]**.

## アクセス制御ポリシーの作成 {#policy}

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about"
>title="ポリシーとは"
>abstract="ポリシーとは、属性を統合して、許容される許容されるアクションと許容されないアクションを確立するステートメントです。 すべての組織にはデフォルトのポリシーが用意されています。このポリシーをアクティブ化して、セグメントやスキーマフィールドなどのリソースのルールを定義する必要があります。 デフォルトのポリシーは、編集も削除もできません。 ただし、デフォルトのポリシーをアクティブ化または非アクティブ化することはできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/policies.html?lang=en" text="ポリシーの管理"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_about_create"
>title="ポリシーを作成する"
>abstract="ポリシーを作成して、セグメントおよびスキーマフィールドに対してユーザーが実行できるアクションと実行できないアクションを定義します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/policies.html?lang=en#create-a-new-policy" text="ポリシーを作成する"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_edit_permitdeny"
>title="ポリシーに対して許容されるアクションと許容されないアクションを構成する"
>abstract="A <b>～へのアクセスを拒否する</b> ポリシーは、条件が満たされた場合にユーザーのアクセスを拒否します。 との組み合わせの場合 <b>次は false です</b>  — 一致する条件セットを満たさない限り、すべてのユーザーはアクセスを拒否されます。 このタイプのポリシーを使用すると、機密リソースを保護し、一致するラベルを持つユーザーへのアクセスのみを許可できます。 <br>A <b>～へのアクセスを許可する</b> ポリシーは、条件が満たされた場合にユーザーのアクセスを許可します。 との組み合わせの場合 <b>次のことは真です</b> ：ユーザーは、一致する条件セットを満たす場合、アクセス権を付与されます。 これにより、ユーザーへのアクセスが明示的に拒否されるわけではなく、許可アクセスが追加されます。 このタイプのポリシーでは、役割権限を持つユーザーに加えて、リソースに追加のアクセス権を付与できます。</br>
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/access-control/abac/permissions-ui/policies.html?lang=en#edit-a-policy" text="ポリシーの編集"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_edit_resource"
>title="リソースの権限の設定"
>abstract="リソースとは、ユーザーがアクセスできるアセットまたはオブジェクトです。 リソースには、セグメントまたはスキーマを使用できます。 セグメントおよびスキーマフィールドに対する書き込み、読み取り、削除の権限を設定できます。"

>[!CONTEXTUALHELP]
>id="platform_permissions_policies_edit_condition"
>title="条件を編集"
>abstract="ポリシーに条件文を適用して、特定のリソースへのユーザーアクセスを設定します。 アクセスを許可するために、ユーザーがリソースとまったく同じラベルを持つロールを持つ必要がある場合は、「すべて一致」を選択します。 リソースに一致する 1 つのラベルのみを持つ役割のみをユーザーに要求する場合は、「一致する任意」を選択します。 ラベルは、コアまたはカスタムのラベルとして定義できます。コアラベルは、Adobeが作成および提供するラベルを表し、カスタムラベルは組織が作成したラベルを表します。"

アクセス制御ポリシーでは、ラベルを使用して、特定の Platform リソースにアクセスできるユーザーの役割を定義します。 ポリシーは、ローカルまたはグローバルに設定でき、他のポリシーを上書きできます。 この例では、スキーマフィールドに対応するラベルを持たないユーザーの場合、すべてのサンドボックスで、スキーマフィールドおよびセグメントへのアクセスが拒否されます。

アクセス制御ポリシーを作成するには、 **[!UICONTROL 権限]** 左のナビゲーションから、 **[!UICONTROL ポリシー]**. 次に、 **[!UICONTROL ポリシーを作成]**.

![権限で作成ポリシーが選択されていることを示す画像](../images/abac-end-to-end-user-guide/abac-create-policy.png)

この **[!UICONTROL 新しいポリシーを作成]** ダイアログが表示され、名前とオプションの説明を入力するよう求められます。 選択 **[!UICONTROL 確認]** 終了したとき。

![新しいポリシーを作成ダイアログを表示し、「確認」を選択する画像](../images/abac-end-to-end-user-guide/abac-create-policy-details.png)

スキーマフィールドへのアクセスを拒否するには、ドロップダウンの矢印を使用して、「 」を選択します。 **[!UICONTROL へのアクセスを拒否]** 次に、 **[!UICONTROL リソースが選択されていません]**. 次に、 **[!UICONTROL スキーマフィールド]** 次に、 **[!UICONTROL すべて]**.

![拒否アクセスと選択されたリソースを示す画像](../images/abac-end-to-end-user-guide/abac-create-policy-deny-access-schema.png)

次の表に、ポリシーを作成する際に使用できる条件を示します。

| 条件 | 説明 |
| --- | --- |
| 次は false です | 「へのアクセスを拒否」が設定されている場合、ユーザーが選択した条件を満たさない場合、アクセスは制限されます。 |
| 次のことは真です | 「アクセスを許可」が設定されている場合、ユーザーが選択した条件を満たす場合、アクセスは制限されます。 |
| いずれかに一致 | ユーザーには、リソースに適用された任意のラベルに一致するラベルがあります。 |
| すべて一致 | ユーザーには、リソースに適用されたすべてのラベルに一致するすべてのラベルがあります。 |
| コアラベル | コアラベルは、すべてのAdobeインスタンスで使用できる、プラットフォーム定義のラベルです。 |
| カスタムラベル | カスタムラベルは、組織が作成したラベルです。 |

選択 **[!UICONTROL 次は false です]** 次に、 **[!UICONTROL 属性が選択されていません]**. 次に、ユーザーを選択します。 **[!UICONTROL コアラベル]**&#x200B;を選択し、「 **[!UICONTROL すべて一致]**. リソースを選択 **[!UICONTROL コアラベル]** 最後に、 **[!UICONTROL リソースを追加]**.

![選択される条件を示す画像と、選択されるリソースを追加](../images/abac-end-to-end-user-guide/abac-create-policy-deny-access-schema-expression.png)

>[!TIP]
>
>リソースとは、主体がアクセスできるアセットまたはオブジェクトです。 リソースには、セグメントまたはスキーマを使用できます。

セグメントへのアクセスを拒否するには、ドロップダウン矢印を使用して、「 」を選択します。 **[!UICONTROL へのアクセスを拒否]** 次に、 **[!UICONTROL リソースが選択されていません]**. 次に、 **[!UICONTROL セグメント]** 次に、 **[!UICONTROL すべて]**.

選択 **[!UICONTROL 次は false です]** 次に、 **[!UICONTROL 属性が選択されていません]**. 次に、ユーザーを選択します。 **[!UICONTROL コアラベル]**&#x200B;を選択し、「 **[!UICONTROL すべて一致]**. リソースを選択 **[!UICONTROL コアラベル]** 最後に、 **[!UICONTROL 保存]**.

![選択した条件を示す画像と、選択中の「保存」](../images/abac-end-to-end-user-guide/abac-create-policy-deny-access-segment.png)

選択 **[!UICONTROL 有効化]** ポリシーをアクティブ化すると、アクティベートを確認するダイアログが表示されます。 選択 **[!UICONTROL 確認]** 次に、 **[!UICONTROL 閉じる]**.

![アクティブ化されているポリシーを示す画像 ](../images/abac-end-to-end-user-guide/abac-create-policy-activation.png)

## 次の手順

ロール、スキーマフィールド、セグメントへのラベルの適用が完了している。 これらの役割に割り当てられた外部エージェントでは、スキーマ、データセット、プロファイル表示で、これらのラベルとその値の表示が制限されます。 また、セグメントビルダーを使用する際、これらのフィールドは、セグメント定義で使用できません。

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](./overview.md)を参照してください。

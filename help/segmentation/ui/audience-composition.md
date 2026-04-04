---
solution: Experience Platform
title: オーディエンス UI ガイド
description: Adobe Experience Platform UI のオーディエンス構成は、プロファイルデータ要素を操作するための機能豊富なワークスペースを提供します。ワークスペースには、組織に合わせてオーディエンスを作成および編集するための直感的なコントロールが用意されています。
exl-id: 0dda0cb1-49e0-478b-8004-84572b6cf625
source-git-commit: 82e41af32468febeda2dce6b471d72ef74359ea9
workflow-type: tm+mt
source-wordcount: '2293'
ht-degree: 55%

---

# オーディエンス構成 UI ガイド

>[!BEGINSHADEBOX]

Adobe Journey Optimizerをご利用のお客様は、Adobe Journey Optimizer ドキュメントの[ オーディエンス構成の基本を学ぶガイド ](https://experienceleague.adobe.com/docs/journey-optimizer/using/audiences-profiles-identities/audiences/audience-orchestration/get-started-audience-orchestration.html?lang=ja)を参照して、そのコンテキストでのオーディエンス構成の操作について詳しくお読みください。

>[!ENDSHADEBOX]

>[!AVAILABILITY]
>
>この機能を使用するには、次の権限が必要です。
>
>- セグメントの管理
>- プロファイルの管理
>- 結合ポリシーの管理
>
>Experience Platform内の権限について詳しくは、[ アクセス制御の概要](../../access-control/home.md#permissions)を参照してください。

>[!NOTE]
>
>このガイドでは、オーディエンス構成を使用してオーディエンスを作成する方法を説明します。セグメントビルダーを使用してセグメント定義を通じてオーディエンスを作成する方法については、[セグメントビルダー UI ガイド](./segment-builder.md)を参照してください。

オーディエンス構成には、様々なアクションの表現に使用されるブロックでオーディエンスを作成および編集するためのワークスペースが用意されています。

![オーディエンス構成 UI.](../images/ui/audience-composition/audience-composition.png)

タイトルや説明など、構成の詳細を変更するには、![スライダー](/help/images/icons/properties.png) ボタンを選択します。

**[!UICONTROL Composition properties]** ポップオーバーが表示されます。 タイトルや説明など、構成の詳細をここに挿入できます。

![構成のプロパティポップオーバーが表示されます。](../images/ui/audience-composition/composition-properties.png)

>[!NOTE]
>
>コンポジションにタイトルを付ける&#x200B;**not**&#x200B;を実行すると、デフォルトでは「コンポジション」のタイトルの後に作成日時が続きます。 さらに、各コンポジション **には独自の一意の名前が必要です。**

コンポジションの詳細を更新した後、**[!UICONTROL Save]**&#x200B;を選択して、これらの更新を確認します。 オーディエンス構成キャンバスが再び表示されます。

オーディエンス構成キャンバスは、**[[!UICONTROL Audience]](#audience-block)**、**[[!UICONTROL Exclude]](#exclude-block)**、**[[!UICONTROL Rank]](#rank-block)**、**[[!UICONTROL Split]](#split-block)**&#x200B;の4つの異なるタイプのブロックで構成されています。

## [!UICONTROL Audience] {#audience-block}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_audience"
>title="オーディエンスブロック"
>abstract="オーディエンスブロックを使用すると、新しいオーディエンスの作成に使用するサブオーディエンスを追加できます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_merge_types"
>title="結合タイプ"
>abstract="結合タイプによって、選択したサブオーディエンスの結合方法が決まります。サポートされている値には、和集合、積集合、重複を除外が含まれます。"

**[!UICONTROL Audience]** ブロックタイプを使用すると、新しい大きなオーディエンスの構成に使用するサブオーディエンスを追加できます。 デフォルトでは、コンポジションキャンバスの上部に&#x200B;**[!UICONTROL Audience]** ブロックが含まれています。

**[!UICONTROL Audience]** ブロックを選択すると、右側のパネルに、オーディエンスのラベル付け、ブロックへのオーディエンスの追加、オーディエンスブロックのカスタムルールの作成のためのコントロールが表示されます。

>[!NOTE]
>
>オーディエンスを追加する&#x200B;**かまたは**&#x200B;カスタムルールを作成できます。これらの 2 つの機能を一緒に使用することは&#x200B;**できません**。

![オーディエンスブロックの詳細が表示されている様子。](../images/ui/audience-composition/audience-block.png)

### [!UICONTROL Add audience] {#add-audience}

オーディエンスブロックにオーディエンスを追加するには。**[!UICONTROL Add Audience]**&#x200B;を選択します。

![「オーディエンスを追加」ボタンがハイライト表示されています。](../images/ui/audience-composition/select-add-audience.png)

>[!IMPORTANT]
>
>デフォルトの結合ポリシーを使用して定義された&#x200B;**のみ**&#x200B;のオーディエンスが表示されます。
>
>さらに、セグメントビルダーを使用して作成された&#x200B;**公開** オーディエンスのみを使用できます。 オーディエンス構成と外部生成オーディエンスを使用して作成されたオーディエンスは&#x200B;**利用できません**。

オーディエンスのリストが表示されます。含めるオーディエンスを選択し、その後&#x200B;**[!UICONTROL Add]**&#x200B;を選択してオーディエンスブロックに追加します。

![オーディエンスのリストが表示されます。このダイアログから、追加するオーディエンスを選択できます。](../images/ui/audience-composition/select-audience.png)

**[!UICONTROL Audience]** ブロックを選択すると、選択したオーディエンスが右側のパネルに表示されるようになりました。 ここから、結合オーディエンスの結合タイプを変更できます。

![オーディエンスで使用可能な結合タイプがハイライト表示されます。](../images/ui/audience-composition/merge-types.png)

| 結合タイプ | 説明 |
| ---------- | ----------- |
| [!UICONTROL Union] | 複数のオーディエンスが 1 つのオーディエンスに結合されます。 これは OR 演算と同じです。 |
| [!UICONTROL Intersection] | オーディエンスが結合され、**すべて**&#x200B;のオーディエンスで共有されているオーディエンスのみが追加されます。これは AND 演算と同じです。 |
| [!UICONTROL Exclude overlap] | オーディエンスが結合され、**すべてではなく、いずれか**&#x200B;のオーディエンスで共有されているオーディエンスのみが追加されます。これは XOR 演算と同じです。 |

### [!UICONTROL Build rule] {#build-rule}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_rule_builder"
>title="セグメントビルダー"
>abstract="セグメントビルダーを使用して、構成にカスタムルールを追加できます。"

オーディエンスブロックにカスタムルールを追加するには、**[!UICONTROL Build rule]**&#x200B;を選択します。

![「ルールを作成」ボタンがハイライト表示されています。](../images/ui/audience-composition/select-build-rule.png)

セグメントビルダーが表示されます。セグメントビルダーを使用して、オーディエンスが従うべきカスタムルールを作成できます。セグメントビルダーの使用について詳しくは、[セグメントビルダーガイド](./segment-builder.md)を参照してください。

![セグメントビルダー UI が表示されます。](../images/ui/audience-composition/segment-builder.png)

カスタムルールを追加したら、**[!UICONTROL Save]**&#x200B;を選択して、ルールをオーディエンスに追加します。

![](../images/ui/audience-composition/custom-rule.png)

## [!UICONTROL Exclude] {#exclude-block}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_exclude"
>title="ブロックを除外"
>abstract="「ブロックを除外」を使用すると、指定したオーディエンスまたは属性を構成から除外できます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_exclude_type"
>title="除外タイプ"
>abstract="特定のオーディエンスに属するプロファイルを除外（オーディエンス別に除外）するか、特定の属性に基づいてプロファイルを除外（属性別に除外）することができます。"

**[!UICONTROL Exclude]** ブロックタイプを使用すると、指定したサブオーディエンスまたは属性を新しい大きなオーディエンスから除外できます。

**[!UICONTROL Exclude]** ブロックを追加するには、**+** アイコンを選択し、次に&#x200B;**[!UICONTROL Exclude]**&#x200B;を選択します。

![「除外」オプションが選択されている様子。](../images/ui/audience-composition/add-exclude-block.png)

**[!UICONTROL Exclude]** ブロックが追加されます。 このブロックを選択すると、除外に関する詳細が右側のパネルに表示されます。これには、ブロックのラベルと除外のタイプが含まれます。[オーディエンス別](#exclude-audience)、または[属性別](#exclude-attribute)に除外できます。

![使用可能な 2 つの除外タイプがハイライト表示された「除外」ブロック。](../images/ui/audience-composition/exclude.png)

### オーディエンス別に除外 {#exclude-audience}

オーディエンスで除外する場合は、**[!UICONTROL Add Audience]**&#x200B;を選択して、除外するオーディエンスを選択できます。

![ 「[!UICONTROL Add audience]」ボタンが選択され、除外するオーディエンスを選択できます。](../images/ui/audience-composition/add-excluded-audience.png)

>[!IMPORTANT]
>
>セグメントビルダーを使用して作成された&#x200B;**公開済み** オーディエンスのみを使用できます。 オーディエンス構成と外部生成オーディエンスを使用して作成されたオーディエンスは&#x200B;**利用できません**。

オーディエンスのリストが表示されます。除外するオーディエンスを除外ブロックに追加するには、**[!UICONTROL Add]**&#x200B;を選択します。

![オーディエンスのリストが表示されます。このダイアログから、追加するオーディエンスを選択できます。](../images/ui/audience-composition/select-audience.png)

### 属性別に除外 {#exclude-attribute}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_exclude_attribute"
>title="属性別に除外"
>abstract="属性別に除外すると、選択した属性に基づいて、特定のプロファイルが構成に表示されないように除外できます。"

属性で除外する場合は、![ セクション内の](/help/images/icons/project-edit.png)filter **[!UICONTROL Exclusion rule]** アイコンを選択して、除外する属性を選択できます。 属性を除外すると、この属性を含むプロファイルを結果のオーディエンスから除外できます。

![属性セクションがハイライト表示され、除外する属性を選ぶために選択すべき場所が示されている様子。](../images/ui/audience-composition/exclude-attribute.png)

プロファイル属性のリストが表示されます。除外する属性タイプを選択し、その後&#x200B;**[!UICONTROL Select]**&#x200B;を選択して除外ブロックに追加します。

![属性のリストが表示されます。](../images/ui/audience-composition/select-attribute-exclude.png)

>[!IMPORTANT]
>
>属性で除外する場合は、除外する値を&#x200B;**one**&#x200B;個のみ指定できます。 コンマやセミコロンなどの任意の種類の区切り記号を使用すると、その正確な値を除外することしかできません。 例えば、値を`red, blue`に設定すると、属性から`red, blue`という用語が除外されますが、**not**&#x200B;は`red`または`blue`という用語が除外されます。

## [!UICONTROL Enrich] {#enrich-block}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_enrich"
>title="エンリッチメントブロック"
>abstract="エンリッチメントブロックを使用すると、Adobe Experience Platform データセットから取得した追加の属性でオーディエンスを強化できます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_dataset"
>title="エンリッチメントデータセット"
>abstract="エンリッチメントデータセットには、構成に関連付けるデータが含まれます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_enrich_criteria"
>title="エンリッチメント条件"
>abstract="エンリッチメント条件には、ソース結合キーとエンリッチメントデータセット結合キーが含まれます。 これらの 2 つのキーは、ソースデータセットとエンリッチメントデータセットを紐付けます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_enrich_attributes"
>title="エンリッチメント属性"
>abstract="エンリッチメント属性は、構成に関連付ける属性です。"

>[!IMPORTANT]
>
>この時点で、エンリッチメント属性は、ダウンストリームの Adobe Journey Optimizer シナリオで&#x200B;**のみ**&#x200B;使用できます。

**[!UICONTROL Enrich]** ブロックタイプを使用すると、データセットの追加の属性を使用してオーディエンスを強化できます。 これらの属性は、パーソナライゼーションのユースケースで使用できます。

**[!UICONTROL Enrich]** ブロックを追加するには、**+** アイコンを選択し、次に&#x200B;**[!UICONTROL Enrich]**&#x200B;を選択します。

![[!UICONTROL Enrich] オプションが選択されています。](../images/ui/audience-composition/add-enrich-block.png)

**[!UICONTROL Enrich]** ブロックが追加されます。 このブロックを選択すると、エンリッチメントに関する詳細が右側のパネルに表示されます。これには、ブロックのラベルとエンリッチメントデータセットが含まれます。

オーディエンスの強化に使用するデータセットを選択するには、![フィルター](/help/images/icons/project-edit.png)アイコンを選択します。

![フィルターボタンがハイライト表示されます。これを選択すると、[!UICONTROL Select dataset] ポップオーバーに移動します。](../images/ui/audience-composition/enrich-select-dataset.png)

**[!UICONTROL Select dataset]** ポップオーバーが表示されます。 エンリッチメント用に追加するデータセットを選択し、エンリッチメント用にデータセットを追加する場合は&#x200B;**[!UICONTROL Select]**&#x200B;を選択します。

![選択したデータセットが選択されます。](../images/ui/audience-composition/select-dataset.png)

>[!IMPORTANT]
>
>選択したデータセットは、**必ず**&#x200B;次の条件を満たす必要があります。
>
>- データセットは、**必ず**&#x200B;レコードタイプである必要があります。
>   - データセットをイベントタイプ、システム生成またはプロファイル用にマークすることは&#x200B;**できません**。
>- データセットは、**必ず** 1 GB 以下にする必要があります。

右側のパネルに&#x200B;**[!UICONTROL Enrichment criteria]** セクションが表示されるようになりました。 このセクションでは、**[!UICONTROL Source join key]**&#x200B;と&#x200B;**[!UICONTROL Enrichment dataset join key]**&#x200B;を選択できます。これにより、エンリッチメントデータセットを作成しようとしているオーディエンスにリンクできます。

![[!UICONTROL Enrichment criteria]領域がハイライト表示されます。](../images/ui/audience-composition/enrichment-criteria.png)

**[!UICONTROL Source join key]**&#x200B;を選択するには、![ フィルター](/help/images/icons/project-edit.png) アイコンを選択します。

**[!UICONTROL Select a profile attribute]** ポップオーバーが表示されます。 ソース結合キーとして使用するプロファイル属性を選択し、その属性をソース結合キーとして選択するには&#x200B;**[!UICONTROL Select]**&#x200B;を選択します。

![ソース結合キーとして使用する属性がハイライト表示されます。](../images/ui/audience-composition/select-source-join-key.png)

**[!UICONTROL Enrichment dataset join key]**&#x200B;を選択するには、![ フィルター](/help/images/icons/project-edit.png) アイコンを選択します。

**[!UICONTROL Enrichment attributes]** ポップオーバーが表示されます。 エンリッチメントデータセット結合キーとして使用する属性を選択し、その属性をエンリッチメントデータセット結合キーとして選択するには&#x200B;**[!UICONTROL Select]**&#x200B;を選択します。

![エンリッチメントデータセット結合キーとして使用する属性がハイライト表示されます。](../images/ui/audience-composition/select-enrichment-dataset-join-key.png)

結合キーの両方を追加すると、**[!UICONTROL Enrichment attributes]** セクションが表示されます。 これで、オーディエンスを強化する属性を追加できます。これらの属性を追加するには、**[!UICONTROL Add attribute]**&#x200B;を選択します。

**[!UICONTROL Enrichment attributes]** ポップオーバーが表示されます。 データセットから属性を選択してオーディエンスをエンリッチメントし、次に&#x200B;**[!UICONTROL Select]**&#x200B;を選択して属性をオーディエンスに追加できます。

![追加するエンリッチメント属性がハイライト表示されます。](../images/ui/audience-composition/select-enrichment-attribute.png)

<!-- 
## [!UICONTROL Join] {#join-block}

The **[!UICONTROL Join]** block type allows you to add in external audiences from datasets that have not yet been processed by Adobe Experience Platform.

To add a **[!UICONTROL Join]** block, select the **+** icon, followed by **[!UICONTROL Join]**.

![The Join option is selected.](../images/ui/audience-composition/add-join-block.png)

When you select the block, details about the join are shown in the right rail, including the block's label and the option to add audiences to the enrichment dataset.

![The join block is highlighted, including details about the join block.](../images/ui/audience-composition/join.png)

After selecting **[!UICONTROL Add Audience]**, a list of audiences appears. Select the audiences you want to include, followed by **[!UICONTROL Add]** to add them to your join block.

![A list of audiences appears. You can select which audience you want to add from this dialog.](../images/ui/audience-composition/select-audience.png)

Your selected audiences now appear within the right rail when the **[!UICONTROL Join]** block is selected. 

![The audiences that were added as part of the Join are shown.](../images/ui/audience-composition/selected-audiences.png) 
-->

## [!UICONTROL Rank] {#rank-block}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_ranking"
>title="ランクブロック"
>abstract="ランクブロックを使用すると、特定の属性に基づいてプロファイルをランク付けし、構成に含めることができます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_rank_profilelimit_text"
>title="プロファイル制限を追加"
>abstract="「プロファイル制限を追加」切替スイッチを使用すると、ランキングプロセスの一部として含めるプロファイルの最大数を指定できます。"

**[!UICONTROL Rank]** ブロックタイプを使用すると、指定した属性に基づいてプロファイルをランク付けおよび並べ替え、これらのランク付けプロファイルをコンポジションに含めることができます。

**[!UICONTROL Rank]** ブロックを追加するには、**+** アイコンを選択し、次に&#x200B;**[!UICONTROL Rank]**&#x200B;を選択します。

![「ランク」オプションが選択されています。](../images/ui/audience-composition/add-rank-block.png)

ブロックを選択すると、右側のパネルにランキングの詳細が表示されます。ここには、ブロックのラベル、ランク付けの基準にする属性、ランク付けの順序、ランク付けするプロファイル数を制限する切替スイッチなどが含まれています。

![ランクブロックがハイライト表示され、ランクブロックの詳細も表示されています。](../images/ui/audience-composition/rank.png)

オーディエンスのランク付けの基準にする属性を選択するには、![フィルター](/help/images/icons/project-edit.png)アイコンを選択します。

![フィルターアイコンがハイライト表示され、プロファイル属性選択画面にアクセスするために選択すべき項目が示されている様子。](../images/ui/audience-composition/select-rank-attribute.png)

プロファイル属性のリストが表示されます。このポップオーバーで、オーディエンスのランク付けの基準にする属性タイプを選択できます。 **[!UICONTROL Select]**&#x200B;を選択して、ランク ブロックに追加します。 選択した属性は、数字&#x200B;**のみ**&#x200B;であることに注意してください。

![属性のリストが表示されています。](../images/ui/audience-composition/rank-attribute.png)

属性を選択したら、ランク付けの順序を選択できます。 昇順（最小から最大）または降順（最大から最小）のいずれかです。

さらに、**[!UICONTROL Add profile limit]** トグルを有効にすることで、返されるプロファイルの数を制限できます。 この切り替えが有効になっている場合、**[!UICONTROL Included profiles]** フィールド内で返されるプロファイルの最大数を設定できます。

![ プロファイル制限を追加トグルがハイライト表示され、返されるプロファイルの数を制限できます。](../images/ui/audience-composition/add-profile-limit-rank.png)

## [!UICONTROL Split] {#split-block}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_split"
>title="ブロックを分割"
>abstract="「ブロックを分割」を使用すると、構成を複数のパスに分割できます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_split_type"
>title="分割タイプ"
>abstract="構成は、割合の分割または属性の分割で分割できます。割合の分割は、プロファイルを複数のパスにランダムに分割します。属性の分割を使用すると、特定の属性に基づいてプロファイルを分割できます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_split_otherprofiles_text"
>title="その他のプロファイル"
>abstract="「その他のプロファイル」切替スイッチを使用すると、他のパスで指定された条件に一致しない残りのプロファイルと共に、追加のパスを作成できます。"

>[!NOTE]
>
>**[!UICONTROL Split]** ブロックを使用するには、**オーディエンスに少なくとも10個のプロファイルが必要です。**

**[!UICONTROL Split]** ブロックタイプを使用すると、新しいオーディエンスをさまざまなサブオーディエンスに分割できます。 オーディエンスは、割合に基づいて分割したり、属性別に分割したりできます。

**[!UICONTROL Split]** ブロックを追加するには、**+** アイコンを選択し、次に&#x200B;**[!UICONTROL Split]**&#x200B;を選択します。

![「分割」オプションが選択されている様子。](../images/ui/audience-composition/add-split-block.png)

オーディエンスを分割する場合は、割合で分割するか、属性で分割します。

### 割合で分割 {#split-percentage}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_split_percentage"
>title="割合で分割"
>abstract="指定されたパスの数と割合に基づいて、オーディエンスを複数のオーディエンスにランダムに分割できます。"

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_split_persistent"
>title="永続的分割"
>abstract="このオプションを有効にして ID 名前空間を選択することで、割合の分割を永続的にすることができます。"

割合で分割する場合、オーディエンスは、指定されたパスの数と割合に基づいてランダムに分割されます。

![分割された割合がハイライト表示されます。](../images/ui/audience-composition/split-by-percentage.png)

または、IDを指定して、パーセンテーションベースの分割を永続的にすることもできます。 使用可能なID タイプには、組織で使用可能なすべてのID名前空間が含まれます。

![IDで分割チェックボックスが強調表示されます。 さらに、分割するIDを選択できるドロップダウンがハイライト表示されます。](../images/ui/audience-composition/split-by-identity.png)

### 属性で分割 {#split-attribute}

属性別に分割する場合、オーディエンスは指定された属性に基づいて分割されます分割する属性を選択するには、**[!UICONTROL Split]** ブロックを選択し、続いて![ フィルター](/help/images/icons/project-edit.png) アイコンを選択します。

![フィルターボタンが選択され、属性別にフィルターする方法が示されています。](../images/ui/audience-composition/split-by-attribute.png)

プロファイル属性のリストが表示されます。属性タイプを選択し、続いて&#x200B;**[!UICONTROL Select]**&#x200B;を選択して、分割ブロックに追加します。

![属性のリストが表示されます。](../images/ui/audience-composition/select-attribute.png)

属性を選択した後、**[!UICONTROL Values]** フィールド内の値を追加して、どのプロファイルがどのサブオーディエンスに属するかを選択できます。

![属性の分割の基準にする値が追加されている様子。](../images/ui/audience-composition/attribute-split-values.png)

さらに、**[!UICONTROL Other profiles]** トグルを有効にして、選択されていないすべてのプロファイルを含むサブオーディエンスを作成することもできます。

![「その他のプロファイル」切替スイッチがハイライト表示されている様子。](../images/ui/audience-composition/split-other-profiles.png)

## オーディエンスのパブリッシュ {#publish}

>[!CONTEXTUALHELP]
>id="platform_segmentation_ao_publish"
>title="公開"
>abstract="構成を公開して、結果のオーディエンスを Adobe Experience Platform で作成できます。"

>[!IMPORTANT]
>
>オーディエンス構成を公開する場合、Real-Time CDPの宛先やAdobe Journey Optimizer チャネルなどの下流サービスで使用するために評価およびアクティブ化するのに最大で48時間かかる場合があることに注意してください。

コンポジションを作成したら、**[!UICONTROL Publish]**&#x200B;を選択して保存し、公開できます。

![公開ボタンがハイライト表示され、コンポジションを保存して公開する方法が表示されます。](../images/ui/audience-composition/publish.png)

オーディエンスの作成でエラーが発生した場合は、アラートが表示され、問題の解決方法を知らせます。

![公開ボタンがハイライト表示され、コンポジションを保存して公開する方法が表示されます。](../images/ui/audience-composition/audience-alert.png)

## 次の手順

オーディエンス構成には豊富なワークフローが用意されており、様々なブロックタイプから構成を作成できます。 セグメント化サービス UI の他の部分について詳しくは、[セグメント化サービスユーザーガイド](./overview.md)を参照してください。

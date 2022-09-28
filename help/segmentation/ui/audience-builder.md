---
keywords: Experience Platform；ホーム；人気の高いトピック；セグメント化；セグメント化サービス；ユーザーガイド；ui ガイド；audiences ui ガイド；audience builder；オーディエンス；オーディエンス；オーディエンス ui ガイド；
solution: Experience Platform
title: Audiences UI ガイド
topic-legacy: ui guide
description: Adobe Experience Platform UI の Audience Builder は、プロファイルデータ要素を操作できる豊富なワークスペースを提供します。 ワークスペースには、組織のオーディエンスを作成および編集するための直感的なコントロールが用意されています。
hide: true
hidefromtoc: true
source-git-commit: f71d49b576059e687c337cbacd6dd3d525e97834
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 1%

---

# Audience Builder UI ガイド

>[!IMPORTANT]
>
>Audience Builder は現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Audience Builder は、様々なアクションを表すブロックを使用して、オーディエンスを作成および編集するためのワークスペースを提供します。

![Audience Builder UI です。](../images/ui/audience-builder/audience-builder.png)

オーディエンス構成キャンバスは、次の 5 種類のブロックで構成されています。 **[[!UICONTROL 対象ユーザ]](#audience-block)**, **[[!UICONTROL 除外]](#exclude-block)**, **[[!UICONTROL 結合]](#join-block)**, **[[!UICONTROL ランク]](#rank-block)**、および **[[!UICONTROL 分割]](#split-block)**.

## [!UICONTROL オーディエンス] {#audience-block}

この **[!UICONTROL 対象ユーザ]** ブロックタイプを使用すると、より大きな新しいオーディエンスを作成するサブオーディエンスを追加できます。 デフォルトでは、 **[!UICONTROL 対象ユーザ]** ブロックがコンポジションキャンバスの上部に含まれます。

を選択し、 **[!UICONTROL 対象ユーザ]** ブロックを選択すると、右側のパネルに、オーディエンスのラベル付けとブロックへの追加を行うためのコントロールが表示されます。

![オーディエンスブロックの詳細が表示されます。](../images/ui/audience-builder/select-audience.png)

選択後 **[!UICONTROL オーディエンスを追加]**&#x200B;に設定すると、オーディエンスのリストが表示されます。 含めるオーディエンスを選択し、その後に **[!UICONTROL 追加]** を追加して、オーディエンスブロックに追加します。

![オーディエンスのリストが表示されます。 このダイアログから、追加するオーディエンスを選択できます。](../images/ui/audience-builder/select-audience.png)

選択したオーディエンスが、 **[!UICONTROL 対象ユーザ]** ブロックが選択されている。 ここから、結合オーディエンスの結合タイプを変更できます。

![オーディエンスで使用可能な結合タイプがハイライト表示されます。](../images/ui/audience-builder/merge-types.png)

| 結合タイプ | 説明 |
| ---------- | ----------- |
| [!UICONTROL 結合] | オーディエンスは 1 つのオーディエンスに結合されます。 これは OR 演算と同じです。 |
| [!UICONTROL Intersection] | オーディエンスは結合され、で共有されるオーディエンスのみが表示されます **すべて** 追加される これは、AND 演算と同じです。 |
| [!UICONTROL 重複を除外] | オーディエンスは結合され、で共有されるオーディエンスのみが表示されます **一つだけではなく、すべてではなく** 追加される これは XOR 操作と同じです。 |

## [!UICONTROL 除外] {#exclude-block}

この **[!UICONTROL 除外]** ブロックタイプを使用すると、指定したサブオーディエンスまたは属性を、新しい大規模なオーディエンスから除外できます。

次の手順で **[!UICONTROL 除外]** ブロック、 **+** アイコン、その後に **[!UICONTROL 除外]**.

![「除外」オプションが選択されている。](../images/ui/audience-builder/add-exclude-block.png)

この **[!UICONTROL 除外]** ブロックが追加されます。 このブロックを選択すると、除外に関する詳細が右側のパネルに表示されます。 これには、ブロックのラベルと除外のタイプが含まれます。 除外できます [閲覧者別](#exclude-audience) または [属性別](#exclude-attribute).

![「除外」ブロック。使用可能な 2 つの異なる除外タイプをハイライト表示します。](../images/ui/audience-builder/exclude.png)

### オーディエンスごとに除外 {#exclude-audience}

オーディエンスごとに除外する場合は、「 」を選択して、除外するオーディエンスを選択できます **[!UICONTROL オーディエンスを追加]**.

![「オーディエンスを追加」ボタンが選択され、除外するオーディエンスを選択できます。](../images/ui/audience-builder/add-excluded-audience.png)

オーディエンスのリストが表示されます。 選択 **[!UICONTROL 追加]** をクリックして、除外するオーディエンスを除外ブロックに追加します。

![オーディエンスのリストが表示されます。 このダイアログから、追加するオーディエンスを選択できます。](../images/ui/audience-builder/select-audience.png)

### 属性別に除外 {#exclude-attribute}

属性別に除外する場合、「 ![フィルター](../images/ui/audience-builder/filter-attribute.png) アイコン **[!UICONTROL 除外ルール]** 」セクションに入力します。

![属性セクションがハイライト表示され、除外する属性を選択する場所が表示されます。](../images/ui/audience-builder/exclude-attribute.png)

プロファイル属性のリストが表示されます。 除外する属性タイプを選択し、その後に **[!UICONTROL 選択]** 「除外」ブロックに追加します。

![属性のリストが表示されます。](../images/ui/audience-builder/select-attribute.png)

## [!UICONTROL 結合] {#join-block}

この **[!UICONTROL 結合]** ブロックタイプを使用すると、Adobe Experience Platformでまだ処理されていないデータセットから外部オーディエンスを追加できます。

を追加するには、以下を実行します。 **[!UICONTROL 結合]** ブロック、 **+** アイコン、その後に **[!UICONTROL 結合]**.

![「結合」オプションが選択されている。](../images/ui/audience-builder/add-join-block.png)

ブロックを選択すると、ブロックのラベルや、オーディエンスをエンリッチメントデータセットに追加するオプションなど、結合の詳細が右側のパネルに表示されます。

![結合ブロックの詳細を含め、結合ブロックがハイライト表示されます。](../images/ui/audience-builder/join.png)

選択後 **[!UICONTROL オーディエンスを追加]**&#x200B;に設定すると、オーディエンスのリストが表示されます。 含めるオーディエンスを選択し、その後に **[!UICONTROL 追加]** をクリックして、結合ブロックに追加します。

![オーディエンスのリストが表示されます。 このダイアログから、追加するオーディエンスを選択できます。](../images/ui/audience-builder/select-audience.png)

選択したオーディエンスが、 **[!UICONTROL 結合]** ブロックが選択されている。

![結合の一部として追加されたオーディエンスが表示されます。](../images/ui/audience-builder/selected-audiences.png)

## [!UICONTROL ランク] {#rank-block}

この **[!UICONTROL ランク]** ブロックタイプを使用すると、新しいオーディエンスが公開される前に、オーディエンスのランク付けと並べ替えをおこなうことができます。

を追加するには、以下を実行します。 **[!UICONTROL ランク]** ブロック、 **+** アイコン、その後に **[!UICONTROL ランク]**.

![「ランク」オプションが選択されている。](../images/ui/audience-builder/add-rank-block.png)

ブロックを選択すると、右側のレールに、ブロックのラベル、ランク付けする属性、ランク付けの順序、ランク付けするプロファイル数を制限する切り替えなど、ランキングの詳細が表示されます。

![ランクブロックがハイライト表示され、ランクブロックの詳細も表示されます。](../images/ui/audience-builder/rank.png)

オーディエンスのランク付けに使用する属性を選択するには、 ![フィルター](../images/ui/audience-builder/filter-attribute.png) アイコン

![フィルターアイコンがハイライト表示され、プロファイル属性選択画面にアクセスするために選択する項目が表示されます。](../images/ui/audience-builder/select-rank-attribute.png)

プロファイル属性のリストが表示されます。 このポップオーバーで、オーディエンスのランク付けに使用する属性タイプを選択できます。 選択 **[!UICONTROL 選択]** をクリックして、ランクブロックに追加します。 選択した属性は、 **のみ** タイプが `int`.

![属性のリストが表示されます。](../images/ui/audience-builder/select-attribute.png)

属性を選択した後、ランク付けの順序を選択できます。 昇順（最低から最高）または降順（最高から最低）です。

さらに、 **[!UICONTROL プロファイル制限を追加]** 切り替え この切り替えを有効にした場合、 **[!UICONTROL 含まれるプロファイル]** フィールドに入力します。

![「プロファイル制限を追加」切り替えが強調表示され、返されるオーディエンスの数を制限できます。](../images/ui/audience-builder/add-profile-limit.png)

## [!UICONTROL 分割] {#split-block}

この **[!UICONTROL 分割]** ブロックタイプを使用すると、新しいオーディエンスを様々なサブオーディエンスに分割できます。 割合または属性に基づいて、このオーディエンスを分割できます。

を追加するには、以下を実行します。 **[!UICONTROL 分割]** ブロック、 **+** アイコン、その後に **[!UICONTROL 分割]**.

![「分割」オプションが選択されている。](../images/ui/audience-builder/add-split-block.png)

### 割合で分割 {#split-percentage}

割合で分割する場合、オーディエンスは、提供されたパスの数と割合に基づいてランダムに分割されます。

例えば、3 つのパスを持ち、それぞれのパスにプロファイルの割合が異なる場合があります。

![保存したオーディエンスの数と割合で分類が表示されます。](../images/ui/audience-builder/percentages.png)

さらに、分割されたオーディエンスの 1 つをコントロール母集団としてマークすることもできます。

![コントロール母集団の切り替えが有効になっています。 これにより、分割されたオーディエンスの 1 つをコントロール母集団としてマークできます。](../images/ui/audience-builder/control-group.png)

### 属性で分割 {#split-attribute}

属性別に分割する場合、オーディエンスは提供された属性に基づいて分割されます。 分割する属性を選択するには、 **[!UICONTROL 分割]** ブロック、続いて ![フィルター](../images/ui/audience-builder/filter-attribute.png) アイコン

![フィルターボタンが選択され、属性でフィルターする方法が表示されます。](../images/ui/audience-builder/select-attribute-split.png)

プロファイル属性のリストが表示されます。 属性タイプを選択し、その後に **[!UICONTROL 選択]** をクリックして、分割ブロックに追加します。

![属性のリストが表示されます。](../images/ui/audience-builder/select-attribute.png)

属性を選択した後、 **[!UICONTROL 値]** フィールドに入力します。

![属性を分割する値が追加されます。](../images/ui/audience-builder/attribute-split-values.png)

また、 **[!UICONTROL その他のプロファイル]** 切り替えて、選択されていないすべてのプロファイルで構成されるサブオーディエンスを作成します。

![「その他のプロファイル」切り替えがハイライト表示されます。](../images/ui/audience-builder/attribute-split-other-profiles.png)

## オーディエンスのパブリッシュ

オーディエンスを作成した後、「 」を選択して保存および公開できます。 **[!UICONTROL 公開]**.

![「公開」ボタンがハイライト表示され、オーディエンスの保存および公開方法が示されます。](../images/ui/audience-builder/publish-audience.png)

オーディエンスの作成でエラーが発生した場合は、アラートが表示され、問題の解決方法を知らせます。

![「公開」ボタンがハイライト表示され、オーディエンスの保存および公開方法が示されます。](../images/ui/audience-builder/audience-alert.png)

## 次の手順

Audience Builder には、様々なブロックタイプからオーディエンスを作成できる豊富なワークフローが用意されています。 セグメント化サービスの UI の他の部分の詳細については、 [セグメント化サービスユーザーガイド](./overview.md).
---
title: アカウントオーディエンス
description: アカウントオーディエンスを作成し、使用してダウンストリームの宛先のアカウントプロファイルをターゲット設定する方法について説明します。
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 047930d6-939f-4418-bbcb-8aafd2cf43ba
source-git-commit: 7d630c3673304060ad26375955602440a495f354
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 29%

---

# アカウントオーディエンス

>[!AVAILABILITY]
>
>アカウントオーディエンスは、 [B2B エディションオブReal-time Customer Data Platform](../../rtcdp/overview.md#rtcdp-b2b) そして [Real-time Customer Data PlatformB2P エディション](../../rtcdp/overview.md#rtcdp-b2p).

Adobe Experience Platformでは、アカウントのセグメント化により、ユーザーベースのオーディエンスからアカウントベースのオーディエンスに至るまで、マーケティングセグメント化のエクスペリエンスを完全に簡単かつ高度にすることができます。

アカウントオーディエンスをアカウントベースの宛先の入力として使用し、ダウンストリームサービス内のこれらのアカウント内のユーザーをターゲティングできます。 例えば、アカウントベースのオーディエンスを使用して、 **not** COO（最高経営責任者）または CMO（最高マーケティング責任者）という役職の人に関する連絡先情報を持っています。

## 用語 {#terminology}

アカウントオーディエンスの使用を開始する前に、様々なオーディエンスタイプの違いを確認してください。

- **アカウントオーディエンス**：アカウントオーディエンスは、 **アカウント** プロファイルデータ。 アカウントプロファイルデータを使用して、ダウンストリームアカウント内のユーザーをターゲットにするオーディエンスを作成できます。 アカウントプロファイルの詳細については、 [アカウントプロファイルの概要](../../rtcdp/accounts/account-profile-overview.md).
- **People オーディエンス**：人物オーディエンスは、 **顧客** プロファイルデータ。 顧客プロファイルデータを使用して、ビジネス顧客をターゲットにするオーディエンスを作成できます。 顧客プロファイルの詳細については、 [リアルタイム顧客プロファイルの概要](../../profile/home.md).
- **見込み客のオーディエンス**：見込み客オーディエンスは、 **見込み客** プロファイルデータ。 見込み客プロファイルデータを使用して、未認証ユーザーからオーディエンスを作成できます。 見込み客プロファイルの詳細については、 [見込み客プロファイルの概要](../../profile/ui/prospect-profile.md).

## アクセス {#access}

アカウントオーディエンスにアクセスするには、 **[!UICONTROL オーディエンス]** （内） **[!UICONTROL アカウント]** 」セクションに入力します。

![「アカウント」セクション内で「オーディエンス」ボタンが強調表示されます。](../images/ui/account-audiences/select.png)

The [!UICONTROL 参照] ページが表示され、組織のすべてのアカウントオーディエンスのリストが表示されます。

![組織に属するアカウントオーディエンスが表示されます。](../images/ui/account-audiences/browse.png)

このビューには、名前、プロファイル数、接触チャネル、ライフサイクルステータス、作成日、最終更新日など、オーディエンスに関する情報が一覧表示されます。

また、検索とフィルタリング機能を使用して、特定のアカウントオーディエンスをすばやく検索したり、並べ替えたりすることもできます。 この機能の詳細については、 [セグメント化 UI ガイド](./overview.md#manage-audiences).

## オーディエンスを作成 {#create}

>[!NOTE]
>
>アカウントのオーディエンスは、 **バッチ** セグメント化を行い、24 時間ごとに評価されます。

アカウントオーディエンスを作成するには、 **[!UICONTROL オーディエンスを作成]** の [!UICONTROL 参照] ページに貼り付けます。

![The [!UICONTROL オーディエンスを作成] アカウントオーディエンスの参照ページでボタンが強調表示されます。](../images/ui/account-audiences/select-create-audience.png)

セグメントビルダーが表示されます。アカウント属性とオーディエンスが左側のナビゲーションバーに表示されます。 の下 [!UICONTROL 属性] 「 」タブでは、プラットフォームで作成した属性とカスタム属性の両方を追加できます。

![セグメントビルダーが表示されています。属性とオーディエンスのみが表示されます。](../images/ui/account-audiences/segment-builder.png)

アカウントオーディエンスを作成する場合、イベントは **[!UICONTROL People]**&#x200B;これらの属性はユーザーに関連付けられるので、個々のタブではなく個々のユーザーのタブになります。

![イベントを検索する場所 ( [!UICONTROL People] フォルダーがハイライト表示されます。](../images/ui/account-audiences/attributes.png)

の下 [!UICONTROL オーディエンス] 「 」タブを使用すると、以前に作成したユーザーベースのオーディエンスを追加して、独自のアカウントオーディエンスを作成する際に基づいてオーディエンスを構築できます。

![セグメントビルダー内の「オーディエンス」タブが強調表示されます。](../images/ui/account-audiences/audiences.png)

セグメントビルダーの使用について詳しくは、[セグメントビルダー UI ガイド](./segment-builder.md)を参照してください。

## オーディエンスを有効化 {#activate}

>[!NOTE]
>
>アカウントオーディエンスをサポートする宛先は限られています。 このプロセスを続行する前に、アクティブ化する宛先でアカウントオーディエンスがサポートされていることを確認してください。

アカウントオーディエンスを作成したら、そのオーディエンスを他のダウンストリームサービスに対してアクティブ化できます。

アクティブ化するオーディエンスを選択し、その後に **[!UICONTROL 宛先に対して有効化]**.

![The [!UICONTROL 宛先に対して有効化] ボタンが、選択したオーディエンスのクイックアクションメニューでハイライト表示されます。](../images/ui/account-audiences/activate.png)

The [!UICONTROL 宛先を有効化] ページが表示されます。 有効化プロセスの詳細については、次を参照してください：サポートされる宛先やフィールドマッピングの詳細など。 [アカウントオーディエンスを有効化](/help/destinations/ui/activate-account-audiences.md) チュートリアル

## 次の手順 {#next-steps}

このガイドを読むと、Adobe Experience Platformでアカウントオーディエンスを作成して使用する方法をより深く理解できます。 Platform での他のタイプのオーディエンスの使用方法については、 [セグメント化サービス UI ガイド](./overview.md).

## 付録 {#appendix}

次の節では、アカウントオーディエンスに関する追加情報を示します。

### アカウントセグメント化の検証 {#validation}

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_eventLookbackWindow"
>title="最大ルックバックウィンドウエラー"
>abstract="エクスペリエンスイベントの最大ルックバックウィンドウは 30 日間です。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_combinationMaxDepth"
>title="ネストされたコンテナの最大深度エラー"
>abstract="ネストされたコンテナの最大深度は **5** です。つまり、オーディエンスを作成する際に、ネストされたコンテナの数を 6 以上にすることは&#x200B;**できません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_combinationMaxBreadth"
>title="ルールの最大数エラー"
>abstract="1 つのコンテナ内のルールの最大数は **5** です。つまり、オーディエンスを作成する際に、1 つのコンテナ内のルールを 6 個以上にすることは&#x200B;**できません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_crossEntityMaxDepth"
>title="クロスエンティティの最大数エラー"
>abstract="1 つのオーディエンス内で使用できるクロスエンティティの最大数は **5** です。クロスエンティティとは、オーディエンス内で異なるエンティティ間を切り替えることです。例えば、アカウントからユーザーに、さらにマーケティングリストに移行するといったことです。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_allowCustomEntity"
>title="カスタムエンティティエラー"
>abstract="カスタムエンティティは使用でき&#x200B;**ません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_b2bBuiltInEntities"
>title="無効な B2B エンティティエラー"
>abstract="`_xdm.context.account`、`_xdm.content.opportunity`、`_xdm.context.profile`、`_xdm.context.experienceevent`、`_xdm.context.account-person`、`_xdm.classes.opportunity-person`、`_xdm.classes.marketing-list-member`、`_xdm.classes.marketing-list`、`_xdm.context.campaign-member`、`_xdm.classes.campaign` の B2B エンティティのみを使用できます。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_rhsMaxOptions"
>title="値の最大数エラー"
>abstract="1 つのフィールドに対して確認できる値の最大数は **50** です。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_allowInSegmentByReference"
>title="inSegment イベントエラー"
>abstract="inSegment イベントは使用でき&#x200B;**ません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_allowInSegmentByValue"
>title="inSegment イベントエラー"
>abstract="inSegment イベントは使用でき&#x200B;**ません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_allowSequentialEvents"
>title="順次イベントエラー"
>abstract="順次イベントは使用でき&#x200B;**ません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_allowMaps"
>title="マップタイププロパティエラー"
>abstract="マップタイププロパティは使用でき&#x200B;**ません**。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_maxNestedAggregationDepth"
>title="ネストされたエンティティの最大深度エラー"
>abstract="ネストされた配列の最大深度は **5** です。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_maxObjectNestingLevel"
>title="ネストされたオブジェクトの最大数エラー"
>abstract="許可されるネストされたオブジェクトの最大数は、**10** です。"

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_generic"
>title="制約違反"
>abstract="オーディエンスが制約に違反しています。詳しくは、リンク先のドキュメントを参照してください。"

アカウントオーディエンスを使用する場合、オーディエンス **必須** 次の制約に従います。

>[!NOTE]
>
>次のリストに、 **デフォルト** アカウントオーディエンスの制約。 これらの値 **may** 組織の管理者が実装した設定に応じて、を変更します。

- エクスペリエンスイベントのルックバックウィンドウの最大数は次のとおりです。 **30 日**.
- ネストされたコンテナの最大の深さは次のとおりです。 **5**.
   - つまり、オーディエンスを作成する際に、ネストされたコンテナの数を 6 以上にすることは&#x200B;**できません**。
- 1 つのコンテナ内のルールの最大数は次のとおりです。 **5**.
   - これは、オーディエンスが **できません** オーディエンスを構成するルールが 5 つ以上ある。
- 使用できるクロスエンティティの最大数は次のとおりです。 **5**.
   - クロスエンティティとは、オーディエンス内で異なるエンティティ間を切り替えることです。例えば、アカウントからユーザーに、さらにマーケティングリストに移行するといったことです。
- カスタムエンティティ **できません** を使用します。
- 1 つのフィールドに対して確認できる値の最大数は **50** です。
   - 例えば、「市区町村名」というフィールドがある場合、その値を 50 の市区町村名に対してチェックできます。
- アカウントオーディエンス **できません** use `inSegment` イベント。
- アカウントオーディエンス **できません** 順次イベントを使用します。
- アカウントオーディエンス **できません** マップを使用します。
- ネストされた配列の最大深度は **5** です。
- ネストされたオブジェクトの最大数は次のとおりです。 **10**.

---
title: アカウントオーディエンス
description: アカウントオーディエンスを作成および使用してダウンストリーム宛先でアカウントプロファイルをターゲットにする方法を説明します。
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 047930d6-939f-4418-bbcb-8aafd2cf43ba
source-git-commit: c2f9bcd9aeb0073b8b26413ec29e2dff1ee5c80d
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 30%

---

# アカウントオーディエンス

>[!AVAILABILITY]
>
>アカウントオーディエンスは、 [Real-time Customer Data Platformの B2B 版](../../rtcdp/overview.md#rtcdp-b2b) および [Real-time Customer Data Platformの B2P 版](../../rtcdp/overview.md#rtcdp-b2p).

アカウントのセグメント化で、Adobe Experience Platformを使用すると、ユーザーベースのオーディエンスからアカウントベースのオーディエンスまで、マーケティングセグメント化エクスペリエンスの完全な使いやすさと洗練さを実現できます。

アカウントオーディエンスは、アカウントベースの宛先の入力として使用できます。これにより、ダウンストリームのサービスでこれらのアカウント内のユーザーをターゲットにすることができます。 例えば、アカウントベースのオーディエンスを使用して、実行するすべてのアカウントのレコードを取得できます **ではない** 最高執行責任者（COO）または最高マーケティング責任者（CMO）という肩書を持つ人物の連絡先情報を持っています。

## 用語 {#terminology}

アカウントオーディエンスを使用する前に、様々なオーディエンスタイプの違いを確認してください。

- **アカウントオーディエンス**：アカウントオーディエンスは、を使用して作成されるオーディエンスです **アカウント** プロファイルデータ。 アカウントプロファイルデータを使用すると、ダウンストリームアカウント内のユーザーをターゲットにしたオーディエンスを作成できます。 アカウントプロファイルの詳細については、を参照してください。 [アカウントプロファイルの概要](../../rtcdp/accounts/account-profile-overview.md).
- **人物オーディエンス**：人物オーディエンスは、を使用して作成されたオーディエンスです **顧客** プロファイルデータ。 顧客プロファイルデータを使用すると、ビジネスの顧客をターゲットにしたオーディエンスを作成できます。 顧客プロファイルについて詳しくは、を参照してください。 [リアルタイム顧客プロファイルの概要](../../profile/home.md).
- **見込み客オーディエンス**：見込み客オーディエンスは、を使用して作成されたオーディエンスです **見込み客** プロファイルデータ。 見込み客プロファイルデータを使用すると、認証されていないユーザーからオーディエンスを作成できます。 見込み客プロファイルについて詳しくは、を参照してください。 [見込み客プロファイルの概要](../../profile/ui/prospect-profile.md).

## アクセス {#access}

アカウントオーディエンスにアクセスするには、次を選択します。 **[!UICONTROL オーディエンス]** が含まれる **[!UICONTROL アカウント]** セクション。

![「アカウント」セクション内で「オーディエンス」ボタンがハイライト表示されます。](../images/ui/account-audiences/select.png)

この [!UICONTROL 参照] ページが表示され、組織のすべてのアカウントオーディエンスのリストが表示されます。

![組織に属するアカウントオーディエンスが表示されます。](../images/ui/account-audiences/browse.png)

この表示には、名前、プロファイル数、接触チャネル、ライフサイクルステータス、作成日、最終更新日など、オーディエンスに関する情報がリストされます。

また、検索機能とフィルター機能を使用して、特定のアカウントオーディエンスをすばやく検索および並べ替えることもできます。 この機能の詳細については、を参照してください。 [オーディエンスポータルの概要](./audience-portal.md#manage-audiences).

## オーディエンスを作成 {#create}

>[!NOTE]
>
>アカウントオーディエンスは次を使用して評価されます **バッチ** セグメント化。24 時間ごとに評価されます。

アカウントオーディエンスを作成するには、次を選択します。 **[!UICONTROL オーディエンスを作成]** 日 [!UICONTROL 参照] ページ。

![この [!UICONTROL オーディエンスを作成] アカウントオーディエンスの参照ページでボタンがハイライト表示されます。](../images/ui/account-audiences/select-create-audience.png)

セグメントビルダーが表示されます。アカウント属性とオーディエンスが左側のナビゲーションバーに表示されます。 の下 [!UICONTROL 属性] タブでは、Platform で作成された属性とカスタム属性の両方を追加できます。

![セグメントビルダーが表示されています。属性とオーディエンスのみが表示されます。](../images/ui/account-audiences/segment-builder.png)

アカウントオーディエンスを作成する場合、イベントはの下に表示されることに注意してください **[!UICONTROL 人物]**&#x200B;これらの属性は人物に関連付けられているので、独自のタブではなく、

![イベントを検索する場所（の範囲内） [!UICONTROL 人物] フォルダーがハイライト表示されている様子](../images/ui/account-audiences/attributes.png)

の下 [!UICONTROL オーディエンス] タブを使用すると、以前に作成した人物ベースのオーディエンスを追加して、独自のアカウントオーディエンスの作成時に構築できます。

![セグメントビルダー内の「オーディエンス」タブがハイライト表示されている様子。](../images/ui/account-audiences/audiences.png)

セグメントビルダーの使用について詳しくは、[セグメントビルダー UI ガイド](./segment-builder.md)を参照してください。

## オーディエンスをアクティベート {#activate}

>[!NOTE]
>
>アカウントオーディエンスをサポートする宛先は、限られています。 このプロセスを続行する前に、アカウントオーディエンスをサポートするアクティブ化する宛先を確認してください。

アカウントオーディエンスを作成したら、そのオーディエンスを他のダウンストリームサービスに対してアクティブ化できます。

アクティベートするオーディエンスを選択し、続いて「」を選択します **[!UICONTROL 宛先に対してアクティブ化]**.

![この [!UICONTROL 宛先に対してアクティブ化] 選択したオーディエンスのクイックアクションメニューで「」ボタンがハイライト表示されます。](../images/ui/account-audiences/activate.png)

この [!UICONTROL 宛先をアクティブ化] ページが表示されます。 サポートされている宛先やフィールドマッピングなど、アクティベーションプロセスの詳細については、を参照してください。 [アカウントオーディエンスの有効化](/help/destinations/ui/activate-account-audiences.md) チュートリアル。

## 次の手順 {#next-steps}

このガイドを読むことで、Adobe Experience Platformでアカウントオーディエンスを作成および使用する方法について、理解が深まりました。 Platform での他のタイプのオーディエンスの使用方法については、を参照してください。 [セグメント化サービス UI ガイド](./overview.md).

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

アカウントオーディエンスを使用する場合、オーディエンス **が** 次の制約に準拠します。

>[!NOTE]
>
>次のリストは、 **default** アカウントオーディエンスの制約。 これらの値 **年 5 月** 組織の管理者が実装した設定に応じて変更します。

- エクスペリエンスイベントの最大ルックバックウィンドウはです **30 日間**.
- ネストされたコンテナの最大深度は次のとおりです **5**.
   - つまり、オーディエンスを作成する際に、ネストされたコンテナの数を 6 以上にすることは&#x200B;**できません**。
- 1 つのコンテナ内のルールの最大数は次のとおりです **5**.
   - これは、オーディエンスを意味します **できません** オーディエンスを構成する 5 つ以上のルールがある。
- 使用できるクロスエンティティの最大数はです **5**.
   - クロスエンティティとは、オーディエンス内で異なるエンティティ間を切り替えることです。例えば、アカウントからユーザーに、さらにマーケティングリストに移行するといったことです。
- カスタムエンティティ **できません** を使用します。
- 1 つのフィールドに対して確認できる値の最大数は **50** です。
   - 例えば、「市区町村名」のフィールドがある場合、50 個の市区町村名とその値を照合できます。
- アカウントオーディエンス **できません** use `inSegment` イベント。
- アカウントオーディエンス **できません** 順次イベントを使用します。
- アカウントオーディエンス **できません** マップを使用します。
- ネストされた配列の最大深度は **5** です。
- ネストされたオブジェクトの最大数はです。 **10**.

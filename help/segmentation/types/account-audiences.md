---
title: アカウントオーディエンス
description: アカウントオーディエンスを作成および使用してダウンストリーム宛先でアカウントプロファイルをターゲットにする方法を説明します。
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 047930d6-939f-4418-bbcb-8aafd2cf43ba
source-git-commit: 6cb5afb78502c34e0eba99af29d7a67119b7e05a
workflow-type: tm+mt
source-wordcount: '1461'
ht-degree: 23%

---

# アカウントオーディエンス

>[!AVAILABILITY]
>
>アカウントオーディエンスは、Real-Time Customer Data Platformの [B2B edition](../../rtcdp/overview.md#rtcdp-b2b) およびReal-Time Customer Data Platformの [B2P Edition](../../rtcdp/overview.md#rtcdp-b2p) でのみ使用できます。

アカウントのセグメント化で、Adobe Experience Platformを使用すると、ユーザーベースのオーディエンスからアカウントベースのオーディエンスまで、マーケティングセグメント化エクスペリエンスの完全な使いやすさと洗練さを実現できます。

アカウントオーディエンスは、アカウントベースの宛先の入力として使用できます。これにより、ダウンストリームのサービスでこれらのアカウント内のユーザーをターゲットにすることができます。 例えば、アカウントベースのオーディエンスを使用して、最高執行責任者（COO）または最高マーケティング責任者（CMO **という肩書を持つ人物の連絡先情報を持たない** 持たない）すべてのアカウントの記録を取得できます。

>[!NOTE]
>
>B2B アーキテクチャのアップグレードの一環として、B2B エンティティを使用するオーディエンスのオーディエンスサイズの予測が、正確な精度で計算されるようになりました。 これらの予測値はプレビュー時に使用でき、B2B の複雑な関係を含むオーディエンスに対して、より正確で信頼性の高いインサイトを提供します。 <br> 詳しくは、[Real-Time CDP B2B edition アーキテクチャのアップグレードの概要 &#x200B;](../../rtcdp/b2b-architecture-upgrade.md) を参照してください。

## 用語 {#terminology}

アカウントオーディエンスを使用する前に、様々なオーディエンスタイプの違いを確認してください。

- **アカウントオーディエンス**：アカウントオーディエンスは、**アカウント** プロファイルデータを使用して作成されたオーディエンスです。 アカウントプロファイルデータを使用すると、ダウンストリームアカウント内のユーザーをターゲットにしたオーディエンスを作成できます。 アカウントプロファイルについて詳しくは、[&#x200B; アカウントプロファイルの概要 &#x200B;](../../rtcdp/accounts/account-profile-overview.md) を参照してください。
- **人物オーディエンス**：人物オーディエンスは、**顧客** プロファイルデータを使用して作成されたオーディエンスです。 顧客プロファイルデータを使用すると、ビジネスの顧客をターゲットにしたオーディエンスを作成できます。 顧客プロファイルについて詳しくは、[&#x200B; リアルタイム顧客プロファイルの概要 &#x200B;](../../profile/home.md) を参照してください。
- **見込み客オーディエンス**：見込み客オーディエンスは、**見込み客** プロファイルデータを使用して作成されたオーディエンスです。 見込み客プロファイルデータを使用すると、認証されていないユーザーからオーディエンスを作成できます。 見込み客プロファイルについて詳しくは、[&#x200B; 見込み客プロファイルの概要 &#x200B;](../../profile/ui/prospect-profile.md) を参照してください。

## アクセス {#access}

アカウントオーディエンスにアクセスするには、「**[!UICONTROL Audiences]**」セクションで「**[!UICONTROL Accounts]**」を選択します。

![&#x200B; 「アカウント」セクション内で「オーディエンス」ボタンがハイライト表示されています。](../images/types/account/select.png)

[!UICONTROL Browse] ページが表示され、組織のすべてのアカウントオーディエンスのリストが表示されます。

![&#x200B; 組織に属するアカウントオーディエンスが表示されます。](../images/types/account/browse.png)

この表示には、名前、プロファイル数、接触チャネル、ライフサイクルステータス、作成日、最終更新日など、オーディエンスに関する情報がリストされます。

また、検索機能とフィルター機能を使用して、特定のアカウントオーディエンスをすばやく検索および並べ替えることもできます。 この機能について詳しくは、[&#x200B; オーディエンスポータルの概要 &#x200B;](../ui/audience-portal.md#manage-audiences) を参照してください。

## オーディエンスを作成 {#create}

>[!NOTE]
>
>アカウントオーディエンスは、**バッチ** セグメント化を使用して評価され、24 時間ごとに評価されます。

アカウントオーディエンスを作成するには、**[!UICONTROL Create audience]** のページで「[!UICONTROL Browse]」を選択します。

![&#x200B; アカウントオーディエンスの参照ページで「[!UICONTROL Create audience]」ボタンがハイライト表示されている様子 &#x200B;](../images/types/account/select-create-audience.png)

セグメントビルダーが表示されます。アカウント属性とオーディエンスが左側のナビゲーションバーに表示されます。 「[!UICONTROL Attributes]」タブで、Experience-Platform-created 属性とカスタム属性の両方を追加できます。

![セグメントビルダーが表示されています。属性とオーディエンスのみが表示されます。](../images/types/account/segment-builder.png)

「[!UICONTROL Audiences]」タブでは、以前に作成した人物ベースのオーディエンスを追加して、独自のアカウントオーディエンスの作成時に構築できます。

![&#x200B; セグメントビルダー内の「オーディエンス」タブがハイライト表示されています。](../images/types/account/audiences.png)

セグメントビルダーの使用について詳しくは、[セグメントビルダー UI ガイド](../ui/segment-builder.md)を参照してください。

### 関係の確立 {#relationships}

アカウントオーディエンスの場合、デフォルトでは、セグメントビルダー UI にアカウントとユーザーの間の直接の関係が表示されます。 ただし、アカウントオーディエンスには、他の関係タイプも使用できます。

別の関係タイプを使用するには、![&#x200B; 設定アイコン &#x200B;](../../images/icons/settings.png) を選択します。

![&#x200B; 「フィールド」セクションで設定アイコンがハイライト表示されている様子 &#x200B;](../images/types/account/select-settings.png)

「[!UICONTROL Settings]」タブで、「**[!UICONTROL Show relationship selectors]**」セクションの「**[!UICONTROL Relationship of fields]**」を選択します。

![&#x200B; 「関係セレクターを表示」切替スイッチは、「設定」タブの「フィールドの関係」セクションで選択します。](../images/types/account/show-relation-selectors.png)

![&#x200B; 設定アイコン &#x200B;](../../images/icons/settings.png) を再度選択して、「設 [!UICONTROL Fields]」タブに戻ります。 **[!UICONTROL Establish relationships]** の節が表示され、アカウントとユーザーとの接続方法およびユーザーと商談との接続方法を確立できます。

![&#x200B; 「関係の確立」セクションがハイライト表示され、アカウントを人物に接続する方法と人物を商談に接続する方法に関するオプションが表示されています。](../images/types/account/establish-relationships.png)

アカウントを人物に接続する際は、次のいずれかのオプションを選択できます。

| オプション | 説明 |
| ------ | ----------- |
| 直接的な関係 | アカウントと人物の間の直接接続。 これは、人物スキーマの `accountID` 配列に含まれる `personComponents` 値の配列を介して、各ユーザーがリンクされているアカウントを指定します。 このパスは、最も頻繁に使用されます。 |
| アカウントと人物の関係 | アカウントとユーザーの関係。`accountPersonRelation` オブジェクトで定義されます。 また、このパスを使用すると、各ユーザーを複数のアカウントに接続することもできます。 組織がソースデータから明示的な関係テーブルを定義した場合に使用されます。 |
| 商談と担当者の関係 | オポチュニティと人物の関係。`opportunityPersonRelation` オブジェクトで定義されます。 これにより、opportunity-person から opportunity に移動してアカウントに接続されます。 これにより、その人物がどの会社の商談に関連付けられているかを説明できます。 |

機会を人物に接続する際は、次のオプションから選択できます。

| オプション | 説明 |
| ------ | ----------- |
| アカウント | アカウントとオポチュニティの間の直接接続。 アカウントオーディエンスでこれを使用する場合、このパスによって会社のすべてのユーザーが機会に接続されます。 |
| 商談と担当者の関係 | 機会と人物の関係。機会 – 人物オブジェクトに基づいています。 このパスは、機会に関与したと特定されたユーザーのみをその機会につなげます。 |

目的の関係を確立したら、必要な人物オーディエンスをセグメント定義に追加できます。

## オーディエンスをアクティベート {#activate}

>[!NOTE]
>
>アカウントオーディエンスをサポートする宛先は、限られています。 このプロセスを続行する前に、アカウントオーディエンスをサポートするアクティブ化する宛先を確認してください。

アカウントオーディエンスを作成したら、そのオーディエンスを他のダウンストリームサービスに対してアクティブ化できます。

アクティベートするオーディエンスを選択し、続いて「**[!UICONTROL Activate to destination]**」を選択します。

![&#x200B; 選択したオーディエンスのクイックアクションメニューで「[!UICONTROL Activate to destination]」ボタンがハイライト表示されます。](../images/types/account/activate.png)

[!UICONTROL Activate destination] ページが表示されます。 サポートされる宛先やフィールドマッピングなど、アクティベーションプロセスについて詳しくは、[&#x200B; アカウントオーディエンスのアクティブ化 &#x200B;](/help/destinations/ui/activate-account-audiences.md) チュートリアルをお読みください。

## 次の手順 {#next-steps}

このガイドを読むことで、Adobe Experience Platformでアカウントオーディエンスを作成および使用する方法について、理解が深まりました。 Experience Platformで他のタイプのオーディエンスを使用する方法については、[&#x200B; オーディエンスタイプの概要 &#x200B;](./overview.md) を参照してください。

## 付録 {#appendix}

次の節では、アカウントオーディエンスに関する追加情報を示します。

### アカウントセグメント化の検証 {#validation}

>[!CONTEXTUALHELP]
>id="platform_audiences_account_constraint_eventLookbackWindow"
>title="ルックバックウィンドウ"
>abstract="ルックバックウィンドウを使用して、ユーザーレベルのイベントの完全な履歴を表示します。"

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

アカウントオーディエンスを使用する場合、オーディエンス **必須** は次の制約に従います。

- ネストされたコンテナの最大深度は **5** です。
   - つまり、オーディエンスを作成する際に、ネストされたコンテナの数を 6 以上にすることは&#x200B;**できません**。
- 1 つのコンテナ内のルールの最大数は **5** です。
   - つまり、オーディエンスには、オーディエンスを構成する 5 つ以上のルールが含まれています **できません**。
- 使用できるクロスエンティティの最大数は **5** です。
   - クロスエンティティとは、オーディエンス内で異なるエンティティ間を切り替えることです。例えば、アカウントからユーザーに、さらにマーケティングリストに移行するといったことです。
- 1 つのフィールドに対して確認できる値の最大数は **50** です。
   - 例えば、「市区町村名」のフィールドがある場合、50 個の市区町村名とその値を照合できます。
- アカウントオーディエンス **使用** マップは使用できません。
- アカウントオーディエンス **使用できません** イベント。
- ネストされた配列の最大深度は **5** です。
- ネストされたオブジェクトの最大数は **10** です。

<!-- - The maximum lookback window for Experience Events is **30 days**. -->
<!-- - Account audiences **cannot** use `inSegment` events. -->
<!-- - Custom entities **cannot** be used. -->
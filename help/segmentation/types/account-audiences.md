---
title: アカウントAudiences
description: アカウントオーディエンスを作成および使用して、ダウンストリームの宛先でアカウントプロファイルターゲット方法について説明します。
badgeB2B: label="B2B版" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
badgeB2P: label="B2P エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 047930d6-939f-4418-bbcb-8aafd2cf43ba
source-git-commit: 1e508ec11b6d371524c87180a41e05ffbacc2798
workflow-type: tm+mt
source-wordcount: '1498'
ht-degree: 23%

---

# アカウントオーディエンス

>[!AVAILABILITY]
>
>アカウント オーディエンスは、Real-時間 Customer データ Platform の [&#128279;](../../rtcdp/overview.md#rtcdp-b2b)B2B Edition と、Real-時間 Customer データ Platform[&#x200B; の &#x200B;](../../rtcdp/overview.md#rtcdp-b2p)B2P Edition でのみ使用できます。

アカウントセグメント化を使用すると、Adobe Experience Platformを使用すると、ユーザーベースのオーディエンスからアカウントベースのオーディエンスにエクスペリエンスマーケティングセグメント化の完全な容易さと洗練をもたらすことができます。

アカウントオーディエンスは、アカウントベースの宛先の入力として使用でき、ダウンストリームサービスでそれらのアカウント内のユーザーをターゲットできます。 たとえば、アカウントベースのオーディエンスを使用して、最高執行責任者 (COO) または最高マーケティング責任者 (CMO) の肩書きを持つユーザーの連絡先情報を持たない **&#x200B;**&#x200B;すべてのアカウントのレコードを取得できます。

>[!NOTE]
>
>B2B アーキテクチャのアップグレードの一環として、B2Bエンティティを持つオーディエンスの オーディエンス サイズの見積もりが正確な精度で計算されるようになりました。 これらの見積もりはプレビュー中に利用でき、複雑なB2B関係を含むオーディエンスに対して、より正確で信頼性の高い分析情報を提供します。 <br>詳細については、 [Real-時間 CDP B2B Edition アーキテクチャ アップグレードの概要](../../rtcdp/b2b-architecture-upgrade.md)を参照してください。

## 用語 {#terminology}

アカウントオーディエンスの使用を始める前に、異なるオーディエンスタイプの違いを確認してください。

- **アカウントオーディエンス**: アカウント オーディエンスは、 **アカウント** プロファイル データを使用して作成されるオーディエンスです。 アカウントプロファイルデータを使用して、ダウンストリームアカウント内のユーザーターゲットオーディエンスを作成できます。 アカウントプロファイルの詳細については、 [アカウントプロファイルの概要](../../rtcdp/accounts/account-profile-overview.md)を参照してください。
- **ユーザーオーディエンス**: ユーザーオーディエンスは、 **顧客** プロファイル データを使用して作成されるオーディエンスです。 顧客プロファイルデータを使用して、ビジネスの顧客をターゲットとするオーディエンスを作成できます。 顧客 プロファイルの詳細については、「 [リアル時間 カスタマー プロフィールの概要](../../profile/home.md)を参照してください。
- **見込み客オーディエンス**: 見込み客オーディエンスは、 **見込み客** プロファイルデータを使用して作成されるオーディエンスです。 見込み客プロファイルデータを使用して、認証されていないユーザーからオーディエンスを作成できます。 見込み客プロファイルの詳細については、 [見込み客プロファイルの概要](../../profile/ui/prospect-profile.md)を参照してください。

## アクセス {#access}

アカウントオーディエンスにアクセスするには、[**[!UICONTROL Audiences]**] セクションで [**[!UICONTROL Accounts]**] を選択します。

![[アカウント] セクション内でAudiencesボタンが強調表示されます。](../images/types/account/select.png)

[!UICONTROL Browse]ページが表示され、組織のすべてのアカウント対象ユーザーのリストが表示されます。

![組織に属するアカウントオーディエンスが表示されます。](../images/types/account/browse.png)

この表示には、名前、プロファイル数、接触チャネル、ライフサイクル ステータス、作成日、最終更新日など、オーディエンスに関する情報が一覧表示されます。

また、検索機能とフィルタリング機能を使用して、特定のアカウントオーディエンスをすばやく検索および並べ替えることもできます。 この機能の詳細については詳細、「 [オーディエンス ポータルの概要](../ui/audience-portal.md#manage-audiences)」を参照してください。

## オーディエンスを作成 {#create}

>[!NOTE]
>
>アカウントオーディエンスは **batch** セグメント化を使用して評価され、24 時間ごとに評価されます。

アカウントオーディエンスを作成するには、**[!UICONTROL Create audience]**&#x200B;ページで「[!UICONTROL Browse]」を選択します。

![参照ページオーディエンスアカウント上で [!UICONTROL Create audience] ボタンがハイライト表示されます。](../images/types/account/select-create-audience.png)

セグメントビルダーが表示されます。アカウント属性とオーディエンスが左側のナビゲーションバーに表示されます。 [!UICONTROL Attributes]タブでは、エクスペリエンスPlatform作成属性とカスタム属性の両方を追加できます。

![セグメントビルダーが表示されています。属性とオーディエンスのみ表示されることに注意してください。](../images/types/account/segment-builder.png)

アカウントオーディエンスを作成する場合、イベントは人に関連付けられているため、イベントは独自のタブではなく **[!UICONTROL People]**&#x200B;の下にリストされることに注意してください。

![[!UICONTROL People]フォルダー内にある、イベントの検索場所が強調表示されます。](../images/types/account/attributes.png)

[!UICONTROL Audiences]タブでは、以前に作成したユーザーベースのオーディエンスを追加して、独自のアカウントオーディエンスを作成するときにビルドできます。

![セグメントビルダー内のAudiencesタブがハイライト表示されます。](../images/types/account/audiences.png)

セグメントビルダーの使用について詳しくは、[セグメントビルダー UI ガイド](../ui/segment-builder.md)を参照してください。

### 関係の確立 {#relationships}

アカウントオーディエンスのデフォルトでは、セグメントビルダー UIにはアカウントと人の直接の関係が表示されます。 ただし、アカウントオーディエンスでは他の関係タイプも使用できます。

代替リレーションシップ タイプを使用するには、 ![設定アイコン](../../images/icons/settings.png)を選択します。

![[フィールド] セクションで設定アイコンが強調表示されます。](../images/types/account/select-settings.png)

[!UICONTROL Settings] タブの [**[!UICONTROL Show relationship selectors]**] セクションの [**[!UICONTROL Relationship of fields]**] を選択します。

![表示リレーションセレクターの切り替えは、設定タブの [フィールドのリレーションシップ] セクションで選択します。](../images/types/account/show-relation-selectors.png)

![設定アイコン](../../images/icons/settings.png)を再度選択して、[!UICONTROL Fields]タブに戻ります。これで、[ **[!UICONTROL Establish relationships]** ] セクションが表示され、アカウントと個人がどのように接続されるか、および個人が機会にどのように接続されているかを確立できます。

![[リレーションシップの確立] セクションが強調表示され、アカウントを個人に接続する方法と、個人を機会に接続する方法のオプションが表示されます。](../images/types/account/establish-relationships.png)

アカウントをユーザーに接続するときは、次のオプションから選択できます。

| オプション | 説明 |
| ------ | ----------- |
| 直接的な関係 | アカウントと人の間の直接的なつながり。 これは、個人スキーマの`accountID`配列内の`personComponents`値の配列を介して、各個人がリンクされているアカウントを指定します。このパスは最も頻繁に使用されます。 |
| 経理と個人の関係 | `accountPersonRelation`オブジェクトによって定義される、アカウントと人の関係。このパスにより、各ユーザーを複数のアカウントに接続することもできます。 これは、組織がソース データから明示的なリレーションシップ テーブルを定義した場合に使用されます。 |
| オポチュニティとパーソンの関係 | `opportunityPersonRelation`オブジェクトによって定義される、機会と人の関係。これは、機会人から機会人、アカウントに移動することによって、人をアカウントに接続します。 これにより、その個人が商談に所属する会社を記述できます。 |

機会をユーザーに接続するときは、次のオプションから選択できます。

| オプション | 説明 |
| ------ | ----------- |
| アカウント | アカウントと機会間の直接接続。 これをアカウントオーディエンスで使用すると、このパス会社にいるすべての人を機会に接続できます。 |
| オポチュニティとパーソンの関係 | 機会人オブジェクトに基づく、機会と人の関係。 このパスは、機会に関与していると明確に特定された人々のみをその機会結び付けます。 |

目的の関係を確立したら、必要な 人とオーディエンスをセグメント定義に追加できます。

## オーディエンスをアクティベート {#activate}

>[!NOTE]
>
>アカウントオーディエンスをサポートする宛先は限られています。 このプロセスを続行する前に、アクティブ化する宛先がアカウントオーディエンスをサポートしていることを確認してください。

アカウント オーディエンスを作成したら、そのオーディエンスを他のダウンストリーム サービスにアクティブ化できます。

アクティベートするオーディエンスを選択してから、 **[!UICONTROL Activate to destination]**&#x200B;を選択します。

![選択したオーディエンスのクイックアクションメニューで [!UICONTROL Activate to destination] ボタンがハイライト表示されます。](../images/types/account/activate.png)

[!UICONTROL Activate destination]ページが表示されます。サポートされている宛先やフィールドマッピングの詳細など、アクティベーションプロセスの詳細については、 [アカウントオーディエンスをアクティブ化](/help/destinations/ui/activate-account-audiences.md) チュートリアルをお読みください。

## 次の手順 {#next-steps}

このガイドを読むと、Adobe Experience Platformでアカウントオーディエンスを作成して使用する方法についての理解が深まりました。 Experience Platformで他のタイプのオーディエンスを使用する方法については、 [オーディエンスタイプの概要](./overview.md)を参照してください。

## 付録 {#appendix}

次の節では、アカウントオーディエンスに関する追加情報について説明します。

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

アカウントオーディエンスを使用する場合、オーディエンスは次の制約に従う必要があります&#x200B;**&#x200B;**

- ネストされたコンテナの最大の深さは **5** です。
   - つまり、オーディエンスを作成する際に、ネストされたコンテナの数を 6 以上にすることは&#x200B;**できません**。
- 1 つのコンテナ内のルールの最大数は **5** です。
   - つまり、オーディエンスには、オーディエンスを構成する 5 つ以上のルールを **できません** 。
- 使用できるクロスエンティティの最大数は **5** です。
   - クロスエンティティとは、オーディエンス内で異なるエンティティ間を切り替えることです。例えば、アカウントからユーザーに、さらにマーケティングリストに移行するといったことです。
- 1 つのフィールドに対して確認できる値の最大数は **50** です。
   - たとえば、「市区町村 名前」のフィールドがある場合、その値を 50 の都市名に対してチェックできます。
- アカウントオーディエンスは順次イベントを使用 **できません** 。
- アカウントオーディエンスはマップを使用 **できません** 。
- ネストされた配列の最大深度は **5** です。
- ネストできるオブジェクトの最大数は **10** です。

<!-- - The maximum lookback window for Experience Events is **30 days**. -->
<!-- - Account audiences **cannot** use `inSegment` events. -->
<!-- - Custom entities **cannot** be used. -->
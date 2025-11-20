---
keywords: Experience Platform;プロフィール;リアルタイム顧客プロファイル;トラブルシューティング;API;統一されたプロファイル;統一されたプロフィール;ユニファイド;プロフィール;RTCP;プロファイルを有効にします。プロファイルを有効にします。ユニオンスキーマ;ユニオンプロファイル;和集合プロファイル
title: Real-時間 Customer プロフィール UI ガイド
description: Real-時間 Customer プロフィールは、オンライン、オフライン、CRM、サードパーティ データなどの複数のチャネルからのデータを組み合わせて、各個人顧客の総合的な表示を作成します。 このドキュメントは、Adobe Experience Platform ユーザー インターフェイスで Real-時間 カスタマー プロフィールと対話するためのガイドとして機能します。
exl-id: 792a3a73-58a4-4163-9212-4d43d24c2770
source-git-commit: d2694170e2860bd32783ad3f1860b0397e847289
workflow-type: tm+mt
source-wordcount: '1921'
ht-degree: 5%

---

# [!DNL Real-Time Customer Profile] UI ガイド

[!DNL Real-Time Customer Profile] は、オンライン、オフライン、CRM、サードパーティ データなどの複数のチャネルからのデータを組み合わせて、各個人顧客の総合的な表示を作成します。 このドキュメントは、Adobe Experience Platform ユーザーインターフェイス（UI）で [!DNL Real-Time Customer Profile] データを操作するためのガイドとして機能します。

## はじめに

このUIガイドには、[!DNL Experience Platform]の管理に関連するさまざまな[!DNL Real-Time Customer Profiles]サービスを理解する必要があります。このガイドを読む前、またはUIで作業する前に、次のサービスのドキュメントを確認してください。

* [[!DNL Real-Time Customer Profile] 概要](../home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムのコンシューマー プロファイルを提供します。
* [[!DNL Identity Service]](../../identity-service/home.md):[!DNL Real-Time Customer Profile]に取り込まれる際に、異なるデータソースからの ID をブリッジすることで、[!DNL Experience Platform]を有効にします。
* [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

## [!UICONTROL Overview]

Experience Platform UI で、左側のナビゲーションで「**[!UICONTROL Profiles]**」を選択して、「**[!UICONTROL Overview]**」タブを開き、プロファイルダッシュボードを表示します。

>[!NOTE]
>
>Experience Platformを初めて使用する組織で、アクティブなプロファイルデータセットや結合ポリシーが作成されていない場合は、[!UICONTROL Profiles] ダッシュボードは表示されません。 代わりに、「[!UICONTROL Overview]」タブに、リアルタイム顧客プロファイルを初めて使用する際に役立つリンクやドキュメントが表示されます。

### プロファイルダッシュボード {#profile-dashboard}

プロファイルダッシュボードは、組織のプロファイルデータに関連する主要指標の概要を示します。

詳しくは、[ プロファイルダッシュボードガイド ](../../dashboards/guides/profiles.md) を参照してください。

![ プロファイルダッシュボードが表示されます。](../../dashboards/images/profiles/dashboard-overview.png)

## 「[!UICONTROL Browse]」タブ

「**[!UICONTROL Browse]**」タブでは、切り替えを選択して、プロファイルを **カード** 表示または **グラフ** 表示で表示できます。

![ カードとグラフの表示切り替えがハイライト表示されています。](../images/user-guide/card-graph-view.png)

さらに、結合ポリシーを使用してプロファイルを参照したり、ID 名前空間と値を使用して特定のプロファイルを検索したりできます。

![ 組織に属するプロファイルが表示されます。](../images/user-guide/profile-browse.png)

### [!UICONTROL Merge policy] で参照

「**[!UICONTROL Browse]**」タブは、デフォルトで組織のデフォルトの結合ポリシーに設定されています。 別の結合ポリシーを選択するには、結合ポリシー名の横にある `X` を選択し、セレクターを使用して結合ダイアログを開 **[!UICONTROL Select merge policy]** ます。

>[!NOTE]
>
>結合ポリシーが選択されていない場合は、**[!UICONTROL Merge policy]** フィールドの横にあるセレクターボタンを使用して、選択ダイアログを開きます。

![ 結合ポリシーセレクターがハイライト表示されている様子 ](../images/user-guide/browse-by-merge-policy.png)

**[!UICONTROL Select merge policy]** ダイアログで結合ポリシーを選択するには、ポリシー名の横のラジオボタンを選択し、**[!UICONTROL Select]** を使用して「[!UICONTROL Browse]」タブに戻ります。 その後、**[!UICONTROL View]** を選択してサンプルプロファイルを更新し、新しい結合ポリシーが適用されたプロファイルのサンプリングを確認できます。

![ フィルターする結合ポリシーを選択できるダイアログが表示されます。](../images/user-guide/select-merge-policy.png)

表示されるプロファイルは、選択した結合ポリシーが適用された後、組織のプロファイルストアからの最大 20 個のプロファイルのサンプルを表します。 組織のプロファイルストアに新しいデータが追加されると、選択した結合ポリシーのサンプルプロファイルが更新されます。

サンプルプロファイルの 1 つの詳細を表示するには、**[!UICONTROL Profile ID]** を選択します。 詳しくは、このガイドの後半の [ プロファイルの詳細の表示 ](#profile-detail) 節を参照してください。

![ 結合ポリシーに一致するサンプルプロファイルが表示されます。](../images/user-guide/profile-browse-table.png)

結合ポリシーとそのExperience Platform内での役割について詳しくは、[ 結合ポリシーの概要 ](../merge-policies/overview.md) を参照してください。

### [!UICONTROL Identity] で参照 {#browse-identity}

「**[!UICONTROL Browse]**」タブでは、ID 名前空間を使用して、ID 値で特定のプロファイルを検索できます。 ID による参照では、結合ポリシー、ID 名前空間および ID 値を指定する必要があります。

![ 結合ポリシーセレクターがハイライト表示されています。](../images/user-guide/browse-by-merge-policy.png)

必要に応じて、**[!UICONTROL Merge policy]** セレクターを使用して **[!UICONTROL Select merge policy]** ダイアログを開き、使用する結合ポリシーを選択します。

![ フィルターする結合ポリシーを選択できるダイアログが表示されます。](../images/user-guide/select-merge-policy.png)

次に、**[!UICONTROL Identity namespace]** セレクターを使用して **[!UICONTROL Select identity namespace]** ダイアログを開き、検索する名前空間を選択します。 組織に多くの名前空間がある場合、ダイアログの検索バーを使用して名前空間の名前の入力を開始できます。

名前空間を選択して追加の詳細を表示するか、ラジオボタンを選択して名前空間を選択できます。 その後、**[!UICONTROL Select]** を使用して続行できます。

![フィルターに使用する ID 名前空間を選択できるダイアログが表示されます。](../images/user-guide/select-identity-namespace.png)

[!UICONTROL Identity namespace]を選択して[!UICONTROL Browse]タブに戻ったら、選択した名前空間に関連する&#x200B;**[!UICONTROL Identity value]**&#x200B;を入力できます。

>[!NOTE]
>
>この値は、個々の顧客プロファイルに固有で、指定された名前空間の有効なエントリである必要があります。 例えば、ID 名前空間「メール」を選択するには、有効なメールアドレスの形式で ID 値が必要です。

![ フィルタリングの基準にする ID 値がハイライト表示されている様子。](../images/user-guide/browse-identity.png)

値を入力したら、「**[!UICONTROL View]**」を選択すると、値に一致する単一のプロファイルが返されます。 プロファイル表示 **[!UICONTROL Profile ID]** を選択します。

![ID 値に一致するプロファイルがハイライト表示されます。](../images/user-guide/filtered-identity-value.png)

## プロファイルを表示 {#view-profile}

>[!CONTEXTUALHELP]
>id="platform_errors_uplib_201001_404"
>title="エンティティが見つかりません"
>abstract="つまり、Experience Platform でリクエストされたエンティティを見つけることができませんでした。このエラーを解決するには、次のいずれかの解決策を試してください。<ul><li>アクセスしようとしているエンティティの URL に正しいプロファイル ID がリストされていることを確認する。</li><li>アクセスしようとしているエンティティの組織とサンドボックスの組み合わせが正しいことを確認する。</li></ul>"

**[!UICONTROL Profile ID]** を選択すると、「**[!UICONTROL Detail]**」タブが開きます。 「**[!UICONTROL Detail]**」タブに表示されるプロファイル情報は、複数のプロファイルフラグメントを結合し、個々の顧客の単一のビューを形成したものです。 これには、基本属性、リンクされた ID、チャネル環境設定などの顧客の詳細が含まれます。

さらに、プロファイルに関するその他の詳細（[ 属性 ](#attributes)、[ イベント ](#events)、[ オーディエンスメンバーシップ ](#audience-membership) など）を表示できます。

### 「詳細」タブ {#profile-detail}

**[!UICONTROL Details]**&#x200B;タブは、選択したプロファイルに関する詳細情報を提供し、顧客プロファイルインサイト、AI インサイトウィジェット、カスタマイズ可能なウィジェット、自動分類ウィジェットの 4 つのセクションに分かれています。

![プロファイル詳細ページが表示されます。](../images/user-guide/profile-details.png)

さらに、AI によって生成された分析情報を表示するかどうかを切り替えたり、エッジと比較したハブの詳細を表示したり、グラフ 表示で詳細を表示したりできます。

![上記のトグル (AI によって生成された分析情報、ハブまたはエッジ データ、カードまたはグラフ 表示) が強調表示されます。](../images/user-guide/profile-toggles.png)

#### 顧客のプロファイルインサイト {#customer-profile-insights}

**[!UICONTROL Customer profile insights]**&#x200B;セクションには、プロファイル属性の簡単な概要が表示されます。これには、プロファイル ID、メール、電話番号、性別、生年月日、およびプロファイルの ID とオーディエンスメンバーシップが含まれます。

![[顧客プロファイルインサイト] セクションが表示されます。](../images/user-guide/customer-profile-insights.png)

#### AI インサイトのウィジェット {#ai-insight-widgets}

[!BADGE アルファ]{type=Informative} この機能は現在アルファ中です。

**[!UICONTROL AI insight widgets]**&#x200B;セクションには、AI によって生成されたウィジェットが表示されます。これらのウィジェットは、人口統計(年齢、性別、場所など)、ユーザー行動(購入履歴、Webサイトのアクティビティ、ソーシャルメディアエンゲージメントなど)、サイコグラフィック(興味、好み、ライフスタイルの選択など)などのプロファイルデータに基づいて、プロファイルに関する迅速な洞察を提供します。 すべての AI ウィジェットは、プロファイルに存在する **既に** データを使用します。

![AI insight Widgets セクションが表示されます。](../images/user-guide/ai-insight-widgets.png)

#### カスタマイズ可能なウィジェット {#customizable-widgets}

**[!UICONTROL Customizable widgets]** のセクションには、ビジネスニーズに合わせてカスタマイズできるウィジェットが表示されます。 属性を別々のウィジェットにグループ化したり、不要なウィジェットを削除したり、ウィジェットのレイアウトを調整したりできます。

表示されるデフォルトのフィールドは、優先プロフィール属性を表示するために組織レベルで変更することもできます。 属性の追加と削除、ダッシュボードパネルのサイズ変更の手順など、これらのフィールドのカスタマイズについて詳しくは、[ プロファイルの詳細カスタマイズガイド ](profile-customization.md) を参照してください。

![ カスタマイズ可能なウィジェット セクションが表示されます。](../images/user-guide/customizable-widgets.png)

また、属性名を表示名として表示するか、フィールドパス名として表示するかを切り替えることもできます。 これら 2 つの表示を切り替えるには、「**[!UICONTROL Show display names]**」切り替えスイッチを選択します。

![ 表示名を表示切替スイッチがハイライト表示されています。](../images/user-guide/show-display-names.png)

#### 自動分類ウィジェット {#auto-classified-widgets}

[!BADGE Alpha]{type=Informative} この機能は現在Alphaにあります。

**[!UICONTROL Auto-classified widgets]** のセクションには、結合スキーマを利用して属性が属するソースフィールドグループを決定するためのウィジェットが表示され、データの元の場所に関するより明確なコンテキストが提供されます。 検索バーを使用すると、ウィジェット内でキーワードをより簡単に検索できます。

これらのウィジェットでは、イベントデータ（エクスペリエンスイベントウィジェットを含む）と属性データの両方を組み合わせることで、プロファイルのビューを統一できます。 これらのウィジェットを使用してプロファイルのデータの構造を調べ、[ カスタマイズ可能なウィジェット ](#customizable-widgets) をより適切に構成できます。

>[!NOTE]
>
>複数のソースフィールドグループがある場合、ウィジェットは使用可能なオプションの **1** のみを使用します。

![ 自動分類ウィジェット セクションが表示されます。](../images/user-guide/auto-classified-widgets.png)

### 「属性」タブ {#attributes}

「**[!UICONTROL Attributes]**」タブには、指定した結合ポリシーが適用された後に、単一のプロファイルに関連するすべての属性を要約したリストが表示されます。

これらの属性は、「」を選択することで、JSON オブジェクトとして表示することも **[!UICONTROL View JSON]** きます。 これは、プロファイル属性がExperience Platformにどのように取り込まれるかを理解を深めたい場合に役立ちます。

![ 「属性」タブがハイライト表示されています。 プロファイル属性が表示されます。](../images/user-guide/attributes.png)

Edgeで使用できる属性を表示するには、データロケーションセレクターで **[!UICONTROL Edge]** を選択します。

![ 「属性」タブ内のデータロケーションセレクターがハイライト表示されています。](../images/user-guide/attributes-select.png)

エッジプロファイルについて詳しくは、[ エッジプロファイルのドキュメント ](../edge-profiles.md) を参照してください。

### 「イベント」タブ {#events}

「**[!UICONTROL Events]**」タブには、顧客に関連付けられた最新 100 件の ExperienceEvents からのデータが含まれています。 このデータには、メールの開封数、買い物かごアクティビティおよびページ表示が含まれます。 個々のイベントで **[!UICONTROL View all]** を選択すると、イベントの一部として追加のフィールドと値のキャプチャが提供されます。

「」を選択して、イベントを JSON オブジェクトとして表示することもでき **[!UICONTROL View JSON]** す。 これは、Experience Platformでのイベントの取得方法を理解するのに役立ちます。

![ 「イベント」タブがハイライト表示されている様子 プロファイルイベントが表示されます。](../images/user-guide/events.png)

### 「オーディエンスメンバーシップ」タブ {#audience-membership}

「**[!UICONTROL Audience membership]**」タブには、個々の顧客プロファイルが現在属しているオーディエンスの名前と説明のリストが表示されます。 このリストは、プロファイルがオーディエンスに適合するか、オーディエンスから期限切れになると、自動的に更新されます。 プロファイルが現在選定されているオーディエンスの合計数は、タブの右側に表示されます。

Experience Platformのセグメント化について詳しくは、[Experience Platform セグメント化サービスの Adobes ドキュメント ](../../segmentation/home.md) を参照してください。

![ 「オーディエンスメンバーシップ」タブがハイライト表示されます。 プロファイルのオーディエンスメンバーシップの詳細が表示されます。](../images/user-guide/audience-membership.png)

Edgeで使用可能なプロファイルのオーディエンスメンバーシップを表示するには、データロケーションセレクターで **[!UICONTROL Edge]** を選択します。 エッジセグメント化について詳しくは、[ エッジセグメント化ガイド ](../../segmentation/methods/edge-segmentation.md) を参照してください。

![ 「オーディエンスメンバーシップ」タブ内のデータロケーションセレクターがハイライト表示されます。](../images/user-guide/audience-membership-select.png)

## 結合ポリシー

メインの **[!UICONTROL Profiles]** メニューから、「**[!UICONTROL Merge Policies]**」タブを選択して、組織に属する結合ポリシーのリストを表示します。 リストされた各ポリシーには、名前、デフォルトの結合ポリシーであるかどうか、適用されるスキーマクラスが表示されます。

結合ポリシーについて詳しくは、「[結合ポリシーの概要](../merge-policies/overview.md)」を参照してください。

![ 「結合ポリシー」タブがハイライト表示されています。 組織に属する結合ポリシーが表示されます。](../images/user-guide/merge-policies.png)

## 和集合スキーマ {#union-schema}

メインの **[!UICONTROL Profiles]** メニューから、「**[!UICONTROL Union Schema]**」タブを選択して、取り込んだデータで使用可能な結合スキーマを表示します。 結合スキーマは、スキーマが [!DNL Experience Data Model] での使用に対して有効になっている、同じクラスのすべての [!DNL Real-Time Customer Profile] （XDM）フィールドを統合したものです。

和集合スキーマについて詳しくは、[ 和集合スキーマ UI ガイド ](union-schema.md) を参照してください。

![ 「和集合スキーマ」タブがハイライト表示されています。 組織に属する結合スキーマが表示されます。](../images/user-guide/union-schema.png)

## 計算属性 {#computed-attributes}

メイン **[!UICONTROL Profiles]** メニューから [ **[!UICONTROL Computed attributes]** ] タブを選択して、組織に属する計算属性のリスト表示します。

![[計算された属性] タブが強調表示されます。](../images/user-guide/computed-attributes.png)

計算属性の詳細については、「 [計算属性の概要](../computed-attributes/overview.md)を参照してください。 Experience Platform UI内で計算属性を使用する方法の詳細については、ガイド[ UI](../computed-attributes/ui.md)計算属性」を参照してください。

## 次の手順

このガイドを読むことで、Experience Platform UI を使用した組織のプロファイルデータの表示および管理方法を知ることができました。 Experience Platform API を使用してプロファイルデータを操作する方法について詳しくは、[ リアルタイム顧客プロファイル API ガイド ](../api/overview.md) を参照してください。

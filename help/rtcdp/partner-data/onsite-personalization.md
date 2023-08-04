---
title: パートナーの支援による訪問者認識を使用して、オンサイトエクスペリエンスをパーソナライズする
description: パートナー支援による訪問者認識を使用して、パーソナライズされたオンサイトエクスペリエンスを訪問者に提供する方法を説明します。
hide: true
hidefromtoc: true
source-git-commit: f63cddc1158e739ce26e0ce1d3d54b491bd80c06
workflow-type: tm+mt
source-wordcount: '2493'
ht-degree: 8%

---

# パートナーの支援による訪問者認識を使用して、オンサイトエクスペリエンスをパーソナライズする

パートナー支援認識を使用して、Web プロパティ訪問者にパーソナライズされたエクスペリエンスを提供する方法を説明します。 このチュートリアルを使用して、Experience Platformおよび他のExperience Cloudソリューションにおける様々な要素の実装順序を理解し、認証済み訪問者と未認証訪問者に対してパーソナライズされたエクスペリエンスを表示します。

![パートナーが提供する属性を使用してパーソナライズされたエクスペリエンスを訪問者に提供する方法を説明する解説図。](/help/rtcdp/assets/partner-data/onsite-personalization/onsite-personalization-steps.png)

## 業界の例 {#industry-example}

例として、認証率が低いホーム改善ブランドを考えてみましょう。 このブランドでは、以前の履歴や認証を伴わず、サードパーティ cookie に依存しないで、パーソナライズされたエクスペリエンスを初めての訪問者に提供したいと考えています。

このブランドでは、パートナー認識テクノロジーを活用して、訪問者を確率論的に認識し、よりパーソナライズされたエクスペリエンスを提供することを選択します。 これは、訪問者がマーケティングファネルを下に移動する際に、事前に検討するのに役立ちます。 例えば、最近移動した人にアピールし、人気のある DIY 製品の割引を提供するオンサイトコンテンツに、パートナーが提供する人口統計学的シグナルを使用する場合があります。

## 前提条件と計画 {#prerequisites-and-planning}

パートナー提供の属性を使用して、認証済み訪問者と未認証訪問者にパーソナライズされたエクスペリエンスを提供する計画を立てる際は、計画プロセスで次の前提条件を考慮します。

* パートナーの認識テクノロジーでは、追加の属性を重ね合わせるために、どのような入力が期待されますか？
* 確定的に確認された属性に対し、確率論的に導き出された属性に基づいて、様々なチャネルや様々な使用例でパーソナライゼーションをどの程度実現したいと考えていますか。
* 認証済みで認識済みの訪問者に対するエクスペリエンスは、認証時にどのように変更される必要がありますか。

### 使用する UI 機能、プラットフォームコンポーネント、Experience Cloud製品 {#ui-functionality-and-elements}

この使用例を正しく実装するには、Real-time Customer Data Platformやその他のExperience Cloudソリューションの複数の領域を使用する必要があります。 必要な [属性ベースのアクセス制御権限](/help/access-control/abac/overview.md) これらすべての領域に対して、必要な権限を付与するようシステム管理者に依頼します。

* データ収集
   * [Adobe Experience Platform Web SDK](/help/edge/home.md)
   * [タグ](/help/tags/home.md)
   * [データストリーム](/help/datastreams/overview.md)
* Real-Time CDPでのデータ管理
   * [ID](/help/identity-service/namespaces.md)
   * [スキーマ](/help/xdm/home.md)
   * [データ使用ラベル](/help/data-governance/labels/overview.md)
   * [データセット](/help/catalog/datasets/overview.md)
* Web プロパティのパーソナライゼーション
   * [エッジセグメント化](/help/segmentation/ui/edge-segmentation.md)
   * [Edge パーソナライゼーションの宛先](/help/destinations/destination-types.md#edge-personalization-destinations)
   * [Adobe Target](/help/destinations/catalog/personalization/adobe-target-connection.md) （または、任意のパーソナライゼーションプラットフォームを使用できます）。 この使用例のチュートリアルでは、Adobe Targetをパーソナライゼーションエンジンとして紹介します )

## ユースケースの達成方法：おおまかな概要 {#achieve-the-use-case-high-level}

![パートナーが提供する属性を使用してパーソナライズされたエクスペリエンスを訪問者に提供する方法を説明する解説図。](/help/rtcdp/assets/partner-data/onsite-personalization/onsite-personalization-steps.png)

1. As a **顧客**、ライセンスを **データパートナー** 匿名の web サイト訪問者が、リアルタイムでインサイトを取得する機能。
2. As a **顧客**&#x200B;を使用して、を呼び出すためにプロパティにクライアント側ライブラリをデプロイします **パートナー** API を設定し、パートナー指定のシグナルをReal-Time CDPに送信するように Web SDK または Mobile SDK を設定します。
3. Web サイトやアプリを参照する際に、 **訪問者** は～によって確率論的に認識される **パートナー**:ID と共に属性を返します。
4. Real-Time CDPは、エッジセグメント化を実行して、受信イベントのヒットを評価し、結果をに対して保持します [ECID 識別子](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja).
5. Adobe Targetは、エッジセグメント化出力を使用して、エクスペリエンスを **訪問者** （セッション内のパーソナライゼーション用）
6. 分析やリターゲティングなどのダウンストリームワークフローに対して、イベント全体が保持されます。

## ユースケースの達成方法：手順 {#step-by-step-instructions}

上記の概要の各手順を完了するには、詳しいドキュメントへのリンクを含む以下の節を参照してください。

### データ管理：データ属性を管理する ID 名前空間、スキーマ、データセットを作成します。 {#data-management}

未認証の訪問者のエクスペリエンスをパーソナライズする使用例を達成するためには、まずReal-Time CDPでデータ管理構造を設定して、受信するリアルタイムイベントとオーディエンスの認定データを受け取る必要があります。

#### パートナー ID の ID 名前空間を作成

まず、パートナー ID 名前空間を作成する必要があります。 に移動します。 **[!UICONTROL 顧客]** > **[!UICONTROL ID]** 左側のレールで「 」を選択し、 **[!UICONTROL ID 名前空間を作成]** をクリックします。

![パートナー ID がハイライト表示された「 ID 名前空間を作成」ダイアログ。](/help/rtcdp/assets/partner-data/onsite-personalization/create-identity-namespace.png)

詳しくは、 [パートナー ID 名前空間の作成](/help/rtcdp/partner-data/prospecting.md#create-partner-id-namespace).

#### スキーマの作成

次に、 [!UICONTROL エクスペリエンスイベント] スキーマを使用して、後で Web プロパティから収集する時系列データを保持し、 [!UICONTROL XDM ExperienceEvent] をスキーマの基本クラスとして使用します。 以下の方法をお読みください。 [スキーマ UI を使用したExperience Platformの作成](/help/xdm/ui/resources/schemas.md#create).

![「スキーマを作成」と「 XDM エクスペリエンス」イベントが強調表示された「スキーマ」ワークスペース。](/help/rtcdp/assets/partner-data/onsite-personalization/create-experience-event-schema.png)

スキーマを作成し、 [スキーマへのフィールドグループの追加](/help/xdm/ui/resources/schemas.md#add-field-groups)を使用する場合は、 [ウェブページにアクセス](/help/xdm/field-groups/event/web-details.md) および [ID マップ](/help/xdm/field-groups/profile/identitymap.md) フィールドグループを使用します。 これは、デジタルプロパティやデータ収集の慣行に適用できる他のフィールドグループに加えておこなわれます。

さらに、既存のフィールドグループを作成または再利用し、スキーマに追加して、訪問者に関するパートナー提供のインサイトを取り込むこともできます。 方法を読む [フィールドグループの作成](/help/xdm/ui/resources/field-groups.md) そして方法は [フィールドを追加](/help/xdm/ui/resources/field-groups.md) をフィールドグループに追加します。 例えば、年齢層、雇用状況、月々の支出力、購入行動など、パートナーが提供するインサイトに対してパーソナライズする予定がある場合は、フィールドグループに適切なフィールドを含めます。

データパートナーが訪問者の安定した識別子を提供し、それをReal-Time CDPに取り込む場合は、カスタムフィールドグループの識別子に対して適切な名前のフィールドを使用する必要があります。 また、フィールドを、前に作成した ID 名前空間の ID としてマークする必要があります。 忘れないでください [プロファイルにスキーマを含める](/help/xdm/ui/resources/schemas.md#profile).

#### データセットの作成

次に、Web プロパティ訪問者から収集し、オンサイトパーソナライゼーションに使用する時系列データを保持するデータセットを作成する必要があります。

に関するチュートリアルをお読みください。 [データセットの作成方法](/help/catalog/datasets/user-guide.md#create) スキーマからデータセットを作成するオプションを必ず選択してください。 前の手順で作成したスキーマに基づいてデータセットを作成します。

スキーマを作成する際の手順と同様に、 [!UICONTROL リアルタイム顧客プロファイル]. でのデータセットの使用を有効にする方法について詳しくは、 [!UICONTROL リアルタイム顧客プロファイル]を読む [スキーマの作成に関するチュートリアル](/help/xdm/tutorials/create-schema-ui.md#profile)

### Web プロパティでのイベントデータ収集の実装 {#implement-data-collection}

データ管理設定をセットアップした後、リアルタイムイベントを実装する必要があります [データ収集](/help/collection/home.md) を Web プロパティに設定します。 プロパティをAdobeのデータ収集ライブラリで実装する必要があります。 [!UICONTROL Web SDK] ：リアルタイムイベント呼び出しを収集し、Real-Time CDPに返します。 この項目は、いくつかのデータ収集コンポーネントをまたいだいくつかの個別のタスクで構成されます。

>[!IMPORTANT]
>
>パートナー指定の属性を取得するには、 *web プロパティをパートナー API や他のメソッドと統合して、データパートナーからリアルタイムで属性を呼び出し、取得します。*. このチュートリアルの対象ではないので、この点については、選択したパートナーと話し合ってください。

まず、画面の右上隅にあるアプリケーション切り替えボタンを使用して、 **[!UICONTROL データ収集]** 」セクションに入力します。

>[!TIP]
>
>が表示されない場合は、システム管理者にアクセス権を問い合わせてください [!UICONTROL データ収集] （アプリケーション切り替えボタン）をクリックします。

![データ収集セクションに移動するには、アプリを切り替えます。](/help/rtcdp/assets/partner-data/onsite-personalization/app-switcher-data-collection.png)

The **[!UICONTROL データ収集]** UI のセクションは、以下の画像のようになります。

![Platform UI のデータ収集に関する節を参照してください。](/help/rtcdp/assets/partner-data/onsite-personalization/data-collection-home.png)

#### データストリームの作成

データ収集の節の最初の手順として、 [新しいデータストリームの作成](/help/datastreams/configure.md). データストリームは、データの収集方法の基盤であり、適切なAdobeアプリ（この場合はアプリ）に正しくルーティングされます。Experience Platform

データストリームを作成する際に、 **[!UICONTROL イベントスキーマ]** 「 」フィールドで、以前に作成したスキーマを選択します。

![新しいデータストリームを設定する際に強調表示されたイベントスキーマセレクター。](/help/rtcdp/assets/partner-data/onsite-personalization/event-schema-selector-datastream.png)

[イベントデータセットを選択](/help/datastreams/configure.md#aep) 前にドロップダウンから作成した内容を、の横にあるチェックボックスをオンにします。 **[!UICONTROL エッジのセグメント化]** および **[!UICONTROL パーソナライズ機能の宛先]**&#x200B;をクリックし、次を選択します。 **[!UICONTROL 保存]**.

イベントベースの時系列データを取り込むので、このシナリオでプロファイルデータセットを選択する必要はありません。

#### タグプロパティを作成

プロパティは、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナと考えてください。

に移動します。 **[!UICONTROL タグ]** を選択し、 **[!UICONTROL 新しいプロパティ]**.

![新しいタグプロパティを作成します。](/help/rtcdp/assets/partner-data/onsite-personalization/create-tag-property.png)

必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 保存]**.

![新しいプロパティの必須フィールドに入力します。](/help/rtcdp/assets/partner-data/onsite-personalization/tag-property-fields.png)

次の方法に関する完全な情報を取得します。 [タグプロパティを作成する](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ja).

次に、プロパティ内に様々な拡張機能をインストールする必要があります。 タグプロパティを選択し、 [!UICONTROL 拡張機能] 」セクションに入力します。

![新しいタグプロパティを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/select-tag-property.png)

次の点に注意してください。 [!UICONTROL コア] 拡張機能は既にインストールされています。 次の節で説明するように、さらに 2 つの拡張機能をインストールする必要があります。

![インストールされた拡張機能を表示します。](/help/rtcdp/assets/partner-data/onsite-personalization/view-existing-extensions.png)

#### Web SDK 拡張機能のインストール

このチュートリアルでは、Web SDK で Web サイトを実装する方法について説明します。 また、 [モバイル SDK](https://developer.adobe.com/client-sdks/documentation/) を使用して、アプリの訪問者に対するエクスペリエンスをパーソナライズします。

![拡張機能カタログ内の Web SDK 拡張機能の表示。](/help/rtcdp/assets/partner-data/onsite-personalization/web-sdk-extension.png)

Web SDK を設定する画面で、 **[!UICONTROL データストリーム]** を参照し、使用しているExperience Platformサンドボックスに関する情報を提供します。 次のドロップダウンから、適切なサンドボックスと前の手順で作成したデータストリームを選択します。 他のすべての環境に対して同じサンドボックスと datastream の値を選択できます。 その他の設定は変更せず、「 」を選択します。 **[!UICONTROL 保存]**.

に関する完全な情報を得る [Web SDK のインストール方法](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html).

#### ID サービス拡張機能のインストール

以下を使用します。 [Experience CloudID サービス拡張機能](/help/tags/extensions/client/id-service/overview.md) を使用して、すべてのデバイスソリューションにわたって訪問者に一意のデバイスベースのファーストパーティ id をExperience Cloudします。 を検索 **[!UICONTROL ID サービス]** 拡張機能カタログで、インストールします。 拡張機能をインストールする際は、すべてのデフォルト設定をそのまま使用します。

![拡張機能カタログ内の ID サービス拡張機能の表示。](/help/rtcdp/assets/partner-data/onsite-personalization/id-service-extension.png)

#### 環境の設定

次に、 **[!UICONTROL 環境]** セクションを開きます。 この手順では、Web サイトをAdobe Edge Network に接続して、訪問者情報をリアルタイムで取得し、配信する必要があります。

開発環境の右側にあるボックスアイコンを選択し、モーダルウィンドウに表示される JavaScript コードスニペットの標準バージョンをコピーします。

![データ収集 UI の「環境」セクションでハイライト表示されているボックスアイコンを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/select-box-icon.png)

このコードスニペットを Web サイトの最上部に追加する必要があります。 その結果、Web サイトはAdobe Edge Network を呼び出して、ページ上で読み込まれ実行される JavaScript ロジックを取得します。 これにより、訪問者 ID 生成、データ収集、リアルタイムエクスペリエンスパーソナライゼーションなどの機能を使用できます。

#### データ要素の設定

データ要素は、データディクショナリ（またはデータマップ）の構築ブロックです。単一のデータ要素は、クエリ文字列、URL、cookie 値、JavaScript 変数などに値をマッピングできる変数です。詳細を表示： [データ要素](/help/tags/ui/managing-resources/data-elements.md).

この使用例では、2 つのデータ要素を設定する必要があります。

まず、 `partnerData` 要素を選択します。 次に移動： **[!UICONTROL データ要素]** 「 」セクションで「 」を選択します。 **[!UICONTROL 新しいデータ要素を作成]**.

![新しいデータ要素を作成します。](/help/rtcdp/assets/partner-data/onsite-personalization/create-data-element.gif)

データ要素に名前を付ける `partnerData`、 [!UICONTROL 拡張] 値： [!UICONTROL コア]、および設定 **[!UICONTROL データ要素タイプ]** as **[!UICONTROL JavaScript 変数]**. 入力 `partnerData` という名前のフィールドで **[!UICONTROL JavaScript 変数名]** を選択し、 **[!UICONTROL 保存]**.

![選択項目が強調表示され、partnerData データ要素が正しく設定されています。](/help/rtcdp/assets/partner-data/onsite-personalization/create-partnerdata-data-element.png)

2 つ目のデータ要素を設定するには、新しい変数に名前を付けます。 `pageVisit`を設定し、 **[!UICONTROL 拡張]** から **[!UICONTROL Adobe Experience Platform]** を選択します。 **[!UICONTROL XDM オブジェクト]** をデータ型として設定します。

![選択項目が強調表示され、pageVisit データ要素が正しく設定されています。](/help/rtcdp/assets/partner-data/onsite-personalization/page-visit-data-element.png)

スキーマから、データパートナーから期待する値に対応するサードパーティの属性を選択します。 次に、「 」というタイトルのラジオボタンを選択します。 **[!UICONTROL オブジェクト全体を提供]**. データベースに似たアイコンを選択し、 `partnerData` 前に作成したデータ要素。

#### ルールの設定

Adobe Analytics の **[!UICONTROL ルール]** セクションでは、作成したデータ要素に読み込まれた属性を含むパーソナライゼーションリクエストをAdobeに送信するように web サイトを設定できます。 詳細を表示： [ルールの作成](/help/tags/ui/managing-resources/rules.md).

選択 **[!UICONTROL 新規ルールを作成]**. このルールに名前を付ける **[!UICONTROL パーソナライズ]** 次の横の「+」記号を選択します。 **[!UICONTROL イベント]**. 選択 **[!UICONTROL Page Bottom]** をイベントとして追加して保存します。

![ルールのイベントタイプ部分の選択。](/help/rtcdp/assets/partner-data/onsite-personalization/add-events-rule.png)

の横の+記号を選択します。 **[!UICONTROL アクション]**. 拡張機能をに更新します。 **[!UICONTROL Adobe Experience Platform Web SDK]** と設定します。 **[!UICONTROL アクションタイプ]** から **[!UICONTROL イベントを送信]**.

![ルールのアクションタイプの部分の選択。](/help/rtcdp/assets/partner-data/onsite-personalization/add-action-rule.png)

次から： **[!UICONTROL タイプ]** 右側のドロップダウンセレクターで、「 」を選択します。 `web.webpagedetails.pageViews` をイベントタイプとして使用します。

![イベントタイプを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/add-pageview-type-rule.png)

XDM データの横のデータベースアイコンを選択し、 `pageVisit` データ要素。

アクション設定のリストを下にスクロールし、「 」というタイトルのボックスを必ずオンにします。 **[!UICONTROL 視覚的なパーソナライゼーションの決定をレンダリング]**. これは、Adobe Targetまたはその他の類似製品を介して提供されたエクスペリエンスを Web ページ上で視覚的にレンダリングするために重要です。 選択 **[!UICONTROL 変更を保持]**&#x200B;を、 **[!UICONTROL 保存]** ルール。

![「視覚的なパーソナライゼーションの決定をレンダリング」チェックボックスを選択します。](/help/rtcdp/assets/partner-data/onsite-personalization/render-visual-personalization-toggle.png)

#### 公開ワークフローを設定する

この設定を Web ページにデプロイするには、次の手順で、作成したリソースを含むライブラリを構築します。 詳細を表示： [公開フローの設定](/help/tags/ui/publishing/publishing-flow.md).

選択 **[!UICONTROL 公開フロー]** その後 **[!UICONTROL ライブラリを追加]**.

選択 **[!UICONTROL 変更されたすべてのリソースを追加]**、ライブラリに名前を付け、環境をに設定します。 **[!UICONTROL 開発]** を選択し、 **[!UICONTROL 開発用に保存およびビルド]**.

![ライブラリの作成と開発環境へのデプロイ](/help/rtcdp/assets/partner-data/onsite-personalization/create-publishing-workflow.gif)

#### Web サイトをテストする

この時点で、Web サイトに Web SDK が完全に実装されている必要があります。 データ収集が期待どおりに動作しているかをテストするには、Web サイトに移動し、ブラウザーの開発者ツールを使用してネットワークトラフィックを調べます。

入力 `interact` 検索ボックスで、ページを更新すると、web サイトからAdobe Edgeネットワークへのネットワーク呼び出しが表示されます。

![開発者ツールでに入力されたネットワークイベントの表示。](/help/rtcdp/assets/partner-data/onsite-personalization/events-filtered.png)

### パーソナライゼーション {#personalization}

これで、パーソナライゼーション用のオーディエンスを作成してアクティブ化する準備が整いました。

#### エッジセグメント化の設定

設定 [エッジセグメント化](/help/segmentation/ui/edge-segmentation.md) したがって、訪問者のオーディエンスメンバーシップは、Web プロパティを訪問する際にリアルタイムに評価されます。

必ず [エッジ上のアクティブな結合ポリシー](/help/destinations/ui/activate-edge-personalization-destinations.md#create-merge-policy) エッジオーディエンス向け。

#### Adobe Targetやその他のカスタムパーソナライゼーションの宛先との統合

これで、パーソナライゼーションエンジンと統合して、Web サイトやアプリの訪問者にパーソナライズされたコンテンツを表示する準備が整いました。 Adobeは、 [Adobe Targetの宛先](/help/destinations/catalog/personalization/adobe-target-connection.md) この目的のために。

>[!IMPORTANT]
>
>に関するチュートリアルをお読みください。 [エッジパーソナライゼーションの宛先へのオーディエンスのアクティブ化](/help/destinations/ui/activate-edge-personalization-destinations.md) を参照してください。

## 制限事項とトラブルシューティング {#limitations-and-troubleshooting}

このページで説明するユースケースを参照する際は、次の制限事項に注意してください。

* パートナー ID を使用する場合、 [ID グラフ](/help/identity-service/ui/identity-graph-viewer.md).

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

Real-Time CDP のパートナーデータサポートを通じて達成されるその他のユースケースを調べます。

* [信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* Real-Time CDPでのサードパーティデータのサポートを使用して、 [データパートナーの見込み客プロファイルを使用してプロファイルベースを拡大し、顧客との関わりを深めて新しい顧客を獲得または獲得する](/help/rtcdp/partner-data/prospecting.md).
* （**近日公開**）[!BADGE ベータ版]{type=Informative}パートナー ID を使用して、PII またはハッシュ化された PII を受け入れないパブリッシングエコシステムに&#x200B;**アクティベーションを拡張しました**。
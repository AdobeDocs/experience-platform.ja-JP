---
title: 同意分析とトラッキング
description: 同意分析ダッシュボードを作成して、ユーザーの同意の経時的なトレンドを追跡する方法を説明します。
exl-id: 34accae5-8b4f-4281-8333-187a91db8199
source-git-commit: e0af1f0110ceb514a5b249c42a05bf780ea834c6
workflow-type: tm+mt
source-wordcount: '1909'
ht-degree: 0%

---

# 同意分析とトラッキング

現在のマーケティング環境では、顧客の同意環境設定を理解し、尊重する必要があります。 Adobe Real-time Customer Data Platformは、マーケターが顧客の同意を分析して、信頼を構築し、プライバシー規制を遵守し、よりパーソナライズされたエクスペリエンスを提供する機能を提供します。

このドキュメントでは、Real-Time CDP データの様々なマーケティングユースケースに対応する同意ダッシュボードの作成方法について詳しく説明します。 特に、ビジネスニーズに適した属性でオーディエンスを作成し、Adobe Experience Platform UI の事前設定済みウィジェットを使用してインサイトを活用する方法に重点を置いています。 ユーザー定義ダッシュボード機能を使用して独自のカスタムウィジェットを作成する代替方法についても説明します。

## ユースケース {#use-cases}

このガイドで扱うユースケースは、同意のトレンドと同意の重複です。

- **同意のトレンド** ユーザーの同意のトレンドを経時的に追跡します。 同意の環境設定の変更を分析すると、マーケターは、これらのユーザーの環境設定の変更に適応するキャンペーンを計画および実行するのに役立ちます。 例えば、ターゲットを絞った教育キャンペーン、透明性と信頼のキャンペーン、同意に関する選択肢を促進するインセンティブキャンペーンなどを実行できます。 また、同意に悪影響を与えた可能性のあるキャンペーンを関連付けて、それらのキャンペーンの頻度を積極的に減らすこともできます。
- **同意の重複** では、同意チャネル間の重複を使用して、複数のチャネルに同意した顧客に対して、複数のチャネルで一貫したパーソナライズされたメッセージを提供します。 マーケターは、ある特定のチャネルにリソースを優先順位付けして割り当てることができます。そのチャネルでは、より高い度合いの同意とパーソナライズされたメッセージが顧客の共感を呼び、高い応答率を生み出す可能性があります。

## 同意したオーディエンスの作成 {#create-consent-audiences}

同意ダッシュボードを作成するには、まず、連絡に同意したすべてのプロファイルのオーディエンスを作成する必要があります。 Real-time Customer Data Platform セグメントビルダーに移動するには、次を選択します。 **[!UICONTROL オーディエンス]** を Platform UI の左側のナビゲーションに表示します。 から [!UICONTROL 顧客] タブ [!UICONTROL オーディエンス] ダッシュボード、選択 **[!UICONTROL オーディエンスを作成]** ビューの右上にある **[!UICONTROL ルールの作成]**.

![この [!UICONTROL オーディエンス] 次を使用したダッシュボード [!UICONTROL 顧客], [!UICONTROL オーディエンス]、および [!UICONTROL セグメントを作成] ハイライト表示](../images/insights-use-cases/consent-analysis/create-audience.png)

セグメントビルダーが表示されます。次に、を選択します **[!UICONTROL XDM 個人プロファイル]** 使用可能なオプションから。 の詳細については、ドキュメントを参照してください。 [ルールビルダーキャンバス](../../segmentation/ui/segment-builder.md#rule-builder-canvas).

![を使用したセグメントビルダー [!UICONTROL XDM 個人プロファイル] 属性フォルダーがハイライト表示されています。](../images/insights-use-cases/consent-analysis/xdm-individual-profile.png)

使用可能なオプションから同意属性を見つけます。 を選択 **[!UICONTROL 同意および環境設定]**.

>[!NOTE]
>
>Adobeが推奨するフィールドグループとは異なる属性でユーザーの同意を保持している場合は、次に示す属性ではなく、それらの属性を選択する必要があります。

詳しくは、 [セグメント化における同意の処理](../../segmentation/consents.md#handling-consent-in-segmentation) ドキュメント。

![を使用したセグメントビルダー [!UICONTROL 同意および環境設定] 属性フォルダーがハイライト表示されています。](../images/insights-use-cases/consent-analysis/consent-and-preferences.png)

様々な同意オプションと環境設定オプションが表示されます。 このデモでは、様々なマーケティングチャネルに関するお問い合わせへの同意に焦点を当てているので、次を選択します。 **[!UICONTROL マーケティング環境設定]**.

![を使用したセグメントビルダー [!UICONTROL マーケティング環境設定] フォルダーがハイライト表示されています。](../images/insights-use-cases/consent-analysis/marketing-preferences.png)

マーケティング環境設定のリストが表示されます。 この例のユースケースは、メール、SMS および呼び出しに焦点を当てていますが、他の組み合わせやオプション全体に関してもインサイトを構築できます。 チャネルごとに、以下の手順を実行してオーディエンスを作成します。

オーディエンスの設定を開始するには、以下を選択します **[!UICONTROL SMS を受信]** / **[!UICONTROL メールを受信]** / **[!UICONTROL 電話を受信]**.

![マーケティングに使用可能な連絡先チャネルは、オーディエンスビルダーでハイライト表示されます。](../images/insights-use-cases/consent-analysis/channels.png)

この [!UICONTROL Subscriptions] フォルダーが表示されます。 使用可能なオプションから、を選択してドラッグします **[!UICONTROL 値を選択]** 属性を中央のペインに設定し、ドロップダウンから目的の値を選択します。 この場合、を選択します。 **はい（オプトイン）**. 次に、ビジネスニーズに応じてオーディエンスに名前を付け、わかりやすい説明を入力します。

>[!NOTE]
>
>作成することを推奨するオーディエンスの数にはソフトリミットがあります。 詳しくは、 [セグメント化ガードレールのドキュメント](../../profile/guardrails.md#segmentation-guardrails).

![この [!UICONTROL 値を選択] を持つ属性 [!UICONTROL はい（オプトイン）] セグメントビルダーでハイライト表示されている値 オーディエンスの名前と説明もハイライト表示されます。](../images/insights-use-cases/consent-analysis/choice-value.png)

必要なオーディエンスの作成が完了すると、に表示されます。 [!UICONTROL オーディエンス] [!UICONTROL 参照] タブ。

>[!NOTE]
>
>オーディエンスを作成する場合は、バッチセグメント化ジョブが完了するのを待ってから、データを使用して同意ダッシュボードの作成を開始する必要があります。 バッチセグメント化では、セグメント定義を通じてすべてのプロファイルデータを一度に移動し、対応するオーディエンスを生成するプロセスを説明します。 作成したオーディエンスは、書き出しと使用のために保存されます。 バッチセグメントは、24 時間ごとに自動評価されます。

## インサイトの使用 {#consume-insights}

Adobeは、プロファイル、オーディエンス、宛先の各ダッシュボードで自動的に使用可能な様々なインサイトを作成しています。 作成したオーディエンスは、これらの事前設定済みのインサイトで自動的に使用できます。 で使用可能なインサイトのリストについては、標準ウィジェットのドキュメントを参照してください [プロファイル](../guides/profiles.md#standard-widgets), [オーディエンス](../guides/audiences.md#standard-widgets)、および [宛先](../guides/destinations.md) ダッシュボード。

## オーディエンスの重複 {#audience-overlap}

2 つの同意オーディエンス間の重複を確認するには、 [!UICONTROL オーディエンスの重複（結合ポリシー別）] プロファイルダッシュボードに移動し、ドロップダウンメニューで目的のオーディエンスを選択します。 ダッシュボードにウィジェットを追加する手順については、ドキュメントを参照してください。 [*オーディエンスの重複（結合ポリシー別）*](../guides/profiles.md#audience-overlap-by-merge-policy) インサイトについて詳しくは、こちらを参照してください。

<!-- Image needs updating to night mode -->

![結合ポリシーウィジェットによるオーディエンスの重複がハイライトされたプロファイルダッシュボード。 このウィジェットは、2 つの同意オーディエンス間の重複を視覚化します。](../images/insights-use-cases/consent-analysis/audience-overlap-by-merge-policy.png)

オーディエンスダッシュボードのオーディエンスの重複レポートを使用して、ユーザーが他のすべてのオーディエンスにわたる呼び出しを受信することに同意したすべてのオーディエンスの重複を表示できます。 同意オーディエンスの重複を表示するには、まず以下に移動します。 [!UICONTROL オーディエンス] [!UICONTROL 概要] タブ。 ここから、 [!UICONTROL オーディエンスの重複レポート] オーディエンスダッシュボードに対するウィジェット。 ウィジェットを作成したら、 **[!UICONTROL ユーザーが呼び出しに同意しました]** ページ上部のオーディエンスの概要ドロップダウンメニューからのオーディエンス。 次に、を選択します **[!UICONTROL さらに表示]** オーディエンスの重複レポート ウィジェットでは、選択したセグメントに関して、上部の重複が最大 50 個、最小重複の最大 50 個が表示されます。

<!-- Image needs updating to night mode -->

![オーディエンスの重複レポートウィジェットを含むオーディエンスダッシュボードが表示されます。 ユーザーが比較オーディエンスとしてオーディエンスの呼び出しに同意し、さらに表示リンクが両方ともハイライト表示されます。](../images/insights-use-cases/consent-analysis/audience-overlap-report-user-consent-to-calls.png)

オーディエンスの重複レポートダイアログが拡張され、追加のオーディエンスの重複データが表示されます。

<!-- Image needs updating to night mode -->

![メールオーディエンスに同意したユーザーがハイライト表示されたオーディエンス重複レポート。](../images/insights-use-cases/consent-analysis/additional-audience-overlap-reports.png)

## オーディエンスサイズのトレンド {#audience-size-trends}

同意ベースのオーディエンスを作成すると、オーディエンスを作成した日から最大 12 か月のトレンドが自動的に表示されます。 顧客の同意に関する完全な機能トレンドを把握するには、次のウィジェットを [!UICONTROL セグメント] [!UICONTROL 概要] ページ。 これらのインサイトは、時間の経過と共に同意の変化を追跡する強力な手段を提供します。 同意にプラスまたはマイナスの影響を与える可能性のある、並行して実行するキャンペーンとも関連付けられます。 これらのウィジェットに提供される説明は、同意のユースケースに適用されます。

- [オーディエンスサイズのトレンド](../guides/audiences.md#audience-size-trend)：このウィジェットは、それぞれの同意が時間の経過と共にどのように変化したかを追跡する方法を提供します。
- [オーディエンスサイズの変更のトレンド](../guides/audiences.md#audience-size-change-trend)：このウィジェットは、顧客の同意が毎日変更された方法を追跡します。 例えば、顧客の同意の数が 100,000 減少した場合、その変更が毎日発生した様子を確認できます。
- [ID 別のオーディエンスサイズのトレンド](../guides/audiences.md#audience-size-trend-by-identity)：このウィジェットを使用すると、それぞれの同意が時間の経過と共にどのように変化したかを追跡できますが、メールなどの特定の ID によってさらにフィルタリングできます。

<!-- Image needs updating to night mode -->

![オーディエンスサイズのトレンド、ID 別のオーディエンスサイズのトレンド、オーディエンスサイズの変化のトレンドのウィジェットが表示されたオーディエンスダッシュボード。 メールオーディエンスに同意したユーザーがハイライト表示されます。](../images/insights-use-cases/consent-analysis/three-audience-trend-widgets.png)

## オーディエンスの概要ダッシュボード {#audiences-overview-dashboard}

「SMS に同意したユーザー」など、同意関連のオーディエンスを作成したら、適切なウィジェットをオーディエンスの概要ダッシュボードに追加することで、オーディエンスに関する主要なパーソナライズされた同意情報を表示できます。 に移動します。 [!UICONTROL オーディエンス] [!UICONTROL 概要] 選択したウィジェットをウィジェットライブラリから追加します。 ダッシュボードのビューに追加されたウィジェットは、 [!UICONTROL ダッシュボードの変更] 機能 パーソナライズされたビューには、経時的なトレンド（最大 12 か月）、他のオーディエンスとの重複、オーディエンスの ID 構成などのインサイトを含めることができます。 ビューの例を次に示します。

![グローバルオーディエンスドロップダウンメニューで、SMS オーディエンスに同意したユーザーがハイライト表示されたオーディエンスダッシュボード。](../images/insights-use-cases/consent-analysis/audience-dashboard-user-consent-to-sms.png)

## ユーザー定義ダッシュボード {#usr-defined-dashboards}

ユーザー定義のダッシュボードを使用して独自のウィジェットを作成することもできます。 独自のウィジェットを作成すると、ウィジェットのタイプを完全に制御できるほか、Adobe Real-Time CDP内で柔軟にフィルターなどを追加できます。

例えば、同じグラフで複数の同意オーディエンスのトレンドを分析し、各同意環境設定の変化を経時的に確認できるようにする場合などです。 このタイプのビジュアライゼーションは、ユーザー定義のダッシュボードを最小限の手順と 1 回設定で使用できます。 まず、を選択します。 **[!UICONTROL ダッシュボード]** 左側のナビゲーションの この [!UICONTROL ダッシュボード] ワークスペースが表示されます。 次に、を選択します **[!UICONTROL ダッシュボードを作成]**. の方法に関する詳しい説明 [ダッシュボードとカスタムウィジェットの作成](../user-defined-dashboards.md) は、ユーザー定義ダッシュボードガイドにあります。

![ダッシュボードと作成ダッシュボードがハイライト表示されたダッシュボードワークスペース。](../images/user-defined-dashboards/create-dashboard.png)

実行する場合 [データモデルを選択](../user-defined-dashboards.md#select-data-model) ウィジェットコンポーザーでを選択します。 `CDPInsights` 続いて **[!UICONTROL 次]**. この [!UICONTROL テーブルを選択] ダイアログが表示されます。

![CDPInsights モデルがハイライト表示されたデータモデル選択ダイアログ](../images/user-defined-dashboards/select-data-model-dialog.png)

次の表示では、使用可能なテーブルのリストが左パネルに表示されます。 「`adwh_fact_profile_by_segment_and_namespace_trendlines`」を選択します。

![「adwh_fact_profile_by_segment_and_namespace_trendlines」テーブルがハイライト表示されたテーブルを選択ダイアログ](../images/insights-use-cases/consent-analysis/select-table.png)

選択したテーブルのデータがウィジェットコンポーザーに入力されたら、次の手順を実行します。

- [検索 [!UICONTROL 属性]](../user-defined-dashboards.md#add-filter-attributes) （用） `[!UICONTROL date]`を選択し、「+」アイコンを使用して `[!UICONTROL date]` ドロップダウンメニューから X 軸の属性を選択します。
  ![アドアイコンとドロップダウンメニューがハイライト表示されているウィジェットコンポーザー。](../images/user-defined-dashboards/attributes-dropdown.png)
- 検索 [!UICONTROL 属性] （用） `[!UICONTROL count_of_profiles]`を選択し、「+」アイコンを使用して `[!UICONTROL count_of_profiles]` ドロップダウンメニューから Y 軸の属性を選択します。
- 「」を選択します `...` （省略記号）アイコン [!UICONTROL Y 軸] フィールドで、 [!UICONTROL SUM] ドロップダウンメニューからの集計関数。
  ![データモデル、テーブル、Y 軸のドロップダウンメニューと SUM 機能がハイライト表示されたウィジェットコンポーザーの同意トレンドウィジェット。 ](../images/insights-use-cases/consent-analysis/y-axis-sum-function.png)
- 「」を選択します [!UICONTROL マーク] ドロップダウンメニューを開き、グラフのタイプをに変更します [!UICONTROL ライン].
- 検索 [!UICONTROL 属性] の場合 `[!UICONTROL segment_name]`を選択し、「+」アイコンを使用して `segment_name` as a [!UICONTROL フィルター] ドロップダウンメニューから。 この [!UICONTROL フィルター：Segment_name] ダイアログが表示されます。 同意に関連する、以前に作成したオーディエンスを選択します。 この例では、を選択します。 **[!UICONTROL ユーザーが呼び出しに同意しました]**, **[!UICONTROL ユーザーが SMS に同意しました]**、および **[!UICONTROL ユーザーがメールに同意しました]**、続いて **[!UICONTROL 適用]**.
- 検索 [!UICONTROL 属性] （用） `[!UICONTROL segment_name]`を選択してから、「+」アイコンを選択して追加します `segment_name` as a [!UICONTROL カラー] ドロップダウンメニューから。
- 開く [この [!UICONTROL プロパティ] panel](../user-defined-dashboards.md#widget-properties) および適切なを指定します [!UICONTROL ウィジェットのタイトル] および [!UICONTROL 軸ラベル].
  ![プロパティアイコンとウィジェットタイトルがハイライト表示されたウィジェットコンポーザー。](../images/user-defined-dashboards/properties-panel.png)
- を選択 **[!UICONTROL 保存して閉じる]** をクリックして設定を確認します。

>[!TIP]
>
>ダッシュボードを保存する前に、ウィジェットのサイズを変更したり、目的のサイズや位置に移動したりできるようになりました。


次の画像は、完成したウィジェットがどのように表示されるかを示し、その他の潜在的なカスタムインサイトを示しています。 作成可能なウィジェットのタイプについて詳しくは、 [データモデルのドキュメント](../data-models/cdp-insights-data-model-b2c.md).

<!-- The diagram shows straight lines due to a lack of data, however in your environment the trends will reflect the actual changes over time. -->

![完了したカスタム同意トレンドウィジェット。](../images/insights-use-cases/consent-analysis/consent-trends-widget.png)

## 同意ポリシーのトラッキング {#consent-policies}

作成する同意ダッシュボードでは、 **同意属性と環境設定属性のみの配布**.

>[!NOTE]
>
>の顧客の場合 **Adobe ヘルスケア シールド** または **Adobeプライバシーとセキュリティシールド**、これらのダッシュボード **実行しない** 同意ポリシーのトラッキングを反映します。 使用可能なトラッキングには、作成および有効化されたポリシーの数、オーディエンスメンバーシップへの影響が含まれます。

## 次の手順

このドキュメントでは、Real-Time CDP Insights を使用して顧客の同意環境設定を包括的に把握できるダッシュボードを作成する方法について説明しました。 このドキュメントでは、Real-Time CDPが、同意データに基づく収集、セグメント化、分析およびパーソナライズされたマーケティングキャンペーンがマーケターにとって重要な、プライバシーに焦点を当てた現在の環境に対して堅牢なソリューションを提供する方法について説明します。

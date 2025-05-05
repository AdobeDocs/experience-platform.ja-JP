---
title: 同意分析とトラッキング
description: 同意分析ダッシュボードを作成して、ユーザーの同意の経時的なトレンドを追跡する方法を説明します。
exl-id: 34accae5-8b4f-4281-8333-187a91db8199
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '1910'
ht-degree: 0%

---

# 同意分析とトラッキング

現在のマーケティング環境では、顧客の同意環境設定を理解し、尊重する必要があります。 Adobe Real-Time Customer Data Platformは、マーケターが顧客の同意を分析して、信頼を構築し、プライバシー規制を遵守し、よりパーソナライズされたエクスペリエンスを提供する機能を提供します。

このドキュメントでは、Real-Time CDP データの様々なマーケティングユースケースに対応する同意ダッシュボードの作成方法について詳しく説明します。 特に、ビジネスニーズに適した属性でオーディエンスを作成し、Adobe Experience Platform UI の事前設定済みウィジェットを使用してインサイトを活用する方法に重点を置いています。 ユーザー定義ダッシュボード機能を使用して独自のカスタムウィジェットを作成する代替方法についても説明します。

## ユースケース {#use-cases}

このガイドで扱うユースケースは、同意のトレンドと同意の重複です。

- **同意トレンド**：ユーザーの同意のトレンドを時系列で追跡します。 同意の環境設定の変更を分析すると、マーケターは、これらのユーザーの環境設定の変更に適応するキャンペーンを計画および実行するのに役立ちます。 例えば、ターゲットを絞った教育キャンペーン、透明性と信頼のキャンペーン、同意に関する選択肢を促進するインセンティブキャンペーンなどを実行できます。 また、同意に悪影響を与えた可能性のあるキャンペーンを関連付けて、それらのキャンペーンの頻度を積極的に減らすこともできます。
- **同意の重複** は、同意チャネル間の重複を使用して、複数のチャネルに同意した顧客に対して、複数のチャネルで一貫したパーソナライズされたメッセージを配信します。 マーケターは、ある特定のチャネルにリソースを優先順位付けして割り当てることができます。そのチャネルでは、より高い度合いの同意とパーソナライズされたメッセージが顧客の共感を呼び、高い応答率を生み出す可能性があります。

## 同意したオーディエンスの作成 {#create-consent-audiences}

同意ダッシュボードを作成するには、まず、連絡に同意したすべてのプロファイルのオーディエンスを作成する必要があります。 Real-Time Customer Data Platform セグメントビルダーに移動するには、Experience Platform UI の左側のナビゲーションで「**[!UICONTROL オーディエンス]**」を選択します。 [!UICONTROL &#x200B; オーディエンス &#x200B;] ダッシュボードの「[!UICONTROL &#x200B; 顧客 &#x200B;]」タブで、表示の右上にある **[!UICONTROL オーディエンスを作成]** を選択したあと、**[!UICONTROL ルールを作成]** を選択します。

![[!UICONTROL &#x200B; 顧客 &#x200B;]、[!UICONTROL &#x200B; オーディエンス &#x200B;]、および [!UICONTROL &#x200B; セグメントを作成 &#x200B;] がハイライト表示された [!UICONTROL &#x200B; オーディエンス &#x200B;] ダッシュボード ](../images/insights-use-cases/consent-analysis/create-audience.png)

セグメントビルダーが表示されます。次に、使用可能なオプションから「**[!UICONTROL XDM 個人プロファイル]**」を選択します。 [ ルールビルダーキャンバス ](../../segmentation/ui/segment-builder.md#rule-builder-canvas) について詳しくは、ドキュメントを参照してください。

![[!UICONTROL XDM 個人プロファイル &#x200B;] 属性フォルダーがハイライト表示されたセグメントビルダー ](../images/insights-use-cases/consent-analysis/xdm-individual-profile.png)

使用可能なオプションから同意属性を見つけます。 **[!UICONTROL 同意および環境設定]** を選択します。

>[!NOTE]
>
>Adobeの推奨フィールドグループとは異なる属性でユーザーの同意を保持している場合は、以下に示す属性ではなく、それらの属性を選択する必要があります。

詳しくは、[ セグメント化での同意の処理 ](../../segmentation/tutorials/consents.md#handling-consent-in-segmentation) ドキュメントを参照してください。

![ 「同意および環境設定 [!UICONTROL &#x200B; 属性フォルダーがハイライト表示されたセグメントビルダー &#x200B;]](../images/insights-use-cases/consent-analysis/consent-and-preferences.png)

様々な同意オプションと環境設定オプションが表示されます。 このデモでは、様々なマーケティングチャネルに関する連絡先への同意に焦点を当てているので、「**[!UICONTROL マーケティング環境設定]**」を選択します。

![ 「マーケティング環境設定 [!UICONTROL &#x200B; フォルダーがハイライト表示され &#x200B;] セグメントビルダー ](../images/insights-use-cases/consent-analysis/marketing-preferences.png)

マーケティング環境設定のリストが表示されます。 この例のユースケースは、メール、SMS および呼び出しに焦点を当てていますが、他の組み合わせやオプション全体に関してもインサイトを構築できます。 チャネルごとに、以下の手順を実行してオーディエンスを作成します。

オーディエンスの設定を開始するには、**[!UICONTROL SMS を受信]**/**[!UICONTROL メールを受信]**/**[!UICONTROL 呼び出しを受信]** を選択します。

![ マーケティングに使用可能な連絡先チャネルは、オーディエンスビルダーでハイライト表示されます。](../images/insights-use-cases/consent-analysis/channels.png)

[!UICONTROL &#x200B; 購読 &#x200B;] フォルダーが表示されます。 使用可能なオプションから、**[!UICONTROL 選択値]** 属性を選択して中央のペインにドラッグし、ドロップダウンから目的の値を選択します。 この場合、「はい **オプトイン）** を選択します。 次に、ビジネスニーズに応じてオーディエンスに名前を付け、わかりやすい説明を入力します。

>[!NOTE]
>
>作成することを推奨するオーディエンスの数にはソフトリミットがあります。 詳しくは、[ セグメント化ガードレールのドキュメント ](../../profile/guardrails.md#segmentation-guardrails) を参照してください。

セグメントビルダーで「[!UICONTROL &#x200B; はい（オプトイン） &#x200B;]」の値がハイライト表示されている ![[!UICONTROL &#x200B; 選択値 &#x200B;] 属性。 オーディエンスの名前と説明もハイライト表示されます。](../images/insights-use-cases/consent-analysis/choice-value.png)

必要なオーディエンスを作成すると、それらのオーディエンスが「[!UICONTROL &#x200B; オーディエンス &#x200B;][!UICONTROL &#x200B; 参照 &#x200B;] タブに表示されます。

>[!NOTE]
>
>オーディエンスを作成する場合は、バッチセグメント化ジョブが完了するのを待ってから、データを使用して同意ダッシュボードの作成を開始する必要があります。 バッチセグメント化では、セグメント定義を通じてすべてのプロファイルデータを一度に移動し、対応するオーディエンスを生成するプロセスを説明します。 作成したオーディエンスは、書き出しと使用のために保存されます。 バッチセグメントは、24 時間ごとに自動評価されます。

## インサイトの使用 {#consume-insights}

Adobeは、プロファイル、オーディエンス、宛先の各ダッシュボードで自動的に利用できる様々なインサイトを作成しています。 作成したオーディエンスは、これらの事前設定済みのインサイトで自動的に使用できます。 [ プロファイル ](../guides/profiles.md#standard-widgets)、[ オーディエンス ](../guides/audiences.md#standard-widgets) および [ 宛先 ](../guides/destinations.md) ダッシュボードで使用できるインサイトのリストについては、標準ウィジェットのドキュメントを参照してください。

## オーディエンスの重複 {#audience-overlap}

2 つの同意オーディエンス間の重複を確認するには、[!UICONTROL &#x200B; 結合ポリシーによるオーディエンスの重複 &#x200B;] をプロファイルダッシュボードに追加し、ドロップダウンメニューで目的のオーディエンスを選択します。 insightについて詳しくは、ダッシュボードにウィジェットを追加する手順に関するドキュメント [*結合ポリシーによるオーディエンスの重複*](../guides/profiles.md#audience-overlap-by-merge-policy) を参照してください。

<!-- Image needs updating to night mode -->

![ 結合ポリシーウィジェットによるオーディエンスの重複がハイライトされたプロファイルダッシュボード。 ウィジェットは、2 つの同意オーディエンス間の重複を視覚化します。](../images/insights-use-cases/consent-analysis/audience-overlap-by-merge-policy.png)

オーディエンスダッシュボードのオーディエンスの重複レポートを使用して、ユーザーが他のすべてのオーディエンスにわたる呼び出しを受信することに同意したすべてのオーディエンスの重複を表示できます。 同意オーディエンスの重複を表示するには、まず「[!UICONTROL &#x200B; オーディエンス &#x200B;][!UICONTROL &#x200B; 概要 &#x200B;] タブに移動します。 ここから、[!UICONTROL &#x200B; オーディエンスの重複レポート &#x200B;] ウィジェットをオーディエンスダッシュボードに追加できます。 ウィジェットを作成したら、ページ上部のオーディエンスの概要ドロップダウンメニューから「**[!UICONTROL ユーザーが呼び出しに同意しました]** オーディエンスを選択します。 次に、オーディエンスの重複レポートウィジェットで **[!UICONTROL さらに表示]** を選択すると、選択したセグメントに関して、最大 50 個の上部の重複と、最大 50 個の最も少ない重複が表示されます。

<!-- Image needs updating to night mode -->

![ オーディエンスの重複レポートウィジェットを含むオーディエンスダッシュボードが表示されます。 ユーザーが比較オーディエンスとしてオーディエンスの呼び出しに同意し、両方の追加を表示リンクがハイライト表示されます。](../images/insights-use-cases/consent-analysis/audience-overlap-report-user-consent-to-calls.png)

オーディエンスの重複レポートダイアログが拡張され、追加のオーディエンスの重複データが表示されます。

<!-- Image needs updating to night mode -->

![ メールオーディエンスに同意したユーザーがハイライト表示されたオーディエンスの重複レポート ](../images/insights-use-cases/consent-analysis/additional-audience-overlap-reports.png)

## オーディエンスサイズのトレンド {#audience-size-trends}

同意ベースのオーディエンスを作成すると、オーディエンスを作成した日から最大 12 か月のトレンドが自動的に表示されます。 顧客の同意に関する完全な機能トレンドを把握するには、次のウィジェットを [!UICONTROL &#x200B; セグメント &#x200B;][!UICONTROL &#x200B; 概要 &#x200B;] ページに追加します。 これらのインサイトは、時間の経過と共に同意の変化を追跡する強力な手段を提供します。 同意にプラスまたはマイナスの影響を与える可能性のある、並行して実行するキャンペーンとも関連付けられます。 これらのウィジェットに提供される説明は、同意のユースケースに適用されます。

- [ オーディエンスサイズのトレンド ](../guides/audiences.md#audience-size-trend)：このウィジェットは、それぞれの同意が時間の経過と共にどのように変化したかを追跡する方法を提供します。
- [ オーディエンスサイズの変化のトレンド ](../guides/audiences.md#audience-size-change-trend)：このウィジェットは、顧客の同意が毎日変更された方法を追跡します。 例えば、顧客の同意の数が 100,000 減少した場合、その変更が毎日発生した様子を確認できます。
- [ID 別のオーディエンスサイズのトレンド ](../guides/audiences.md#audience-size-trend-by-identity)：このウィジェットを使用すると、それぞれの同意が時間の経過と共にどのように変化したかを追跡でき、さらにメールなどの特定の ID でフィルタリングできます。

<!-- Image needs updating to night mode -->

![ オーディエンスサイズのトレンド、ID 別のオーディエンスサイズのトレンドおよびオーディエンスサイズの変化のトレンドウィジェットが表示されたオーディエンスダッシュボード。 メールオーディエンスに同意したユーザーがハイライト表示されます。](../images/insights-use-cases/consent-analysis/three-audience-trend-widgets.png)

## オーディエンスの概要ダッシュボード {#audiences-overview-dashboard}

「SMS に同意したユーザー」など、同意関連のオーディエンスを作成したら、適切なウィジェットをオーディエンスの概要ダッシュボードに追加することで、オーディエンスに関する主要なパーソナライズされた同意情報を表示できます。 [!UICONTROL Audiences] [!UICONTROL &#x200B; 概要 &#x200B;] に移動して、ウィジェットライブラリから選択したウィジェットを追加します。 ダッシュボードのビューに追加されたウィジェットは、[!UICONTROL &#x200B; ダッシュボードを変更 &#x200B;] 機能を使用してサイズ変更および移動できます。 パーソナライズされたビューには、経時的なトレンド（最大 12 か月）、他のオーディエンスとの重複、オーディエンスの ID 構成などのインサイトを含めることができます。 ビューの例を次に示します。

![ グローバルオーディエンスドロップダウンメニューで SMS オーディエンスに同意したユーザーがハイライト表示されたオーディエンスダッシュボード。](../images/insights-use-cases/consent-analysis/audience-dashboard-user-consent-to-sms.png)

## ユーザー定義ダッシュボード {#usr-defined-dashboards}

ユーザー定義のダッシュボードを使用して独自のウィジェットを作成することもできます。 独自のウィジェットを作成すると、ウィジェットのタイプを完全に制御できるほか、Adobe Real-Time CDP内で柔軟にフィルターなどを追加できます。

例えば、同じグラフで複数の同意オーディエンスのトレンドを分析し、各同意環境設定の変化を経時的に確認できるようにする場合などです。 このタイプのビジュアライゼーションは、ユーザー定義のダッシュボードを最小限の手順と 1 回設定で使用できます。 まず、左側のナビゲーションで **[!UICONTROL ダッシュボード]** を選択します。 [!UICONTROL &#x200B; ダッシュボード &#x200B;] ワークスペースが表示されます。 次に、「**[!UICONTROL ダッシュボードを作成]**」を選択します。 [ ダッシュボードとカスタムウィジェットを作成 ](../standard-dashboards.md) する方法の完全な手順については、ユーザー定義ダッシュボードガイドを参照してください。

![ ダッシュボードと作成ダッシュボードがハイライト表示されたダッシュボードワークスペース。](../images/standard-dashboards/create-dashboard.png)

ウィジェットコンポーザーで [ データモデルを選択 ](../standard-dashboards.md#select-data-model) したら、「`CDPInsights`」に続いて **[!UICONTROL 次へ]** を選択します。 [!UICONTROL &#x200B; テーブルを選択 &#x200B;] ダイアログが表示されます。

![CDPInsights モデルがハイライト表示されたデータモデルを選択ダイアログ ](../images/standard-dashboards/select-data-model-dialog.png)

次の表示では、使用可能なテーブルのリストが左パネルに表示されます。 「`adwh_fact_profile_by_segment_and_namespace_trendlines`」を選択します。

![ 「adwh_fact_profile_by_segment_and_namespace_trendlines」テーブルがハイライト表示されたテーブルを選択ダイアログ。](../images/insights-use-cases/consent-analysis/select-table.png)

選択したテーブルのデータがウィジェットコンポーザーに入力されたら、次の手順を実行します。

- [`[!UICONTROL date]` の [!UICONTROL &#x200B; 属性 &#x200B;]](../standard-dashboards.md#add-filter-attributes) を検索し、「+」アイコンを使用して、ドロップダウンメニューから `[!UICONTROL date]` 属性を X 軸に追加します。
  ![ 追加アイコンとドロップダウンメニューがハイライト表示されているウィジェットコンポーザー。](../images/standard-dashboards/attributes-dropdown.png)
- `[!UICONTROL count_of_profiles]` の [!UICONTROL &#x200B; 属性 &#x200B;] を検索し、「+」アイコンを使用して、ドロップダウンメニューから `[!UICONTROL count_of_profiles]` 属性を Y 軸に追加します。
- 「[!UICONTROL Y 軸 &#x200B;]」フィールドで「`...`」（省略記号）アイコンを選択し、ドロップダウンメニューから [!UICONTROL SUM] 集計関数を選択します。
  ![ データモデル、テーブル、Y 軸のドロップダウンメニュー、SUM 機能がハイライト表示されたウィジェットコンポーザーの同意トレンドウィジェット。](../images/insights-use-cases/consent-analysis/y-axis-sum-function.png)
- [!UICONTROL &#x200B; マーク &#x200B;] ドロップダウンメニューを選択し、グラフのタイプを [!UICONTROL &#x200B; 折れ線グラフ &#x200B;] に変更します。
- `[!UICONTROL segment_name]` の [!UICONTROL &#x200B; 属性 &#x200B;] を検索し、「+」アイコンを使用して、ドロップダウンメニューから `segment_name` を [!UICONTROL &#x200B; フィルター &#x200B;] として追加します。 [!UICONTROL &#x200B; フィルター：Segment_name] ダイアログが表示されます。 同意に関連する、以前に作成したオーディエンスを選択します。 この例では、「**[!UICONTROL ユーザーが呼び出しに同意した]**」、「**[!UICONTROL ユーザーが SMS に同意した]**」、「**[!UICONTROL ユーザーがメールに同意した]** **[!UICONTROL 」を選択した後、「適用]** を選択します。
- `[!UICONTROL segment_name]` の [!UICONTROL &#x200B; 属性 &#x200B;] を検索し、「+」アイコンを選択して、ドロップダウンメニューから `segment_name` を [!UICONTROL &#x200B; カラー &#x200B;] として追加します。
- [ プロパティ [!UICONTROL &#x200B; パネル &#x200B;] を開き ](../standard-dashboards.md#widget-properties) 適切な [!UICONTROL &#x200B; ウィジェットタイトル &#x200B;] と [!UICONTROL &#x200B; 軸ラベル &#x200B;] を入力します。
  ![ プロパティアイコンとウィジェットタイトルがハイライト表示されたウィジェットコンポーザー。](../images/standard-dashboards/properties-panel.png)
- **[!UICONTROL 保存して閉じる]** を選択して、設定を確定します。

>[!TIP]
>
>ダッシュボードを保存する前に、ウィジェットのサイズを変更したり、目的のサイズや位置に移動したりできるようになりました。


次の画像は、完成したウィジェットがどのように表示されるかを示し、その他の潜在的なカスタムインサイトを示しています。 作成可能なウィジェットのタイプについて詳しくは、[ データモデルのドキュメント ](../data-models/cdp-insights-data-model-b2c.md) を参照してください。

<!-- The diagram shows straight lines due to a lack of data, however in your environment the trends will reflect the actual changes over time. -->

![ 完成したカスタム同意トレンドウィジェット ](../images/insights-use-cases/consent-analysis/consent-trends-widget.png)

## 同意ポリシーのトラッキング {#consent-policies}

作成する同意ダッシュボードでは、**同意属性と環境設定属性の配布のみ** を取り込みます。

>[!NOTE]
>
>**Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** のお客様の場合、これらのダッシュボード **同意ポリシーのトラッキングは反映されません**。 使用可能なトラッキングには、作成および有効化されたポリシーの数、オーディエンスメンバーシップへの影響が含まれます。

## 次の手順

このドキュメントでは、Real-Time CDP Insights を使用して顧客の同意環境設定を包括的に把握できるダッシュボードを作成する方法について説明しました。 このドキュメントでは、Real-Time CDPが、同意データに基づく収集、セグメント化、分析およびパーソナライズされたマーケティングキャンペーンがマーケターにとって重要な、プライバシーに焦点を当てた現在の環境に対して堅牢なソリューションを提供する方法について説明します。

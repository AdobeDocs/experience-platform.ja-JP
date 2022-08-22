---
keywords: Experience Platform、プロファイル、セグメント、セグメント、セグメント化、ユーザーインターフェイス、UI、カスタマイズ、セグメントダッシュボード、ダッシュボード
title: セグメントダッシュボードガイド
description: 'Adobe Experience Platformには、組織が作成したセグメントに関する重要な情報を表示できるダッシュボードが用意されています。 '
type: Documentation
exl-id: de5e07bc-2c44-416e-99db-7607059117cb
source-git-commit: 05e63064dc8eb3f070a383f508cc4a86d4f5e9cc
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 11%

---

# セグメントダッシュボード {#segment-dashboard}

Adobe Experience Platformのユーザーインターフェイス (UI) には、毎日のスナップショットでキャプチャされた、セグメントに関する重要な情報を表示できるダッシュボードが用意されています。 このガイドでは、UI でのセグメントダッシュボードへのアクセスと操作の方法を説明し、ダッシュボードに表示されるビジュアライゼーションに関する詳細を提供します。

Platform ユーザーインターフェイス内のAdobe Experience Platform Segmentation Service のすべての機能の概要については、 [セグメント化サービス UI ガイド](../../segmentation/ui/overview.md).

## セグメントダッシュボードデータ

セグメントダッシュボードには、組織がExperience Platform内のプロファイルストアに保持している属性（レコード）データのスナップショットが表示されます。 スナップショットには、イベント（時系列）データは含まれていません。

スナップショットの属性データは、スナップショットが作成された特定の時点でのデータと完全に同じ状態で表示されます。 つまり、スナップショットはデータの近似やサンプルではなく、セグメントダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに加えられた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

## セグメントダッシュボードの詳細

次に移動するには： [!UICONTROL セグメント] Platform UI 内のダッシュボードで、「 **[!UICONTROL セグメント]** 左側のレールで、 **[!UICONTROL 概要]** タブをクリックして、ダッシュボードを表示します。

>[!NOTE]
>
>組織が Platform を初めて使用し、アクティブなプロファイルデータセットや結合ポリシーがまだ作成されていない場合、 [!UICONTROL セグメント] ダッシュボードが表示されません。 代わりに、 [!UICONTROL 概要] 「 」タブには、セグメント化を開始する際に役立つリンクとドキュメントが表示されます。

![](../images/segments/dashboard-overview.png)

### の変更 [!UICONTROL セグメント] dashboard

次の外観を変更できます： [!UICONTROL セグメント] 選択してダッシュボード **[!UICONTROL ダッシュボードを変更]**. これにより、ダッシュボードからウィジェットを移動、追加、削除したり、 **[!UICONTROL Widget ライブラリ]** 使用可能なウィジェットを参照し、組織のカスタムウィジェットを作成するには。

詳しくは、 [ダッシュボードの変更](../customize/modify.md) および [ウィジェットライブラリの概要](../customize/widget-library.md) ドキュメントを参照してください。

## セグメントを選択

ダッシュボードは表示するセグメントを自動的に選択しますが、ドロップダウンメニューまたはセグメントセレクターを使用してセグメントを変更できます。

別のセグメントを選択するには、セグメント名の横にあるドロップダウンを選択するか、セグメントセレクターを使用して、セグメント選択ダイアログを開きます。

![](../images/segments/change-segment.png)

![](../images/segments/select-segment-dialog.png)

## ウィジェットと指標

セグメントダッシュボードはウィジェットで構成されています。ウィジェットは、選択したセグメントに関する重要な情報を提供する読み取り専用の指標です。

最新のスナップショットの日時が [!UICONTROL 概要] タブをクリックします。 すべてのウィジェットデータは、その日時時点で正確です。 スナップショットのタイムスタンプは UTC で提供されます。個々のユーザーや組織のタイムゾーンに含まれていません。

![ウィジェットのタイムスタンプがハイライトされた「セグメントの概要」タブ。](../images/segments/widget-timestamp.png)

## 標準ウィジェット {#standard-widgets}

Adobeには、セグメントに関連する様々な指標を視覚化するために使用できる、複数の標準ウィジェットが用意されています。 また、 [!UICONTROL Widget ライブラリ]. カスタムウィジェットの作成の詳細については、まず [ウィジェットライブラリの概要](../customize/widget-library.md).

使用可能な各標準ウィジェットの詳細を確認するには、次のリストからウィジェットの名前を選択します。

* [[!UICONTROL オーディエンスサイズ]](#audience-size)
* [[!UICONTROL Audience Activation の順序]](#audience-activation-order)
* [[!UICONTROL オーディエンスサイズのトレンド]](#audience-size-trend)
* [[!UICONTROL オーディエンスサイズの変更のトレンド]](#audience-size-change-trend)
* [[!UICONTROL ID 別のオーディエンスサイズのトレンド]](#audience-size-trend-by-identity)
* [[!UICONTROL オーディエンスの重複]](#audience-overlap)
* [[!UICONTROL ID の重複]](#identity-overlap)
* [[!UICONTROL ID 別プロファイル]](#profiles-by-identity)

### [!UICONTROL オーディエンスサイズ] {#audience-size}

>[!CONTEXTUALHELP]
>id="platform_dashboards_segments_audiencesize"
>title="オーディエンスサイズ"
>abstract="このウィジェットには、選択したセグメント内の結合プロファイルの合計数が表示されます。 この数は、データに適用される結合ポリシーによって異なり、最新のスナップショットの時点で正しくなります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/segments.html#audience-size" text="詳しくは、ドキュメントを参照してください。"

この **[!UICONTROL オーディエンスサイズ]** ウィジェットには、スナップショットが作成された時点での、選択したセグメント内の結合プロファイルの合計数が表示されます。 この数は、プロファイルフラグメントを結合してセグメント内の個々のプロファイルを 1 つ形成するために、セグメント結合ポリシーをプロファイルデータに適用した結果です。

フラグメントと結合されたプロファイルの詳細については、まず [リアルタイム顧客プロファイルの概要](../../profile/home.md).

![](../images/segments/audience-size.png)

### [!UICONTROL オーディエンスサイズのトレンド] {#audience-size-trend}

>[!CONTEXTUALHELP]
>id="platform_dashboards_segments_audiencesizetrend"
>title="オーディエンスサイズのトレンド"
>abstract="このウィジェットは、 **任意** 日別のスナップショット中に、過去 30 日間、90 日間、12 ヶ月間にキャプチャされたセグメント定義。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/segments.html#audience-size-trend" text="詳しくは、ドキュメントを参照してください。"

この **[!UICONTROL オーディエンスサイズのトレンド]** ウィジェットには、次の条件を満たすプロファイルの総数を示す線グラフの図が表示されます： **任意** 特定の期間のセグメント定義。 オーディエンスサイズのトレンドを 30 日、90 日、12 ヶ月の期間で視覚化できます。 期間は、ウィジェットのドロップダウンメニューから選択します。 オーディエンスサイズは、x 軸の y 軸と時間に反映されます。

このウィジェットには、自動 [!UICONTROL キャプション] 機械学習モデルがグラフとセグメントのデータを分析し、主要な傾向と重要なイベントを説明するキャプションを自動的に生成する機能。 選択 **[!UICONTROL キャプション]** 自動キャプションダイアログを開く。

![セグメントの概要には、オーディエンスサイズのトレンドウィジェットが表示されます。](../images/segments/audience-size-trend-captions.png)

自動キャプションダイアログが開き、データに関するインサイトが表示されます。

![オーディエンスサイズトレンドウィジェット用の自動キャプションダイアログ。](../images/segments/audience-size-trend-automatic-captions-dialog.png)

セグメント評価と、プロファイルがセグメントからどのように適合および出るかについて詳しくは、 [セグメント化サービスのドキュメント](../../segmentation/home.md).

### [!UICONTROL オーディエンスサイズの変更のトレンド] {#audience-size-change-trend}

このウィジェットは、最新の日別スナップショット間の特定のセグメントに対して選定されたプロファイルの合計数の違いを示す折れ線グラフを提供します。分析の対象として選択したセグメントは、概要ドロップダウンから選択されます。 30 日、90 日および 12 か月の期間のトレンド分析の期間を可視化できます。期間は、ウィジェットのドロップダウンメニューから選択します。 オーディエンスサイズは、x 軸の y 軸と時間に反映されます。

![オーディエンスサイズ変更トレンドウィジェット。](../images/segments/audience-size-change-trend.png)

### [!UICONTROL ID 別のオーディエンスサイズのトレンド] {#audience-size-trend-by-identity}

このウィジェットは、ウィジェットドロップダウンメニューから選択した ID タイプに基づいて、特定のセグメントに関するオーディエンスサイズのトレンドを示します。 分析に使用されるセグメントは、概要ドロップダウンから選択します。 30 日、90 日および 12 か月の期間のトレンド分析の期間を可視化できます。期間は、ウィジェットのドロップダウンメニューから選択します。

![ID ウィジェット別のオーディエンスサイズのトレンド。](../images/segments/audience-size-trend-by-identity.png)

### [!UICONTROL Audience Activation の順序] {#audience-activation-order}

この [!UICONTROL オーディエンスのアクティベーションの順序] ウィジェットには、 [!UICONTROL 宛先名]、 [!UICONTROL platform]、およびアクティベーション [!UICONTROL 日付] オーディエンスの リストは、最新性に応じて高い順から低い順に並べられ、最大 10 行まで収容できます。

![オーディエンスのアクティベーションの順序ウィジェット。](../images/segments/audience-activation-order.png)

### [!UICONTROL オーディエンスの重複] {#audience-overlap}

このウィジェットは、2 つのセグメントのうち、両方のセグメント定義の条件を満たすプロファイルの数を表します。 比較に使用するセグメントは、ウィジェットのドロップダウンメニューから選択します。 関連するセグメント定義内に含まれるプロファイルの合計数は、ベン図の円または積集合にカーソルを合わせると確認できます。

このウィジェットを使用すると、セグメント定義の結果の類似性を視覚化することで、セグメント化戦略を最適化できます。

![オーディエンスの重複ウィジェット。](../images/segments/audience-overlap.png)

<!-- * [[!UICONTROL Audience overlap report]](#audience-overlap-report) -->
<!-- ### [!UICONTROL Audience overlap report] {#audience-overlap-report} -->

<!-- View an ordered list of audiences by Highest or Lowest overlap percentages. -->

<!-- ![The Audience overlap report widget.]() -->

<!-- https://jira.corp.adobe.com/browse/PLAT-125511 -->

### [!UICONTROL ID の重複] {#identity-overlap}

>[!CONTEXTUALHELP]
>id="platform_dashboards_segments_identityoverlap"
>title="ID の重複"
>abstract="このウィジェットは、選択された ID の両方を含むセグメント内のプロファイルの重複を表示します。 円には、各 ID の相対サイズが表示されます。 両方の名前空間を含むプロファイルの数は、円の間の重複で表されます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/segments.html#identity-overlap" text="詳しくは、ドキュメントを参照してください。"

この **[!UICONTROL ID の重複]** ウィジェットには、複数の ID を含むセグメント内のプロファイルの重複を示す、ベン図（セット図）が表示されます。

ウィジェットのドロップダウンメニューを使用して、比較する ID を選択します。 円には、選択した各 ID の相対サイズが表示され、両方の名前空間を含むプロファイルの数は、円間の重複のサイズで表されます。

顧客が複数のチャネルでブランドとやり取りする場合、複数の ID がその顧客に関連付けられるので、組織は複数の ID のフラグメントを含む複数のプロファイルを持つ可能性が高くなります。

ID の詳細については、 [Adobe Experience Platform ID サービスドキュメント](../../identity-service/home.md).

![](../images/segments/identity-overlap.png)

### [!UICONTROL ID 別プロファイル] {#profiles-by-identity}

>[!CONTEXTUALHELP]
>id="platform_dashboards_segments_profilesbyidentity"
>title="ID 別プロファイル"
>abstract="このウィジェットは、選択したセグメント内のすべての結合プロファイルで ID の分類を表示します。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/segments.html#profiles-by-identity" text="詳しくは、ドキュメントを参照してください。"

この **[!UICONTROL ID 別プロファイル]** ウィジェットは、選択したセグメント内のすべての結合プロファイルで id の分類を表示します。 1 つのプロファイルが複数の ID を関連付けている可能性があるので、ID 別のプロファイルの合計数は、セグメント内のプロファイルの合計数より多くなる場合があります。 つまり、顧客が複数のチャネルでブランドとやり取りする場合、複数の ID が個々の顧客に関連付けられる可能性があるので、各 ID に表示される値を合計すると、セグメントの合計オーディエンスサイズよりも大きくなる場合があります。

選択 **[!UICONTROL キャプション]** 自動キャプションダイアログを開く。

![ID キャプション別のプロファイルダイアログ。](../images/segments/profiles-by-identity.png)

機械学習モデルは、データの全体的な分布と主要なディメンションを分析することで、データインサイトを自動的に生成します。

ID の詳細については、 [Adobe Experience Platform ID サービスドキュメント](../../identity-service/home.md).

## 次の手順

このドキュメントに従うと、セグメントダッシュボードを見つけ、表示するセグメントを選択できるようになります。 また、使用可能なウィジェットに表示される指標も理解する必要があります。 Experience PlatformUI でのセグメントの操作について詳しくは、 [セグメント化サービス UI ガイド](../../segmentation/ui/overview.md).

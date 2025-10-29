---
title: エッジパーソナライゼーションの宛先に対してオーディエンスをアクティブ化する
description: 同じページと次のページのパーソナライゼーションのユースケースに対して、Adobe Experience Platformからエッジパーソナライゼーションの宛先へのオーディエンスをアクティブ化する方法を説明します。
type: Tutorial
exl-id: cd7132eb-4047-4faa-a224-47366846cb56
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 8%

---


# エッジパーソナライゼーションの宛先に対してオーディエンスをアクティブ化する

## 概要 {#overview}

Adobe Experience Platformでは、[&#x200B; エッジのセグメント化 &#x200B;](../../segmentation/methods/edge-segmentation.md) と [&#x200B; エッジの宛先 &#x200B;](/help/destinations/destination-types.md#edge-personalization-destinations) を使用して、お客様が大規模かつリアルタイムでオーディエンスを作成およびターゲット化できるようにします。 この機能は、同じページおよび次のページのパーソナライゼーションのユースケースを設定するのに役立ちます。

エッジの宛先の例としては、[Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) や [&#x200B; カスタムパーソナライゼーション &#x200B;](../../destinations/catalog/personalization/custom-personalization.md) 接続があります。

>[!NOTE]
>
>データストリーム ID を使用して [Adobe Target接続の設定 &#x200B;](../catalog/personalization/adobe-target-connection.md) なし *を行う場合*、この記事で説明するユースケースはサポートされていません。 データストリームがない場合は、次のセッションのパーソナライゼーションのユースケースのみがサポートされます。

>[!IMPORTANT]
> 
>* データをアクティブ化し、ワークフローの [&#x200B; マッピングステップ &#x200B;](#mapping) を有効にするには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]** および **[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。
>* ワークフローの [&#x200B; マッピングステップ &#x200B;](#mapping) を実行せずにデータをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Segment without Mapping]**、**[!UICONTROL View Profiles]** および **[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}
> 
> [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この記事では、Adobe Experience Platform Edge 宛先に対してオーディエンスをアクティブ化するために必要なワークフローについて説明します。 これらの宛先を [&#x200B; エッジセグメント化 &#x200B;](../../segmentation/methods/edge-segmentation.md) およびオプションの [&#x200B; プロファイル属性マッピング &#x200B;](#mapping) と併用すると、web プロパティやモバイルプロパティで、同じページおよび次のページのパーソナライゼーションのユースケースが可能になります。

エッジパーソナライゼーション用にAdobe Target接続を設定する方法の概要については、以下のビデオをご覧ください。

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新の情報については、以下の節で説明する設定手順を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3449794/?captions=jpn&quality=12&learn=on)

Adobe Targetとカスタムパーソナライゼーションの宛先に対してオーディエンスとプロファイル属性を共有する方法の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3447356/?captions=jpn&quality=12&learn=on)

## ユースケース {#use-cases}

Adobe TargetなどのAdobe パーソナライゼーションソリューション、または独自のパーソナライゼーションパートナープラットフォーム（[!DNL Optimizely]、[!DNL Pega] など）および独自のシステム（社内CMSなど）を使用して、[&#x200B; カスタム Personalization](../catalog/personalization/custom-personalization.md) 宛先を介してより深いカスタマーパーソナライゼーションエクスペリエンスを強化します。 これらをすべて、また、Experience Platform Edge Networkのデータ収集およびセグメント化機能も活用します。

以下に説明するユースケースには、サイトのパーソナライゼーションとターゲットを設定したオンサイト広告の両方が含まれます。

これらの使用例を有効にするには、Experience Platformからオーディエンスとプロファイル属性情報の両方を取得し、その情報をExperience Platform UI の [Adobe Target](../catalog/personalization/adobe-target-connection.md) または [&#x200B; カスタム Personalization](../catalog/personalization/custom-personalization.md) 接続に送信する、迅速で合理化された方法が必要です。

### 同じページのパーソナライゼーション {#same-page}

ユーザーが web サイトのページにアクセスします。 Adobe以外のプラットフォーム（[、](../catalog/personalization/custom-personalization.md) など）では [!DNL Pega] カスタムパーソナライゼーション [!DNL Optimizely] 接続を使用して、現在のページ訪問情報（参照 URL、ブラウザー言語、埋め込み商品情報など）を使用して次の操作または決定（パーソナライゼーションなど）を選択できます。

### 次のページのパーソナライゼーション {#next-page}

ユーザーが web サイトのページ A にアクセスします。 このインタラクションに基づいて、ユーザーは一連のオーディエンスに対して選定されました。 次に、ユーザーがページ A からページ B に移動するリンクをクリックします。ページ A での以前のインタラクション中にユーザーが選定したオーディエンスは、現在の web サイト訪問によって決定されたプロファイルの更新と共に、次のアクションまたは決定（例えば、訪問者に表示する広告バナー、A/B テストの場合は表示するページのバージョン）の強化に使用されます。

### 次のセッションのパーソナライゼーション {#next-session}

ユーザーが web サイトの複数のページにアクセスします。 これらのインタラクションに基づいて、ユーザーは一連のオーディエンスに対して選定されました。 次に、ユーザーは現在の閲覧セッションを終了します。

次の日に、ユーザーは同じ顧客 web サイトに戻ります。 訪問したすべての web サイトページとの以前のインタラクション中に選定したオーディエンスは、現在の web サイト訪問によって決定されたプロファイルの更新と共に、次のアクション/決定（例えば、訪問者に表示する広告バナー、A/B テストの場合は表示するページのバージョン）の選択に使用されます。

### ホームページバナーのパーソナライズ {#home-page-banner}

あるホームレンタルや販売会社が、Adobe Experience Platformでのオーディエンスの選定に基づいて、バナー付きのホームページをパーソナライズしたいと考えています。 会社は、パーソナライズされたエクスペリエンスを体験できるオーディエンスを選択し、それらのオーディエンスを Target オファーのターゲティング条件としてAdobe Targetに送信できます。

## 前提条件 {#prerequisites}

### データ収集 UI でのデータストリームの設定 {#configure-datastream}

パーソナライゼーションの宛先の設定における最初の手順は、Experience Platform Web SDK のデータストリームを設定することです。 これは Data Collection UI で行います。

データストリームを設定する際に、の下で **[!UICONTROL Adobe Experience Platform]** と **[!UICONTROL Edge Segmentation]** の両方が選択されていることを **[!UICONTROL Personalization Destinations]** 認します。

>[!TIP]
>
>2024 年 4 月リリース以降、（Adobe Targetへの接続の設定 [&#x200B; の際に「Edgeのセグメント化」チェックボックスをオンにする必要はあり &#x200B;](/help/destinations/catalog/personalization/adobe-target-connection.md) せん。 この場合、[&#x200B; 次のセッションのパーソナライゼーション &#x200B;](#next-session) が利用可能な唯一のパーソナライゼーションのユースケースです。

![Edgeのセグメント化とPersonalizationの宛先がハイライト表示されたデータストリーム設定 &#x200B;](../assets/ui/activate-edge-personalization-destinations/datastream-config.png)

データストリームの設定方法の詳細については、[Experience Platform Web SDK ドキュメント &#x200B;](../../datastreams/configure.md#aep) に記載されている手順に従ってください。

### [!DNL Active-On-Edge] 結合ポリシーの作成 {#create-merge-policy}

宛先接続を作成したら、[!DNL Active-On-Edge] 結合ポリシーを作成する必要があります。 [!DNL Active-On-Edge] 結合ポリシーは、オーディエンスが常に [&#x200B; エッジ上で &#x200B;](../../segmentation/methods/edge-segmentation.md) 評価され、リアルタイムおよび次のページのパーソナライゼーションのユースケースで利用できるようにします。

>[!IMPORTANT]
>
>現在、エッジ宛先では、デフォルトとして設定された [Edgeでのアクティブ化結合ポリシー &#x200B;](../../segmentation/ui/segment-builder.md#merge-policies) を使用するオーディエンスのアクティブ化のみをサポートしています。 異なる結合ポリシーを使用するオーディエンスをエッジ宛先にマッピングした場合、それらのオーディエンスは評価されません。

[&#x200B; 結合ポリシーの作成 &#x200B;](../../profile/merge-policies/ui-guide.md#create-a-merge-policy) の手順に従い、必ず「**[!UICONTROL Active-On-Edge Merge Policy]**」切り替えスイッチを有効にします。

### Experience Platformでの新しいオーディエンスの作成 {#create-audience}

[!DNL Active-On-Edge] 結合ポリシーを作成したら、Experience Platformで新しいオーディエンスを作成する必要があります。

[&#x200B; オーディエンスビルダー &#x200B;](../../segmentation/ui/segment-builder.md) ガイドに従って新しいオーディエンスを作成し、前の手順で作成した [&#x200B; 結合ポリシーを必ず &#x200B;](../../segmentation/ui/segment-builder.md#merge-policies) 割り当て [!DNL Active-On-Edge] します。

### 宛先接続の作成 {#connect-destination}

データストリームを設定したら、パーソナライゼーションの宛先の設定を開始できます。

新しい宛先接続の作成方法に関する詳細な手順については、[宛先接続の作成チュートリアル](../ui/connect-destination.md)に従ってください。

設定する宛先に応じて、次の記事で宛先固有の前提条件と関連情報について確認してください。

* [Adobe Target 接続](../catalog/personalization/adobe-target-connection.md#parameters)
* [カスタムパーソナライゼーション接続](../catalog/personalization/custom-personalization.md#parameters)

## 宛先の選択 {#select-destination}

前提条件を満たしたら、同じページと次のページのパーソナライゼーションに使用するエッジパーソナライゼーションの宛先を選択できます。

1. **[!UICONTROL Connections > Destinations]** に移動して、「**[!UICONTROL Catalog]**」タブを選択します。

   ![Experience Platform UI でハイライト表示された「宛先カタログ」タブ &#x200B;](../assets/ui/activate-edge-personalization-destinations/catalog-tab.png)

1. 次の画像に示すように、オーディエンスをアクティベートするパーソナライゼーションの宛先に対応するカードで **[!UICONTROL Activate audiences]** を選択します。

   ![&#x200B; カタログの宛先カードでハイライト表示されたオーディエンスコントロールをアクティブ化 &#x200B;](../assets/ui/activate-edge-personalization-destinations/activate-audiences-button.png)

1. オーディエンスをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL Next]**」を選択します。

   ![&#x200B; アクティベーションワークフローの宛先手順を選択します。](../assets/ui/activate-edge-personalization-destinations/select-destination.png)

1. 次の節の「オーディエンスを選択 [&#x200B; に移動し &#x200B;](#select-audiences) す。

## オーディエンスを選択 {#select-audiences}

オーディエンス名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするオーディエンスを選択し、「アクティベート **[!UICONTROL Next]**」を選択します。

宛先に対してアクティベートするオーディエンスを選択するには、オーディエンス名の左側にあるチェックボックスを使用したあと、「アクティベート **[!UICONTROL Next]**」を選択します。

接触チャネルに応じて、複数のタイプのオーディエンスから選択できます。

* **[!UICONTROL Segmentation Service]**: Segmentation Service によってExperience Platform内で生成されたオーディエンス。 詳しくは、[&#x200B; セグメント化ドキュメント &#x200B;](../../segmentation/ui/overview.md) を参照してください。
* **[!UICONTROL Custom upload]**:Experience Platform以外で生成され、CSV ファイルとしてExperience Platformにアップロードされたオーディエンス。 外部オーディエンスについて詳しくは、[&#x200B; オーディエンスの読み込み &#x200B;](../../segmentation/ui/audience-portal.md#import-audience) に関するドキュメントを参照してください。
* その他のタイプのオーディエンス。他のAdobe ソリューション（[!DNL Audience Manager] など）から派生します。

![&#x200B; 複数のオーディエンスがハイライト表示されたアクティベーションワークフローのオーディエンス選択手順。](../assets/ui/activate-edge-personalization-destinations/select-audiences.png)

## 属性のマッピング {#mapping}

>[!IMPORTANT]
>
>プロファイル属性には、機密データが含まれている場合があります。 このデータを保護するために、**[!UICONTROL Custom Personalization]** 宛先では、属性ベースのパーソナライゼーション用に宛先を設定する際に [1&rbrace;Edge Network API&rbrace; を使用する必要があります。 &#x200B;](https://developer.adobe.com/data-collection-apis/docs/)すべてのEdge Network API 呼び出しは、[&#x200B; 認証済みコンテキスト &#x200B;](https://developer.adobe.com/data-collection-apis/docs/getting-started/authentication/) で行う必要があります。
>
><br> 既に Web SDKまたはモバイル SDKを使用している場合は、サーバーサイド統合を追加することで、Edge Network API を介して属性を取得できます。
>
><br> 上記の要件に従わない場合、パーソナライゼーションはオーディエンスメンバーシップのみに基づきます。

ユーザーに対してパーソナライゼーションのユースケースを有効にする対象に基づいて属性を選択します。 つまり、属性の値が変更された場合や属性がプロファイルに追加された場合、そのプロファイルはオーディエンスのメンバーになり、パーソナライゼーションの宛先に対してアクティブ化されます。

属性の追加はオプションです。引き続き次の手順に進み、属性を選択せずに同じページおよび次のページのパーソナライゼーションを有効にすることができます。 この手順で属性を追加しない場合でも、プロファイルのオーディエンスメンバーシップおよび ID マップの選定に基づいて、パーソナライゼーションが実行されます。

![&#x200B; 属性が選択されたマッピングステップを示す画像 &#x200B;](../assets/ui/activate-edge-personalization-destinations/mapping-step.png)

### ソース属性を選択 {#select-source-attributes}

ソース属性を追加するには、次に示すように、**[!UICONTROL Add new field]** 列の **[!UICONTROL Source field]** コントロールを選択し、目的の XDM 属性フィールドを検索するか、移動します。

![&#x200B; マッピングステップでターゲット属性を選択する方法を示す画面録画。](../assets/ui/activate-edge-personalization-destinations/mapping-step-select-attribute.gif)

### ターゲット属性を選択 {#select-target-attributes}

ターゲット属性を追加するには、「**[!UICONTROL Add new field]**」列の **[!UICONTROL Target field]** コントロールを選択し、ソース属性をマッピングするカスタム属性名を入力します。

>[!NOTE]
>
>ターゲット属性の選択は、宛先プラットフォームでわかりやすい名前のフィールドマッピングをサポートするために、[&#x200B; カスタム Personalization](../catalog/personalization/custom-personalization.md) アクティベーションワークフローにのみ適用されます。

![&#x200B; マッピングステップで XDM 属性を選択する方法を示す画面録画 &#x200B;](../assets/ui/activate-edge-personalization-destinations/mapping-step-select-target-attribute.gif)

## オーディエンスの書き出しのスケジュール {#scheduling}

デフォルトでは、[!UICONTROL Audience schedule] ページには、現在のアクティベーションフローで選択した、新しく選択されたオーディエンスのみが表示されます。

宛先に対してアクティブ化されているすべてのオーディエンスを表示するには、「フィルター」オプションを使用し、**[!UICONTROL Show new audiences only]** フィルターを無効にします。

![&#x200B; すべてのオーディエンスフィルターがハイライト表示されています &#x200B;](../assets/ui/activate-edge-personalization-destinations/all-audiences.png)。

**[!UICONTROL Audience schedule]** ページで、各オーディエンスを選択し、**[!UICONTROL Start date]** セレクターと **[!UICONTROL End date]** セレクターを使用して、宛先にデータを送信する時間間隔を設定します。

![&#x200B; 開始日と終了日がハイライト表示された、アクティベーションワークフローのオーディエンススケジュールステップ。](../assets/ui/activate-edge-personalization-destinations/audience-schedule.png)

「**[!UICONTROL Next]**」を選択して、[!UICONTROL Review] のページに移動します。

## レビュー {#review}

**[!UICONTROL Review]** のページには、選択内容の概要が表示されます。 **[!UICONTROL Cancel]** を選択してフローを中断する **[!UICONTROL Back]**、設定を変更する、または **[!UICONTROL Finish]** を選択して選択内容を確認し、宛先へのデータの送信を開始します。

![&#x200B; レビュー手順の選択の概要。](../assets/ui/activate-edge-personalization-destinations/review.png)

### 同意ポリシーの評価 {#consent-policy-evaluation}

お客様の組織で **Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した場合、**[!UICONTROL View applicable consent policies]** を選択すると、どの同意ポリシーが適用され、その結果、いくつのプロファイルがアクティベーションに含まれるかを確認することができます。 詳しくは、[&#x200B; 同意ポリシーの評価 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を参照してください。

### データ使用ポリシーのチェック {#data-usage-policy-checks}

**[!UICONTROL Review]** の手順では、Experience Platformはデータ使用ポリシーの違反もチェックします。 ポリシーに違反した場合の例を次に示します。違反を解決するまで、Audience Activation ワークフローを完了することはできません。 ポリシー違反の解決方法については、データガバナンスに関するドキュメントの [&#x200B; データ使用ポリシー違反 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#data-usage-violation) を参照してください。

![&#x200B; データポリシー違反の例 &#x200B;](../assets/common/data-policy-violation.png)

### オーディエンスのフィルタリング {#filter-audiences}

この手順では、ページで使用可能なフィルターを使用して、このワークフローの一環としてスケジュールまたはマッピングが更新されたオーディエンスのみを表示できます。 また、表示するテーブル列を切り替えることもできます。

![&#x200B; レビューステップで使用可能なオーディエンスフィルターを示す画面録画。](../assets/ui/activate-edge-personalization-destinations/filter-audiences-review-step.gif)

選択内容に満足し、ポリシー違反が検出されていない場合は、「**[!UICONTROL Finish]**」を選択して選択内容を確定し、宛先へのデータの送信を開始します。

<!--

Commenting out this part since destination monitoring is not available currently for the Adobe Target and Custom Personalization destinations.

## Verify audience activation {#verify}

Check the [destination monitoring documentation](../../dataflows/ui/monitor-destinations.md) for detailed information on how to monitor the flow of data to your destinations.

-->

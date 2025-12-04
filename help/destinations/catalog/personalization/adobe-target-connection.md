---
keywords: ターゲットのパーソナライゼーション;宛先;Experience Platform ターゲットの宛先:Adobe Target の宛先;
title: Adobe Target 接続
description: Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。
exl-id: 3e3c405b-8add-4efb-9389-5ad695bc9799
source-git-commit: f8863b79d0524ad3221d594756c027956e836070
workflow-type: tm+mt
source-wordcount: '1755'
ht-degree: 30%

---

# Adobe Target 接続 {#adobe-target-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2024年4月 | 機能とドキュメントの更新 | Target の宛先に接続し、データストリームを使用する際に、必ずしもエッジのセグメント化のためにデータストリームを有効にする必要がなくなりました *。* つまり、Target の宛先は、実行できるユースケースは異なりますが、バッチオーディエンスとストリーミングオーディエンスで機能します。 詳しくは、「[&#x200B; 接続パラメーター &#x200B;](#parameters)」セクションのテーブルを参照してください。 |
| 2024年1月 | 機能とドキュメントの更新 | デフォルトの実稼動サンドボックスおよびその他のデフォルト以外のサンドボックス用に、Adobe Target接続に対してオーディエンスとプロファイル属性を共有できるようになりました。 |
| 2023年6月 | 機能とドキュメントの更新 | 2023 年 6 月現在、新しいAdobe Target宛先接続を設定する際に、オーディエンスを共有するAdobe Target Workspace を選択できます。 詳しくは、[接続パラメーター](#parameters)の節を参照してください。また、ワークスペースについて詳しくは、Adobe Target での[ワークスペースの設定](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja)に関するチュートリアルを参照してください。 |
| 2023年5月 | 機能とドキュメントの更新 | 2023 年 5 月の時点で、**[!UICONTROL Adobe Target]** 接続は [&#x200B; 属性ベースのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#map-attributes) をサポートしており、すべてのお客様が一般に利用できます。 |

{style="table-layout:auto"}

## 概要 {#overview}

Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。

Adobe Targetは、Adobe Experience Platformの宛先カタログ内のパーソナライゼーション接続です。

## ビデオの概要 {#video-overview}

Experience PlatformでAdobe Target接続を設定する方法の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3449794/?captions=jpn&quality=12&learn=on)

## 実装タイプに基づいてサポートされるユースケース {#supported-use-cases}

次の表に、「エッジセグメント化 [&#x200B; が有効な場合と有効になっていない場合で、web SDKを使用する場合としない場合で、実装タイプに基づいてAdobe Target宛先でサポートされるユースケースを示し &#x200B;](/help/segmentation/home.md#edge) す。

| Adobe Targetの実装 *Web SDKを使用しない* | Adobe Target実装 *Web SDK* 使用 | Adobe Target実装 *Web SDK* および *エッジセグメンテーション* オフ |
|---|---|---|
| <ul><li>データストリームは必須ではありません。 Adobe Targetは、[at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html)、[&#x200B; サーバーサイド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html?lang=ja#server-side-implementation) または [&#x200B; ハイブリッド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html?lang=ja#hybrid-implementation) の実装方法を使用してデプロイできます。</li><li>[Edgeのセグメント化 &#x200B;](../../../segmentation/methods/edge-segmentation.md) はサポートされていません。</li><li>[&#x200B; 同じページと次のページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md) はサポートされていません。</li><li>*デフォルトの実稼動サンドボックス* および非デフォルトのサンドボックス用のAdobe Target接続に、オーディエンスとプロファイル属性を共有できます。</li><li>データストリームを使用せずに次のセッションのパーソナライゼーションを設定するには、[at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html?lang=ja) を使用します。</li></ul> | <ul><li>Adobe TargetとExperience Platformがサービスとして設定されたデータストリームが必要です。</li><li>Edgeのセグメント化が期待どおりに機能します。</li><li>[&#x200B; 同じページと次のページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#use-cases) がサポートされています。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有がサポートされています。</li></ul> | <ul><li>Adobe TargetとExperience Platformがサービスとして設定されたデータストリームが必要です。</li><li>[&#x200B; データストリームの設定 &#x200B;](/help/destinations/ui/activate-edge-personalization-destinations.md#configure-datastream) 時に、「**Edgeのセグメント化**」チェックボックスをオンにしないでください。</li><li>[&#x200B; 次のセッションのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#next-session) がサポートされます。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有がサポートされています。</li></ul> |


## 前提条件 {#prerequisites}

### データストリーム {#datastream}

[&#x200B; データストリームを使用 &#x200B;](#parameters) するようにAdobe Target接続を設定する場合、[Adobe Experience Platform Data Collection](/help/collection/home.md) を実装する必要があります。

データストリームを使用せずにAdobe Target接続を設定する場合は、web SDKを実装する必要はありません。

>[!IMPORTANT]
>
>[!DNL Adobe Target] 接続を作成する前に、[同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定](../../ui/activate-edge-personalization-destinations.md)を行う方法を参照してください。このガイドでは、複数のExperience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションのユースケースに必要な設定手順を説明します。 同じページと次のページのパーソナライゼーションのユースケースを実現するには、Adobe Target接続の設定時にデータストリームを使用する必要があります。

### Adobe Targetの前提条件 {#prerequisites-in-adobe-target}

Adobe Targetで、ユーザーが以下を持っていることを確認します。

* [&#x200B; デフォルトワークスペース &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=ja#default-workspace) へのアクセス
* **承認者** [&#x200B; 役割 &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=ja#roles-and-permissions)。

詳しくは、[Target Premiumおよび &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja#section_8C425E43E5DD4111BBFC734A2B7ABC80)2&rbrace;Target Standard&rbrace; の権限の付与を参照してくだ [&#x200B; い。](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html?lang=ja#roles-permissions)

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

>[!IMPORTANT]
>
>*同じページと次のページのパーソナライゼーションのユースケースに対するエッジオーディエンス* をアクティブ化する場合、オーディエンス *必須*[&#x200B; エッジでのアクティブ化結合ポリシー &#x200B;](../../../segmentation/ui/segment-builder.md#merge-policies) を使用します。 [!DNL active-on-edge] 結合ポリシーにより、オーディエンスは常に [&#x200B; エッジ上で &#x200B;](../../../segmentation/methods/edge-segmentation.md) 評価され、リアルタイムおよび次のページのパーソナライゼーションのユースケースで利用できるようになります。  実装タイプに基づいて、[&#x200B; 利用可能なすべてのユースケース &#x200B;](#parameter) をお読みください。
>別の結合ポリシーを使用するエッジオーディエンスをAdobe Targetの宛先にマッピングした場合、それらのオーディエンスは、リアルタイムおよび次のページのユースケースでは評価されません。
>[&#x200B; 結合ポリシーの作成 &#x200B;](../../../profile/merge-policies/ui-guide.md#create-a-merge-policy) の手順に従い、必ず「**[!UICONTROL Active-On-Edge Merge Policy]**」切り替えスイッチを有効にします。


| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | X | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 単一のプロファイルに対して、Adobe Targetの宛先にマッピングされているすべてのオーディエンスを要求しています。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_datastream"
>title="データストリームについて"
>abstract="このオプションは、オーディエンスを含めるデータ収集データストリームを決定します。ドロップダウンメニューには、ターゲット設定が有効になっているデータストリームのみが表示されます。エッジセグメント化を使用するには、データストリームを選択する必要があります。なにも選択しないと、エッジセグメント化を使用するすべてのユースケースが無効になります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/personalization/adobe-target-connection.html?lang=ja#parameters" text="データストリームの選択についての詳細"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

Adobe Experience Platform は、会社の Adobe Target インスタンスに自動的に接続します。認証は必要ありません。

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_workspace"
>title="Adobe Target Workspaces について"
>abstract="オーディエンスを共有する Adobe Target Workspaces を選択します。Adobe Target 接続ごとに 1 つのワークスペースを選択できます。アクティブ化すると、該当する Experience Platform データ使用ラベルに従って、オーディエンスは選択したワークスペースにルーティングされます。"
>additional-url="https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja" text="Adobe Target Workspaces の詳細情報"

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **名前**：この宛先に希望する名前を入力します。
* **説明**：宛先についての説明を入力します。例えば、この宛先を使用しているキャンペーンを指定できます。このフィールドはオプションです。
* **Datastream**：これにより、オーディエンスを含めるデータ収集データストリームが決定されます。 ドロップダウンメニューには、Target サービスとAdobe Experience Platform サービスが有効になっているデータストリームのみが表示されます。 Adobe Experience PlatformおよびAdobe Targetのデータストリームを設定する方法について詳しくは、[&#x200B; データストリームの設定 &#x200B;](../../../datastreams/configure.md#aep) を参照してください。

  >[!IMPORTANT]
  >
  >**組織全体でのデータストリームの一意性**：データストリーム ID とサンドボックス名の組み合わせは、IMS 組織内のAdobe Target宛先接続で一意である必要があります。つまり、
  >
  >* データストリーム ID とサンドボックス名の同じ組み合わせを、組織全体の複数のAdobe Target宛先接続に使用することはできません
  >* 接続が異なるサンドボックスにある限り、異なる宛先接続に同じデータストリーム ID を使用できます
  >* このルールは、「**[!UICONTROL None]**」を選択した場合を含め、すべてのデータストリーム選択に適用されます

   * **[!UICONTROL None]**:Adobe Experience Platformのパーソナライゼーションを設定する必要があるが、Adobe Target Web SDKを実装できない場合は、このオプションを選択します。 このオプションを使用する場合、Experience Platformから Target に書き出されたオーディエンスは、次のセッションのパーソナライゼーションのみをサポートし、エッジのセグメント化は無効になります。 実装タイプごとに使用可能なユースケースの比較については、[&#x200B; サポートされているユースケース &#x200B;](#supported-use-cases) の節の表を参照してください。

  | Adobe Targetの実装 *Web SDKを使用しない* | Adobe Target実装 *Web SDK* 使用 | Adobe Target実装 *Web SDK* および *エッジセグメンテーション* オフ |
  |---|---|---|
  | <ul><li>データストリームは必須ではありません。 Adobe Targetは、[at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html)、[&#x200B; サーバーサイド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html?lang=ja#server-side-implementation) または [&#x200B; ハイブリッド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html?lang=ja#hybrid-implementation) の実装方法を使用してデプロイできます。</li><li>[Edgeのセグメント化 &#x200B;](../../../segmentation/methods/edge-segmentation.md) はサポートされていません。</li><li>[&#x200B; 同じページと次のページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md) はサポートされていません。</li><li>*デフォルトの実稼動サンドボックス* および非デフォルトのサンドボックス用のAdobe Target接続に、オーディエンスとプロファイル属性を共有できます。</li><li>データストリームを使用せずに次のセッションのパーソナライゼーションを設定するには、[at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html?lang=ja) を使用します。</li></ul> | <ul><li>Adobe TargetとExperience Platformがサービスとして設定されたデータストリームが必要です。</li><li>Edgeのセグメント化が期待どおりに機能します。</li><li>[&#x200B; 同じページと次のページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#use-cases) がサポートされています。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有がサポートされています。</li></ul> | <ul><li>Adobe TargetとExperience Platformがサービスとして設定されたデータストリームが必要です。</li><li>[&#x200B; データストリームの設定 &#x200B;](/help/destinations/ui/activate-edge-personalization-destinations.md#configure-datastream) 時に、「**Edgeのセグメント化**」チェックボックスをオンにしないでください。</li><li>[&#x200B; 次のセッションのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#next-session) がサポートされます。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有がサポートされています。</li></ul> |

* **Workspace**: オーディエンスを共有するAdobe Target [&#x200B; ワークスペース &#x200B;](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja) を選択します。 Adobe Target 接続ごとに 1 つのワークスペースを選択できます。アクティブ化すると、該当する [Experience Platform データ使用ラベル &#x200B;](../../../data-governance/labels/overview.md) に従って、オーディエンスは選択したワークスペースにルーティングされます。

>[!NOTE]
>
>[&#x200B; 属性を使用した同じページと次のページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md) にカスタム Target ワークスペースを使用すると、[&#x200B; 選択したオーディエンス &#x200B;](../../ui/activate-edge-personalization-destinations.md#select-audiences) のみが選択した Target ワークスペースに送信されます。 [&#x200B; マッピングされた属性 &#x200B;](../../ui/activate-edge-personalization-destinations.md#mapping) は、デフォルトの Target ワークスペースに送信されます。
><br>
>この動作は、今後の更新で変更される予定です。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先にオーディエンスをアクティブ化する手順については、[&#x200B; エッジパーソナライゼーションの宛先に対するオーディエンスのアクティブ化 &#x200B;](../../ui/activate-edge-personalization-destinations.md) を参照してください。

## Target の宛先からのオーディエンスの削除 {#remove}

オーディエンスが既にAdobe Target [&#x200B; アクティビティ &#x200B;](https://experienceleague.adobe.com/ja/docs/target/using/activities/activities) で使用されている場合は、そのオーディエンスを既存のAdobe Target接続から削除するために必要な追加の手順があります。 オーディエンスがAdobe Target アクティビティで使用されている場合、Adobe Target接続からオーディエンスを削除しようとすると、エラーが発生します。

![Target アクティビティで使用されるオーディエンスを削除しようとするとエラーが発生するExperience Platform UI 画像 &#x200B;](../../assets/catalog/personalization/adobe-target-connection/remove-audience-error.png)

オーディエンスがアクティビティで使用されているときに Target の宛先からオーディエンスを削除するには、最初に、オーディエンスを使用している Target アクティビティからオーディエンスを削除するか、アクティビティを完全に削除する必要があります。 次に、Target 接続からオーディエンスを削除できます。

オーディエンスがアクティビティで使用されていない場合は、**[!UICONTROL Destinations]** / **[!UICONTROL Browse]** / **[!UICONTROL Select destination dataflow]** / **[!UICONTROL Activation data]** に移動し、削除するオーディエンスを選択してから、「**[!UICONTROL Remove audiences]**」を選択します。

## 書き出したデータ {#exported-data}

Adobe Targetは、Adobe Experience Platform Edge Networkからプロファイルデータを *読み取り* するので、データは書き出されません。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

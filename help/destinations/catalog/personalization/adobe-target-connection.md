---
keywords: ターゲットのパーソナライゼーション;宛先;Experience Platform ターゲットの宛先:Adobe Target の宛先;
title: Adobe Target 接続
description: Adobe Target は、web サイトやモバイルアプリなど、すべてのインバウンド顧客とのインタラクションで、AI を利用したリアルタイムのパーソナライズと実験の機能を提供するアプリケーションです。
exl-id: 3e3c405b-8add-4efb-9389-5ad695bc9799
source-git-commit: 1b1864c3957c080c0f1d9e823e5c816970aed087
workflow-type: tm+mt
source-wordcount: '2330'
ht-degree: 25%

---

# [!DNL Adobe Target] 接続 {#adobe-target-connection}

## 宛先の変更ログ {#changelog}

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2024年4月 | 機能とドキュメントの更新 | ターゲット宛先に接続してデータストリームを使用する場合、エッジセグメント化のためにデータストリームを必ずしも有効にするために&#x200B;*が*&#x200B;必要なくなりました。 つまり、ターゲットの宛先はバッチおよびストリーミングオーディエンスと連携しますが、実現できるユースケースは異なります。 詳細については、[接続パラメーター](#parameters) セクションのテーブルを参照してください。 |
| 2024年1月 | 機能とドキュメントの更新 | デフォルトの実稼動サンドボックスおよびその他のデフォルト以外のサンドボックスの[!DNL Adobe Target]接続にオーディエンスとプロファイル属性を共有できるようになりました。 |
| 2023年6月 | 機能とドキュメントの更新 | 2023年6月現在、新しい[!DNL Adobe Target]宛先接続を設定する際に、オーディエンスを共有する[!DNL Adobe Target] ワークスペースを選択できます。 詳しくは、[接続パラメーター](#parameters)の節を参照してください。 さらに、ワークスペースについて詳しくは、[!DNL Adobe Target]の[&#x200B; ワークスペースの設定](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja)に関するチュートリアルを参照してください。 |
| 2023年5月 | 機能とドキュメントの更新 | 2023年5月の時点で、**[!UICONTROL Adobe Target]**&#x200B;接続は[属性ベースのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#map-attributes)をサポートしており、すべての顧客が一般に利用できます。 |

{style="table-layout:auto"}

## 概要 {#overview}

[!DNL Adobe Target]は、web サイトやモバイルアプリなどのあらゆるインバウンドの顧客インタラクションにおいて、AIを活用したリアルタイムのパーソナライゼーション機能と実験機能を提供するアプリケーションです。

[!DNL Adobe Target]は、[!DNL Adobe Experience Platform]宛先カタログ内のパーソナライゼーション接続です。

## ビデオの概要 {#video-overview}

Experience Platformで[!DNL Adobe Target]接続を設定する方法の概要については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3418799/?quality=12&learn=on)

## 実装タイプに基づいてサポートされるユースケース {#supported-use-cases}

次の表は、Web SDKまたはEdge Network APIの有無、および[&#x200B; エッジセグメント化](/help/segmentation/home.md#edge)の有効化の有無に関わらず、実装タイプに基づいて、Adobe Targetの宛先でサポートされているユースケースを示しています。

| Adobe Targetの実装&#x200B;*なしで* Web SDK | Web SDKまたはEdge Network APIを使用した&#x200B;*Adobe Targetの実装* | Adobe Targetの実装&#x200B;*と* Web SDKまたはEdge Network API *および* エッジのセグメント化オフ |
|---|---|---|
| <ul><li>データストリームは必要ありません。 Adobe Targetは、[at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html?lang=ja)、[&#x200B; サーバーサイド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html#server-side-implementation)、または[&#x200B; ハイブリッド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html#hybrid-implementation)の実装メソッドを通じてデプロイできます。</li><li>[Edge セグメンテーション &#x200B;](../../../segmentation/methods/edge-segmentation.md)はサポートされていません。</li><li>[同一ページおよび次ページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md)はサポートされていません。</li><li>*デフォルトの実稼動サンドボックス*&#x200B;とデフォルト以外のサンドボックスのAdobe Target接続には、オーディエンスとプロファイル属性を共有できます。</li><li>データストリームを使用せずに次セッションのパーソナライゼーションを設定するには、[at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html)を使用します。</li></ul> | <ul><li>Adobe Targetは、[Web SDK](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/home)または[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)を使用して統合できます。</li><li>サービスとして設定されたAdobe TargetとExperience Platformを含むデータストリームが必要です。</li><li>Edgeのセグメンテーションは期待通りに機能します。</li><li>[同一ページと次ページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#use-cases)がサポートされています。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有もサポートされています。</li></ul> | <ul><li>Adobe Targetは、[Web SDK](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/home)または[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)を使用して統合できます。</li><li>サービスとして設定されたAdobe TargetとExperience Platformを含むデータストリームが必要です。</li><li>[&#x200B; データストリームを設定する場合](/help/destinations/ui/activate-edge-personalization-destinations.md#configure-datastream)、「**Edgeのセグメント化**」チェックボックスを選択しないでください。</li><li>[次セッションのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#next-session)はサポートされています。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有もサポートされています。</li></ul> |

## 前提条件 {#prerequisites}

### データストリーム {#datastream}

[!DNL Adobe Target]接続を[使用データストリーム &#x200B;](#parameters)に設定する場合、[Adobe Experience Platform Data Collection](/help/collection/home.md)を実装する必要があります。

データストリームを使用せずに[!DNL Adobe Target]接続を設定する場合、Web SDKを実装する必要はありません。

>[!IMPORTANT]
>
>[!DNL Adobe Target] 接続を作成する前に、[同じページと次のページのパーソナライズのためのパーソナライズ機能宛先の設定](../../ui/activate-edge-personalization-destinations.md)を行う方法を参照してください。 このガイドでは、複数の Experience Platform コンポーネントをまたいで、同じページおよび次のページのパーソナライゼーションの使用例に必要な設定手順を説明します。 同ページと次ページのパーソナライゼーションのユースケースを実現するには、[!DNL Adobe Target]接続の設定時にデータストリームを使用する必要があります。

### [!DNL Adobe Target]の前提条件 {#prerequisites-in-adobe-target}

[!DNL Adobe Target]で、ユーザーが次を持っていることを確認します。

* [&#x200B; デフォルトワークスペース &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html#default-workspace)へのアクセス
* **承認者** [役割](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html#roles-and-permissions)。

[Target Standard](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html#section_8C425E43E5DD4111BBFC734A2B7ABC80)および[Target Premium](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html#roles-permissions)に対する権限の付与について詳しくは、こちらを参照してください。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

>[!IMPORTANT]
>
>同一ページと次ページのパーソナライゼーションのユースケース *に対して* エッジオーディエンスをアクティブ化する場合、オーディエンス *は[&#x200B; アクティブなエッジ結合ポリシー](../../../segmentation/ui/segment-builder.md#merge-policies)を使用する必要があります。*[!DNL active-on-edge]結合ポリシーにより、オーディエンスは常に[&#x200B; エッジ &#x200B;](../../../segmentation/methods/edge-segmentation.md)で評価され、リアルタイムおよび次ページのパーソナライゼーションのユースケースで利用できるようになります。  実装タイプに基づいて、[すべての使用可能なユースケース &#x200B;](#parameter)について説明します。
>異なる結合ポリシーを使用するエッジオーディエンスを[!DNL Adobe Target]宛先にマッピングする場合、これらのオーディエンスはリアルタイムおよび次ページのユースケースで評価されません。
>「[結合ポリシーの作成](../../../profile/merge-policies/ui-guide.md#create-a-merge-policy)」の指示に従い、**[!UICONTROL Active-On-Edge Merge Policy]**&#x200B;切り替えを必ず有効にしてください。


| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!DNL Profile request]** | 1つのプロファイルの[!DNL Adobe Target]宛先にマッピングされているすべてのオーディエンスを要求しています。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。 オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 導入の考慮事項 {#implementation-considerations}

Adobe Targetを導入する際は、次の点に注意してください。

* **同じIMS組織**: Adobe Experience PlatformとAdobe Targetは同じIMS組織に属している必要があります。
* **ファーストログインパーソナライゼーションのID管理**：ファーストログインパーソナライゼーションを実行するには、Real-Time CDP ID グラフの既知の顧客IDと一致するように、パーソナライゼーションリクエストのプライマリ IDが必要です。 プライマリ IDがECIDまたは結合されていないIDの場合、IDの紐付けは、エッジパーソナライゼーションが既知の顧客データを反映するまでに数分かかる場合があります。
* **カスタム ID名前空間の検索**：カスタム ID名前空間の検索には、Web SDKまたはEdge Network APIのデプロイメントが必要で、関連IDが適切にマッピングされている必要があります。
* **Edge プロファイルの有効期間（TTL）**:Edge プロファイルのTTLは14日間です。 非アクティブ状態が14日後にエッジプロファイルは期限切れになり、次回のページ訪問時にハブから取得する必要があります。 これは、有効期限が切れた後の最初の訪問が、リアルタイムデータではなく過去のプロファイルビューを使用する可能性があることを意味します。

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_datastream"
>title="データストリームについて"
>abstract="このオプションは、オーディエンスを含めるデータ収集データストリームを決定します。 ドロップダウンメニューには、ターゲット設定が有効になっているデータストリームのみが表示されます。 エッジセグメント化を使用するには、データストリームを選択する必要があります。 なにも選択しないと、エッジセグメント化を使用するすべてのユースケースが無効になります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/personalization/adobe-target-connection.html?lang=ja#parameters" text="データストリームの選択についての詳細"

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

[!DNL Adobe Experience Platform]は、会社の[!DNL Adobe Target] インスタンスに自動的に接続します。 認証は必要ありません。

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_target_workspace"
>title="約[!DNL Adobe Target] ワークスペース"
>abstract="オーディエンスを共有する[!DNL Adobe Target] ワークスペースを選択します。 [!DNL Adobe Target]接続ごとに1つのワークスペースを選択できます。 アクティブ化すると、該当する Experience Platform データ使用ラベルに従って、オーディエンスは選択したワークスペースにルーティングされます。"
>additional-url="https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja" text=" [!DNL Adobe Target]  ワークスペースについて詳しく見る"

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **名前**：この宛先に希望する名前を入力します。
* **説明**：宛先についての説明を入力します。 例えば、この宛先を使用しているキャンペーンを指定できます。 このフィールドはオプションです。
* **データストリーム**：これにより、オーディエンスを含めるデータ収集データストリームが決まります。 ドロップダウンメニューには、Targetおよび[!DNL Adobe Experience Platform] サービスが有効になっているデータストリームのみが表示されます。 [!DNL Adobe Experience Platform]と[!DNL Adobe Target]のデータストリームを設定する方法について詳しくは、[&#x200B; データストリームの設定](../../../datastreams/configure.md#aep)を参照してください。

  >[!IMPORTANT]
  >
  >**組織全体でのデータストリームの一意性**: IMS組織内の[!DNL Adobe Target]宛先接続では、データストリーム IDとサンドボックス名の組み合わせが一意である必要があります。つまり次の式になります。
  >
  >* データストリーム ID + サンドボックス名の同じ組み合わせを、組織全体の複数の[!DNL Adobe Target]宛先接続に使用することはできません
  >* 接続が異なるサンドボックス上にある限り、異なる宛先接続に同じデータストリーム IDを使用できます
  >* このルールは、**[!UICONTROL None]**&#x200B;を選択した場合を含め、すべてのデータストリーム選択に適用されます

   * **[!UICONTROL None]**: [!DNL Adobe Target] パーソナライゼーションを設定する必要があるものの、[!DNL Adobe Experience Platform] Web SDKを実装できない場合は、このオプションを選択します。 このオプションを使用する場合、Experience PlatformからTargetに書き出されたオーディエンスは、次セッションのパーソナライゼーションのみをサポートし、エッジセグメント化は無効になります。 実装タイプごとに使用可能なユースケースを比較するには、[&#x200B; サポートされているユースケース &#x200B;](#supported-use-cases) セクションのテーブルを参照してください。

  | Adobe Targetの実装&#x200B;*なしで* Web SDK | Web SDKまたはEdge Network APIを使用した&#x200B;*Adobe Targetの実装* | Adobe Targetの実装&#x200B;*と* Web SDKまたはEdge Network API *および* エッジのセグメント化オフ |
  |---|---|---|
  | <ul><li>データストリームは必要ありません。 Adobe Targetは、[at.js](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html?lang=ja)、[&#x200B; サーバーサイド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html#server-side-implementation)、または[&#x200B; ハイブリッド &#x200B;](https://experienceleague.adobe.com/docs/target-dev/developer/overview.html#hybrid-implementation)の実装メソッドを通じてデプロイできます。</li><li>[Edge セグメンテーション &#x200B;](../../../segmentation/methods/edge-segmentation.md)はサポートされていません。</li><li>[同一ページおよび次ページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md)はサポートされていません。</li><li>*デフォルトの実稼動サンドボックス*&#x200B;とデフォルト以外のサンドボックスのAdobe Target接続には、オーディエンスとプロファイル属性を共有できます。</li><li>データストリームを使用せずに次セッションのパーソナライゼーションを設定するには、[at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/at-js/how-atjs-works.html)を使用します。</li></ul> | <ul><li>Adobe Targetは、[Web SDK](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/home)または[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)を使用して統合できます。</li><li>サービスとして設定されたAdobe TargetとExperience Platformを含むデータストリームが必要です。</li><li>Edgeのセグメンテーションは期待通りに機能します。</li><li>[同一ページと次ページのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#use-cases)がサポートされています。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有もサポートされています。</li></ul> | <ul><li>Adobe Targetは、[Web SDK](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/home)または[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/)を使用して統合できます。</li><li>サービスとして設定されたAdobe TargetとExperience Platformを含むデータストリームが必要です。</li><li>[&#x200B; データストリームを設定する場合](/help/destinations/ui/activate-edge-personalization-destinations.md#configure-datastream)、「**Edgeのセグメント化**」チェックボックスを選択しないでください。</li><li>[次セッションのパーソナライゼーション &#x200B;](../../ui/activate-edge-personalization-destinations.md#next-session)はサポートされています。</li><li>他のサンドボックスからのオーディエンスとプロファイル属性の共有もサポートされています。</li></ul> |

* **Workspace**: オーディエンスを共有する[!DNL Adobe Target] [&#x200B; ワークスペース &#x200B;](https://experienceleague.adobe.com/docs/target-learn/tutorials/administration/set-up-workspaces.html?lang=ja)を選択します。 [!DNL Adobe Target]接続ごとに1つのワークスペースを選択できます。 アクティブ化すると、該当する[Experience Platform データ使用ラベル &#x200B;](../../../data-governance/labels/overview.md)に従いながら、オーディエンスが選択したワークスペースにルーティングされます。

>[!NOTE]
>
>[同じページと次のページのパーソナライゼーションにカスタム Target ワークスペースを属性](../../ui/activate-edge-personalization-destinations.md)と共に使用する場合、選択した[個のオーディエンス &#x200B;](../../ui/activate-edge-personalization-destinations.md#select-audiences)のみが選択したTarget ワークスペースに送信されます。 マッピングされた[属性](../../ui/activate-edge-personalization-destinations.md#mapping)は、デフォルトのTarget ワークスペースに送信されます。
><br>>この動作は、今後のアップデートで変更されます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。 アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に対してオーディエンスをアクティブ化する手順については、[&#x200B; エッジパーソナライゼーション宛先に対してオーディエンスをアクティブ化](../../ui/activate-edge-personalization-destinations.md)を参照してください。

## ターゲット宛先からのオーディエンスの削除 {#remove}

[!DNL Adobe Target] [&#x200B; アクティビティ &#x200B;](https://experienceleague.adobe.com/en/docs/target/using/activities/activities)でオーディエンスが既に使用されている場合、既存の[!DNL Adobe Target]接続からオーディエンスを削除するには、追加の手順が必要です。 [!DNL Adobe Target]接続からオーディエンスを削除しようとすると、[!DNL Adobe Target] アクティビティでオーディエンスが使用されている場合にエラーが発生します。

![Target アクティビティで使用されているオーディエンスを削除しようとしたときにエラーが発生したExperience Platform UI画像](../../assets/catalog/personalization/adobe-target-connection/remove-audience-error.png)

オーディエンスがアクティビティで使用されている場合にターゲット宛先からオーディエンスを削除するには、まずオーディエンスを使用しているTarget アクティビティからオーディエンスを削除するか、アクティビティ全体を削除する必要があります。 次に、Target接続からオーディエンスを削除します。

オーディエンスがアクティビティで使用されていない場合は、**[!UICONTROL Destinations]** > **[!UICONTROL Browse]** > **[!UICONTROL Select destination dataflow]** > **[!UICONTROL Activation data]**&#x200B;に移動し、削除するオーディエンスを選択してから、**[!UICONTROL Remove audiences]**&#x200B;を選択します。

## 書き出したデータ {#exported-data}

[!DNL Adobe Target] *は[!DNL Adobe Experience Platform] Edge Networkから* プロファイルデータを読み取るので、データは書き出されません。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

---
title: 未認証の訪問者のオフサイトリターゲティング
description: 見込み客 ID を使用して、未認証ユーザーのオーディエンスの作成に使用できる計算属性を作成することで、未認証ユーザーをリターゲットする方法を説明します。
feature: Use Cases, Customer Acquisition
exl-id: cffa3873-d713-445a-a3e1-1edf1aa8eebb
source-git-commit: 5b37b51308dc2097c05b0e763293467eb12a2f21
workflow-type: tm+mt
source-wordcount: '1462'
ht-degree: 1%

---

# 未認証の訪問者のオフサイトリターゲティング

>[!AVAILABILITY]
>
>この機能は、Real-Time CDP（アプリサービス）、Adobe Experience Platform Activation、Real-Time CDP、Real-Time CDP Prime、Real-Time CDP Ultimate のライセンスを取得したお客様が利用できます。 これらのパッケージについて詳しくは、[製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)を参照し、アドビ担当者にお問い合わせください。

認証されていない訪問者のオーディエンスを作成し、パートナーが提供した永続的な ID を使用して再ターゲットにする方法を説明します。

![Adobe Experience Platformへの取り込みからダウンストリーム宛先へのオーディエンス経由で出力へのパートナーデータのフローを示すインフォグラフィック。](../assets/offsite-retargeting/header.png)

## このユースケースを検討する理由 {#why-use-case}

サードパーティ cookie が徐々に廃止されていく中で、デジタルマーケターは匿名訪問者との再エンゲージメントに関する戦略を再考する必要があります。 リアルタイムの訪問者認識のために ID ベンダーとの統合を選択したブランドは、オフサイトの有料メディアリターゲティング用にパートナーが提供した永続的な識別子も活用できます。

大量のトラフィックがあるにもかかわらず、多くのブランドでは、コンバージョン段階で大幅な下降が見られます。 訪問者は、コンテンツや製品のデモに関与しますが、新規登録や購入はせずに離脱します。

オンサイトエンゲージメントに基づいてオーディエンスを作成してマーケティングメッセージをパーソナライズできるだけでなく、Adobeのパートナー ID のサポートを使用して、有料メディア配信の宛先で訪問者と再びエンゲージメントを行うこともできます。

## 前提条件と計画 {#prerequisites-and-planning}

未認証の訪問者のリターゲティングを計画する場合は、計画プロセス中に次の前提条件を考慮してください。

- 適切な ID 名前空間を持つパートナー ID を設定していますか。

さらに、このユースケースを実装するために、次のReal-Time CDP機能と UI 要素を利用します。 これらすべての領域に対して必要な属性ベースのアクセス制御権限があることを確認するか、システム管理者に必要な権限の付与を依頼します。

- [オーディエンス](../../segmentation/home.md)
- [計算属性](../../profile/computed-attributes/overview.md)
- [宛先](../../destinations/home.md)
- [Web SDK](../../web-sdk/home.md)

## パートナーデータをReal-Time CDPに取り込む {#get-data-in}

認証されていない訪問者のオーディエンスを作成するには、まずパートナーデータをReal-Time CDPに取り込む必要があります。

Web SDK を使用してReal-Time CDPにデータを最適に読み込む方法を学ぶには、オンサイトパーソナライゼーションのユースケースの [&#x200B; データ管理とイベントデータ収集の節 &#x200B;](./onsite-personalization.md#data-management) を参照してください。

## パートナー提供 ID を転送しています {#bring-partner-ids-forward}

パートナーが提供した ID をイベントデータセットに読み込んだ後、このデータをプロファイルレコードに取り込む必要があります。 これを行うには、計算済み属性を利用します。

計算属性を使用すると、プロファイルの行動データをプロファイルレベルでの集計値にすばやく変換できます。 その結果、「ライフタイム購入合計」などの式をプロファイルに使用して、オーディエンス内で計算属性を簡単に使用できます。 計算済み属性について詳しくは、[&#x200B; 計算属性の概要 &#x200B;](../../profile/computed-attributes/overview.md) を参照してください。

計算属性にアクセスするには、「**[!UICONTROL プロファイル]**」、「**[!UICONTROL 計算属性]**」および「**[!UICONTROL 計算属性の作成]** の順に選択します。

![&#x200B; 「[!UICONTROL &#x200B; プロファイル &#x200B;] ワークスペース内の「[!UICONTROL &#x200B; 計算済み属性 &#x200B;] タブに加えて、「[!UICONTROL &#x200B; 計算済み属性を作成 &#x200B;]」ボタンがハイライト表示されます。](../assets/offsite-retargeting/create-ca.png)

**[!UICONTROL 計算属性を作成]** ページが表示されます。 このページでは、コンポーネントを使用して計算属性を作成できます。

![&#x200B; 計算属性を作成ワークスペースが表示されます。](../assets/offsite-retargeting/ca-page.png)

>[!NOTE]
>
>計算属性の作成について詳しくは、[&#x200B; 計算属性 UI ガイド &#x200B;](../../profile/computed-attributes/ui.md) を参照してください。

このユースケースでは、パートナー ID が存在する場合に、過去 24 時間以内にパートナー ID の最新の値を取得する計算属性を作成できます。

検索バーを使用して、（オンサイトのパーソナライゼーションのユースケースで作成した [&#x200B; 「パートナー ID」イベントを見つけて &#x200B;](#get-data-in) 計算属性キャンバスに追加できます。

![&#x200B; 「[!UICONTROL &#x200B; イベント &#x200B;] タブと検索バーがハイライト表示されています。](../assets/offsite-retargeting/ca-add-partner-id.png)

「パートナー ID」イベントを定義に追加した後、イベントフィルタリング条件を **[!UICONTROL 存在]** に設定し、追加したパートナー ID の **[!UICONTROL 最新]** 値に設定し、ルックバック期間を 24 時間にします。

![&#x200B; 作成する計算属性の定義がハイライト表示されている様子 &#x200B;](../assets/offsite-retargeting/ca-add-definition.png)

計算属性に適切な名前（「パートナー ID」など）と説明を付け、「**[!UICONTROL Publish]**」を選択して計算属性の作成プロセスを完了します。

![&#x200B; 作成する計算属性の基本情報がハイライト表示されている様子 &#x200B;](../assets/offsite-retargeting/ca-publish.png)

## 計算属性を使用したオーディエンスの作成 {#create-audience}

計算属性を作成したので、この計算属性を使用してオーディエンスを作成できます。 この例では、今月に 5 回以上 web サイトを訪問したが、まだサインアップしていない訪問者で構成されるオーディエンスを作成します。

オーディエンスを作成するには、**[!UICONTROL オーディエンス]**、**[!UICONTROL オーディエンスを作成]** の順に選択します。

![&#x200B; 「[!UICONTROL &#x200B; オーディエンスを作成 &#x200B;] ボタンがハイライト表示されます。](../assets/offsite-retargeting/create-audience.png)

[!UICONTROL &#x200B; オーディエンスを作成 &#x200B;] と [!UICONTROL &#x200B; ルールを作成 &#x200B;] のどちらかを選択するように求めるダイアログが表示されます。 **[!UICONTROL ルールを作成]**/**[!UICONTROL 作成]** を選択します。

![&#x200B; 「[!UICONTROL &#x200B; ルールを作成 &#x200B;]」ボタンがハイライト表示されます。](../assets/offsite-retargeting/select-build-rule.png)

セグメントビルダーページが表示されます。 このページでは、コンポーネントを使用してオーディエンスを作成できます。

![&#x200B; セグメントビルダーが表示されます。](../assets/offsite-retargeting/segment-builder.png)

>[!NOTE]
>
>セグメントビルダーの使用について詳しくは、『 [&#x200B; セグメントビルダー UI ガイド &#x200B;](../../segmentation/ui/segment-builder.md) 』を参照してください。

これらの訪問者を見つけるには、まずオーディエンスに **[!UICONTROL ページビュー]** イベントを追加する必要があります。 **[!UICONTROL フィールド]** の下の「**[!UICONTROL イベント]**」タブを選択し、**[!UICONTROL ページビュー]** イベントをドラッグ&amp;ドロップして、「イベント」セクションキャンバスに追加します。

![&#x200B; 「[!UICONTROL &#x200B; フィールド &#x200B;]」セクションの「[!UICONTROL &#x200B; イベント &#x200B;]」タブがハイライト表示され、[!UICONTROL &#x200B; ページビュー &#x200B;] イベント &#x200B;](../assets/offsite-retargeting/add-page-view.png) が表示されます。

新しく追加された **[!UICONTROL ページビュー]** イベントを選択します。 ルックバック期間を **[!UICONTROL いつでも]** から **[!UICONTROL 今月]** に変更し、イベントルールを **5 以上** を含むように変更します。

![&#x200B; 追加された [!UICONTROL &#x200B; ページビュー &#x200B;] イベントの詳細が表示されます。](../assets/offsite-retargeting/edit-event.png)

イベントを追加したら、属性を追加する必要があります。 認証されていない訪問者を扱っているので、作成した計算属性を追加できます。 この新しく作成された計算属性を使用すると、パートナー ID をオーディエンスにリンクできます。

計算属性を追加するには、「**[!UICONTROL 属性]**」で **[!UICONTROL XDM 個人プロファイル]**、「組織のテナント ID **[の順に選択し &#x200B;](../../xdm/api/getting-started.md#know-your-tenant-id) す。**、**[!UICONTROL SystemComputedAttributes]**、および **[!UICONTROL PartnerID]**。 次に、計算属性の **[!UICONTROL Value]** をキャンバスの attributes セクションに追加します。

![&#x200B; 計算属性にアクセスするためのフォルダーパスが表示されます。](../assets/offsite-retargeting/access-computed-attribute.png)

さらに、「**[!UICONTROL 個人のメール]**」を検索し、「**[!UICONTROL PartnerID]**」の下の「**[!UICONTROL アドレス]**」属性をキャンバスの「属性」セクションに追加します。

![&#x200B; セグメントビルダーキャンバスで [!UICONTROL PartnerID] 計算属性と [!UICONTROL Personal Email Address] 属性がハイライト表示されます。](../assets/offsite-retargeting/added-attributes.png)

属性を追加したら、評価条件を設定する必要があります。 **[!UICONTROL PartnerID]** の場合は条件を **[!UICONTROL 存在する]** に設定し、**[!UICONTROL Address]** の場合は条件を **[!UICONTROL 存在しない]** に設定します。

![&#x200B; 属性の適切な値がハイライト表示されます。](../assets/offsite-retargeting/set-attribute-values.png)

これで、パートナーが提供した ID を持っているが、まだサイトにサインアップしていない高密度訪問者を検索するオーディエンスを正常に作成しました。 オーディエンスに「未認証ユーザーのリターゲティング」という名前を付け、「保存 **を選択して** オーディエンスの作成を完了します。

![&#x200B; オーディエンスプロパティがハイライト表示されている様子 &#x200B;](../assets/offsite-retargeting/save-audience-properties.png)

## オーディエンスのアクティベート {#activate-audience}

オーディエンスを正常に作成したら、このオーディエンスをダウンストリームの宛先に対してアクティブ化できます。 左側のナビゲーションパネルで **[!UICONTROL オーディエンス]** を選択し、新しく作成したオーディエンスを探して、省略記号アイコンを選択して **[!UICONTROL 宛先に対してアクティブ化]** を選択します。

![&#x200B; 「[!UICONTROL &#x200B; 宛先に対してアクティブ化 &#x200B;] ボタンがハイライト表示されます。](../assets/offsite-retargeting/activate-to-destination.png)

>[!NOTE]
>
>ファイルベースの宛先を含むすべての宛先タイプでは、パートナー ID を使用したオーディエンスのアクティベーションをサポートしています。
>
>宛先へのオーディエンスのアクティブ化について詳しくは、[&#x200B; アクティベーションの概要 &#x200B;](../../destinations/ui/activation-overview.md) を参照してください。

**[!UICONTROL 宛先のアクティブ化]** ページが表示されます。 このページでは、宛先をアクティベートする宛先を選択できます。 宛先を選択したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; オーディエンスをアクティベートする宛先がハイライト表示されている様子。](../assets/offsite-retargeting/select-destination.png)

**[!UICONTROL スケジュール設定]** ページが表示されます。 このページでは、オーディエンスをアクティブ化する頻度を決定するスケジュールを作成できます。 「**[!UICONTROL スケジュールを作成]**」を選択して、オーディエンスアクティベーションのスケジュールを作成します。

![&#x200B; 「[!UICONTROL &#x200B; スケジュールを作成 &#x200B;] ボタンがハイライト表示されます。](../assets/offsite-retargeting/select-create-schedule.png)

[!UICONTROL &#x200B; スケジュール設定 &#x200B;] ポップオーバーが表示されます。 このページでは、オーディエンスのアクティベーションのスケジュールを作成できます。 スケジュールを設定したら、「**[!UICONTROL 作成]**」を選択して続行します。

![&#x200B; スケジュールを設定ポップオーバーが表示されます。](../assets/offsite-retargeting/configure-schedule.png)

スケジュールの詳細を確認したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; スケジュールの詳細が表示されます。](../assets/offsite-retargeting/created-schedule.png)

**[!UICONTROL 属性を選択]** ページが表示されます。 このページでは、アクティブ化されたオーディエンスと共に書き出す属性を選択できます。 少なくとも、パートナー ID を含める必要があります。これにより、再ターゲットを計画している訪問者を識別できるからです。 **[!UICONTROL 新しいマッピングを追加]** を選択して、計算属性を検索します。 必要な属性を追加したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; 「[!UICONTROL &#x200B; 新しいマッピングを追加 &#x200B;]」ボタンと計算属性の両方がハイライト表示されます。](../assets/offsite-retargeting/add-new-mapping.png)

**[!UICONTROL レビュー]** ページが表示されます。 このページでは、Audience Activation の詳細を確認できます。 指定した詳細が正しければ、「**[!UICONTROL 終了]**」を選択します。

![&#x200B; オーディエンスアクティベーションの詳細を示す [!UICONTROL &#x200B; レビュー &#x200B;] ページが表示されます。](../assets/offsite-retargeting/review-destination-activation.png)

これで、さらなるリターゲティングのために、認証されていないユーザーのオーディエンスをダウンストリーム宛先に対してアクティブ化しました。

## その他のユースケース {#other-use-cases}

Real-Time CDPのパートナーデータサポートを通じて有効になるユースケースについて確認できます。

- パートナーデータを使用して [&#x200B; 新規顧客を獲得および獲得 &#x200B;](./prospecting.md) します。
- パートナー支援による訪問者認識を使用して [&#128279;](./offsite-retargeting.md) オンサイトエクスペリエンスをパーソナライズ  します。
- パートナー提供の属性を使用して [&#128279;](./supplement-first-party-profiles.md) ファーストパーティプロファイルを補完  ます。

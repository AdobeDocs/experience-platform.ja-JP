---
keywords: 宛先；質問；よくある質問；faq；宛先 faq
title: よくある質問
description: Adobe Experience Platformの宛先に関するよくある質問への回答
exl-id: 2c34ecd0-a6d0-48dd-86b0-a144a6acf61a
source-git-commit: 7f3459f678c74ead1d733304702309522dd0018b
workflow-type: tm+mt
source-wordcount: '1673'
ht-degree: 1%

---

# よくある質問 {#faq}

## 概要 {#overview}

このドキュメントでは、Adobe Experience Platformの宛先に関するよくある質問に対する回答を示します。 すべての [!DNL Experience Platform] API で発生する問題を含め、他の [!DNL Experience Platform] サービスに関する質問とトラブルシューティングについては、[Experience Platform トラブルシューティングガイド &#x200B;](../landing/troubleshooting.md) を参照してください。

## 宛先に関する一般的な質問 {#general}

### Experience Platform UI と書き出された CSV ファイルで、異なるプロファイル数が表示されるのはなぜですか？

+++回答
Experience Platformのセグメント化の実行方法による通常の動作です。

ストリーミングセグメント化では、ストリーミングオーディエンスのプロファイル数が 1 日を通じて更新されるのに対して、バッチセグメント化では、バッチオーディエンスのプロファイル数が 24 時間ごとに 1 回更新されます。

オーディエンスの書き出しスケジュールがセグメント化スケジュールと異なる場合、特にストリーミングオーディエンスに関しては、UI と書き出された [!DNL CSV] ファイルの間のプロファイル数が異なります。

詳しくは、[&#x200B; セグメント化サービスのドキュメント &#x200B;](../segmentation/home.md) を参照してください。
+++

### 更新したオーディエンスを同じ宛先に対して非アクティブ化して再アクティブ化すると、マッチ率が低くなるのはなぜですか？

+++回答

ストリーミング宛先からのオーディエンスの非アクティブ化と非アクティブ化では、オーディエンスが同じストリーミング宛先に再アクティブ化されるときにバックフィルがトリガーされません。

**例**

ストリーミング宛先に対して 10 個のプロファイルで構成されるオーディエンスをアクティブ化しました。

オーディエンスをアクティブ化した後に、オーディエンスの設定を変更する必要があることに気付き、オーディエンスを非アクティブ化して母集団条件を変更し、オーディエンス母集団が 100 個のプロファイルになります。

同じ宛先に対して更新されたオーディエンスを再アクティブ化しますが、バックフィルがトリガーされないので、宛先は追加の 90 個のプロファイルを受け取りません。

**ソリューション**

すべてのプロファイルが確実に宛先に送信されるようにするには、新しい設定で新しいオーディエンスを作成し、宛先に対してアクティブ化する必要があります。

+++

### オーディエンスが宛先から削除された場合、オーディエンスが削除されたことを示すシグナルが宛先に送信されることはありますか？

+++回答

いいえ。Experience Platformのインストール先と対象システムのお客様のインスタンスとの間に依存関係はありません。 受信側では、ターゲットシステムに表示される唯一の指標は、オーディエンスデータの受信を停止したことです。

+++

<!--
## [!DNL Experience Cloud Audiences] {#eca-faq}

### What are the differences between the Experience Cloud Audiences and Adobe Target destinations?

+++Answer

See the table below for a feature comparison between the Experience Cloud Audiences and Adobe Target destinations.

||Experience Cloud Audiences|Adobe Target|
|---|---|---|
| **Supported Experience Cloud apps** | Supports audience activation to Audience Manager, Adobe Target, Adobe Analytics, Advertising Cloud, Marketo, Adobe Campaign | Supports audience activation only to Adobe Target |
| **Supports audience activation** | ✓ | ✓ |
| **Supports attribute activation** | X | ✓ |
| **Latency** | Profiles begin activating in 6 hours. Full population is visible in 48 hours​. |Depends on implementation​ type. <ul><li>Web SDK enables same-page/next-page​ personalization.</li><li>AT.js enables next-session personalization.</li></ul> |
| **DULE support** | ✓ | ✓ |
| **Marketing actions support** | ✓ | ✓ |
| **Supported IDs** | [!DNL ECID], [!DNL GAID], [!DNL IDFA], [!DNL email_lc_sha256] | Any ID type |
| **Sandbox support** | One sandbox | Multiple sandboxes |
| **Consent support** | X | Yes. Requires Privacy & Security Shield. |
| **Edge segmentation support** | Supports activation of edge audiences. Does not support edge segmentation. | Supports edge segmentation and activation of edge audiences. |
| **Supported audiences** | All types of audiences  | Edge merge policy required for activation.|

+++

-->

## [!DNL Facebook Custom Audiences] {#facebook-faq}

### [!DNL Facebook Custom Audiences] でオーディエンスをアクティブ化するには、どうすればよいですか？

+++回答
オーディエンスを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* 使用する広告アカウントに対して、[!DNL Facebook] ユーザーアカウントで **[!DNL Manage campaigns]** 権限が有効になっている必要があります。
* **Adobe Experience Cloud** ビジネス アカウントは、広告パートナーとして [!DNL Facebook Ad Account] に追加される必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebook ドキュメントの [&#x200B; ビジネスマネージャーへのパートナーの追加 &#x200B;](https://www.facebook.com/business/help/1717412048538897) を参照してください。

  >[!IMPORTANT]
  >
  > Adobe Experience Cloudの権限を設定する場合は、**キャンペーンの管理** 権限を有効にする必要があります。 これは [!DNL Adobe Experience Platform] 統合に必要です。
* [!DNL Facebook Custom Audiences] のサービス利用規約を読み、署名します。 これを行うには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に移動します。ここで、`accountID` はあなたの [!DNL Facebook Ad Account ID] です。
+++

### [!DNL Facebook] 広告主アカウントにアプリやピクセルを追加する必要がありますか？

+++回答
いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。
+++

### Facebook がAdobe Experience Platformからの情報を処理するのにどのくらい時間がかかりますか？

+++回答
2021 年 3 月の時点で、[!DNL Facebook Custom Audiences] は [!DNL Experience Platform] から受信した情報を処理するために最大 1 時間が必要です。
+++

### [!DNL Instagram] など、他の [!DNL Facebook] アプリでオーディエンスのターゲティングに [!DNL Facebook Custom Audiences] を使用できますか？

+++アムスワー
[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]、[!DNL Messenger] など、[!DNL Facebook Custom Audiences] でサポートされている Facebook のアプリファミリー全体で、オーディエンスターゲット設定に [!DNL Facebook Custom Audiences] の宛先を使用できます。 キャンペーンを実行するアプリの選択は、[!DNL Facebook Ads Manager] のプレースメントレベルに示されます。
+++

### [!DNL Facebook Custom Audiences] 接続と [!DNL Facebook Pixel] 拡張機能の違いは何ですか？

+++回答
[!DNL Facebook Custom Audiences] 接続では、オーディエンスを [!DNL Facebook] に送信する際に [!DNL Experience Platform] ID を使用するのに対して、[[!DNL Facebook Pixel]  接続 &#x200B;](../destinations/catalog/advertising/facebook-pixel.md) では、web サイトに統合された [!DNL Facebook] ピクセルを使用します。

これらの 2 つの統合は補完的です。どちらかを使用して、オーディエンスの範囲を広げることができます。 例えば、[!DNL Facebook Pixel] 拡張機能を使用して、アカウントを作成していない web サイトの訪問者を予測できます。一方、ID に基づい [!DNL Facebook Custom Audiences] 既存の顧客をターゲティングする [!DNL Experience Platform] ともできます。
+++

### Adobe Experience Platformと [!DNL Facebook Custom Audiences] の統合では、オーディエンスの対象から外されたユーザーを、そのオーディエンスに適合しなくなったときにサポートしま？**。

+++回答
はい。統合では、資格がなくなったときにユーザーを [!DNL Facebook Custom Audiences] から削除することをサポートしています。
+++

### オーディエンスデータを [!DNL Facebook] に送信する前に、どのようにハッシュ化する必要がありますか？

+++回答
[!DNL Facebook] では、個人を特定できる情報（PII）が明確に送信されないことが必要です。 したがって、[!DNL Facebook] に対してアクティブ化されたオーディエンスは、メールアドレスや電話番号などの識別子を *ハッシュ化* してオフにすることができます。

ID 一致要件について詳しくは、[ID 一致要件 &#x200B;](catalog/social/facebook.md#id-matching-requirements) を参照してください。
+++

### [!DNL Facebook Custom Audiences] では、どのような種類の ID をアクティブ化できますか？

+++回答
[!DNL Facebook Custom Audiences] では、ハッシュ化されたメール、ハッシュ化された電話番号、[!DNL GAID]、[!DNL IDFA] およびカスタム外部 ID の ID のアクティベーションをサポートしています。
+++

### Experience Platform UI で、個別の Facebook アカウントに対して複数の Facebook 宛先を作成できますか？

+++回答
はい。 Experience Platformの Facebook の宛先は、Facebook の広告アカウントに対して 1 対 1 です。 会社内の Facebook 広告アカウントごとに個別の Facebook 宛先を作成できます。 [&#x200B; 宛先接続チュートリアル &#x200B;](/help/destinations/ui/connect-destination.md) に従い、Experience Platform UI で新しい Facebook 宛先ごとに個別の Facebook アカウントに接続します。 接続できる Facebook 広告アカウントの数に制限はありません。
+++

## Google カスタマーマッチ {#google-customer-match}

### オーディエンスをGoogle Customer Match に書き出すと、Google インターフェイスのオーディエンス名の末尾に追加の数字が追加されるのはなぜですか。

+++回答
Googleでは、オーディエンス名が一意である必要があります。 表示される数値は [UNIX タイムスタンプ &#x200B;](https://www.unixtimestamp.com/) で、同じオーディエンスを複数のGoogleの宛先にマッピングした場合に、オーディエンス名を一意に保つために追加されます。
+++

## LinkedIn でマッチしたオーディエンス {#linkedin}

### [!DNL LinkedIn] 広告主アカウントにアプリやピクセルを追加する必要がありますか？

+++回答
いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。
+++

### [!DNL LinkedIn Matched Audiences] でオーディエンスをアクティブ化するには、どうすればよいですか？

+++回答
[!UICONTROL LinkedIn でマッチしたオーディエンス &#x200B;] の宛先を使用する前に、[!DNL LinkedIn Campaign Manager] アカウントの権限レベルが [!DNL Creative Manager] 以上であることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、LinkedIn ドキュメントの [Advertising アカウントのユーザー権限の追加、編集、削除 &#x200B;](https://www.linkedin.com/help/lms/answer/5753) を参照してください。
+++

### オーディエンスデータを [!DNL LinkedIn] に送信する前に、どのようにハッシュ化する必要がありますか？

+++回答
[!DNL LinkedIn] では、個人を特定できる情報（PII）が明確に送信されないことが必要です。 したがって、[!DNL LinkedIn] に対してアクティブ化されたオーディエンスは、メールアドレスや電話番号などの識別子を *ハッシュ化* してオフにすることができます。

ID 一致要件について詳しくは、[ID 一致要件 &#x200B;](catalog/social/linkedin.md#id-matching-requirements) を参照してください。
+++

### [!DNL LinkedIn] では、どのような種類の ID をアクティブ化できますか？

+++回答
[!DNL LinkedIn Matched Audiences] では、ハッシュ化されたメール、[!DNL GAID] および [!DNL IDFA] の id のアクティベーションをサポートしています。

+++

## Adobe Targetとカスタム Personalizationの宛先を使用した、同じページおよび次のページのパーソナライゼーション {#same-next-page-personalization}

### オーディエンスや属性をAdobe Targetに送信するには、Experience Platform Web SDKを使用する必要がありますか？

+++回答
いいえ。[Adobe Target](../web-sdk/home.md) に対してオーディエンスをアクティブ化するために、[Web SDK](catalog/personalization/adobe-target-connection.md) は必要ありません。

ただし、Web SDKの代わりに [[!DNL at.js]](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html?lang=ja) を使用する場合は、次のセッションのパーソナライゼーションのみがサポートされます。

[&#x200B; 同じページと次のページのパーソナライゼーション &#x200B;](ui/activate-edge-personalization-destinations.md) のユースケースについては、[Web SDK](../web-sdk/home.md) または [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/) を使用する必要があります。 実装について詳しくは、[edge 宛先へのオーディエンスのアクティブ化 &#x200B;](ui/activate-edge-personalization-destinations.md) に関するドキュメントを参照してください。
+++

### Real-time Customer Data Platform からAdobe Targetまたはカスタム Personalizationの宛先に送信できる属性の数に制限はありますか。

+++回答
はい。同じページと次のページのパーソナライゼーションのユースケースでは、Adobe Targetまたはカスタム Personalizationの宛先に対してオーディエンスをアクティブ化する際に、サンドボックスごとに最大 30 個の属性をサポートします。 アクティベーションガードレールについて詳しくは、[&#x200B; ガードレールのドキュメント &#x200B;](guardrails.md#edge-destinations-activation) を参照してください。
+++

### アクティブ化でサポートされている属性のタイプ （配列、マップなど）。

+++回答
現在、`person.name.firstName` など、静的な単一値の属性のみがサポートされています。 配列属性は現在サポートされていません。
+++

<!-- **Is there a limit on the number of audiences that can be activated to Adobe Target and Custom Personalization destinations?**

Yes, you can activate a maximum of 150 edge audiences per sandbox.  For more information on activation guardrails, see the [default guardrails for activation](guardrails.md#edge-destinations-activation). -->

### Experience Platformでオーディエンスを作成した後、そのオーディエンスがエッジセグメント化のユースケースで使用できるようになるまでに、どのくらい時間がかかりますか？

+++回答
オーディエンス定義は、最大 1 時間で [&#128279;](../web-sdk/home.md)0&rbrace;Edge Network&rbrace; に生成されます。 ただし、この最初の 1 時間以内にオーディエンスがアクティブ化されると、オーディエンスに適合した一部の訪問者を見逃す可能性があります。
+++

### Adobe Targetでアクティブ化された属性はどこで確認できますか？

+++回答
属性は、{JSON[&#128279;](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html?lang=ja) および [2}HTML](https://experienceleague.adobe.com/docs/target/using/experiences/offers/manage-content.html?lang=ja) オファーの Target で使用できるようになります。
+++

### データストリームのない宛先を作成し、後で同じ宛先にデータストリームを追加できますか？

+++回答
これは、現在、宛先 UI ではサポートされていません。 この場合にサポートが必要な場合は、Adobe担当者にお問い合わせください。
+++

### Adobe Targetの宛先を削除するとどうなりますか？

+++回答
宛先を削除すると、その宛先でマッピングされているすべてのオーディエンスおよび属性がAdobe Targetから削除され、Edge Networkからも削除されます。
+++

### 統合はEdge Network API を使用して機能しますか？

+++回答
はい、Edge Network API はカスタム Personalizationの宛先と連携します。 プロファイル属性には機密データが含まれている可能性があるので、このデータを保護するために、カスタム Personalizationの宛先では、データ収集にEdge Network API を使用する必要があります。 さらに、すべての API 呼び出しは、[&#x200B; 認証済みコンテキスト &#x200B;](https://developer.adobe.com/data-collection-apis/docs/getting-started/authentication/) で行う必要があります。
+++

### エッジでアクティブになっている結合ポリシーは 1 つだけです。 別の結合ポリシーを使用するオーディエンスを作成し、それをストリーミングオーディエンスとしてAdobe Targetに送信できますか。

+++回答
いいえ。Adobe Targetに対してアクティブ化するすべてのオーディエンスは、アクティブオンエッジ [&#x200B; 結合ポリシー &#x200B;](../profile/merge-policies/ui-guide.md) を使用する必要があります。
+++

### データ使用のラベル付けと実施（DULE）および同意ポリシーは適用されますか。

+++回答
はい。 選択したマーケティングアクションに作成および関連付けられた [&#x200B; データガバナンスポリシーと同意ポリシー &#x200B;](../data-governance/home.md) は、選択した属性のアクティブ化を制御します。
+++

### [!DNL Adobe Target] と [!DNL Custom Personalization] の宛先は [!DNL HIPAA] に準拠していますか。

+++回答
[!DNL Adobe Target] は [[!DNL Adobe Healthcare Shield]](https://business.adobe.com/solutions/industries/healthcare.html) に [!DNL HIPPA] 準拠していません。 お客様は、[!DNL Adobe Target] または [!DNL Custom Personalization] の宛先を介してエッジパーソナライゼーションを使用する前に、カスタム最適化チャネルの [!DNL HIPPA] 対応に関して、自社の法務チームに確認する必要があります。

同意ポリシー管理を大規模に適用する必要があるユースケースでは、お客様は [!DNL Adobe Privacy & Security Shield] を購入する必要があります。 [!DNL Adobe Privacy & Security Shield] 機能は高度な機能スイートとして販売されており、別途購入することはできません。

このサービスには、顧客データのライフサイクルを管理するための顧客管理キーとしきい値の引き上げが含まれています。

[!DNL Adobe Target] と [!DNL Custom Personalization] の宛先は、[Experience Platform データ使用ラベル &#x200B;](../data-governance/labels/overview.md) および [&#x200B; 同意ポリシー適用サービス &#x200B;](../data-governance/enforcement/overview.md) と統合されています。 これらの機能は、すべてのお客様が利用できます。




+++


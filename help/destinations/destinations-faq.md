---
keywords: 宛先；質問；よくある質問；faq；宛先faq
title: よくある質問
description: Adobe Experience Platformの配信先に関するよくある質問への回答
exl-id: 2c34ecd0-a6d0-48dd-86b0-a144a6acf61a
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '1632'
ht-degree: 3%

---

# よくある質問 {#faq}

## 概要 {#overview}

このドキュメントでは、[!DNL Adobe Experience Platform]宛先に関するよくある質問に対する回答を提供します。 すべての[!DNL Experience Platform] APIで発生したものなど、他の[!DNL Experience Platform] サービスに関連する質問とトラブルシューティングについては、[Experience Platform トラブルシューティング ガイド &#x200B;](../landing/troubleshooting.md)を参照してください。

## 一般的な宛先に関する質問 {#general}

### Experience Platform UIと書き出されたCSV ファイルで異なるプロファイル数が表示されるのはなぜですか？ {#profile-count-discrepancy}

+++回答
これは、Experience Platformのセグメンテーションの実行方法による通常の動作です。

ストリーミングセグメンテーションは、ストリーミングオーディエンスのプロファイル数を1日を通して更新します。一方、バッチセグメンテーションは、バッチオーディエンスのプロファイル数を24時間ごとに更新します。

オーディエンスの書き出しスケジュールがセグメント化スケジュールと異なる場合、特にストリーミングオーディエンスに関しては、UIと書き出された[!DNL CSV] ファイルのプロファイル数が異なります。

詳しくは、[&#x200B; セグメント化サービスのドキュメント &#x200B;](../segmentation/home.md)を参照してください。
+++

### 更新されたオーディエンスを同じ宛先に対して非アクティブ化して再アクティブ化すると、マッチ率が低くなるのはなぜですか？ {#low-match-rates-reactivation}

+++回答

ストリーミング宛先からのオーディエンスの非アクティベーションとアクティベーションは、同じストリーミング宛先へのオーディエンスの再アクティベーション時にバックフィルをトリガーしません。

**例**

ストリーミング宛先に対して10個のプロファイルで構成されるオーディエンスをアクティブ化しました。

オーディエンスをアクティベートした後で、オーディエンスの設定を変更する必要があることに気づきます。そのため、オーディエンスを非アクティベートにし、その母集団条件を変更すると、100 プロファイルのオーディエンス母集団が作成されます。

更新されたオーディエンスを同じ宛先に再アクティブ化しますが、バックフィルがトリガーされないため、宛先は追加の90 プロファイルを受け取りません。

**ソリューション**

すべてのプロファイルが宛先に送信されるようにするには、新しい設定で新しいオーディエンスを作成し、宛先に対してアクティブ化する必要があります。

+++

### オーディエンスが宛先から削除された場合、そのオーディエンスが削除されたことを示す信号が宛先に送信されますか？ {#audience-removal-signal}

+++回答

いいえ。Experience Platformの宛先とターゲットシステムのお客様のインスタンスの間には依存関係はありません。 受信側では、ターゲットシステムが表示する唯一の兆候は、そのオーディエンスデータの受信を停止したことです。

+++

<!--
## [!DNL Experience Cloud Audiences] {#eca-faq}

### What are the differences between the Experience Cloud Audiences and Adobe Target destinations?

+++Answer

See the table below for a feature comparison between the Experience Cloud Audiences and Adobe Target destinations.

||Experience Cloud Audiences|Adobe Target|
|---|---|---|
| **Supported Experience Cloud apps** | Supports audience activation to Audience Manager, [!DNL Adobe Target], [!DNL Adobe Analytics], Adobe Advertising, Marketo, [!DNL Adobe Campaign] | Supports audience activation only to [!DNL Adobe Target] |
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

### [!DNL Facebook Custom Audiences]でオーディエンスをアクティブ化する前に、どうすればよいですか？ {#facebook-activate-prerequisites}

+++回答
オーディエンスを[!DNL Facebook]に送信する前に、次の要件を満たしていることを確認してください。

* [!DNL Facebook] ユーザーアカウントでは、使用するAd アカウントに対して&#x200B;**[!DNL Manage campaigns]**&#x200B;権限を有効にする必要があります。
* **[!DNL Adobe Experience Cloud]** ビジネス アカウントを[!DNL Facebook Ad Account]の広告パートナーとして追加する必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebook ドキュメントの「[&#x200B; ビジネスマネージャーにパートナーを追加](https://www.facebook.com/business/help/1717412048538897)」を参照してください。

  >[!IMPORTANT]
  >
  > [!DNL Adobe Experience Cloud]の権限を設定する場合、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。 これは、[!DNL Adobe Experience Platform]統合に必要です。
* [!DNL Facebook Custom Audiences]の利用条件を読んで署名します。 これを行うには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]`に移動します。ここで、`accountID`は[!DNL Facebook Ad Account ID]です。
+++

### [!DNL Facebook]広告主アカウントにアプリまたはピクセルを追加する必要がありますか？ {#facebook-pixels}

+++回答
いいえ。ピクセルベースの統合ではないため、広告主のアカウントにピクセルを追加する必要はありません。
+++

### Facebookが[!DNL Adobe Experience Platform]からの情報を処理するのにどのくらいの時間がかかりますか？ {#facebook-processing-time}

+++回答
2021年3月現在、[!DNL Facebook Custom Audiences]は[!DNL Experience Platform]から受信した情報を処理するのに最大1時間かかる必要があります。
+++

### [!DNL Facebook Custom Audiences]など、他の[!DNL Facebook] アプリで[!DNL Instagram]をオーディエンスターゲティングに使用できますか？ {#facebook-cross-app-targeting}

+++回答
[!DNL Facebook Custom Audiences]の宛先は、[!DNL Facebook Custom Audiences]、[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]など、[!DNL Messenger]でサポートされているFacebookのアプリファミリー全体でオーディエンスターゲティングに使用できます。 広告主がキャンペーンを実行するアプリの選択は、[!DNL Facebook Ads Manager]のプレースメントレベルに示されます。
+++

### [!DNL Facebook Custom Audiences]接続と[!DNL Facebook Pixel]拡張機能の違いは何ですか？ {#facebook-custom-audiences-vs-pixel}

+++回答
[!DNL Facebook Custom Audiences]接続は[!DNL Experience Platform]にオーディエンスを送信する際に[!DNL Facebook]のIDを使用し、[[!DNL Facebook Pixel] 接続](../destinations/catalog/advertising/facebook-pixel.md)はweb サイトに統合された[!DNL Facebook] ピクセルを使用します。

これらの2つの統合は補完的なもので、両方を使用することで、より優れたオーディエンスをカバーすることができます。 例として、[!DNL Facebook Pixel]拡張機能は、アカウントを作成していない見込みweb サイト訪問者に対して使用できます。一方、[!DNL Facebook Custom Audiences]は、[!DNL Experience Platform]のIDに基づいて、既存顧客をターゲットにするのに役立ちます。
+++

### [!DNL Adobe Experience Platform]との[!DNL Facebook Custom Audiences]統合は、ユーザーがオーディエンスに対する資格を失った場合に、オーディエンスの資格を失うことをサポートしますか？ {#facebook-disqualify-users}

+++回答
はい。統合では、ユーザーが資格を失った場合に[!DNL Facebook Custom Audiences]からユーザーを削除することをサポートしています。
+++

### オーディエンスデータを[!DNL Facebook]に送信する前に、どのようにハッシュすればよいですか？ {#facebook-hashing}

+++回答
[!DNL Facebook]では、個人を特定できる情報（PII）が明確に送信されていないことが必要です。 したがって、[!DNL Facebook]にアクティブ化されたオーディエンスは、電子メールアドレスや電話番号などの&#x200B;*ハッシュ*&#x200B;識別子にキーを設定できます。

ID照合要件について詳しくは、[ID照合要件](catalog/social/facebook.md#id-matching-requirements)を参照してください。
+++

### [!DNL Facebook Custom Audiences]でアクティブ化できるIDの種類を教えてください。 {#facebook-identities}

+++回答
[!DNL Facebook Custom Audiences]では、ハッシュ化された電子メール、ハッシュ化された電話番号、[!DNL GAID]、[!DNL IDFA]およびカスタム外部IDのアクティブ化がサポートされています。
+++

### Experience Platform UIで、個別のFacebook アカウント用に複数のFacebook宛先を作成できますか？ {#facebook-multiple-destinations}

+++回答
はい。 Experience PlatformのFacebookの宛先は、Facebookの広告アカウントへの1:1です。 会社のFacebook広告アカウントごとに個別のFacebook宛先を作成できます。 [destination connection tutorial](/help/destinations/ui/connect-destination.md)に従って、Experience Platform UIの新しいFacebook宛先ごとに個別のFacebook アカウントに接続します。 接続できるFacebook広告アカウントの数に制限はありません。
+++

## Google カスタマーマッチ {#google-customer-match}

### Google Customer Matchにオーディエンスを書き出す際に、Google インターフェイスのオーディエンス名の末尾に追加の数字が追加されているのはなぜですか？ {#google-customer-match-audience-name-numbers}

+++回答
Googleでは、オーディエンス名は一意である必要があります。 表示される数値は[UNIX タイムスタンプ &#x200B;](https://www.unixtimestamp.com/)で、同じオーディエンスを複数のGoogleの宛先にマッピングした場合、オーディエンス名を一意に保つために追加されます。
+++

## LinkedIn Matched Audiences {#linkedin}

### [!DNL LinkedIn]広告主アカウントにアプリまたはピクセルを追加する必要がありますか？ {#linkedin-pixels}

+++回答
いいえ。ピクセルベースの統合ではないため、広告主のアカウントにピクセルを追加する必要はありません。
+++

### [!DNL LinkedIn Matched Audiences]でオーディエンスをアクティブ化する前に、どうすればよいですか？ {#linkedin-activate-prerequisites}

+++回答
[!UICONTROL LinkedIn Matched Audience]宛先を使用する前に、[!DNL LinkedIn Campaign Manager] アカウントが[!DNL Creative Manager]権限レベル以上であることを確認してください。

[!DNL LinkedIn Campaign Manager]のユーザー権限を編集する方法については、LinkedIn ドキュメントの「[Advertising アカウントのユーザー権限を追加、編集、削除](https://www.linkedin.com/help/lms/answer/5753)」を参照してください。
+++

### オーディエンスデータを[!DNL LinkedIn]に送信する前に、どのようにハッシュすればよいですか？ {#linkedin-hashing}

+++回答
[!DNL LinkedIn]では、個人を特定できる情報（PII）が明確に送信されていないことが必要です。 したがって、[!DNL LinkedIn]にアクティブ化されたオーディエンスは、電子メールアドレスや電話番号などの&#x200B;*ハッシュ*&#x200B;識別子にキーを設定できます。

ID照合要件について詳しくは、[ID照合要件](catalog/social/linkedin.md#id-matching-requirements)を参照してください。
+++

### [!DNL LinkedIn]でアクティブ化できるIDの種類を教えてください。 {#linkedin-identities}

+++回答
[!DNL LinkedIn Matched Audiences]は、ハッシュ化された電子メール、[!DNL GAID]および[!DNL IDFA]のIDのアクティブ化をサポートしています。

+++

## [!DNL Adobe Target]およびカスタム Personalizationの宛先を通じた同一ページおよび次ページのパーソナライゼーション {#same-next-page-personalization}

### オーディエンスと属性を[!DNL Adobe Target]に送信するには、Experience Platform Web SDKを使用する必要がありますか？ {#target-web-sdk}

+++回答
いいえ。オーディエンスを[[!DNL Adobe Target]](catalog/personalization/adobe-target-connection.md)にアクティベートするためにWeb SDKは必要ありません。

ただし、Web SDKの代わりに[[!DNL at.js]](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html?lang=ja)を使用する場合は、次セッションのパーソナライゼーションのみがサポートされます。

[同じページと次のページのパーソナライゼーション &#x200B;](ui/activate-edge-personalization-destinations.md)のユースケースでは、Web SDKまたは[Edge Network API](https://developer.adobe.com/data-collection-apis/docs/api/)のいずれかを使用する必要があります。 実装の詳細については、[&#x200B; エッジ宛先へのオーディエンスのアクティブ化](ui/activate-edge-personalization-destinations.md)に関するドキュメントを参照してください。
+++

### Real-time Customer Data Platformから[!DNL Adobe Target]またはカスタム Personalizationの宛先に送信できる属性の数に制限はありますか？ {#target-attributes-limit}

+++回答
はい、同一ページおよび次ページのパーソナライゼーションのユースケースでは、サンドボックスごとに最大30個の属性をサポートします。これは、[!DNL Adobe Target]またはカスタム Personalizationの宛先に対してオーディエンスをアクティブ化する場合です。 アクティベーションガードレールについて詳しくは、[&#x200B; ガードレールのドキュメント &#x200B;](guardrails.md#edge-destinations-activation)を参照してください。
+++

### アクティベーションでサポートされる属性の種類（配列、マップなど） {#target-supported-attribute-types}

+++回答
現在、静的な単一の値の属性（`person.name.firstName`など）のみがサポートされています。 配列属性は現在サポートされていません。
+++

<!-- 
**Is there a limit on the number of audiences that can be activated to Adobe Target and Custom Personalization destinations?**

Yes, you can activate a maximum of 150 edge audiences per sandbox.  For more information on activation guardrails, see the [default guardrails for activation](guardrails.md#edge-destinations-activation). 
-->

### Experience Platformでオーディエンスを作成した後、そのオーディエンスがエッジセグメント化ユースケースで使用できるようになるまでにどのくらいの時間がかかりますか？ {#edge-segmentation-availability}

+++回答
オーディエンス定義は、最大1時間でEdge Networkに反映されます。 しかし、この1時間以内にオーディエンスがアクティブ化されると、オーディエンスに適格であった訪問者の一部が欠落する可能性があります。
+++

### [!DNL Adobe Target]のアクティブ化された属性はどこで確認できますか？ {#target-activated-attributes-location}

+++回答
属性は、[JSON](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html)および[HTML](https://experienceleague.adobe.com/docs/target/using/experiences/offers/manage-content.html)のオファーでTargetで使用できます。
+++

### データストリームを使用せずに宛先を作成し、後で同じ宛先にデータストリームを追加できますか？ {#destination-without-datastream}

+++回答
これは現在、宛先UIではサポートされていません。 この場合についてサポートが必要な場合は、Adobe担当者にお問い合わせください。
+++

### [!DNL Adobe Target]宛先を削除するとどうなりますか？ {#delete-target-destination}

+++回答
宛先を削除すると、宛先の下にマッピングされたすべてのオーディエンスと属性が[!DNL Adobe Target]から削除され、Edge Networkからも削除されます。
+++

### Edge Network APIを使用して連携できますか？ {#edge-network-api-integration}

+++回答
はい、Edge Network APIはカスタム Personalizationの宛先と連携します。 プロファイル属性には機密データが含まれる場合があるため、このデータを保護するために、カスタム Personalizationの宛先では、データ収集にEdge Network APIを使用する必要があります。 さらに、すべてのAPI呼び出しは[認証済みコンテキスト &#x200B;](https://developer.adobe.com/data-collection-apis/docs/getting-started/authentication/)で行う必要があります。
+++

### エッジ上でアクティブな結合ポリシーは1つだけです。 別の結合ポリシーを使用するオーディエンスを作成しても、ストリーミングオーディエンスとして[!DNL Adobe Target]に送信できますか？ {#edge-merge-policy}

+++回答
いいえ。[!DNL Adobe Target]に対してアクティブ化するすべてのオーディエンスは、アクティブなエッジ [結合ポリシー](../profile/merge-policies/ui-guide.md)を使用する必要があります。
+++

### データ使用のラベル付けと施行（DULE）と同意ポリシーは適用されますか？ {#dule-consent-enforcement}

+++回答
はい。選択したマーケティングアクションに作成され、関連付けられた[&#x200B; データガバナンスおよび同意ポリシー](../data-governance/home.md)が、選択した属性のアクティベーションを管理します。
+++

### [!DNL Adobe Target]および[!DNL Custom Personalization]の宛先は[!DNL HIPAA]に準拠していますか？ {#hipaa-compliance}

+++回答
[!DNL Adobe Target]は[!DNL HIPPA][[!DNL Adobe Healthcare Shield]に対して](https://business.adobe.com/solutions/industries/healthcare.html)に準拠していません。 [!DNL HIPPA]または[!DNL Adobe Target]の宛先を介したエッジパーソナライゼーションを使用する前に、カスタム最適化チャネルの[!DNL Custom Personalization]の準備状況について、お客様が独自の法務チームに確認する必要があります。

同意ポリシー管理を大規模に適用する必要があるユースケースの場合、顧客は[!DNL Adobe Privacy & Security Shield]を購入する必要があります。 [!DNL Adobe Privacy & Security Shield]機能は高度な機能スイートとして販売されており、個別に購入することはできません。

このサービスには、顧客が管理するキーと、顧客データのライフサイクルを管理するためのしきい値の上昇が含まれます。

[!DNL Adobe Target]および[!DNL Custom Personalization]の宛先は、[Experience Platform Data Usage Labels](../data-governance/labels/overview.md)および[Consent Policy Enforcement Service](../data-governance/enforcement/overview.md)と統合されています。 これらの機能は、すべてのお客様が利用できます。




+++


---
keywords: 宛先；質問；よくある質問； faq；宛先 faq
title: よくある質問
description: Adobe Experience Platformの宛先に関するよくある質問への回答
exl-id: 2c34ecd0-a6d0-48dd-86b0-a144a6acf61a
source-git-commit: 5b37b51308dc2097c05b0e763293467eb12a2f21
workflow-type: tm+mt
source-wordcount: '1673'
ht-degree: 4%

---

# よくある質問 {#faq}

## 概要 {#overview}

このドキュメントでは、Adobe Experience Platformの宛先に関するよくある質問に対する回答を示します。 他の [!DNL Platform] すべての [!DNL Platform] API（を参照） [Experience Platformトラブルシューティングガイド](../landing/troubleshooting.md).

## 宛先に関する一般的な質問 {#general}

### Experience PlatformUI と書き出された CSV ファイルで異なるプロファイル数が表示されるのはなぜですか？

+++回答これは、セグメント化の実行方法が原因で発生する通常のExperience Platformです。

ストリーミングセグメント化では、1 日を通じてストリーミングオーディエンスのプロファイル数が更新され、バッチセグメント化では、24 時間に 1 回、バッチオーディエンスのプロファイル数が更新されます。

オーディエンスの書き出しスケジュールがセグメント化スケジュールと異なる場合、UI と書き出した UI の間のプロファイル数 [!DNL CSV] ファイルは、特にストリーミングオーディエンスの場合は異なります。

詳しくは、 [セグメント化サービスのドキュメント](../segmentation/home.md) を参照してください。
+++

### 同じ宛先に対して更新されたオーディエンスを非アクティブ化し、再アクティブ化すると、一致率が低くなるのはなぜですか。

+++回答

ストリーミング先からのオーディエンスのアクティベーション解除とは、同じストリーミング先に対してオーディエンスの再アクティベーションを行ったときに、バックフィルをトリガーしません。

**例**

10 個のプロファイルから成るオーディエンスをストリーミング先に対してアクティブ化しました。

オーディエンスをアクティブ化した後、オーディエンスの設定を変更したいことに気がついたので、オーディエンスを非アクティブ化し、その母集団の条件を変更すると、100 個のプロファイルのオーディエンス母集団が作成されます。

同じ宛先に対して更新されたオーディエンスを再度アクティブ化したときに、バックフィルがトリガーされないので、宛先は追加の 90 件のプロファイルを受け取りません。

**ソリューション**

すべてのプロファイルを確実に宛先に送信するには、新しい設定で新しいオーディエンスを作成し、宛先に対してアクティブ化する必要があります。

+++

### オーディエンスが宛先から削除された場合、そのオーディエンスが削除されたことを示すシグナルが宛先に送信されることはありますか。

+++回答

いいえ。ターゲットシステムのExperience Platform先と顧客インスタンスとの間に依存関係はありません。 受信側では、ターゲットシステムが認識する唯一の兆候は、そのオーディエンスデータの受信を停止したことです。

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

### でオーディエンスをアクティブ化する前に必要なこと [!DNL Facebook Custom Audiences]?

+++回答オーディエンスを次に送信する前に： [!DNL Facebook]次の要件を満たしていることを確認します。

* お使いの [!DNL Facebook] ユーザーアカウントには **[!DNL Manage campaigns]** 使用する予定の広告アカウントに対して有効になっている権限です。
* The **Adobe Experience Cloud** ビジネスアカウントは、 [!DNL Facebook Ad Account]. `business ID=206617933627973`.を使用します。詳しくは、 [ビジネスマネージャにパートナーを追加する](https://www.facebook.com/business/help/1717412048538897) ( Facebookのドキュメント ) を参照してください。

  >[!IMPORTANT]
  >
  > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Experience Platform] 統合に必要です。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。
+++

### アプリやピクセルを [!DNL Facebook] 広告主の口座？

+++回答
いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。
+++

### facebookがAdobe Experience Platformからの情報を処理するのにどのくらい時間がかかりますか？

+++2021 年 3 月現在の回答 [!DNL Facebook Custom Audiences] から受け取った情報を処理するには、最大 1 時間必要です。 [!DNL Platform].
+++

### 使用可能な機能 [!DNL Facebook Custom Audiences] （他のオーディエンスのターゲティング用） [!DNL Facebook] アプリ、など [!DNL Instagram]?

+++Amswer [!DNL Facebook Custom Audiences] facebookファミリーのアプリをまたいでオーディエンスのターゲティングをおこなう宛先。 [!DNL Facebook Custom Audiences]を含む [!DNL Facebook], [!DNL Instagram], [!DNL Audience Network]、および [!DNL Messenger]. キャンペーンを実行する広告主のアプリの選択は、 [!DNL Facebook Ads Manager].
+++

### 違いは何ですか？ [!DNL Facebook Custom Audiences] 接続と [!DNL Facebook Pixel] 拡張？

+++ [!DNL Facebook Custom Audiences] 接続を使用 [!DNL Platform] オーディエンスを次に送信する際の id [!DNL Facebook]を返し、 [[!DNL Facebook Pixel] 接続](../destinations/catalog/advertising/facebook-pixel.md) は [!DNL Facebook] web サイトに統合されたピクセル。

これら 2 つの統合は補完的です。両方を使用して、オーディエンス対象範囲を改善することができます。例えば、 [!DNL Facebook Pixel] アカウントを作成していない見込み Web サイト訪問者を対象とする拡張 [!DNL Facebook Custom Audiences] は、 [!DNL Platform] ID。
+++

### Adobe Experience Platformと [!DNL Facebook Custom Audiences] 資格がなくなったユーザーのオーディエンスからの除外をサポートします?**

+++回答はい。統合は、次の場所からのユーザーの削除をサポートします： [!DNL Facebook Custom Audiences] 条件を満たさなくなったときに
+++

### に送信する前にオーディエンスデータをハッシュ化する方法 [!DNL Facebook]?

+++回答
[!DNL Facebook] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL Facebook] キーオフできる *ハッシュ* 識別子（電子メールアドレスや電話番号など）。

ID 照合の要件について詳しくは、 [ID 一致要件](catalog/social/facebook.md#id-matching-requirements).
+++

### でアクティブ化できる ID の種類 [!DNL Facebook Custom Audiences]?

+++回答
[!DNL Facebook Custom Audiences] は、次の ID の有効化をサポートしています：ハッシュ化された電子メール、ハッシュ化された電話番号、 [!DNL GAID], [!DNL IDFA]、およびカスタム外部 ID。
+++

### Platform UI で、個別のFacebookアカウントに対して複数のFacebook宛先を作成できますか？

+++回答
はい。 facebookのExperience Platform先は、Facebookの広告アカウントに対して 1:1 です。 会社内のFacebook広告アカウントごとに別々のFacebookの宛先を作成できます。 フォロー： [宛先接続のチュートリアル](/help/destinations/ui/connect-destination.md) をクリックし、Platform UI の新しいFacebookの宛先ごとに個別のFacebookアカウントに接続します。 接続できるFacebook広告アカウントの数に制限はありません。
+++

## Google Customer Match {#google-customer-match}

### オーディエンスをGoogle Customer Match にエクスポートする際に、Googleインターフェイスのオーディエンス名の末尾に余分な数字が追加されているのはなぜですか。

+++回答Googleは一意のオーディエンス名を必要とします。 表示される数は次のとおりです。 [UNIX タイムスタンプ](https://www.unixtimestamp.com/) 同じオーディエンスを複数のGoogle宛先にマッピングした場合、オーディエンス名を一意にするために、が追加されます。
+++

## LinkedIn Matched Audiences {#linkedin}

### アプリやピクセルを [!DNL LinkedIn] 広告主の口座？

+++回答
いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。
+++

### でオーディエンスをアクティブ化する前に必要なこと [!DNL LinkedIn Matched Audiences]?

+++回答使用する前に [!UICONTROL LinkedIn Matched Audience] 宛先、 [!DNL LinkedIn Campaign Manager] アカウントに [!DNL Creative Manager] 権限レベル以上。

編集方法を学ぶには [!DNL LinkedIn Campaign Manager] ユーザー権限： [Advertising アカウントに対するユーザー権限の追加、編集、削除](https://www.linkedin.com/help/lms/answer/5753) (LinkedInドキュメント ) を参照してください。
+++

### に送信する前にオーディエンスデータをハッシュ化する方法 [!DNL LinkedIn]?

+++回答
[!DNL LinkedIn] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL LinkedIn] キーオフできる *ハッシュ* 識別子（電子メールアドレスや電話番号など）。

ID 照合の要件について詳しくは、 [ID 一致要件](catalog/social/linkedin.md#id-matching-requirements).
+++

### でアクティブ化できる ID の種類 [!DNL LinkedIn]?

+++回答
[!DNL LinkedIn Matched Audiences] は、次の ID の有効化をサポートしています：ハッシュ化された電子メール、 [!DNL GAID]、および [!DNL IDFA].

+++

## Adobe Targetおよびカスタムパーソナライゼーションの宛先を使用した、同じページおよび次のページのパーソナライゼーション {#same-next-page-personalization}

### Experience PlatformWeb SDK を使用して、オーディエンスと属性をAdobe Targetに送信する必要がありますか？

+++答えはいいえ。 [Web SDK](../web-sdk/home.md) は、オーディエンスをアクティブ化する必要はありません。 [Adobe Target](catalog/personalization/adobe-target-connection.md).

ただし、 [[!DNL at.js]](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/overview.html?lang=ja) が Web SDK の代わりに使用され、次回のセッションのパーソナライゼーションのみがサポートされます。

の場合 [同じページと次のページのパーソナライゼーション](ui/activate-edge-personalization-destinations.md) ユースケースでは、次のいずれかを使用する必要があります。 [Web SDK](../web-sdk/home.md) または [Edge Network Server API](../server-api/overview.md). 次のドキュメントを参照してください： [エッジ宛先へのオーディエンスのアクティブ化](ui/activate-edge-personalization-destinations.md) を参照してください。
+++

### Real-time Customer Data PlatformからAdobe Targetまたはカスタムパーソナライゼーションの宛先に送信できる属性の数に制限はありますか。

+++回答はい。同じページおよび次のページのパーソナライゼーションの使用例では、Adobe Targetまたはカスタムパーソナライゼーションの宛先に対してオーディエンスをアクティブ化する際に、サンドボックスあたり最大 30 個の属性をサポートします。 アクティベーションガードレールについて詳しくは、 [guardrails のドキュメント](guardrails.md#edge-destinations-activation).
+++

### アクティベーションでサポートされる属性の種類（配列、マップなど）は何ですか。

+++回答現在、静的な単一値属性（例： ）のみがサポートされています。 `person.name.firstName`. 配列属性は現在サポートされていません。
+++

<!-- **Is there a limit on the number of audiences that can be activated to Adobe Target and Custom Personalization destinations?**

Yes, you can activate a maximum of 150 edge audiences per sandbox.  For more information on activation guardrails, see the [default guardrails for activation](guardrails.md#edge-destinations-activation). -->

### Experience Platformでオーディエンスを作成した後、そのオーディエンスがエッジセグメント化の使用例で使用できるようになるまで、どの程度の時間がかかりますか？

+++回答オーディエンスの定義は、 [Edge Network](../web-sdk/home.md) 最大 1 時間で ただし、この最初の 1 時間以内にオーディエンスがアクティブ化された場合、そのオーディエンスの資格を持つ一部の訪問者が欠落する可能性があります。
+++

### アクティベートされた属性はAdobe Targetのどこで確認できますか？

+++回答属性は、 [JSON](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html) および [HTML](https://experienceleague.adobe.com/docs/target/using/experiences/offers/manage-content.html?lang=ja) オファー。
+++

### データストリームを使用せずに宛先を作成し、後で同じ宛先にデータストリームを追加することはできますか？

+++回答：これは、現在、宛先 UI ではサポートされていません。 この場合に不明な点がある場合は、Adobe担当者にお問い合わせください。
+++

### Adobe Targetの宛先を削除するとどうなりますか。

+++回答宛先を削除すると、宛先にマッピングされているすべてのオーディエンスと属性がAdobe Targetから削除され、Edge ネットワークからも削除されます。
+++

### 統合は Edge Network Server API を使用して動作しますか？

+++回答はい、Edge Network Server API はカスタムパーソナライゼーションの宛先で機能します。 プロファイル属性には機密データが含まれている場合があるので、このデータを保護するために、カスタムパーソナライゼーションの宛先では、データ収集に Edge Network Server API を使用する必要があります。 さらに、すべての API 呼び出しは、 [認証済みコンテキスト](../server-api/authentication.md).
+++

### エッジ上でアクティブな結合ポリシーは 1 つだけです。 別の結合ポリシーを使用し、ストリーミングオーディエンスとしてAdobe Targetに送信するオーディエンスを構築できますか？

+++回答
いいえ。Adobe Targetに対してアクティブ化するすべてのオーディエンスは、エッジ上でアクティブである必要があります [結合ポリシー](../profile/merge-policies/ui-guide.md).
+++

### Data Usage Labeling and Enforcement(DULE) と同意ポリシーは適用されますか。

+++回答
はい。 The [データガバナンスと同意ポリシー](../data-governance/home.md) 選択したマーケティングアクションに作成され、関連付けられている場合、選択した属性のアクティベーションが制御されます。
+++

### は [!DNL Adobe Target] および [!DNL Custom Personalization] 宛先 [!DNL HIPAA]準拠しているか？

+++回答
[!DNL Adobe Target] 等しくない [!DNL HIPPA]～に準拠している [[!DNL Adobe Healthcare Shield]](https://business.adobe.com/solutions/industries/healthcare.html). 顧客は、に関して自社の法務チームに確認する必要があります。 [!DNL HIPPA]を介してエッジパーソナライゼーションを使用する前のカスタム最適化チャネルへの対応 [!DNL Adobe Target] または [!DNL Custom Personalization] 宛先。

同意ポリシー管理を大規模に適用する必要がある使用例の場合、お客様は [!DNL Adobe Privacy & Security Shield]. [!DNL Adobe Privacy & Security Shield] 機能は高度な機能スイートとして販売され、別途購入することはできません。

このサービスには、顧客が管理する鍵と、顧客データのライフサイクルを管理するための高いしきい値が含まれます。

The [!DNL Adobe Target] および [!DNL Custom Personalization] の宛先は、 [Experience Platformデータ使用ラベル](../data-governance/labels/overview.md) そして [同意ポリシー実施サービス](../data-governance/enforcement/overview.md). これらの機能は、すべてのお客様が利用できます。




+++


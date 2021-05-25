---
keywords: 宛先；質問；よくある質問faq;宛先のfaq
title: よくある質問
seo-title: よくある質問
description: Adobe Experience Platformの宛先に関するよくある質問への回答
seo-description: Adobe Experience Platformの宛先に関するよくある質問への回答
source-git-commit: 47b3ef28281e3480e8b194486845f4fb4326b7d4
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 17%

---


# よくある質問 {#faq}

## 概要 {#overview}

このドキュメントでは、Adobe Experience Platformの宛先に関するよくある質問に対する回答を示します。 すべての[!DNL Platform] APIで発生する問題を含め、他の[!DNL Platform]サービスに関する質問とトラブルシューティングについては、[Experience Platformのトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

## [!DNL Facebook Custom Audiences] {#facebook-faq}

**でオーディエンスをアクティブ化する前に、何をする必要がありま [!DNL Facebook Custom Audiences]すか？**

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* お使いの[!DNL Facebook]ユーザーアカウントで、使用する予定の広告アカウントに対する&#x200B;**[!DNL Manage campaigns]**&#x200B;権限が有効になっている必要があります。
* **Adobe Experience Cloud**&#x200B;ビジネスアカウントは、[!DNL Facebook Ad Account]に広告パートナーとして追加する必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの「[Add Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897)」を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Experience Platform] 統合に必要です。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

**広告主アカウントにアプリやピクセルを追加する必要が [!DNL Facebook] ありますか？**

いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**facebookがAdobe Experience Platformからの情報を処理するのにどれくらいかかりますか？**

2021年3月現在、[!DNL Platform]から受信した情報を処理するには、最大1時間必要です。[!DNL Facebook Custom Audiences]

**を他のアプリ(な [!DNL Facebook Custom Audiences] ど)でのオーディエンスのターゲ [!DNL Facebook] ティングに使用できま [!DNL Instagram]すか？**

[!DNL Facebook Custom Audiences]の宛先を、[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]、[!DNL Messenger]など、[!DNL Facebook Custom Audiences]でサポートされるFacebookのアプリファミリーをまたいでオーディエンスのターゲティングに使用できます。 広告主がキャンペーンを実行するアプリの選択は、[!DNL Facebook Ads Manager]の配置レベルで示されます。

**接続と拡張機能の違い [!DNL Facebook Custom Audiences] は何で [!DNL Facebook Pixel] すか。**

[!DNL Facebook Custom Audiences]接続は[!DNL Facebook]にオーディエンスを送信する際に[!DNL Platform] IDを使用し、[[!DNL Facebook Pixel] 接続](../destinations/catalog/advertising/facebook-pixel.md)はWebサイトに統合された[!DNL Facebook]ピクセルを使用します。

これら 2 つの統合は補完的です。両方を使用して、オーディエンス対象範囲を改善することができます。例えば、[!DNL Facebook Pixel]拡張機能を使用して、アカウントを作成していないWebサイト訪問者を探すことができます。[!DNL Facebook Custom Audiences]は、[!DNL Platform] IDに基づいて、既存の顧客のターゲティングに役立ちます。

**Adobe Experience Platformとの統合は、資格がなくな [!DNL Facebook Custom Audiences] ったユーザーのオーディエンスからの除外をサポートしますか？**

はい。統合では、ユーザーが資格を失った場合の[!DNL Facebook Custom Audiences]からの削除をサポートします。

**に送信する前に、オーディエンスデータをどのようにハッシュ化すればよいで [!DNL Facebook]すか？**

[!DNL Facebook] では、個人を特定できる情報(PII)を明確に送信しないことが求められます。したがって、[!DNL Facebook]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにできます。

ID一致要件について詳しくは、[ID一致要件](catalog/social/facebook.md#id-matching-requirements)を参照してください。

**でアクティブ化できるIDの種類を教えてくだ [!DNL Facebook Custom Audiences]さい。**

[!DNL Facebook Custom Audiences] は、次のIDのアクティブ化をサポートしています。ハッシュ化された電子メール、ハッシュ化され [!DNL GAID]た電話番号、 [!DNL IDFA] 、およびカスタム外部ID。

## linkedIn Matched Audiences {#linkedin}

**広告主アカウントにアプリやピクセルを追加する必要が [!DNL LinkedIn] ありますか？**

いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**でオーディエンスをアクティブ化する前に、何をする必要がありま [!DNL LinkedIn Matched Audiences]すか？**

[!UICONTROL LinkedInの一致するオーディエンス]の宛先を使用する前に、[!DNL LinkedIn Campaign Manager]アカウントに[!DNL Creative Manager]権限レベル以上があることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

**に送信する前に、オーディエンスデータをどのようにハッシュ化すればよいで [!DNL LinkedIn]すか？**

[!DNL LinkedIn] では、個人を特定できる情報(PII)を明確に送信しないことが求められます。したがって、[!DNL LinkedIn]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにできます。

ID一致要件について詳しくは、[ID一致要件](catalog/social/linkedin.md#id-matching-requirements)を参照してください。

**でアクティブ化できるIDの種類を教えてくだ [!DNL LinkedIn]さい。**

[!DNL LinkedIn Matched Audiences] は、次のIDのアクティブ化をサポートしています。ハッシュ化された電子メ [!DNL GAID]ール、および [!DNL IDFA]。

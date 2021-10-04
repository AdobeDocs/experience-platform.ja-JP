---
keywords: 宛先；質問；よくある質問faq;宛先の faq
title: よくある質問
seo-title: Frequently asked questions
description: Adobe Experience Platformの宛先に関するよくある質問への回答
seo-description: Answers to the most frequently asked questions about Adobe Experience Platform destinations
exl-id: 2c34ecd0-a6d0-48dd-86b0-a144a6acf61a
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 14%

---

# よくある質問 {#faq}

## 概要 {#overview}

このドキュメントでは、Adobe Experience Platformの宛先に関するよくある質問に対する回答を示します。 すべての [!DNL Platform] API で発生する問題を含め、他の [!DNL Platform] サービスに関する質問とトラブルシューティングについては、[Experience Platformのトラブルシューティングガイド ](../landing/troubleshooting.md) を参照してください。

## 宛先に関する一般的な質問 {#general}

**Experience PlatformUI と書き出された CSV ファイルで異なるプロファイル数が表示されるのはなぜですか？**

これは、セグメント化の実行方法による通常のExperience Platform動作です。

ストリーミングセグメント化では 1 日を通してストリーミングセグメントのプロファイル数が更新され、バッチセグメント化では 24 時間に 1 回、バッチセグメントのプロファイル数が更新されます。

セグメントの書き出しスケジュールがセグメント化スケジュールと異なる場合、特にストリーミングセグメントに関しては、UI と書き出される [!DNL CSV] ファイルの間のプロファイル数が異なります。

詳しくは、[ セグメント化サービスのドキュメント ](../segmentation/home.md) を参照してください。

## [!DNL Facebook Custom Audiences] {#facebook-faq}

**でオーディエンスをアクティブ化する前におこなう必要があることを教えてくだ [!DNL Facebook Custom Audiences]さい。**

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* お使いの [!DNL Facebook] ユーザーアカウントで、使用する予定の広告アカウントに対する **[!DNL Manage campaigns]** 権限が有効になっている必要があります。
* **Adobe Experience Cloud** ビジネスアカウントは、[!DNL Facebook Ad Account] に広告パートナーとして追加する必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの [Add Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897) を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Experience Platform] 統合に必要です。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

**広告主アカウントにアプリやピクセルを追加する必要が [!DNL Facebook] ありますか？**

いいえ。ピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**facebookがAdobe Experience Platformからの情報を処理するのにどれくらいかかりますか？**

2021 年 3 月現在、[!DNL Platform] から受信した情報を処理するには、最大 1 時間必要です。[!DNL Facebook Custom Audiences]

**を他のアプリ ( な [!DNL Facebook Custom Audiences] ど ) でのオーディエンスのターゲテ [!DNL Facebook] ィングに使用できま [!DNL Instagram]すか？**

[!DNL Facebook Custom Audiences] の宛先を使用して、[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]、[!DNL Messenger] など、[!DNL Facebook Custom Audiences] でサポートされるFacebookのアプリファミリーをまたいでオーディエンスのターゲティングをおこなうことができます。 広告主がキャンペーンを実行するアプリの選択は、[!DNL Facebook Ads Manager] の配置レベルで示されます。

**接続と拡張機能の違い [!DNL Facebook Custom Audiences] は何で [!DNL Facebook Pixel] すか。**

[!DNL Facebook Custom Audiences] 接続は [!DNL Facebook] にオーディエンスを送信する際に [!DNL Platform] ID を使用し、[[!DNL Facebook Pixel]  接続 ](../destinations/catalog/advertising/facebook-pixel.md) は Web サイトに統合された [!DNL Facebook] ピクセルを使用します。

これら 2 つの統合は補完的です。両方を使用して、オーディエンス対象範囲を改善することができます。例えば、アカウントを作成していない見込みの Web サイト訪問者に対して [!DNL Facebook Pixel] 拡張機能を使用できます。[!DNL Facebook Custom Audiences] は、[!DNL Platform] ID に基づいて、既存の顧客のターゲティングに役立ちます。

**Adobe Experience Platformとの統合は、資格がなくな [!DNL Facebook Custom Audiences] ったユーザーのオーディエンスからの除外をサポートしますか？**

はい。統合では、ユーザーが資格を失った場合の [!DNL Facebook Custom Audiences] からの削除をサポートします。

**に送信する前に、オーディエンスデータをハッシュ化する方法を教えてくだ [!DNL Facebook]さい。**

[!DNL Facebook] では、個人を特定できる情報 (PII) を明確に送信しないことが必要です。したがって、[!DNL Facebook] に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された* 識別子をキーオフにできます。

ID 一致要件について詳しくは、[ID 一致要件 ](catalog/social/facebook.md#id-matching-requirements) を参照してください。

**でアクティブ化できる ID の種類を教えてくだ [!DNL Facebook Custom Audiences]さい。**

[!DNL Facebook Custom Audiences] は、次の ID のアクティブ化をサポートしています。ハッシュ化された電子メール、ハッシュ化さ [!DNL GAID]れた電話番号、 [!DNL IDFA] 、およびカスタム外部 ID。

## linkedIn Matched Audiences {#linkedin}

**広告主アカウントにアプリやピクセルを追加する必要が [!DNL LinkedIn] ありますか？**

いいえ。ピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**でオーディエンスをアクティブ化する前におこなう必要があることを教えてくだ [!DNL LinkedIn Matched Audiences]さい。**

[!UICONTROL LinkedInの一致するオーディエンス ] の宛先を使用する前に、[!DNL LinkedIn Campaign Manager] アカウントに [!DNL Creative Manager] 権限レベル以上があることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

**に送信する前に、オーディエンスデータをハッシュ化する方法を教えてくだ [!DNL LinkedIn]さい。**

[!DNL LinkedIn] では、個人を特定できる情報 (PII) を明確に送信しないことが必要です。したがって、[!DNL LinkedIn] に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された* 識別子をキーオフにできます。

ID 一致要件について詳しくは、[ID 一致要件 ](catalog/social/linkedin.md#id-matching-requirements) を参照してください。

**でアクティブ化できる ID の種類を教えてくだ [!DNL LinkedIn]さい。**

[!DNL LinkedIn Matched Audiences] は、次の ID のアクティブ化をサポートしています。ハッシュ化された電子メ [!DNL GAID]ール、および [!DNL IDFA]。

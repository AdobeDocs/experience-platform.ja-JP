---
keywords: 宛先；質問；よくある質問faq;リンク先faq
title: 宛先FAQ
seo-title: 宛先FAQ
description: Adobe Experience Platformの目的地に関して最もよくある質問への回答
seo-description: Adobe Experience Platformの目的地に関して最もよくある質問への回答
source-git-commit: 61678c5a62980cdb81714420016b7c4b2093f5c6
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 16%

---


# 宛先FAQ {#faq}

## [!DNL Facebook Custom Audiences] {#facebook-faq}

**でオーディエンスをアクティベートする前に、何を行う必要があり [!DNL Facebook Custom Audiences]ますか？**

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* [!DNL Facebook]ユーザーアカウントで、使用する広告アカウントに対して&#x200B;**[!DNL Manage campaigns]**&#x200B;権限を有効にする必要があります。
* **Adobe Experience Cloud**&#x200B;のビジネスアカウントは、貴社の[!DNL Facebook Ad Account]に広告パートナーとして追加する必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebookのドキュメントの[追加 Partners to Your Business Manager](https://www.facebook.com/business/help/1717412048538897)を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Experience Platform] 統合に必要です。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

**広告主アカウントにアプリやピクセルを追加する必要があり [!DNL Facebook] ますか？**

いいえ。ピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**facebookはAdobe Experience Platformからの情報を処理するのにどのくらいかかりますか。**

2021年3月現在、[!DNL Facebook Custom Audiences]は、[!DNL Platform]から受け取った情報を処理するのに最大1時間必要です。

**他の [!DNL Facebook Custom Audiences] アプリ（など）でのオーディエンスのターゲット設定 [!DNL Facebook] に使用できま [!DNL Instagram]すか。**

[!DNL Facebook Custom Audiences]リンク先は、[!DNL Facebook]、[!DNL Instagram]、[!DNL Audience Network]、[!DNL Messenger]など、[!DNL Facebook Custom Audiences]でサポートされるFacebookのアプリファミリーでのオーディエンスターゲティングに使用できます。 広告主がキャンペーンを実行するアプリの選択は、[!DNL Facebook Ads Manager]の配置レベルで示されます。

**[!DNL Facebook Custom Audiences] 接続と [!DNL Facebook Pixel] 拡張の違いは何ですか。**

[!DNL Facebook Custom Audiences]接続は[!DNL Facebook]にオーディエンスを送信する際に[!DNL Platform] IDを使用し、[[!DNL Facebook Pixel] 接続](../destinations/catalog/advertising/facebook-pixel.md)はWebサイトに統合された[!DNL Facebook]ピクセルを使用します。

これら 2 つの統合は補完的です。両方を使用して、オーディエンス対象範囲を改善することができます。例えば、[!DNL Facebook Custom Audiences]は[!DNL Platform] IDに基づいて既存の顧客をターゲットするのに役立ちますが、[!DNL Facebook Pixel]はアカウントを作成していないWebサイト訪問者を調査するのに役立ちます。

**Adobe Experience Platformは、オーディエンスの資格がなくなった場合に、ユーザーの資格を [!DNL Facebook Custom Audiences] 失うユーザーをサポートしますか。**

はい、統合では、資格がなくなったユーザーを[!DNL Facebook Custom Audiences]から削除できます。

**オーディエンスデータを送信する前に、そのデータをどのようにハッシュする必要があり [!DNL Facebook]ますか。**

[!DNL Facebook] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL Facebook]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

IDの一致要件について詳しくは、[IDの一致要件](catalog/social/facebook.md#id-matching-requirements)を参照してください。

**どのようなIDを有効にでき [!DNL Facebook Custom Audiences]ますか。**

[!DNL Facebook Custom Audiences] は、次のIDのアクティベーションをサポートします。電子メール、ハッシュの電話番号、 [!DNL GAID]およびカスタム外部IDをハッシュ化し [!DNL IDFA]ます。

## linkedInマッチオーディエンス {#linkedin}

**広告主アカウントにアプリやピクセルを追加する必要があり [!DNL LinkedIn] ますか？**

いいえ。ピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**でオーディエンスをアクティベートする前に、何を行う必要があり [!DNL LinkedIn Matched Audiences]ますか？**

[!UICONTROL LinkedIn一致オーディエンス]の宛先を使用する前に、[!DNL LinkedIn Campaign Manager]アカウントに[!DNL Creative Manager]権限レベル以上があることを確認してください。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

**オーディエンスデータを送信する前に、そのデータをどのようにハッシュする必要があり [!DNL LinkedIn]ますか。**

[!DNL LinkedIn] 個人識別情報(PII)を明確に送信しないようにする必要があります。したがって、[!DNL LinkedIn]に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された*&#x200B;識別子をキーオフにすることができます。

IDの一致要件について詳しくは、[IDの一致要件](catalog/social/linkedin.md#id-matching-requirements)を参照してください。

**どのようなIDを有効にでき [!DNL LinkedIn]ますか。**

[!DNL LinkedIn Matched Audiences] は、次のIDのアクティベーションをサポートします。ハッシュ化された電子メール、 [!DNL GAID]および [!DNL IDFA]。

---
keywords: 宛先；質問；よくある質問faq;宛先の faq
title: よくある質問
seo-title: Frequently asked questions
description: Adobe Experience Platformの宛先に関するよくある質問への回答
seo-description: Answers to the most frequently asked questions about Adobe Experience Platform destinations
exl-id: 2c34ecd0-a6d0-48dd-86b0-a144a6acf61a
source-git-commit: 8f9a601a833149c83d465f68d16ca362ed730b8a
workflow-type: tm+mt
source-wordcount: '781'
ht-degree: 13%

---

# よくある質問 {#faq}

## 概要 {#overview}

このドキュメントでは、Adobe Experience Platformの宛先に関するよくある質問に対する回答を示します。 他の [!DNL Platform] すべての [!DNL Platform] API（を参照） [Experience Platformトラブルシューティングガイド](../landing/troubleshooting.md).

## 宛先に関する一般的な質問 {#general}

**Experience PlatformUI と書き出された CSV ファイルで異なるプロファイル数が表示されるのはなぜですか？**

これは、セグメント化の実行方法による通常のExperience Platform動作です。

ストリーミングセグメント化では、1 日を通じてストリーミングセグメントのプロファイル数が更新され、バッチセグメント化では、24 時間に 1 回、バッチセグメントのプロファイル数が更新されます。

セグメントの書き出しスケジュールがセグメント化スケジュールと異なる場合、プロファイルは UI と書き出した [!DNL CSV] ファイルは、特にストリーミングセグメントに関しては異なります。

詳しくは、 [セグメント化サービスのドキュメント](../segmentation/home.md) を参照してください。

## [!DNL Facebook Custom Audiences] {#facebook-faq}

**でオーディエンスをアクティブ化する前に必要なこと [!DNL Facebook Custom Audiences]?**

オーディエンスセグメントを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* お使いの [!DNL Facebook] ユーザーアカウントには **[!DNL Manage campaigns]** 使用する予定の広告アカウントに対して有効になっている権限です。
* この **Adobe Experience Cloud** ビジネスアカウントは、 [!DNL Facebook Ad Account]. `business ID=206617933627973`.を使用します。詳しくは、 [ビジネスマネージャにパートナーを追加する](https://www.facebook.com/business/help/1717412048538897) ( Facebookのドキュメント ) を参照してください。
   >[!IMPORTANT]
   >
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Experience Platform] 統合に必要です。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。

**アプリやピクセルを [!DNL Facebook] 広告主の口座？**

いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**facebookがAdobe Experience Platformからの情報を処理するのにどのくらい時間がかかりますか？**

2021 年 3 月現在 [!DNL Facebook Custom Audiences] から受け取った情報を処理するには、最大 1 時間必要です [!DNL Platform].

**使用可能な [!DNL Facebook Custom Audiences] （他のオーディエンスのターゲティング用） [!DNL Facebook] アプリ、など [!DNL Instagram]?**

以下を使用して、 [!DNL Facebook Custom Audiences] facebookファミリーのアプリをまたいでオーディエンスのターゲティングをおこなう宛先。 [!DNL Facebook Custom Audiences]を含む [!DNL Facebook], [!DNL Instagram], [!DNL Audience Network]、および [!DNL Messenger]. キャンペーンを実行するアプリの選択は、 [!DNL Facebook Ads Manager].

**違いは何ですか？ [!DNL Facebook Custom Audiences] 接続と [!DNL Facebook Pixel] 拡張？**

この [!DNL Facebook Custom Audiences] 接続を使用 [!DNL Platform] オーディエンスを次に送信する際の id [!DNL Facebook]、 [[!DNL Facebook Pixel] 接続](../destinations/catalog/advertising/facebook-pixel.md) は [!DNL Facebook] web サイトに統合されたピクセル。

これら 2 つの統合は補完的です。両方を使用して、オーディエンス対象範囲を改善することができます。例えば、 [!DNL Facebook Pixel] アカウントを作成していない見込み Web サイト訪問者を対象とする拡張 [!DNL Facebook Custom Audiences] は、 [!DNL Platform] id.

**Adobe Experience Platformと [!DNL Facebook Custom Audiences] 資格がなくなったユーザーのオーディエンスの不適格をサポートしますか？**

はい、統合では、 [!DNL Facebook Custom Audiences] 条件を満たさなくなったとき

**に送信する前にオーディエンスデータをハッシュ化する方法 [!DNL Facebook]?**

[!DNL Facebook] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL Facebook] キーオフできる *ハッシュ* 識別子（電子メールアドレスや電話番号など）。

ID 照合の要件について詳しくは、 [ID 一致要件](catalog/social/facebook.md#id-matching-requirements).

**でアクティブ化できる ID の種類 [!DNL Facebook Custom Audiences]?**

[!DNL Facebook Custom Audiences] は、次の ID のアクティブ化をサポートしています。ハッシュ化された電子メール、ハッシュ化された電話番号 [!DNL GAID], [!DNL IDFA]、およびカスタム外部 ID。

**Platform UI で、個別のFacebookアカウントに対して複数のFacebook宛先を作成できますか？**

facebookのExperience Platform先は、Facebookの広告アカウントに 1:1 です。 会社内のFacebook広告アカウントごとに別々のFacebookの宛先を作成できます。 フォロー： [宛先接続のチュートリアル](/help/destinations/ui/connect-destination.md) をクリックし、Platform UI の新しいFacebookの宛先ごとに個別のFacebookアカウントに接続します。

## linkedIn Matched Audiences {#linkedin}

**アプリやピクセルを [!DNL LinkedIn] 広告主の口座？**

いいえ。これはピクセルベースの統合ではないので、広告主アカウントにピクセルを追加する必要はありません。

**でオーディエンスをアクティブ化する前に必要なこと [!DNL LinkedIn Matched Audiences]?**

使用する前に [!UICONTROL linkedIn Matched Audience] 宛先、 [!DNL LinkedIn Campaign Manager] アカウントに [!DNL Creative Manager] 権限レベル以上。

[!DNL LinkedIn Campaign Manager] ユーザー権限の編集方法については、「[Add, Edit, and Remove User Permissions on Advertising Accounts](https://www.linkedin.com/help/lms/answer/5753)」（LinkedIn ドキュメント）を参照してください。

**に送信する前にオーディエンスデータをハッシュ化する方法 [!DNL LinkedIn]?**

[!DNL LinkedIn] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL LinkedIn] キーオフできる *ハッシュ* 識別子（電子メールアドレスや電話番号など）。

ID 照合の要件について詳しくは、 [ID 一致要件](catalog/social/linkedin.md#id-matching-requirements).

**でアクティブ化できる ID の種類 [!DNL LinkedIn]?**

[!DNL LinkedIn Matched Audiences] は、次の ID のアクティブ化をサポートしています。ハッシュ化された電子メール [!DNL GAID]、および [!DNL IDFA].

---
keywords: Google の顧客一致；Google の顧客一致；Google の顧客一致
title: Google カスタマーマッチ接続
description: Google カスタマーマッチを使用すると、オンラインおよびオフラインのデータを使用して、Google が所有し、運用する Search、Shopping、Gmail、YouTubeなどのプロパティをまたいで顧客にリーチし、再び関与することができます。
exl-id: 8209b5eb-b05c-4ef7-9fdc-22a528d5f020
source-git-commit: d0112cb26fcb85ad91ba403f81ee7f11d0889046
workflow-type: tm+mt
source-wordcount: '1557'
ht-degree: 1%

---

# [!DNL Google Customer Match] 接続

## 概要 {#overview}

[Google カスタマーマ](https://support.google.com/google-ads/answer/6379332?hl=en) ッチレットを使用すると、オンラインおよびオフラインのデータを使用して、Google が所有および運用する次のようなプロパティをまたいで顧客にリーチし、顧客と再び関わり合うことができます。 [!DNL Search]、  [!DNL Shopping]、  [!DNL Gmail]、および [!DNL YouTube]。

![Adobe Experience Platform UI での Google カスタマーマッチの宛先](../../assets/catalog/advertising/google-customer-match/catalog.png)

## ユースケース

[!DNL Google Customer Match] の宛先をいつどのように使用するかをより深く理解できるように、Adobe Experience Platformのお客様がこの機能を使用して解決できる使用例を以下に示します。

### 使用例#1

スポーツアパレルブランドは、[!DNL Google Search] と [!DNL Google Shopping] を通じて既存の顧客にリーチし、過去の購入や閲覧履歴に基づいてオファーや品目をパーソナライズしたいと考えています。 アパレルブランドは、独自の CRM からExperience Platformに電子メールアドレスを取り込み、独自のオフラインデータからセグメントを作成できます。 次に、これらのセグメントを [!DNL Google Customer Match] に送信して [!DNL Search] と [!DNL Shopping] で使用し、広告費用を最適化できます。

### 使用例#2

有名なテクノロジー企業が新しい電話を始めた。 この新しい電話モデルを推進するために、以前の電話モデルを所有するお客様に、電話の新機能の認識を促進しようとしています。

このリリースを促進するには、電子メールアドレスを識別子として使用して、CRM データベースからExperience Platformに電子メールアドレスをアップロードします。 セグメントは、古い電話モデルを所有する顧客に基づいて作成されます。 次に、セグメントが [!DNL Google Customer Match] に送信されるので、現在の顧客、古い電話モデルを所有している顧客、および [!DNL YouTube] 上で類似の顧客をターゲットにすることができます。

## [!DNL Google Customer Match] 宛先のデータガバナンス {#data-governance}

Experience Platform内の一部の宛先には、宛先プラットフォームに送信または宛先プラットフォームから受信するデータに関する特定のルールと義務があります。 お客様は、データの制限事項と義務、およびAdobe Experience Platformと宛先プラットフォームでのデータの使用方法について理解する必要があります。 Adobe Experience Platformは、これらのデータ使用上の義務の一部を管理するのに役立つデータガバナンスツールを提供します。 [データガバ](../../../data-governance/labels/overview.md) ナンスツールとポリシーについて詳しく説明します。

## サポートされる ID {#supported-identities}

[!DNL Google Customer Match] では、以下の表で説明する ID のアクティブ化をサポートしています。[ID](/help/identity-service/namespaces.md) の詳細をご覧ください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google 広告 ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| phone_sha256_e.164 | E164 形式の電話番号。SHA256 アルゴリズムでハッシュ化 | プレーンテキストと SHA256 ハッシュ化された電話番号の両方が、Adobe Experience Platformでサポートされています。 「[ID 一致要件 ](#id-matching-requirements-id-matching-requirements)」の手順に従い、プレーンテキストとハッシュ化された電話番号にそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform] |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「[ID 一致要件 ](#id-matching-requirements-id-matching-requirements)」の手順に従い、プレーンテキストとハッシュ化された電子メールアドレスにそれぞれ適切な名前空間を使用します。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform] |
| user_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  — セグメント（オーディエンス）のすべてのメンバーを、宛先で使用されている識別子（名前、電話番号など）と共に書き出 [!DNL Google Customer Match] します。

## [!DNL Google Customer Match] アカウントの前提条件 {#google-account-prerequisites}

Experience Platformで [!DNL Google Customer Match] の宛先を設定する前に、[Google サポートドキュメント ](https://support.google.com/google-ads/answer/6299717) で概要を説明している Google の [!DNL Customer Match] 使用に関するポリシーを読み、順守していることを確認してください。

次に、[!DNL Google] アカウントが [!DNL Standard] 以上の権限レベルに設定されていることを確認します。 詳しくは、[Google 広告のドキュメント ](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&amp;rd=1) を参照してください。

### 許可リスト {#allowlist}

Experience Platformで [!DNL Google Customer Match] の宛先を作成する前に、[!DNL Google Ads] アカウントが [Google カスタマーマッチポリシー ](https://support.google.com/google-ads/answer/6299717/customer-match-policy) に準拠していることを確認してください。

準拠しているアカウントを持つお客様は、Google によって自動的に許可リストに登録されます。

## ID の一致要件 {#id-matching-requirements}

[!DNL Google] では、個人を特定できる情報 (PII) を明確に送信しないことが必要です。したがって、[!DNL Google Customer Match] に対してアクティブ化されたオーディエンスは、電子メールアドレスや電話番号など、*ハッシュ化された* 識別子をキーオフにできます。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。

## 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

[!DNL Google Customer Match] で電話番号をアクティブにする方法は 2 つあります。

* **生の電話番号の取り込み**:形式の生の電話番号をに取り込むことがで [!DNL E.164] き、アクティ [!DNL Platform]ブ化時に自動的にハッシュ化されます。このオプションを選択する場合は、生の電話番号を必ず `Phone_E.164` 名前空間に取り込んでください。
* **ハッシュ化された電話番号の取り込み**:に取り込む前に電話番号を事前にハッシュ化できま [!DNL Platform]す。このオプションを選択する場合は、必ずハッシュ化された電話番号を `PHONE_SHA256_E.164` 名前空間に取り込んでください。

>[!NOTE]
>
>`Phone` 名前空間に取り込まれた電話番号は、[!DNL Google Customer Match] で有効化できません。

## 電子メールのハッシュ要件 {#hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platform内で明確に電子メールアドレスを使用し、アクティブ化時に [!DNL Platform] ハッシュ化したりできます。

Google のハッシュ要件およびアクティベーションに関するその他の制限について詳しくは、Google のドキュメントの次の節を参照してください。

* [[!DNL Customer Match] 電子メールアドレス、アドレスまたはユーザー ID を含む](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_email_address_address_or_user_id)
* [[!DNL Customer Match] 考慮事項](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_considerations)
* [電話番号との照合](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_phone_number)
* [モバイルデバイス ID との顧客一致](https://developers.google.com/adwords/api/docs/guides/remarketing#customer_match_with_mobile_device_ids)


Experience Platformでの電子メールアドレスの取り込みについて詳しくは、「[ バッチ取り込みの概要 ](../../../ingestion/batch-ingestion/overview.md)」および「[ ストリーミング取り込みの概要 ](../../../ingestion/streaming-ingestion/overview.md)」を参照してください。

電子メールアドレスを自分でハッシュ化する場合は、上記のリンクで概要を説明した Google の要件に従ってください。

## カスタム名前空間の使用 {#custom-namespaces}

`User_ID` 名前空間を使用して Google にデータを送信する前に、[!DNL gTag] を使用して独自の識別子を同期してください。 詳しくは、[Google 公式ドキュメント ](https://support.google.com/google-ads/answer/9199250) を参照してください。

<!-- Data from unhashed namespaces is automatically hashed by [!DNL Platform] upon activation.

Attribute source data is not automatically hashed. When your source field contains unhashed attributes, check the **[!UICONTROL Apply transformation]** option, to have [!DNL Platform] automatically hash the data on activation.
![Identity mapping transformation](../../assets/ui/activate-destinations/identity-mapping-transformation.png) -->

<!-- ## Configure destination - video walkthrough {#video}

The video below demonstrates the steps to configure a [!DNL Google Customer Match] destination and activate segments. The steps are also laid out sequentially in the next sections.

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng) -->

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先接続の名前を指定する
* **[!UICONTROL 説明]**:この宛先接続の説明を入力します
* **[!UICONTROL アカウント ID]**:Google 顧客クライアント ID。ID の形式は xxx-xxx-xxxx です。

>[!IMPORTANT]
>
> * **[!UICONTROL 「PII と結合]**」マーケティングアクションは、デフォルトで [!DNL Google Customer Match] の宛先に対して選択され、削除できません。


## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメントの書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

**[!UICONTROL セグメントスケジュール]** の手順で、[!DNL IDFA] または [!DNL GAID] セグメントを [!DNL Google Customer Match] に送信する際に、[!UICONTROL  アプリ ID] を指定する必要があります。

![Google カスタマーマッチアプリ ID](../../assets/catalog/advertising/google-customer-match/gcm-destination-appid.png)

[!DNL App ID] の見つけ方について詳しくは、[Google 公式ドキュメント ](https://developers.google.com/adwords/api/docs/reference/v201809/AdwordsUserListService.CrmBasedUserList#appid) を参照してください。

### マッピングの例：[!DNL Google Customer Match] でのオーディエンスデータのアクティブ化 {#example-gcm}

これは、[!DNL Google Customer Match] でオーディエンスデータをアクティブ化する際の正しい ID マッピングの例です。

ソースフィールドの選択：

* 使用している電子メールアドレスがハッシュ化されていない場合は、`Email` 名前空間をソース ID として選択します。
* [!DNL Google Customer Match] [ 電子メールハッシュ要件 ](#hashing-requirements) に従って、データの取り込み時に顧客の電子メールアドレスを [!DNL Platform] にハッシュ化した場合は、`Email_LC_SHA256` 名前空間をソース ID として選択します。
* データがハッシュ化されていない電話番号で構成されている場合は、`PHONE_E.164` 名前空間をソース ID として選択します。 [!DNL Platform] は、要件に従って電話番号をハッシュ化 [!DNL Google Customer Match] します。
* [!DNL Facebook] [ 電話番号のハッシュ要件 ](#phone-number-hashing-requirements) に従って [!DNL Platform] にデータを取り込む際に電話番号をハッシュ化した場合は、`Phone_SHA256_E.164` 名前空間をソース ID として選択します。
* データが [!DNL Apple] デバイス ID で構成されている場合は、`IDFA` 名前空間をソース ID として選択します。
* データが [!DNL Android] デバイス ID で構成されている場合は、`GAID` 名前空間をソース ID として選択します。
* データが他のタイプの識別子で構成されている場合は、`Custom` 名前空間をソース ID として選択します。

ターゲットフィールドの選択：

* ソース名前空間が `Email` または `Email_LC_SHA256` の場合、`Email_LC_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `PHONE_E.164` または `Phone_SHA256_E.164` の場合、`Phone_SHA256_E.164` 名前空間をターゲット ID として選択します。
* ソース名前空間が `IDFA` または `GAID` の場合、`IDFA` または `GAID` 名前空間をターゲット ID として選択します。
* ソース名前空間がカスタムの名前空間の場合は、`User_ID` 名前空間をターゲット ID として選択します。

![ID マッピング](../../assets/ui/activate-segment-streaming-destinations/identity-mapping-gcm.png)

ハッシュ化されていない名前空間のデータは、アクティベート時に [!DNL Platform] によって自動的にハッシュ化されます。

属性ソースのデータは自動的にハッシュ化されません。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform]

![ID マッピング変換](../../assets/ui/activate-segment-streaming-destinations/identity-mapping-gcm-transformation.png)

## セグメントのアクティベーションが成功したことを確認します。  {#verify-activation}

アクティベーションフローが完了したら、**[!UICONTROL Google Ads]** アカウントに切り替えます。 アクティブ化されたセグメントは、顧客リストとして Google アカウントに表示されます。 セグメントサイズによっては、提供するアクティブユーザーが 100 人を超えない限り、一部のオーディエンスは設定されないことに注意してください。

セグメントを [!DNL IDFA] と [!DNL GAID] の両方のモバイル ID にマッピングする場合、 [!DNL Google Customer Match] は ID マッピングごとに別のセグメントを作成します。 [!DNL Google Ads] アカウントには、[!DNL IDFA] と [!DNL GAID] マッピングの 2 つの異なるセグメントが表示されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定すると、次のエラーが発生する場合があります。

`{"message":"Google Customer Match Error: OperationAccessDenied.ACTION_NOT_PERMITTED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [ 前提条件 ](#google-account-prerequisites) を満たしていない場合に発生します。 この問題を解決するには、Google に問い合わせ、お使いのアカウントが許可リストに登録され、[!DNL Standard] 以上の権限レベルで設定されていることを確認してください。 詳しくは、[Google 広告のドキュメント ](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&amp;rd=1) を参照してください。

## その他のリソース {#additional-resources}

* [Google カスタマーマッチの統合 — ビデオチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/integrate-with-google-customer-match.html)


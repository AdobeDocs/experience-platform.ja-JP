---
keywords: Google カスタマーマッチ；Googleカスタマーマッチ；Googleカスタマーマッチ
title: Google Customer Match 接続
description: Google Customer Match を使用すると、Search、Shopping、Gmail、YouTubeなど、Googleが所有および運用するプロパティをまたいで、オンラインデータとオフラインデータを使用して顧客にリーチし、再び関与することができます。
exl-id: 8209b5eb-b05c-4ef7-9fdc-22a528d5f020
source-git-commit: d6b34f3bd3a432e1cf7d3dcce242934391b65d78
workflow-type: tm+mt
source-wordcount: '1763'
ht-degree: 27%

---

# [!DNL Google Customer Match] 接続

## 概要 {#overview}

[[!DNL Google Customer Match]](https://support.google.com/google-ads/answer/6379332?hl=en) では、オンラインデータとオフラインデータを使用して、Googleが所有および操作する次のようなプロパティをまたいで、顧客にリーチし、再び関わることができます。 [!DNL Search], [!DNL Shopping], [!DNL Gmail]、および [!DNL YouTube].

![Adobe Experience Platform UI でのGoogle Customer Match の宛先。](../../assets/catalog/advertising/google-customer-match/catalog.png)

## ユースケース {#use-cases}

を使用する方法とタイミングをより深く理解するために [!DNL Google Customer Match] の宛先について、Adobe Experience Platformのお客様がこの機能を使用して解決できる使用例を以下に示します。

### 使用例#1

スポーツアパレルブランドは、を通じて既存の顧客にリーチしたいと考えています [!DNL Google Search] および [!DNL Google Shopping] 過去の購入と閲覧履歴に基づいてオファーと品目をパーソナライズする。 アパレルブランドは、独自の CRM からExperience Platformに電子メールアドレスを取り込み、独自のオフラインデータからセグメントを作成できます。 その後、これらのセグメントを [!DNL Google Customer Match] 広く用いられる [!DNL Search] および [!DNL Shopping]広告費用を最適化しています。

### 使用例#2

有名なテクノロジー企業が新しい電話を始めました。 この新しい電話モデルを推進するため、電話の新機能を、以前のモデルの携帯電話を所有するお客様に知らせようとしています。

このリリースを促進するには、電子メールアドレスを識別子として使用して、CRM データベースからExperience Platformに電子メールアドレスをアップロードします。 セグメントは、古い電話モデルを所有している顧客に基づいて作成されます。 次に、セグメントがに送信されます。 [!DNL Google Customer Match]を使用することで、現在の顧客、古い電話モデルを所有している顧客および類似の顧客を [!DNL YouTube].

## のデータガバナンス [!DNL Google Customer Match] 宛先 {#data-governance}

Experience Platform内の一部の宛先には、宛先プラットフォームに送信または宛先プラットフォームから受信するデータに関する特定のルールと義務があります。 お客様は、データの制限事項と義務、およびAdobe Experience Platformと宛先プラットフォームでのそのデータの使用方法について理解する必要があります。 Adobe Experience Platformは、データ使用上の義務の一部を管理するのに役立つデータガバナンスツールを提供します。 [詳細情報](../../../data-governance/labels/overview.md) データガバナンスツールとポリシーについて

## サポートされている ID {#supported-identities}

[!DNL Google Customer Match] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| phone_sha256_e.164 | E164 形式の電話番号。SHA256 アルゴリズムでハッシュ化されています | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。「 [ID 一致要件](#id-matching-requirements-id-matching-requirements) セクションとでは、プレーンテキストとハッシュ化された電話番号に適した名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。「 [ID 一致要件](#id-matching-requirements-id-matching-requirements) を参照し、プレーンテキストとハッシュ化された電子メールアドレスに適切な名前空間をそれぞれ使用してください。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| user_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | セグメント（オーディエンス）のすべてのメンバーを、 [!DNL Google Customer Match] 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## [!DNL Google Customer Match] アカウントの前提条件 {#google-account-prerequisites}

を設定する前に [!DNL Google Customer Match] の宛先をExperience Platformして、Googleの使用ポリシーを読み、準拠していることを確認します。 [!DNL Customer Match]( [Googleサポートドキュメント](https://support.google.com/google-ads/answer/6299717).

次に、[!DNL Google] アカウントが [!DNL Standard] 以上の権限レベルに設定されていることを確認してください。詳しくは、[Google 広告のドキュメント](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&amp;rd=1)を参照してください。

### 許可リスト {#allowlist}

を作成する前に [!DNL Google Customer Match] 宛先をExperience Platformで指定する場合は、 [!DNL Google Ads] アカウントが次に準拠 [[!DNL Google Customer Match] ポリシー](https://support.google.com/google-ads/answer/6299717/customer-match-policy).

準拠しているアカウントを持つお客様は、Google によってリストへの掲載が自動的に許可されます。

## ID 一致要件 {#id-matching-requirements}

[!DNL Google] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL Google Customer Match] キーオフできる *ハッシュ* 識別子（電子メールアドレスや電話番号など）。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。

### 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

で電話番号を有効にする方法は 2 つあります [!DNL Google Customer Match]:

* **生の電話番号の取り込み**:生の電話番号を [!DNL E.164] 形式を変える [!DNL Platform]有効化時に自動的にハッシュ化されます。 このオプションを選択する場合は、生の電話番号を必ず `Phone_E.164` 名前空間。
* **ハッシュ化された電話番号の取り込み**:次に取り込む前に電話番号を事前にハッシュ化できます： [!DNL Platform]. このオプションを選択する場合は、常にハッシュ化された電話番号を `PHONE_SHA256_E.164` 名前空間。

>[!NOTE]
>
>に取り込まれる電話番号 `Phone` で名前空間を有効化できません [!DNL Google Customer Match].

### 電子メールのハッシュ要件 {#hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、電子メールアドレスをExperience Platformで明確に使用したり、 [!DNL Platform] 有効化時にハッシュ化します。

Googleのハッシュ要件とアクティベーションに関するその他の制限について詳しくは、Googleのドキュメントの次の節を参照してください。

* [[!DNL Customer Match] 電子メールアドレス、アドレスまたはユーザー ID を含む](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_email_address_address_or_user_id)
* [[!DNL Customer Match] 考慮事項](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_considerations)
* [[!DNL Customer Match] 電話番号](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_phone_number)
* [[!DNL Customer Match] モバイルデバイス ID を使用](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_mobile_device_ids)


E メールアドレスの取り込みについて詳しくは、Experience Platform [バッチ取得の概要](../../../ingestion/batch-ingestion/overview.md) そして [ストリーミング取得の概要](../../../ingestion/streaming-ingestion/overview.md).

電子メールアドレスを自分でハッシュ化する場合は、上記のリンクで概要を説明した、Googleの要件に従ってください。

### カスタム名前空間の使用 {#custom-namespaces}

使用する前に `User_ID` Googleにデータを送信するための名前空間で、 [!DNL gTag]. 詳しくは、 [Google公式ドキュメント](https://support.google.com/google-ads/answer/9199250) を参照してください。

<!-- Data from unhashed namespaces is automatically hashed by [!DNL Platform] upon activation.

Attribute source data is not automatically hashed. When your source field contains unhashed attributes, check the **[!UICONTROL Apply transformation]** option, to have [!DNL Platform] automatically hash the data on activation.
![Identity mapping transformation](../../assets/ui/activate-destinations/identity-mapping-transformation.png) -->

<!-- ## Configure destination - video walkthrough {#video}

The video below demonstrates the steps to configure a [!DNL Google Customer Match] destination and activate segments. The steps are also laid out sequentially in the next sections.

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng) -->

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先接続の名前を指定
* **[!UICONTROL 説明]**:この宛先接続の説明を入力
* **[!UICONTROL アカウント ID]**:あなたの [Google Ads 顧客 ID](https://support.google.com/google-ads/answer/1704344?hl=en). ID の形式は、xxx-xxx-xxxx です。 を使用している場合、 [!DNL Google Ads Manager Account (My Client Center)]の場合は、Manager アカウント ID を使用しないでください。 以下を使用： [Google Ads 顧客 ID](https://support.google.com/google-ads/answer/1704344?hl=en) 代わりに、

>[!IMPORTANT]
>
> * この **[!UICONTROL PII との組み合わせ]** マーケティングアクションは、デフォルトで [!DNL Google Customer Match] 宛先および削除できません。


### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティブ化する手順は、[ストリーミングセグメント書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

内 **[!UICONTROL セグメントスケジュール]** 手順に従って、 [!UICONTROL アプリ ID] 送信時 [!DNL IDFA] または [!DNL GAID] セグメントを [!DNL Google Customer Match].

![Google Customer Match App ID](../../assets/catalog/advertising/google-customer-match/gcm-destination-appid.png)

の検索方法の詳細 [!DNL App ID]（を参照） [Google公式ドキュメント](https://developers.google.com/adwords/api/docs/reference/v201809/AdwordsUserListService.CrmBasedUserList#appid).

### マッピングの例：オーディエンスデータのアクティブ化 [!DNL Google Customer Match] {#example-gcm}

これは、 [!DNL Google Customer Match].

ソースフィールドを選択しています。

* を選択します。 `Email` 使用している電子メールアドレスがハッシュ化されていない場合、名前空間をソース ID として使用する。
* を選択します。 `Email_LC_SHA256` データ取り込み時に顧客の電子メールアドレスをハッシュ化した場合、名前空間をソース id として [!DNL Platform]( [!DNL Google Customer Match] [電子メールハッシュ要件](#hashing-requirements).
* を選択します。 `PHONE_E.164` データがハッシュ化されていない電話番号で構成されている場合、名前空間をソース ID にします。 [!DNL Platform] に準拠するために電話番号をハッシュ化します [!DNL Google Customer Match] 要件
* を選択します。 `Phone_SHA256_E.164` データの取り込み時に電話番号をハッシュ化した場合は、ソース ID として名前空間を使用 [!DNL Platform]( [!DNL Facebook] [電話番号のハッシュ要件](#phone-number-hashing-requirements).
* を選択します。 `IDFA` データが次のもので構成される場合は、ソース ID としての名前空間： [!DNL Apple] デバイス ID。
* を選択します。 `GAID` データが次のもので構成される場合は、ソース ID としての名前空間： [!DNL Android] デバイス ID。
* を選択します。 `Custom` データが他のタイプの識別子で構成されている場合は、名前空間をソース id として指定します。

ターゲットフィールドの選択：

* を選択します。 `Email_LC_SHA256` ソース名前空間が次のいずれかの場合は、ターゲット ID としての名前空間 `Email` または `Email_LC_SHA256`.
* を選択します。 `Phone_SHA256_E.164` ソース名前空間が次のいずれかの場合は、ターゲット ID としての名前空間 `PHONE_E.164` または `Phone_SHA256_E.164`.
* を選択します。 `IDFA` または `GAID` ソース名前空間が使用されている場合のターゲット ID としての名前空間 `IDFA` または `GAID`.
* を選択します。 `User_ID` ソース名前空間がカスタムの名前空間の場合は、ターゲット id として名前空間を設定します。

![ID マッピング](../../assets/ui/activate-segment-streaming-destinations/identity-mapping-gcm.png)

ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます。 [!DNL Platform] 有効化時。

属性ソースデータは自動的にハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。

![ID マッピング変換](../../assets/ui/activate-segment-streaming-destinations/identity-mapping-gcm-transformation.png)

## セグメントのアクティベーションが成功したことを確認します。  {#verify-activation}

アクティベーションフローが完了したら、 **[!UICONTROL Google Ads]** アカウント アクティブ化されたセグメントは、顧客リストとしてGoogleアカウントに表示されます。 なお、一部のオーディエンスは、セグメントサイズに応じて、提供するアクティブユーザーが 100 人を超えない限り、設定されません。

セグメントを [!DNL IDFA] および [!DNL GAID] モバイル ID [!DNL Google Customer Match] は ID マッピングごとに個別のセグメントを作成します。 お使いの [!DNL Google Ads] アカウントには 2 つの異なるセグメントが表示され、1 つは [!DNL IDFA]で、1 つは [!DNL GAID] マッピング。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Customer Match Error: OperationAccessDenied.ACTION_NOT_PERMITTED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [前提条件](#google-account-prerequisites). この問題を修正するには、Googleに連絡し、お使いのアカウントが許可リストに登録され、 [!DNL Standard] 以上の権限レベル。 詳しくは、[Google 広告のドキュメント](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&amp;rd=1)を参照してください。

## その他のリソース {#additional-resources}

* [統合 [!DNL Google Customer Match]  — ビデオチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/tutorials/rtcdp/integrate-with-google-customer-match.html?lang=ja)


---
keywords: google カスタマーマッチ；Google カスタマーマッチ；Google カスタマーマッチ
title: Google Customer Match 接続
description: Google カスタマーマッチを使用すると、オンラインおよびオフラインのデータを使用して、検索、ショッピング、Gmail など、Googleが所有および運営するプロパティをまたいで顧客にリーチし、再びエンゲージできます。
exl-id: 8209b5eb-b05c-4ef7-9fdc-22a528d5f020
source-git-commit: a119418e8da7594a99116b4de65f60fdaa95ba8e
workflow-type: tm+mt
source-wordcount: '2719'
ht-degree: 11%

---

# [!DNL Google Customer Match] 接続

>[!IMPORTANT]
>
> Googleは、欧州連合（EU）の [&#x200B; デジタル市場法 &#x200B;](https://developers.google.com/google-ads/api/docs/start) （DMA[）（](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)EU ユーザー同意ポリシー [）で定義されているコンプライアンスおよび同意関連の要件をサポートするために、](https://developers.google.com/display-video/api/guides/getting-started/overview)Google Ads API[、](https://digital-markets-act.ec.europa.eu/index_en)Customer Match および [Display &amp; Video 360 API](https://www.google.com/about/company/user-consent-policy/) に対する変更内容をリリースしています。 同意要件に対するこれらの変更の適用は 2024 年 3 月 6 日（PT）から開始されます。
><br/>
>EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーであるAdobeは、欧州連合の DMA に基づく同意要件に準拠するために必要なツールを提供します。
><br/>
>Adobe Privacy &amp; Security Shield を購入し、同意のないプロファイルを除外する [&#x200B; 同意ポリシー &#x200B;](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を設定している場合は、何もする必要はありません。
><br/>
>Adobe Privacy &amp; Security Shield を購入されていないお客様が既存のReal-Time CDP Googleの宛先を引き続き中断することなく使用するには、[&#x200B; セグメントビルダー &#x200B;](../../../segmentation/home.md#segment-definitions) 内の [&#x200B; セグメント定義 &#x200B;](../../../segmentation/ui/segment-builder.md) 機能を使用して、同意のないプロファイルを除外する必要があります。

[[!DNL Google Customer Match]](https://support.google.com/google-ads/answer/6379332?hl=en) を使用すると、Googleが所有および運営するプロパティ（[!DNL Search]、[!DNL Shopping]、[!DNL Gmail] など）をまたいでオンラインおよびオフラインのデータを使用して、顧客にリーチし、再びエンゲージできます。

>[!TIP]
>
>[!DNL YouTube] の在庫のお客様にリーチするには、[Google Customer Match + DV360](/help/destinations/catalog/advertising/google-customer-match-dv360.md) 宛先を使用します。この宛先には、Google Audience Partner API を使用します。

![Adobe Experience Platform UI のGoogle カスタマーマッチの宛先。](../../assets/catalog/advertising/google-customer-match/catalog.png)

## ユースケース {#use-cases}

[!DNL Google Customer Match] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの機能を使用して解決できるサンプルユースケースを以下に示します。

### のユースケース#1

アスレチックアパレルブランドは、過去の購入と閲覧履歴に基づいてオファーとアイテムをパーソナライズするために、[!DNL Google Search] ールと [!DNL Google Shopping] ールを通じて既存の顧客にリーチしたいと考えています。 アパレルブランドは、メールアドレスを独自の CRM からExperience Platformに取り込み、独自のオフラインデータからオーディエンスを作成できます。 その後、これらのオーディエンスを [!DNL Google Customer Match] に送信し、[!DNL Search] と [!DNL Shopping] で使用することで、広告費用を最適化できます。

### のユースケース#2

>[!TIP]
>
>このユースケースを [!DNL YouTube] インベントリで実行するには、新しい [Google Customer Match + DV360](/help/destinations/catalog/advertising/google-customer-match-dv360.md) 宛先を使用します。この宛先には、Google Audience Partner API が使用されます。

ある有名なテクノロジー会社が新しい電話を発売しました。 この新しい電話モデルを宣伝するために、彼らは電話の新機能と機能を彼らの電話の以前のモデルを所有しているお客様に認識を高めることを目指しています。

リリースを促進するために、メールアドレスを識別子として使用して、CRM データベースからExperience Platformにメールアドレスをアップロードします。 オーディエンスは、古い電話モデルを所有する顧客に基づいて作成されます。 その後、オーディエンスが [!DNL Google Customer Match] に送信されるので、会社は現在の顧客、古い電話モデルを所有している顧客および同様の顧客を [!DNL YouTube] でターゲットにすることができます。

## [!DNL Google Customer Match] 宛先のデータガバナンス {#data-governance}

Experience Platformの一部の宛先には、宛先プラットフォームに送信される、または宛先プラットフォームから受信するデータに対する、特定のルールと義務があります。 お客様は、お客様のデータの制限事項や義務を理解し、Adobe Experience Platformおよび宛先プラットフォームでのデータの使用方法を理解する責任を負います。 Adobe Experience Platformには、これらのデータ使用義務の一部を管理するのに役立つデータガバナンスツールが用意されています。 データガバナンスツールとポリシーについて [&#x200B; 詳細情報 &#x200B;](../../../data-governance/labels/overview.md) します。

## サポートされている ID {#supported-identities}

[!DNL Google Customer Match] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `GAID` | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、このターゲット ID を選択します。 |
| `IDFA` | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、このターゲット ID を選択します。 |
| `phone_sha256_e.164` | SHA256 アルゴリズムでハッシュ化された E164 形式の電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。[ID の一致要件 &#x200B;](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストには適切な名前空間を、ハッシュ化された電話番号には適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| `email_lc_sha256` | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。[ID の一致要件 &#x200B;](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストには適切な名前空間を、ハッシュ化されたメールアドレスには適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| `user_id` | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |
| `address_info_first_name` | ユーザーの名 | このターゲット ID は、宛先に郵送先住所のデータを送信する際に、`address_info_last_name`、`address_info_country_code`、`address_info_postal_code` と共に使用することを目的としています。 <br><br>Googleがアドレスと一致するようにするには、4 つのアドレスフィールド（`address_info_first_name`、`address_info_last_name`、`address_info_country_code` および `address_info_postal_code`）をすべてマッピングし、書き出されたプロファイルでこれらのフィールドに欠けているデータがないことを確認する必要があります。 <br> いずれかのフィールドがマッピングされていないか、データが欠落している場合、Googleはアドレスと一致しません。 |
| `address_info_last_name` | ユーザーの姓 | このターゲット ID は、宛先に郵送先住所のデータを送信する際に、`address_info_first_name`、`address_info_country_code`、`address_info_postal_code` と共に使用することを目的としています。 <br><br>Googleがアドレスと一致するようにするには、4 つのアドレスフィールド（`address_info_first_name`、`address_info_last_name`、`address_info_country_code` および `address_info_postal_code`）をすべてマッピングし、書き出されたプロファイルでこれらのフィールドに欠けているデータがないことを確認する必要があります。 <br> いずれかのフィールドがマッピングされていないか、データが欠落している場合、Googleはアドレスと一致しません。 |
| `address_info_country_code` | ユーザーの住所の国コード | このターゲット ID は、宛先に郵送先住所のデータを送信する際に、`address_info_first_name`、`address_info_last_name`、`address_info_postal_code` と共に使用することを目的としています。 <br><br>Googleがアドレスと一致するようにするには、4 つのアドレスフィールド（`address_info_first_name`、`address_info_last_name`、`address_info_country_code` および `address_info_postal_code`）をすべてマッピングし、書き出されたプロファイルでこれらのフィールドに欠けているデータがないことを確認する必要があります。 <br> いずれかのフィールドがマッピングされていないか、データが欠落している場合、Googleはアドレスと一致しません。 <br><br> 使用可能な形式：小文字、[ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 形式の 2 文字の国コード。 |
| `address_info_postal_code` | ユーザーの住所の郵便番号 | このターゲット ID は、宛先に郵送先住所のデータを送信する際に、`address_info_first_name`、`address_info_last_name`、`address_info_country_code` と共に使用することを目的としています。 <br><br>Googleがアドレスと一致するようにするには、4 つのアドレスフィールド（`address_info_first_name`、`address_info_last_name`、`address_info_country_code` および `address_info_postal_code`）をすべてマッピングし、書き出されたプロファイルでこれらのフィールドに欠けているデータがないことを確認する必要があります。 <br> いずれかのフィールドがマッピングされていないか、データが欠落している場合、Googleはアドレスと一致しません。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディ [!DNL Google Customer Match] ンスの宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## [!DNL Google Customer Match] アカウントの前提条件 {#google-account-prerequisites}

Experience Platformで [!DNL Google Customer Match] の宛先を設定する前に、[!DNL Customer Match]Googleのサポートドキュメント [&#x200B; に概説されている &#x200B;](https://support.google.com/google-ads/answer/6299717) の使用に関するGoogleのポリシーを読んで遵守してください。

次に、[!DNL Google] アカウントが [!DNL Standard] 以上の権限レベルに設定されていることを確認してください。 詳しくは、[Google広告のドキュメント &#x200B;](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&rd=1) を参照してください。

### 許可リスト {#allowlist}

Experience Platformで [!DNL Google Customer Match] の宛先を作成する前に、[!DNL Google Ads] アカウントが [[!DNL Google Customer Match] policy](https://support.google.com/google-ads/answer/6299717/customer-match-policy) に準拠していることを確認してください。

準拠しているアカウントを持つお客様は、Googleによって自動的に許可リストに加えるされます。

## ID の一致要件 {#id-matching-requirements}

[!DNL Google] では、個人を特定できる情報（PII）が明確に送信されないことが必要です。 したがって、[!DNL Google Customer Match] に対してアクティブ化されたオーディエンスは、メールアドレスや電話番号などの識別子を *ハッシュ化* してオフにすることができます。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。

### 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

[!DNL Google Customer Match] で電話番号をアクティブ化する方法は 2 つあります。

* **生の電話番号の取り込み**:[!DNL E.164] 形式の生の電話番号を [!DNL Experience Platform] に取り込むことができ、アクティベーション時に自動的にハッシュ化されます。 このオプションを選択する場合は、常に生の電話番号を `Phone_E.164` 名前空間に取り込んでください。
* **ハッシュ化された電話番号の取り込み**:[!DNL Experience Platform] に取り込む前に、電話番号を事前にハッシュ化できます。 このオプションを選択した場合は、ハッシュ化された電話番号を常に `PHONE_SHA256_E.164` 名前空間に取り込んでください。

>[!NOTE]
>
>`Phone` 名前空間に取り込まれた電話番号は、[!DNL Google Customer Match] でアクティブ化できません。

### メールハッシュ要件 {#hashing-requirements}

メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platformでメールアドレスを明確に使用して、アクティベーション時 [!DNL Experience Platform] ハッシュ化したりできます。

Googleのハッシュ要件およびその他のアクティベーションに関する制限事項について詳しくは、Googleのドキュメントの次の節を参照してください。

* [[!DNL Customer Match]  メールアドレス、アドレスまたはユーザー ID を使用 &#x200B;](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_email_address_address_or_user_id)
* [[!DNL Customer Match]  考慮事項 &#x200B;](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_considerations)
* [[!DNL Customer Match]  電話番号を使用 &#x200B;](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_phone_number)
* [[!DNL Customer Match]  モバイルデバイス ID](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_mobile_device_ids)


Experience Platformでのメールアドレスの取り込みについて詳しくは、[&#x200B; バッチ取り込みの概要 &#x200B;](../../../ingestion/batch-ingestion/overview.md) および [&#x200B; ストリーミング取り込みの概要 &#x200B;](../../../ingestion/streaming-ingestion/overview.md) を参照してください。

メールアドレスを自分でハッシュ化する場合は、上記のリンクで概説されているGoogleの要件に必ず準拠してください。

### フィールドハッシュ要件への対応 {#address-field-hashing}

アドレス関連のフィールドを [!DNL Google Customer Match] にマッピングする場合、Experience Platformは **と** の値を `address_info_first_name` 自動ハッシュ化 `address_info_last_name` してからGoogleに送信します。 この自動ハッシュは、Googleのセキュリティおよびプライバシー要件に準拠するために必要です。

**または** に対して `address_info_first_name` 事前にハッシュされた値を提供しない `address_info_last_name` ください。 既にハッシュ化された値を指定すると、一致させるプロセスが失敗します。

### カスタム名前空間の使用 {#custom-namespaces}

`User_ID` 名前空間を使用してGoogleにデータを送信する前に、[!DNL gTag] を使用して独自の識別情報を同期させる必要があります。 詳しくは、[Google公式ドキュメント &#x200B;](https://support.google.com/google-ads/answer/9199250) を参照してください。

<!-- Data from unhashed namespaces is automatically hashed by [!DNL Experience Platform] upon activation.

Attribute source data is not automatically hashed. When your source field contains unhashed attributes, check the **[!UICONTROL Apply transformation]** option, to have [!DNL Experience Platform] automatically hash the data on activation.
![Identity mapping transformation](../../assets/ui/activate-destinations/identity-mapping-transformation.png) -->

<!-- ## Configure destination - video walkthrough {#video}

The video below demonstrates the steps to configure a [!DNL Google Customer Match] destination and activate audiences. The steps are also laid out sequentially in the next sections.

>[!VIDEO](https://video.tv.adobe.com/v/3411787/?quality=12&learn=on&captions=jpn) -->

## ビデオの概要 {#video-overview}

メリットとGoogle カスタマーマッチへのデータのアクティベート方法について詳しくは、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/326487?captions=jpn)

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：この宛先接続の名前を指定します
* **[!UICONTROL Description]**：この宛先接続の説明を入力
* **[!UICONTROL Account ID]**: [Google Ads 顧客 ID](https://support.google.com/google-ads/answer/1704344?hl=en)。 ID の形式は xxx-xxx-xxxx です。 [!DNL Google Ads Manager Account (My Client Center)] を使用している場合は、マネージャーアカウント ID を使用しないでください。 代わりに、[Google Ads 顧客 ID](https://support.google.com/google-ads/answer/1704344?hl=en) を使用します。

>[!NOTE]
>
>OAuth2 接続プロセス中に、Google OAuth プロジェクト名として「Marketo テスト」が表示される場合があります。 AdobeはGoogle Customer Match の統合にこのプロジェクト名を使用するので、これは通常の動作です。 これは、宛先設定には影響しません。

>[!IMPORTANT]
>
> * **[!UICONTROL Combine with PII]** の宛先に対して、[!DNL Google Customer Match] マーケティングアクションがデフォルトで選択されており、削除できません。

### 認証と権限 {#authentication-permissions}

Google Ads アカウントを接続すると、Adobeのアプリケーションへのアクセス権を付与するようGoogleから求められます。 Adobeがカスタマーリストを作成および管理できるように、Google Ads API 権限を承認する必要があります。 アクティベーションを行う対象のカスタマーアカウントで、Standard 以上のアクセス権を持つGoogle Ads ユーザーを使用します。 管理者アカウント（MCC）を使用している場合は、顧客アカウントのユーザーでログインし、（MCC ID ではなく）顧客アカウント ID を指定します。

OAuth フローの実行中にGoogle Ads 権限が付与されない場合、アクティベーションは、後でGoogle Ads API からのエラーで失敗する場合があります。 権限関連のエラーを解決する方法について詳しくは、[&#x200B; トラブルシューティングの節 &#x200B;](#troubleshooting) を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 宛先に *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

**[!UICONTROL Segment schedule]** の手順では、[!UICONTROL App ID] または [!DNL IDFA] オーディエンスを [!DNL GAID] に送信する際に、[!DNL Google Customer Match] を指定する必要があります。

![&#x200B; アクティベーションワークフローのセグメントスケジュール手順でハイライト表示された「Google顧客一致アプリ ID」フィールド &#x200B;](../../assets/catalog/advertising/google-customer-match/gcm-destination-appid.png)

[!DNL App ID] の検索方法について詳しくは、[Googleの公式ドキュメントを参照するか &#x200B;](https://developers.google.com/adwords/api/docs/reference/v201809/AdwordsUserListService.CrmBasedUserList#appid)Google担当者にお問い合わせください。

### マッピング例：[!DNL Google Customer Match] でのオーディエンスデータのアクティブ化 {#example-gcm}

これは、[!DNL Google Customer Match] でオーディエンスデータをアクティブ化する際の、正しい ID マッピングの例です。

ソースフィールドを選択中：

* 使用しているメールアドレスがハッシュ化されていない場合は、`Email` 名前空間をソース ID として選択します。
* `Email_LC_SHA256` の [!DNL Experience Platform] メールハッシュ要件 [!DNL Google Customer Match] に従って、[&#x200B; へのデータ取り込み時に顧客のメールアドレスをハッシュ化した場合は、](#hashing-requirements) 名前空間をソース ID として選択します。
* ハッシュ化されていない電話番号がデータに含まれている場合は、`PHONE_E.164` 名前空間をソース ID として選択します。 [!DNL Experience Platform] は、[!DNL Google Customer Match] の要件に準拠するために電話番号をハッシュ化します。
* `Phone_SHA256_E.164`[!DNL Experience Platform] 電話番号ハッシュ要件 [!DNL Google Customer Match] に従って、[&#x200B; へのデータ取り込みで電話番号をハッシュ化した場合は、](#phone-number-hashing-requirements) 名前空間をソース ID として選択します。
* データがデバイス ID で構成されている場合は、ソース ID として `IDFA` 名前空間 [!DNL Apple] 選択します。
* データがデバイス ID で構成されている場合は、ソース ID として `GAID` 名前空間 [!DNL Android] 選択します。
* データが他のタイプの識別子で構成されている場合は、ソース ID として `Custom` 名前空間を選択します。

ターゲットフィールドを選択：

* ソース名前空間が `Email_LC_SHA256` または `Email` の場合は、`Email_LC_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `Phone_SHA256_E.164` または `PHONE_E.164` の場合は、`Phone_SHA256_E.164` 名前空間をターゲット ID として選択します。
* ソース名前空間が `IDFA` または `GAID` の場合は、`IDFA` または `GAID` 名前空間をターゲット ID として選択します。
* ソース名前空間がカスタム名前空間の場合は、`User_ID` 名前空間をターゲット ID として選択します。

![&#x200B; アクティベーションワークフローのマッピング手順に表示される、ソースフィールドとターゲットフィールドの ID マッピング。](../../assets/ui/activate-segment-streaming-destinations/identity-mapping-gcm.png)

ハッシュ化されていない名前空間のデータは、アクティベーション時に [!DNL Experience Platform] によって自動的にハッシュ化されます。

属性ソースデータは、自動的にはハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。

![&#x200B; アクティベーションワークフローのマッピング手順でハイライト表示されている変換コントロールを適用 &#x200B;](../../assets/ui/activate-segment-streaming-destinations/identity-mapping-gcm-transformation.png)

## 宛先の監視 {#monitor-destination}

宛先に接続し、宛先データフローを確立したら、Real-Time CDPの [&#x200B; モニタリング機能 &#x200B;](/help/dataflows/ui/monitor-destinations.md) を使用して、各データフロー実行で宛先に対してアクティブ化されたプロファイルレコードに関する詳細な情報を取得できます。

>[!IMPORTANT]
>
>4 つのアドレス関連のターゲット ID （`address_info_first_name`、`address_info_last_name`、`address_info_country_code` および `address_info_postal_code`）をマッピングすると、それらは、データフロー監視ページのプロファイルごとに別々の個別の ID としてカウントされます。

## Audience Activation が成功したことの確認 {#verify-activation}

アクティベーションフローが完了したら、**[!UICONTROL Google Ads]** アカウントに切り替えます。 アクティブ化されたオーディエンスは、Google アカウントに顧客リストとして表示されます。 オーディエンスサイズによっては、提供するアクティブユーザーが 100 人以上でない限り、一部のオーディエンスが入力されない場合があります。

オーディエンスを [!DNL IDFA] と [!DNL GAID] の両方のモバイル ID にマッピングす [!DNL Google Customer Match] 場合、ID マッピングごとに個別のオーディエンスが作成されます。 [!DNL Google Ads] アカウントには 2 つの異なるセグメントが表示されます。1 つは [!DNL IDFA] 用、もう 1 つは [!DNL GAID] マッピング用です。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Customer Match Error: OperationAccessDenied.ACTION_NOT_PERMITTED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [&#x200B; 前提条件 &#x200B;](#google-account-prerequisites) に準拠していない場合に発生します。 この問題を修正するには、Googleに問い合わせて、自分のアカウントが許可リストに登録されており、[!DNL Standard] 以上の権限レベルに設定されていることを確認してください。 詳しくは、[Google広告のドキュメント &#x200B;](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&rd=1) を参照してください。

### 500 内部サーバーエラー – 認証範囲が不十分です {#insufficient-scopes}

この宛先に対してオーディエンスをアクティブ化すると、次のエラーが発生する場合があります。

`{"message":"com.google.api.gax.rpc.PermissionDeniedException: io.grpc.StatusRuntimeException: PERMISSION_DENIED: Request had insufficient authentication scopes.","code":"500 INTERNAL_SERVER_ERROR"}`

このエラーは、この宛先接続に使用されるGoogle OAuth トークンが必須のGoogle Ads API スコープなしで作成された場合、またはログインユーザーが対象のカスタマーアカウントに対して十分な権限を持っていない場合に発生します。

この問題を修正するには、次の手順に従います。

1. この宛先アカウントの **Google認証を再生成** し、リクエストされたGoogle Ads 権限に同意していることを確認します。
   * Experience Platformで、**[!UICONTROL Destinations]**/**[!UICONTROL Accounts]** に移動します。
   * Google カスタマーマッチアカウントを見つける
   * **[!UICONTROL More actions]** （⋯） > **[!UICONTROL Edit]** > **[!UICONTROL Renew]** を選択します。
   * Googleのログインと同意のフローを完了し、リクエストされたすべての権限を承認する
2. 管理者アカウント（MCC）を使用して広告を管理する場合は、ターゲット顧客アカウントに対するアクセス権が [!DNL Standard] 以上のユーザーで認証を行っていることと、宛先に設定されている **[!UICONTROL Account ID]** が（MCC ID ではなく）顧客アカウント ID であることを確認します。
3. アクティベーションを再実行します。

問題が解決しない場合：

* Google Ads アカウントがカスタマーマッチ用に許可リストに加えるされ、[&#x200B; ポリシー要件 &#x200B;](#google-account-prerequisites) を満たしていることを確認します。
* ユーザーのアクセスレベルがGoogle Ads 顧客アカウントで [!DNL Standard] 以上であることを確認します。 詳しくは、[Google広告のドキュメント &#x200B;](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&rd=1) を参照してください。
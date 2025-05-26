---
title: Google Customer Match + Display & Video 360 connection
description: Google Customer Match + Display & Video 360 宛先コネクタを使用すると、Experience Platformのオンラインおよびオフラインデータを使用して、検索、ショッピング、Gmail、YouTubeなど、Googleが所有および運営するプロパティをまたいで顧客にリーチし、再びエンゲージできます。
badgeBeta: label="ベータ版" type="Informative"
exl-id: f6da3eae-bf3f-401a-99a1-2cca9a9058d2
source-git-commit: 08a880bac8e06627ae59ef036877791f8771c87a
workflow-type: tm+mt
source-wordcount: '2032'
ht-degree: 18%

---

# [!DNL Google Customer Match + Display & Video 360] 接続

この宛先を使用して、ファーストパーティ PII ベースの [[!DNL Google Customer Match]](https://support.google.com/google-ads/answer/6379332?hl=en) リストをアクティブ化し、[!DNL Search]、[!DNL YouTube]、[!DNL Gmail]、[!DNL Google Display Network] などの [!DNL Google Display & Video 360] プロパティに直接アクティブ化します。

Adobe Real-Time CDPなど、Googleと統合された特定のサードパーティでは、[!DNL Google Audience Partner API] を使用して、顧客の [!DNL Display & Video 360] アカウントに [!DNL Customer Match] オーディエンスを直接作成できます。

[!DNL Display & Video 360] 全体で [!DNL Customer Matched] オーディエンスを利用できる新しく導入された機能により、拡張されたインベントリソース全体でオーディエンスをターゲットに設定できるようになりました。

>[!IMPORTANT]
>
>この宛先コネクタはベータ版で、一部のお客様のみご利用いただけます。 アクセス権をリクエストするには、Adobe担当者にお問い合わせください。

![Adobe Experience Platform UI のGoogle カスタマーマッチ + DV360 の宛先。](/help/destinations/assets/catalog/advertising/gcm-dv360/catalog.png)

## 欧州連合（EU）での同意要件の更新に関連するGoogleの宛先の変更に関する重要な通知

>[!IMPORTANT]
>
> Googleは、欧州連合（EU）の [ デジタル市場法 ](https://developers.google.com/google-ads/api/docs/start) （DMA](https://digital-markets-act.ec.europa.eu/index_en)）（[EU ユーザー同意ポリシー ](https://www.google.com/about/company/user-consent-policy/)）で定義されているコンプライアンスおよび同意関連の要件をサポートするために、[Google Ads API](https://ads-developers.googleblog.com/2023/10/updates-to-customer-match-conversion.html)、[Customer Match および [Display &amp; Video 360 API](https://developers.google.com/display-video/api/guides/getting-started/overview) に対する変更内容をリリースしています。 同意要件に対するこれらの変更の適用は 2024 年 3 月 6 日（PT）から開始されます。
><br/>
>EU のユーザー同意ポリシーに準拠し、欧州経済領域（EEA）のユーザーに対するオーディエンスリストの作成を続行するには、広告主およびパートナーは、オーディエンスデータをアップロードする際にエンドユーザーの同意を渡していることを確認する必要があります。 Google パートナーであるAdobeは、欧州連合の DMA に基づく同意要件に準拠するために必要なツールを提供します。
><br/>
>Adobe Privacy &amp; Security Shield を購入し、同意のないプロファイルを除外する [ 同意ポリシー ](../../../data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を設定している場合は、何もする必要はありません。
><br/>
>Adobe Privacy &amp; Security Shield を購入されていないお客様が既存のReal-Time CDP Googleの宛先を引き続き中断することなく使用するには、[ セグメントビルダー ](../../../segmentation/home.md#segment-definitions) 内の [ セグメント定義 ](../../../segmentation/ui/segment-builder.md) 機能を使用して、同意のないプロファイルを除外する必要があります。

## この宛先を使用するタイミング

宛先カタログではGoogleとのいくつかの統合機能を利用できます。使用可能な各Google宛先を使用するタイミングを理解するのは難しい場合があります。 次の表の情報を読んで、様々なユースケースを理解します。

| [Google カスタマーマッチ](/help/destinations/catalog/advertising/google-customer-match.md) | [Google Display と Video 360](/help/destinations/catalog/advertising/google-dv360.md) | [!DNL Google Customer Match] + [!DNL Display & Video 360] （このコネクタ） |
|---------|----------|---------|
| PII ベースのオーディエンスを書き出し、[!DNL Google Customer Match] で使用可能な在庫で到達します。 | [!DNL Google Display & Video 360] を介して利用可能なインベントリ、Youtube や [!DNL Search] などのGoogleが所有および運営するプロパティで利用可能なインベントリ、およびそれ以降のインベントリにわたって、cookie ベースのオーディエンスにリーチします。 | [!DNL Google Customer Match] で PII ベースのオーディエンスを作成し、Googleが所有および運営するプロパティでのみ、[!DNL Google Display & Video 360] で使用可能なインベントリで到達させます。 |

## ユースケース {#use-cases}

この宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの機能を使用して解決できるユースケースのサンプルを以下に示します。

### のユースケース#1

アスレチックアパレルブランドは、過去の購入と閲覧履歴に基づいてオファーとアイテムをパーソナライズするために、[!DNL Google Search] ールと [!DNL Google Shopping] ールを通じて既存の顧客にリーチしたいと考えています。 アパレルブランドは、メールアドレスを独自の CRM からExperience Platformに取り込み、独自のオフラインデータからオーディエンスを作成できます。 その後、これらのオーディエンスを [!DNL Google Customer Match + Display & Video 360] の宛先に送信し、[!DNL Search]、[!DNL YouTube]、[!DNL Gmail]、[!DNL Google Display Network] などの [!DNL Google Display & Video 360] プロパティで使用できます。

### のユースケース#2

ある有名なテクノロジー会社が新しい電話を発売しました。 この新しい電話モデルを宣伝するために、彼らは電話の新機能と機能を彼らの電話の以前のモデルを所有しているお客様に認識を高めることを目指しています。

リリースを促進するために、メールアドレスを識別子として使用して、CRM データベースからExperience Platformにメールアドレスをアップロードします。 オーディエンスは、古い電話モデルを所有する顧客に基づいて作成されます。 その後、オーディエンスが [!DNL Google Customer Match] に送信されます。これにより、会社は現在の顧客、古い電話モデルを所有している顧客および同様の顧客を、[!DNL Search]、[!DNL YouTube]、[!DNL Gmail]、[!DNL Google Display Network] などの [!DNL Google Display & Video 360] のプロパティでターゲットにすることができます。

## サポートされている ID {#supported-identities}

[!DNL Google Customer Match] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| phone_sha256_e.164 | SHA256 アルゴリズムでハッシュ化された E164 形式の電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。[ID の一致要件 ](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストには適切な名前空間を、ハッシュ化された電話番号には適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。[ID の一致要件 ](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストには適切な名前空間を、ハッシュ化されたメールアドレスには適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

<!-- not supported in beta

|GAID|Google Advertising ID|Select this target identity when your source identity is a GAID namespace.|
|IDFA|Apple ID for Advertisers|Select this target identity when your source identity is an IDFA namespace.|
|user_id|Custom user IDs|Select this target identity when your source identity is a custom namespace.| 

-->

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディ [!DNL Google Customer Match] ンスの宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## [!DNL Google Customer Match] アカウントの前提条件 {#google-account-prerequisites}

Experience Platformで [!DNL Google Customer Match] の宛先を設定する前に、[Googleのサポートドキュメント ](https://support.google.com/google-ads/answer/6299717) に概説されている [!DNL Customer Match] の使用に関するGoogleのポリシーを読んで遵守してください。

次に、[!DNL Google] アカウントが [!DNL Standard] 以上の権限レベルに設定されていることを確認してください。 詳しくは、[Google広告のドキュメント ](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&amp;rd=1) を参照してください。

### 許可リスト {#allowlist}

Experience Platformで [!DNL Google Customer Match] の宛先を作成する前に、[!DNL Google Ads] アカウントが [[!DNL Google Customer Match] policy](https://support.google.com/google-ads/answer/6299717/customer-match-policy) に準拠していることを確認してください。

準拠しているアカウントを持つお客様は、Googleによって自動的に許可リストに加えるされます。

## ID の一致要件 {#id-matching-requirements}

[!DNL Google] では、個人を特定できる情報（PII）が明確に送信されないことが必要です。 したがって、[!DNL Google Customer Match] に対してアクティブ化されたオーディエンスは、ハッシュ化されたメールアドレスや電話番号などの *ハッシュ化された* 識別子をキーオフにする必要があります。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。

### 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

[!DNL Google Customer Match] で電話番号をアクティブ化する方法は 2 つあります。

* **生の電話番号の取り込み**:[!DNL E.164] 形式の生の電話番号を [!DNL Experience Platform] に取り込むことができ、アクティベーション時に自動的にハッシュ化されます。 このオプションを選択する場合は、常に生の電話番号を `Phone_E.164` 名前空間に取り込んでください。
* **ハッシュ化された電話番号の取り込み**:[!DNL Experience Platform] に取り込む前に、電話番号を事前にハッシュ化できます。 このオプションを選択した場合は、ハッシュ化された電話番号を常に `PHONE_SHA256_E.164` 名前空間に取り込んでください。

>[!NOTE]
>
>`Phone` 名前空間に取り込まれた電話番号は、[!DNL Google Customer Match + DV360] の宛先に対してアクティブ化できません。

### メールハッシュ要件 {#hashing-requirements}

メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platformでメールアドレスを明確に使用して、アクティベーション時 [!DNL Experience Platform] ハッシュ化したりできます。

Googleのハッシュ要件およびその他のアクティベーションに関する制限事項について詳しくは、Googleのドキュメントの次の節を参照してください。

* [[!DNL Customer Match]  メールアドレス、アドレスまたはユーザー ID を使用 ](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_email_address_address_or_user_id)
* [[!DNL Customer Match]  考慮事項 ](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_considerations)
* [[!DNL Customer Match]  電話番号を使用 ](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_phone_number)
* [[!DNL Customer Match]  モバイルデバイス ID](https://developers.google.com/google-ads/api/docs/remarketing/audience-types/customer-match#customer_match_with_mobile_device_ids)


Experience Platformでのメールアドレスの取り込みについて詳しくは、[ バッチ取り込みの概要 ](../../../ingestion/batch-ingestion/overview.md) および [ ストリーミング取り込みの概要 ](../../../ingestion/streaming-ingestion/overview.md) を参照してください。

メールアドレスを自分でハッシュ化する場合は、上記のリンクで概説されているGoogleの要件に必ず準拠してください。

<!-- ### Using custom namespaces {#custom-namespaces}

Before you can use the `User_ID` namespace to send data to Google, make sure you synchronize your own identifiers using [!DNL gTag]. Refer to the [Google official documentation](https://support.google.com/google-ads/answer/9199250) for detailed information. -->

<!-- Data from unhashed namespaces is automatically hashed by [!DNL Experience Platform] upon activation.

Attribute source data is not automatically hashed. When your source field contains unhashed attributes, check the **[!UICONTROL Apply transformation]** option, to have [!DNL Experience Platform] automatically hash the data on activation.
![Identity mapping transformation](../../assets/ui/activate-destinations/identity-mapping-transformation.png) -->

<!-- ## Configure destination - video walkthrough {#video}

The video below demonstrates the steps to configure a [!DNL Google Customer Match] destination and activate audiences. The steps are also laid out sequentially in the next sections.

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng) -->

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：この宛先接続の名前を指定します
* **[!UICONTROL 説明]**：この宛先接続の説明を入力します
* **[!UICONTROL アカウント ID]**：お使いの [Google Ads カスタマー ID](https://support.google.com/google-ads/answer/1704344?hl=en)。 ID の形式は xxx-xxx-xxxx です。 [!DNL Google Ads Manager Account (My Client Center)] を使用している場合は、マネージャーアカウント ID を使用しないでください。 代わりに、[Google Ads 顧客 ID](https://support.google.com/google-ads/answer/1704344?hl=en) を使用します。
* **[!UICONTROL アカウントタイプ]**：お使いのGoogle アカウントのタイプ。 Googleでの広告アカウントの種類に応じて、次のいずれかのオプションを選択します。
   * **[!UICONTROL ビデオ パートナーの表示]**
   * **[!UICONTROL ビデオ広告主を表示]**

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を宛先に書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](../../assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

<!-- In the **[!UICONTROL Segment schedule]** step, you must provide the [!UICONTROL App ID] when sending [!DNL IDFA] or [!DNL GAID] audiences to [!DNL Google Customer Match].

![Google Customer Match App ID field highlighted in the Segment schedule step of the activation workflow.](../../assets/catalog/advertising/google-customer-match/gcm-destination-appid.png)

For details on how to find the [!DNL App ID], refer to the [Google official documentation](https://developers.google.com/adwords/api/docs/reference/v201809/AdwordsUserListService.CrmBasedUserList#appid) or ask your Google representative. -->

### マッピング例：[!DNL Google Customer Match + Display & Video 360] でのオーディエンスデータのアクティブ化 {#example-gcm}

これは、[!DNL Google Customer Match + Display & Video 360] でオーディエンスデータをアクティブ化する際の、正しい ID マッピングの例です。

ソースフィールドを選択中：

* 使用しているメールアドレスがハッシュ化されていない場合は、`Email` 名前空間をソース ID として選択します。
* [!DNL Google Customer Match] の [ メールハッシュ要件 ](#hashing-requirements) に従って、[!DNL Experience Platform] へのデータ取り込み時に顧客のメールアドレスをハッシュ化した場合は、`Email_LC_SHA256` 名前空間をソース ID として選択します。
* ハッシュ化されていない電話番号がデータに含まれている場合は、`PHONE_E.164` 名前空間をソース ID として選択します。 [!DNL Experience Platform] は、[!DNL Google Customer Match] の要件に準拠するために電話番号をハッシュ化します。
* [!DNL Facebook][ 電話番号ハッシュ要件 ](#phone-number-hashing-requirements) に従って、[!DNL Experience Platform] へのデータ取り込みで電話番号をハッシュ化した場合は、`Phone_SHA256_E.164` 名前空間をソース ID として選択します。

ターゲットフィールドを選択：

* ソース名前空間が `Email` または `Email_LC_SHA256` の場合は、`Email_LC_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `PHONE_E.164` または `Phone_SHA256_E.164` の場合は、`Phone_SHA256_E.164` 名前空間をターゲット ID として選択します。

![ アクティベーションワークフローのマッピング手順に表示される、ソースフィールドとターゲットフィールドの ID マッピング。](../../assets/catalog/advertising/google-customer-match-dv360/identity-mapping-gcm-dv360.png)

ハッシュ化されていない名前空間のデータは、アクティベーション時に [!DNL Experience Platform] によって自動的にハッシュ化されます。

属性ソースデータは、自動的にはハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。

![ アクティベーションワークフローのマッピング手順でハイライト表示されている変換コントロールを適用 ](../../assets/catalog/advertising/google-customer-match-dv360/transformation.png)

## 宛先の監視 {#monitor-destination}

宛先に接続し、宛先データフローを確立したら、Real-Time CDPの [ モニタリング機能 ](/help/dataflows/ui/monitor-destinations.md) を使用して、各データフロー実行で宛先に対してアクティブ化されたプロファイルレコードに関する詳細な情報を取得できます。

[!DNL Google Customer Match + Display & Video 360] 接続の監視情報には、各データフローおよびデータフロー実行のアクティブ化、除外、失敗した ID に関するオーディエンスレベルの情報が含まれます。 機能について ](/help/dataflows/ui/monitor-destinations.md#segment-level-view) 詳細を参照 [。

## Audience Activation が成功したことの確認 {#verify-activation}

アクティベーションフローを完了したら、**[!UICONTROL Google広告]** アカウントに切り替えます。 アクティブ化されたオーディエンスは、Google アカウントに顧客リストとして表示されます。 オーディエンスサイズによっては、提供するアクティブユーザーが 1000 人以上でない限り、一部のオーディエンスが入力されない場合があります。 詳しくは、[Google オーディエンスパートナーのドキュメント ](https://developers.google.com/audience-partner/api/docs/customer-match/get-started#verify-list) を参照してください。 リンク内のドキュメントにアクセスするには、Googleに問い合わせる必要があります。

## データガバナンス

Experience Platformの一部の宛先には、宛先プラットフォームに送信される、または宛先プラットフォームから受信するデータに対する、特定のルールと義務があります。 お客様は、お客様のデータの制限事項や義務を理解し、Adobe Experience Platformおよび宛先プラットフォームでのデータの使用方法を理解する責任を負います。 Adobe Experience Platformには、これらのデータ使用義務の一部を管理するのに役立つデータガバナンスツールが用意されています。 データガバナンスツールとポリシーについて [ 詳細情報 ](../../../data-governance/labels/overview.md) します。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Google Customer Match Error: OperationAccessDenied.ACTION_NOT_PERMITTED","code":"400 BAD_REQUEST"}`

このエラーは、顧客アカウントが [ 前提条件 ](#google-account-prerequisites) に準拠していない場合に発生します。 この問題を修正するには、Googleに問い合わせて、自分のアカウントが許可リストに登録されており、[!DNL Standard] 以上の権限レベルに設定されていることを確認してください。 詳しくは、[Google広告のドキュメント ](https://support.google.com/google-ads/answer/9978556?visit_id=637611563637058259-4176462731&amp;rd=1) を参照してください。

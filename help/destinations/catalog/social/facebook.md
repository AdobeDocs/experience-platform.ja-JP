---
keywords: facebook接続；facebook接続；facebookの宛先；facebook;instagram;messenger;facebook messenger
title: Facebook 接続
description: ハッシュ化されたメールに基づいてオーディエンスのターゲティング、パーソナライゼーションおよび抑制を行うための、Facebook キャンペーン用のプロファイルをアクティブ化します。
exl-id: 51e8c8f0-5e79-45b9-afbc-110bae127f76
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1952'
ht-degree: 29%

---

# [!DNL Facebook] 接続

## 概要 {#overview}

のプロファイルのアクティブ化 [!DNL Facebook] ハッシュ化されたメールに基づくオーディエンスのターゲティング、パーソナライゼーションおよび抑制のキャンペーン。

この宛先は、以下でのオーディエンスターゲティングに使用できます [!DNL Facebook's] がサポートするアプリケーションのファミリー [!DNL Custom Audiences]を含む [!DNL Facebook], [!DNL Instagram], [!DNL Audience Network]、および [!DNL Messenger]. キャンペーンの実行対象となるアプリの選択は、のプレースメントレベルに示されます。 [!DNL Facebook Ads Manager].

![Adobe Experience Platform UI でのFacebookの宛先。](../../assets/catalog/social/facebook/catalog.png)

## ユースケース

を使用する方法とタイミングをより深く理解するために [!DNL Facebook] 出力先、ここでは、Adobe Experience Platformのお客様がこの機能を使用して解決できる 2 つのユースケースの例を示します。

### のユースケース#1

オンライン小売業者は、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと考えています。 オンライン小売業者は、メールアドレスを独自の CRM からAdobe Experience Platformに取り込み、独自のオフラインデータからオーディエンスを作成し、これらのオーディエンスをに送信できます。 [!DNL Facebook] ソーシャルプラットフォーム、広告支出の最適化。

### のユースケース#2

ある航空会社には様々な顧客レベル（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じて各レベルにパーソナライズされたオファーを提供したいと考えています。 ただし、すべての顧客が航空会社のモバイルアプリを使用しているわけではなく、一部の顧客は会社の web サイトにログオンしていません。 これらの顧客に関して会社が持っている識別子は、メンバーシップ ID とメールアドレスのみです。

ソーシャルメディア全体でターゲットを設定するには、メールアドレスを識別子として使用して、CRM からAdobe Experience Platformに顧客データをオンボーディングします。

次に、関連するメンバーシップ ID や顧客層などのオフラインデータを使用して、を通じてターゲットにできる新しいオーディエンスを作成できます [!DNL Facebook] の宛先。

## サポートされている ID {#supported-identities}

[!DNL Facebook Custom Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。の指示に従います [ID の一致要件](#id-matching-requirements-id-matching-requirements) およびは、プレーンテキストとハッシュ化された電話番号に適した名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。の指示に従います [ID の一致要件](#id-matching-requirements-id-matching-requirements) およびは、プレーンテキストとハッシュ化されたメールアドレスに適した名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | facebookの宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## Facebook アカウントの前提条件 {#facebook-account-prerequisites}

オーディエンスをに送信する前に [!DNL Facebook]次の要件を満たしていることを確認します。

* あなたの [!DNL Facebook] ユーザーアカウントには、へのフルアクセス権が必要です。 [!DNL Facebook Business Account] 使用している広告アカウントを所有しています。
* あなたの [!DNL Facebook] ユーザーアカウントには次が必要です **[!DNL Manage campaigns]** 使用する広告アカウントに対して権限が有効になっています。
* この **Adobe Experience Cloud** ビジネスアカウントは、の広告パートナーとして追加される必要があります [!DNL Facebook Ad Account]. `business ID=206617933627973`.を使用します。参照： [ビジネスマネージャーへのパートナーの追加](https://www.facebook.com/business/help/1717412048538897) 詳しくは、Facebook ドキュメントを参照してください。
  >[!IMPORTANT]
  >
  > Adobe Experience Cloudの権限を設定する場合は、を有効にする必要があります **キャンペーンの管理** 権限。 この権限は、 [!DNL Adobe Experience Platform] 統合。
* を読み、署名する [!DNL Facebook Custom Audiences] サービス利用規約。 これを行うには、に移動します。 `https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]`、ここで `accountID` が自分 [!DNL Facebook Ad Account ID].
  >[!IMPORTANT]
  >
  >に署名する場合 [!DNL Facebook Custom Audiences] サービス利用規約では、必ずFacebook API で認証に使用したのと同じユーザーアカウントを使用してください。

## ID の一致要件 {#id-matching-requirements}

[!DNL Facebook] では、個人を特定できる情報（PII）を明確に送信しないようにする必要があります。 したがって、オーディエンスはに対してアクティブ化されました [!DNL Facebook] キーオフできる *ハッシュ化* 識別子（メールアドレスや電話番号など）。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。

## 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

で電話番号を有効にする方法は 2 つあります [!DNL Facebook]:

* **生の電話番号の取り込み**：で生の電話番号を取り込むことができます [!DNL E.164] フォーマット先 [!DNL Platform]. それらはアクティベーション時に自動的にハッシュ化されます。 このオプションを選択した場合は、必ず生の電話番号をに取り込んでください `Phone_E.164` 名前空間。
* **ハッシュ化された電話番号の取り込み**：に取り込む前に、電話番号を事前にハッシュ化できます [!DNL Platform]. このオプションを選択した場合は、ハッシュ化された電話番号を常にに取り込んでください `Phone_SHA256` 名前空間。

>[!NOTE]
>
>に取り込まれた電話番号 `Phone` で名前空間をアクティブ化できません [!DNL Facebook].

## メールハッシュ要件 {#email-hashing-requirements}

メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、メールアドレスをExperience Platformで明確に使用したりできます。 [!DNL Platform] アクティブ化時にハッシュ化します。

Experience Platformでのメールアドレスの取り込みについては、以下を参照してください [バッチ取り込みの概要](/help/ingestion/batch-ingestion/overview.md) および [ストリーミング取得の概要](/help/ingestion/streaming-ingestion/overview.md).

メールアドレスを自分でハッシュ化することを選択する場合は、次の要件に必ず従ってください。

* メール文字列の先頭と末尾のスペースをすべてトリミングします。例： `johndoe@example.com`ではなく `<space>johndoe@example.com<space>`;
* メール文字列をハッシュ化する場合は、小文字の文字列もハッシュ化します。
   * 例： `example@email.com`ではなく `EXAMPLE@EMAIL.COM`;
* ハッシュ化された文字列がすべて小文字であることを確認します
   * 例： `55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`ではなく `55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149`;
* ひもに塩をかけるな。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます [!DNL Platform] アクティブ化時。
> 属性ソースデータは、自動的にはハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。
> この **[!UICONTROL 変換を適用]** オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![マッピングステップでハイライト表示されている変換コントロールを適用します。](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## カスタム名前空間の使用 {#custom-namespaces}

使用する前に `Extern_ID` データを送信する名前空間 [!DNL Facebook]を使用する場合は、必ず独自の識別子を同期してください。 [!DNL Facebook Pixel]. を参照してください。 [Facebook公式ドキュメント](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers) を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

次のビデオでは、の設定手順も示しています。 [!DNL Facebook] オーディエンスの宛先とアクティブ化。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新の情報については、を参照してください [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 宛先に対する認証 {#authenticate}

1. 宛先カタログでFacebookの宛先を見つけて、以下を選択します **[!UICONTROL 設定]**.
2. を選択 **[!UICONTROL 宛先への接続]**.
   ![アクティベーションワークフローに表示されるFacebookへの認証手順。](/help/destinations/assets/catalog/social/facebook/authenticate-facebook-destination.png)
3. facebookの資格情報を入力し、を選択します。 **ログイン**.

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_facebook_accountid"
>title="アカウント ID"
>abstract="Facebook 広告アカウント ID。この ID は、Facebook 広告マネージャアカウントで確認できます。この ID を入力する際には、常に `act_` を先頭に追加します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**：あなたの [!DNL Facebook Ad Account ID]. この ID は、 [!DNL Facebook Ads Manager] アカウント。 この ID を入力する際には、常に `act_` を先頭に追加します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_facebook_originofaudience"
>title="オーディエンスのオリジン"
>abstract="オーディエンス内の顧客データが最初に収集された方法を選択します。ユーザーがセグメントのターゲットになっている場合、このデータが Facebook に表示されます"

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_facebook_originofaudience_customers"
>title="オーディエンスのオリジン"
>abstract="顧客からデータを直接収集した広告主。"

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_facebook_originofaudience_partners"
>title="オーディエンスのオリジン"
>abstract="パートナーからデータを直接収集した広告主。"

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_facebook_originofaudience_customersandpartners"
>title="オーディエンスのオリジン"
>abstract="顧客およびパートナーからデータを直接収集した広告主。"

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

が含まれる **[!UICONTROL セグメントスケジュール]** 手順で、を指定する必要があります [!UICONTROL オーディエンスの接触チャネル] オーディエンスをに送信する場合 [!DNL Facebook Custom Audiences].

![facebook アクティベーションステップに表示されるオーディエンスの接触チャネル ドロップダウン。](../../assets/catalog/social/facebook/facebook-origin-audience.png)

### マッピング例：でのオーディエンスデータのアクティブ化 [!DNL Facebook Custom Audience] {#example-facebook}

以下は、でオーディエンスデータをアクティブ化する際の正しい ID マッピングの例です [!DNL Facebook Custom Audience].

ソースフィールドを選択中：

* 「」を選択します `Email` 使用しているメールアドレスがハッシュ化されていない場合の、ソース id としての名前空間。
* 「」を選択します `Email_LC_SHA256` へのデータ取り込み時に顧客のメールアドレスをハッシュ化した場合のソース id としての名前空間 [!DNL Platform]に従う [!DNL Facebook] [メールハッシュ要件](#email-hashing-requirements).
* 「」を選択します `PHONE_E.164` ハッシュ化されていない電話番号でデータが構成されている場合、ソース id としての名前空間。 [!DNL Platform] は準拠する電話番号をハッシュ化します [!DNL Facebook] 要件
* 「」を選択します `Phone_SHA256` へのデータ取り込みで電話番号をハッシュ化した場合のソース ID としての名前空間 [!DNL Platform]に従う [!DNL Facebook] [電話番号のハッシュ要件](#phone-number-hashing-requirements).
* 「」を選択します `IDFA` データが次の要素で構成されている場合、ソース id としての名前空間 [!DNL Apple] デバイス ID。
* 「」を選択します `GAID` データが次の要素で構成されている場合、ソース id としての名前空間 [!DNL Android] デバイス ID。
* 「」を選択します `Custom` データが他のタイプの識別子で構成される場合のソース id としての名前空間。

ターゲットフィールドを選択：

* 「」を選択します `Email_LC_SHA256` ソース名前空間が次のいずれかである場合の、ターゲット id としての名前空間 `Email` または `Email_LC_SHA256`.
* 「」を選択します `Phone_SHA256` ソース名前空間が次のいずれかである場合の、ターゲット id としての名前空間 `PHONE_E.164` または `Phone_SHA256`.
* 「」を選択します `IDFA` または `GAID` ソース名前空間が次のような場合に、ターゲット id としての名前空間 `IDFA` または `GAID`.
* 「」を選択します `Extern_ID` ソース名前空間がカスタム名前空間の場合、ターゲット id としての名前空間。

>[!IMPORTANT]
>
>ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます [!DNL Platform] アクティブ化時。
> 
>属性ソースデータは、自動的にはハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。

![マッピングステップでハイライト表示されている変換コントロールを適用します。](../../assets/ui/activate-segment-streaming-destinations/mapping-summary.png)

## 書き出したデータ {#exported-data}

の場合 [!DNL Facebook]が成功した場合、アクティブ化は [!DNL Facebook] カスタムオーディエンスは、次の場所でプログラムにより作成されます [[!DNL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). オーディエンスメンバーシップは、アクティブ化されたオーディエンスに対してユーザーが適格となったり不適格になったりすると、追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformとの統合 [!DNL Facebook] は、履歴オーディエンスバックフィルをサポートします。 すべてのオーディエンス選定履歴はに送信されます [!DNL Facebook] 宛先に対してオーディエンスをアクティブ化するとき。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Facebook Error: Permission error","code":"400 BAD_REQUEST"}`

このエラーは、顧客が新しく作成したアカウントと、 [!DNL Facebook] 権限がまだアクティブではありません。

を受け取った場合 `400 Bad Request` の手順を実行した後のエラーメッセージ [Facebook アカウントの前提条件](#facebook-account-prerequisites)、に対して数日間許可します [!DNL Facebook] 発効する許可。

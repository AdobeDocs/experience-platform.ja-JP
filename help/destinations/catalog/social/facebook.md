---
keywords: facebook 接続；facebook 接続；facebook 宛先；facebook;instagram;messenger;facebook messenger
title: Facebook 接続
description: ハッシュ化されたメールに基づいてオーディエンスのターゲティング、パーソナライゼーションおよび抑制を行うための、Facebook キャンペーン用のプロファイルをアクティブ化します。
exl-id: 51e8c8f0-5e79-45b9-afbc-110bae127f76
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '2636'
ht-degree: 17%

---

# [!DNL Facebook] 接続

## 概要 {#overview}

ハッシュ化されたメールに基づいてオーディエンスのターゲティング、パーソナライゼーションおよび抑制を行うための、[!DNL Facebook] キャンペーン用のプロファイルをアクティブ化します。

この宛先は、[!DNL Facebook's]、[!DNL Custom Audiences]、[!DNL Facebook]、[!DNL Instagram] など、[!DNL Audience Network] でサポートされてい [!DNL Messenger] アプリファミリ全体でのオーディエンスのターゲティングに使用できます。 Campaign を実行するアプリの選択は、[!DNL Facebook Ads Manager] のプレースメントレベルに示されます。

![Adobe Experience Platform UI での Facebook の宛先。](../../assets/catalog/social/facebook/catalog.png)

## ユースケース

[!DNL Facebook] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの機能を使用して解決できる、2 つのサンプルユースケースを以下に示します。

### のユースケース#1

オンラインretailerは、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと考えています。 オンラインのretailerは、電子メールアドレスを独自の CRM からAdobe Experience Platformに取り込み、独自のオフラインデータからオーディエンスを作成し、これらのオーディエンスを [!DNL Facebook] ソーシャルプラットフォームに送信して、広告費用を最適化できます。

### のユースケース#2

ある航空会社には様々な顧客レベル（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じて各レベルにパーソナライズされたオファーを提供したいと考えています。 ただし、すべての顧客が航空会社のモバイルアプリを使用しているわけではなく、一部の顧客は会社の web サイトにログオンしていません。 これらの顧客に関して会社が持っている識別子は、メンバーシップ ID とメールアドレスのみです。

ソーシャルメディア全体でターゲットを設定するには、メールアドレスを識別子として使用して、CRM からAdobe Experience Platformに顧客データをオンボーディングします。

次に、関連するメンバーシップ ID や顧客層などのオフラインデータを使用して、[!DNL Facebook] 宛先を通じてターゲットにできる新しいオーディエンスを構築できます。

## サポートされている ID {#supported-identities}

[!DNL Facebook Custom Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| `GAID` | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| `IDFA` | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| `phone_sha256` | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。[ID の一致要件 &#x200B;](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストには適切な名前空間を、ハッシュ化された電話番号には適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| `email_lc_sha256` | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。[ID の一致要件 &#x200B;](#id-matching-requirements-id-matching-requirements) の節の手順に従って、プレーンテキストには適切な名前空間を、ハッシュ化されたメールアドレスには適切な名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| `extern_id` | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |
| `gender` | 性別 | 使用できる値： <ul><li>男性用 `m`</li><li>女性用 `f`</li></ul> Experience Platform **自動的にハッシュ化** この値を Facebook に送信する前に行います。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `date_of_birth` | 誕生日 | 使用できる形式：`yyyy-MM-DD`。 <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `last_name` | 姓 | 使用できる形式：小文字、`a-z` 文字のみ、句読点なし。 特殊文字には UTF-8 エンコーディングを使用します。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `first_name` | 名 | 使用できる形式：小文字、`a-z` 文字のみ、句読点なし、スペースなし。 特殊文字には UTF-8 エンコーディングを使用します。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `first_name_initial` | 名の頭文字 | 使用できる形式：小文字、`a-z` 文字のみ。 特殊文字には UTF-8 エンコーディングを使用します。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `state` | 都道府県 | [2 文字の ANSI 略語コード &#x200B;](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code) を小文字で使用します。 米国以外の州では、小文字、句読点、特殊文字、スペースを使用しません。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `city` | 市区町村 | 使用できる形式：小文字、`a-z` 文字のみ、句読点なし、特殊文字なし、スペースなし。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `zip` | 郵便番号 | 使用できる形式：小文字、スペースなし。 米国の郵便番号の場合は、最初の 5 桁のみを使用します。 英国の場合は、`Area/District/Sector` 形式を使用します。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |
| `country` | 国 | 使用可能な形式：小文字、[ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 形式の 2 文字の国コード。  <br>Experience Platform **は自動的にハッシュ化** します。この値は Facebook に送信される前に確認してください。 この自動ハッシュは、Facebook のセキュリティとプライバシーの要件に準拠するために必要です。 このフィールドには事前にハッシュされた値を指定しない **ください** これは、一致するプロセスが失敗する原因となります。 |

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
| 書き出しタイプ | **[!UICONTROL Audience export]** | Facebook の宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## Facebook アカウントの前提条件 {#facebook-account-prerequisites}

オーディエンスを [!DNL Facebook] に送信する前に、次の要件を満たしていることを確認してください。

* [!DNL Facebook] ユーザーアカウントには、使用している広告アカウントを所有する [!DNL Facebook Business Account] へのフルアクセス権が必要です。
* 使用する広告アカウントに対して、[!DNL Facebook] ユーザーアカウントで **[!DNL Manage campaigns]** 権限が有効になっている必要があります。
* **Adobe Experience Cloud** ビジネス アカウントは、広告パートナーとして [!DNL Facebook Ad Account] に追加される必要があります。 `business ID=206617933627973`.を使用します。詳しくは、Facebook ドキュメントの [&#x200B; ビジネスマネージャーへのパートナーの追加 &#x200B;](https://www.facebook.com/business/help/1717412048538897) を参照してください。

  >[!IMPORTANT]
  >
  > Adobe Experience Cloudの権限を設定する場合は、**キャンペーンの管理** 権限を有効にする必要があります。 [!DNL Adobe Experience Platform] 統合には権限が必要です。

* [!DNL Facebook Custom Audiences] のサービス利用規約を読み、署名します。 これを行うには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]&business_id=206617933627973` に移動します。`accountID` れは [!DNL Facebook Ad Account ID] です。 サービス利用規約に署名する際は、URL に `business_id=206617933627973` セクションが存在することを確認してください。

  >[!IMPORTANT]
  >
  >[!DNL Facebook Custom Audiences] サービス利用規約に署名する場合は、Facebook API での認証に使用したのと同じユーザーアカウントを使用してください。

## ID の一致要件 {#id-matching-requirements}

[!DNL Facebook] では、個人を特定できる情報（PII）が明確に送信されないことが必要です。 したがって、[!DNL Facebook] に対してアクティブ化されたオーディエンスは、メールアドレスや電話番号などの識別子を *ハッシュ化* してオフにすることができます。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。

## オーディエンスのマッチ率の最大化 {#match-rates}

[!DNL Facebook] で最高のオーディエンス一致率を達成するには、`phone_sha256` および `email_lc_sha256` のターゲット ID を使用することを強くお勧めします。

これらの識別子は、[!DNL Facebook] がプラットフォーム間でオーディエンスを照合するために使用する主な識別子です。 ソースデータがこれらのターゲット ID に適切にマッピングされ、ハッシュ要件に準拠してい [!DNL Facebook's] ことを確認します。

## 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

[!DNL Facebook] で電話番号をアクティブ化する方法は 2 つあります。

* **生の電話番号の取り込み**:[!DNL E.164] 形式の生の電話番号を [!DNL Experience Platform] に取り込むことができます。 それらはアクティベーション時に自動的にハッシュ化されます。 このオプションを選択する場合は、常に生の電話番号を `Phone_E.164` 名前空間に取り込んでください。
* **ハッシュ化された電話番号の取り込み**:[!DNL Experience Platform] に取り込む前に、電話番号を事前にハッシュ化できます。 このオプションを選択した場合は、ハッシュ化された電話番号を常に `Phone_SHA256` 名前空間に取り込んでください。

>[!NOTE]
>
>`Phone` 名前空間に取り込まれた電話番号は、[!DNL Facebook] でアクティブ化できません。

## メールハッシュ要件 {#email-hashing-requirements}

メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、Experience Platformでメールアドレスを明確に使用して、アクティベーション時 [!DNL Experience Platform] ハッシュ化したりできます。

Experience Platformでのメールアドレスの取り込みについて詳しくは、[&#x200B; バッチ取り込みの概要 &#x200B;](/help/ingestion/batch-ingestion/overview.md) および [&#x200B; ストリーミング取り込みの概要 &#x200B;](/help/ingestion/streaming-ingestion/overview.md) を参照してください。

メールアドレスを自分でハッシュ化することを選択する場合は、次の要件に必ず従ってください。

* メール文字列の先頭と末尾のスペースをすべてトリミングします。例：`johndoe@example.com` ではなく `<space>johndoe@example.com<space>`;
* メール文字列をハッシュ化する場合は、小文字の文字列もハッシュ化します。
   * 例：`example@email.com` ではなく `EXAMPLE@EMAIL.COM`;
* ハッシュ化された文字列がすべて小文字であることを確認します
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149` ではなく `55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149`;
* ひもに塩をかけるな。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、アクティベーション時に [!DNL Experience Platform] によって自動的にハッシュ化されます。
> 属性ソースデータは、自動的にはハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。
> 「**[!UICONTROL Apply transformation]**」オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![&#x200B; マッピングステップでハイライト表示されている「変換コントロールを適用」 &#x200B;](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## カスタム名前空間の使用 {#custom-namespaces}

`Extern_ID` 名前空間を使用して [!DNL Facebook] にデータを送信する前に、[!DNL Facebook Pixel] を使用して独自の識別子を同期させる必要があります。 詳しくは、[Facebook 公式ドキュメント &#x200B;](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers) を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

次のビデオでは、[!DNL Facebook] しい宛先を設定し、オーディエンスをアクティブ化する手順も示します。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新情報については、[&#x200B; 宛先設定のチュートリアル &#x200B;](../../ui/connect-destination.md) を参照してください。

### 宛先に対する認証 {#authenticate}

1. 宛先カタログで Facebook の宛先を見つけて、**[!UICONTROL Set Up]** を選択します。
2. **[!UICONTROL Connect to destination]** を選択します。
   ![&#x200B; アクティベーションワークフローに表示される Facebook への認証手順 &#x200B;](/help/destinations/assets/catalog/social/facebook/authenticate-facebook-destination.png)
3. Facebook の認証情報を入力し、「**ログイン**」を選択します。

### 認証資格情報を更新 {#refresh-authentication-credentials}

Facebook 認証トークンは 60 日ごとに期限切れになります。 トークンの有効期限が切れると、宛先へのデータの書き出しは機能しなくなります。

トークンの有効期限は、「**[!UICONTROL Account expiration date]**」タブまたは「**[[!UICONTROL Accounts]](../../ui/destinations-workspace.md#accounts)**」タブの「**[[!UICONTROL Browse]](../../ui/destinations-workspace.md#browse)**」列から監視できます。

![&#x200B; 「参照」タブの Facebook アカウントトークンの有効期限の列 &#x200B;](../../assets/catalog/social/facebook/account-expiration-browse.png)

![&#x200B; 「アカウント」タブの Facebook アカウントトークンの有効期限の列 &#x200B;](../../assets/catalog/social/facebook/account-expiration-accounts.png)

トークンの有効期限が原因でアクティベーションデータフローが中断されるのを防ぐには、次の手順を実行して再認証を行います。

1. **[!UICONTROL Destinations]**/**[!UICONTROL Accounts]** に移動します。
2. （オプション）ページで使用可能なフィルターを使用して、Facebook アカウントのみを表示します。
   ![Facebook アカウントのみを表示するようにフィルター &#x200B;](/help/destinations/assets/catalog/social/facebook/refresh-oauth-filters.png)
3. 更新するアカウントを選択し、省略記号を選択して「**[!UICONTROL Edit details]**」を選択します。
   ![&#x200B; 詳細コントロールの編集を選択 &#x200B;](/help/destinations/assets/catalog/social/facebook/refresh-oauth-edit-details.png)
4. モーダルウィンドウで「**[!UICONTROL Reconnect OAuth]**」を選択し、Facebook の資格情報を使用して再認証します。
   ![&#x200B; 「OAuth に再接続」オプションを使用したモーダルウィンドウ &#x200B;](/help/destinations/assets/catalog/social/facebook/reconnect-oauth-control.png)

>[!SUCCESS]
> 
>認証資格情報が更新され、有効期限が 60 日にリセットされます。

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_facebook_accountid"
>title="アカウント ID"
>abstract="Facebook 広告アカウント ID。この ID は、Facebook 広告マネージャアカウントで確認できます。この ID を入力する際には、常に接頭辞 `act_` を追加します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Account ID]**：あなたの [!DNL Facebook Ad Account ID]。 この ID は [!DNL Facebook Ads Manager] アカウントで確認できます。 この ID を入力する際には、常に接頭辞 `act_` を追加します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

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
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

**[!UICONTROL Segment schedule]** の手順では、オーディエンスを [!UICONTROL Origin of audience] に送信する際に、[!DNL Facebook Custom Audiences] を指定する必要があります。

![Facebook アクティベーションステップに表示されるオーディエンスの接触チャネルドロップダウン。](../../assets/catalog/social/facebook/facebook-origin-audience.png)

### マッピング例：[!DNL Facebook Custom Audience] でのオーディエンスデータのアクティブ化 {#example-facebook}

以下は、[!DNL Facebook Custom Audience] でオーディエンスデータをアクティブ化する際の正しい ID マッピングの例です。

ソースフィールドを選択中：

* 使用しているメールアドレスがハッシュ化されていない場合は、`Email` 名前空間をソース ID として選択します。
* `Email_LC_SHA256` の [!DNL Experience Platform] メールハッシュ要件 [!DNL Facebook] に従って、[&#x200B; へのデータ取り込み時に顧客のメールアドレスをハッシュ化した場合は、](#email-hashing-requirements) 名前空間をソース ID として選択します。
* ハッシュ化されていない電話番号がデータに含まれている場合は、`PHONE_E.164` 名前空間をソース ID として選択します。 [!DNL Experience Platform] は、[!DNL Facebook] の要件に準拠するために電話番号をハッシュ化します。
* `Phone_SHA256`[!DNL Experience Platform] 電話番号ハッシュ要件 [!DNL Facebook] に従って、[&#x200B; へのデータ取り込みで電話番号をハッシュ化した場合は、](#phone-number-hashing-requirements) 名前空間をソース ID として選択します。
* データがデバイス ID で構成されている場合は、ソース ID として `IDFA` 名前空間 [!DNL Apple] 選択します。
* データがデバイス ID で構成されている場合は、ソース ID として `GAID` 名前空間 [!DNL Android] 選択します。
* データが他のタイプの識別子で構成されている場合は、ソース ID として `Custom` 名前空間を選択します。

ターゲットフィールドを選択：

* ソース名前空間が `Email_LC_SHA256` または `Email` の場合は、`Email_LC_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `Phone_SHA256` または `PHONE_E.164` の場合は、`Phone_SHA256` 名前空間をターゲット ID として選択します。
* ソース名前空間が `IDFA` または `GAID` の場合は、`IDFA` または `GAID` 名前空間をターゲット ID として選択します。
* ソース名前空間がカスタム名前空間の場合は、`Extern_ID` 名前空間をターゲット ID として選択します。

>[!IMPORTANT]
>
>ハッシュ化されていない名前空間のデータは、アクティベーション時に [!DNL Experience Platform] によって自動的にハッシュ化されます。
> 
>属性ソースデータは、自動的にはハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。

![&#x200B; マッピングステップでハイライト表示されている「変換コントロールを適用」 &#x200B;](../../assets/ui/activate-segment-streaming-destinations/mapping-summary.png)

## 書き出したデータ {#exported-data}

[!DNL Facebook] えば、アクティベーションが成功すると、[!DNL Facebook] のカスタムオーディエンスが [[!DNL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/) でプログラムによって作成されます。 オーディエンスメンバーシップは、アクティブ化されたオーディエンスに対してユーザーが適格となったり不適格になったりすると、追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと [!DNL Facebook] の統合は、履歴オーディエンスバックフィルをサポートします。 宛先に対してオーディエンスをアクティブ化すると、すべてのオーディエンス選定履歴が [!DNL Facebook] に送信されます。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Facebook Error: Permission error","code":"400 BAD_REQUEST"}`

このエラーは、顧客が新しく作成したアカウントを使用していて、[!DNL Facebook] の権限がまだアクティブでない場合に発生します。

>[!IMPORTANT]
>
>「[!DNL Facebook Custom Audience Terms of Service] アカウントの前提条件 `business ID 206617933627973`」セクションの URL テンプレートに示されているように、[&#x200B; の下の &#x200B;](#facebook-account-prerequisites) を必ず受け入れてください。

`400 Bad Request`Facebook アカウントの前提条件 [&#x200B; の手順に従った後に &#x200B;](#facebook-account-prerequisites) のエラーメッセージが表示された場合は、[!DNL Facebook] の権限が有効になるまで数日間待ちます。



---
keywords: facebook接続；facebook接続；facebookの宛先；facebook;instagram;messenger;facebook messenger
title: Facebook 接続
description: ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーション、抑制のためのFacebookキャンペーンのプロファイルをアクティブ化します。
exl-id: 51e8c8f0-5e79-45b9-afbc-110bae127f76
source-git-commit: 37e8d36d89bf984673345743b371c31b4bb1f94d
workflow-type: tm+mt
source-wordcount: '1926'
ht-degree: 34%

---

# [!DNL Facebook] 接続

## 概要 {#overview}

のプロファイルをアクティブ化する [!DNL Facebook] ハッシュ化された電子メールに基づいて、オーディエンスのターゲティング、パーソナライゼーションおよび抑制のためのキャンペーン。

この宛先を、 [!DNL Facebook's] 次の機能をサポートするアプリのファミリー [!DNL Custom Audiences]を含む [!DNL Facebook], [!DNL Instagram], [!DNL Audience Network]、および [!DNL Messenger]. キャンペーンを実行するアプリの選択範囲が、[!DNL Facebook Ads Manager] の配置レベルで示されます。

![Adobe Experience Platform UI でのfacebookの宛先](../../assets/catalog/social/facebook/catalog.png)

## ユースケース

を使用する方法とタイミングをより深く理解するために [!DNL Facebook] の宛先に関しては、Adobe Experience Platformのお客様がこの機能を使用して解決できる 2 つの使用例を示します。

### 使用例#1

オンライン小売業者は、ソーシャルプラットフォームを通じて既存の顧客にリーチし、以前の注文に基づいてパーソナライズされたオファーを表示したいと願っています。オンライン小売業者は、独自の CRM からAdobe Experience Platformに電子メールアドレスを取り込み、独自のオフラインデータからオーディエンスを構築し、それらのオーディエンスを [!DNL Facebook] ソーシャルプラットフォームを使用して、広告費用を最適化します。

### 使用例#2

航空会社には異なる顧客階層（ブロンズ、シルバー、ゴールド）があり、ソーシャルプラットフォームを通じてパーソナライズされたオファーを各層に提供したいと考えています。ただし、すべての顧客が航空会社のモバイルアプリを使用するわけではなく、会社の Web サイトにログオンしていない顧客もいます。 会社がこれらの顧客に関して持っている識別子は、メンバーシップ ID と電子メールアドレスのみです。

ソーシャルメディアをまたいでターゲットを設定するには、電子メールアドレスを識別子として使用して、顧客データを CRM からAdobe Experience Platformにオンボーディングできます。

次に、関連するメンバーシップ ID や顧客層を含むオフラインデータを使用して、を通じてターゲットに設定できる新しいオーディエンスを構築できます。 [!DNL Facebook] 宛先。

## サポートされている ID {#supported-identities}

[!DNL Facebook Custom Audiences] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。詳しくは、 [ID 一致要件](#id-matching-requirements-id-matching-requirements) セクションとでは、プレーンテキストとハッシュ化された電話番号に適した名前空間をそれぞれ使用します。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。詳しくは、 [ID 一致要件](#id-matching-requirements-id-matching-requirements) を参照し、プレーンテキスト用とハッシュ化された電子メールアドレス用に適切な名前空間をそれぞれ使用してください。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるすべてのオーディエンスについて説明します。

この宛先では、Experience Platform [セグメント化サービス](../../../segmentation/home.md).

*さらに*&#x200B;の場合、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | オーディエンス [インポート済み](../../../segmentation/ui/overview.md#import-audience) を CSV ファイルからExperience Platformに追加します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | facebookの宛先で使用されている識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## Facebookアカウントの前提条件 {#facebook-account-prerequisites}

オーディエンスをに送信する前に [!DNL Facebook]次の要件を満たしていることを確認します。

* お使いの [!DNL Facebook] ユーザーアカウントは、 [!DNL Facebook Business Account] 使用する広告アカウントを所有する
* お使いの [!DNL Facebook] ユーザーアカウントには **[!DNL Manage campaigns]** 使用する予定の広告アカウントに対して有効になっている権限です。
* The **Adobe Experience Cloud** ビジネスアカウントは、 [!DNL Facebook Ad Account]. `business ID=206617933627973`.を使用します。詳しくは、 [ビジネスマネージャにパートナーを追加する](https://www.facebook.com/business/help/1717412048538897) ( Facebookのドキュメント ) を参照してください。
  >[!IMPORTANT]
  >
  > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。権限が必要なのは、 [!DNL Adobe Experience Platform] 統合とも呼ばれます。
* [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、に移動します。 `https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]`です。 `accountID` は、 [!DNL Facebook Ad Account ID].
  >[!IMPORTANT]
  >
  >署名時 [!DNL Facebook Custom Audiences] 利用条件については、Facebook API での認証に使用したのと同じユーザーアカウントを必ず使用してください。

## ID 一致要件 {#id-matching-requirements}

[!DNL Facebook] では、個人を特定できる情報 (PII) を明確に送信しないことが求められています。 したがって、オーディエンスは [!DNL Facebook] キーオフできる *ハッシュ* 識別子（電子メールアドレスや電話番号など）。

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。

## 電話番号のハッシュ要件 {#phone-number-hashing-requirements}

で電話番号を有効にする方法は 2 つあります。 [!DNL Facebook]:

* **生の電話番号の取り込み**：生の電話番号を取り込むことができます [!DNL E.164] 形式を変える [!DNL Platform]. アクティベーション時に自動的にハッシュ化されます。 このオプションを選択する場合は、生の電話番号を必ず `Phone_E.164` 名前空間。
* **ハッシュ化された電話番号の取り込み**：に取り込む前に電話番号を事前にハッシュ化できます。 [!DNL Platform]. このオプションを選択する場合は、常にハッシュ化された電話番号を `Phone_SHA256` 名前空間。

>[!NOTE]
>
>に取り込まれる電話番号 `Phone` で名前空間を有効化できません [!DNL Facebook].

## 電子メールのハッシュ要件 {#email-hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、電子メールアドレスをExperience Platformで明確に使用したり、 [!DNL Platform] アクティベーション時にハッシュ化します。

E メールアドレスの取り込みについて詳しくは、Experience Platformの [バッチ取得の概要](/help/ingestion/batch-ingestion/overview.md) そして [ストリーミング取り込みの概要](/help/ingestion/streaming-ingestion/overview.md).

電子メールアドレスを自分でハッシュ化する場合は、次の要件に従ってください。

* 電子メール文字列から先頭および末尾の空白文字をすべてトリミングします。例：`johndoe@example.com`（`<space>johndoe@example.com<space>` ではない）
* 電子メール文字列をハッシュする場合は、必ず小文字の文字列をハッシュ化するようにしてください。
   * 例：`example@email.com`（`EXAMPLE@EMAIL.COM` ではない）
* ハッシュ化された文字列がすべて小文字であることを確認します。
   * 例：`55e79200c1635b37ad31a378c39feb12f120f116625093a19bc32fff15041149`（`55E79200C1635B37AD31A378C39FEB12F120F116625093A19bC32FFF15041149` ではない）
* 文字列にソルトを使用しないでください。

>[!NOTE]
>
>ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます。 [!DNL Platform] 有効化時。
> 属性ソースデータは自動的にハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。
> The **[!UICONTROL 変換を適用]** オプションは、属性をソースフィールドとして選択した場合にのみ表示されます。 名前空間を選択した場合は表示されません。

![ID マッピング変換](../../assets/ui/activate-destinations/identity-mapping-transformation.png)

## カスタム名前空間の使用 {#custom-namespaces}

使用する前に、 `Extern_ID` にデータを送信する名前空間 [!DNL Facebook]を使用する場合は、次を使用して独自の ID を必ず同期してください： [!DNL Facebook Pixel]. 詳しくは、 [Facebook公式ドキュメント](https://developers.facebook.com/docs/marketing-api/audiences/guides/custom-audiences/#external_identifiers) を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

次のビデオでは、 [!DNL Facebook] の宛先に移動して、オーディエンスをアクティブ化します。

>[!VIDEO](https://video.tv.adobe.com/v/332599/?quality=12&learn=on&captions=eng)

>[!NOTE]
>
>Adobe Experience Platform のユーザーインターフェイスは頻繁に更新され、このビデオが録画された後に変更されている可能性があります。 最新の情報については、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 宛先に対する認証 {#authenticate}

1. 宛先カタログでFacebookの宛先を見つけ、「 」を選択します。 **[!UICONTROL 設定]**.
2. 選択 **[!UICONTROL 宛先に接続]**.
   ![facebookへの認証](/help/destinations/assets/catalog/social/facebook/authenticate-facebook-destination.png)
3. facebook資格情報を入力し、「 」を選択します。 **ログイン**.

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_facebook_accountid"
>title="アカウント ID"
>abstract="Facebook 広告アカウント ID。この ID は、Facebook 広告マネージャアカウントで確認できます。この ID を入力する際には、常に `act_` を先頭に追加します。"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**: [!DNL Facebook Ad Account ID]. この ID は、 [!DNL Facebook Ads Manager] アカウント。 この ID を入力する際には、常に `act_` を先頭に追加します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

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
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

Adobe Analytics の **[!UICONTROL セグメントスケジュール]** 手順に従って、 [!UICONTROL オーディエンスの起源] オーディエンスを [!DNL Facebook Custom Audiences].

![Facebook Origin of Audience](../../assets/catalog/social/facebook/facebook-origin-audience.png)

### マッピングの例：でのオーディエンスデータのアクティブ化 [!DNL Facebook Custom Audience] {#example-facebook}

以下に、でオーディエンスデータをアクティブ化する際の正しい ID マッピングの例を示します。 [!DNL Facebook Custom Audience].

ソースフィールドを選択しています。

* を選択します。 `Email` 使用している電子メールアドレスがハッシュ化されていない場合、名前空間をソース ID として使用する。
* を選択します。 `Email_LC_SHA256` データ取り込み時に顧客の電子メールアドレスをハッシュ化した場合、名前空間をソース id として [!DNL Platform]( [!DNL Facebook] [電子メールハッシュ要件](#email-hashing-requirements).
* を選択します。 `PHONE_E.164` データがハッシュ化されていない電話番号で構成されている場合、名前空間をソース ID にします。 [!DNL Platform] 次に従うために電話番号をハッシュ化します： [!DNL Facebook] 要件。
* を選択します。 `Phone_SHA256` データの取り込み時に電話番号をハッシュ化した場合は、ソース ID として名前空間を使用 [!DNL Platform]( [!DNL Facebook] [電話番号のハッシュ要件](#phone-number-hashing-requirements).
* を選択します。 `IDFA` データが次のもので構成される場合は、ソース ID としての名前空間： [!DNL Apple] デバイス ID。
* を選択します。 `GAID` データが次のもので構成される場合は、ソース ID としての名前空間： [!DNL Android] デバイス ID。
* を選択します。 `Custom` データが他のタイプの識別子で構成されている場合は、名前空間をソース id として指定します。

ターゲットフィールドの選択：

* を選択します。 `Email_LC_SHA256` ソース名前空間が次のいずれかの場合は、ターゲット ID としての名前空間 `Email` または `Email_LC_SHA256`.
* を選択します。 `Phone_SHA256` ソース名前空間が次のいずれかの場合は、ターゲット ID としての名前空間 `PHONE_E.164` または `Phone_SHA256`.
* を選択します。 `IDFA` または `GAID` ソース名前空間が使用されている場合のターゲット ID としての名前空間 `IDFA` または `GAID`.
* を選択します。 `Extern_ID` ソース名前空間がカスタムの名前空間の場合は、ターゲット id として名前空間を設定します。

>[!IMPORTANT]
>
>ハッシュ化されていない名前空間のデータは、によって自動的にハッシュ化されます。 [!DNL Platform] 有効化時。
> 
>属性ソースデータは自動的にハッシュ化されません。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。

![ID マッピング](../../assets/ui/activate-segment-streaming-destinations/mapping-summary.png)

## 書き出したデータ {#exported-data}

の場合 [!DNL Facebook]が成功した場合、アクティベーションは [!DNL Facebook] カスタムオーディエンスは、 [[!DNL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). ユーザーがアクティブ化されたオーディエンスの対象として認定または不適格となるので、オーディエンスメンバーシップは追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと [!DNL Facebook] は、履歴オーディエンスのバックフィルをサポートします。 すべての履歴オーディエンスの資格がに送信されます。 [!DNL Facebook] オーディエンスを宛先に対してアクティブ化する場合。

## トラブルシューティング {#troubleshooting}

### 400 Bad Request エラーメッセージ {#bad-request}

この宛先を設定する際に、次のエラーが発生する場合があります。

`{"message":"Facebook Error: Permission error","code":"400 BAD_REQUEST"}`

このエラーは、新しく作成されたアカウントを使用している場合に発生し、 [!DNL Facebook] 権限はまだアクティブではありません。

以下を受け取った場合、 `400 Bad Request` 次の手順に従った後のエラーメッセージ： [Facebookアカウントの前提条件](#facebook-account-prerequisites)を使用する場合、 [!DNL Facebook] 有効にする権限。

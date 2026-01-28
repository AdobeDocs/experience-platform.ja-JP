---
title: The Trade Desk - CRM 接続
description: CRM データに基づくオーディエンスのターゲティングおよび抑制のために、プロファイルを Trade Desk アカウントに対してアクティブ化します。
last-substantial-update: 2025-01-16T00:00:00Z
exl-id: e09eaede-5525-4a51-a0e6-00ed5fdc662b
source-git-commit: 47d4078acc73736546d4cbb2d17b49bf8945743a
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 8%

---

# [!DNL Trade Desk] - CRM 接続

>[!IMPORTANT]
>
>[&#x200B; 宛先カタログ &#x200B;](/help/destinations/catalog/overview.md) には 2 つの The Trade Desk - CRM 宛先があります。
>
>* EU でデータをソースにする場合は、**[!DNL The Trade Desk - CRM (EU)]** の宛先を使用します。
>* APAC または NAMER 地域でデータをソースにする場合は、**[!DNL The Trade Desk - CRM (NAMER & APAC)]** の宛先を使用します。
>
>この宛先コネクタとドキュメントページは、*[!DNL Trade Desk]* チームが作成および管理します。 お問い合わせや更新のリクエストについては、[!DNL Trade Desk] 担当者にお問い合わせください。

## 概要 {#overview}

CRM データに基づくオーディエンスのターゲティングおよび抑制のために、[!DNL Trade Desk] アカウントに対してプロファイルをアクティブ化する方法について説明します。

このコネクタは、ファーストパーティデータのアクティベーション用に [!DNL The Trade Desk] にデータを送信します。 ハッシュ化されていない生のメールと電話番号を保存してくださ [!DNL The Trade Desk]。

>[!TIP]
>
>宛先 [!DNL The Trade Desk - CRM] 使用して、CRM データ（メール、電話番号など）およびその他のファーストパーティデータ識別子（Cookie やデバイス ID など）を送信します。 Cookie とデバイス ID のマッピングについては、Experience Platform カタログ内の [Trade Desk 宛先 &#x200B;](/help/destinations/catalog/advertising/tradedesk.md) を引き続き使用できます。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>The Trade Desk に対してオーディエンスをアクティブ化する前に、[!DNL Trade Desk] アカウントマネージャーに連絡してこの機能を有効にしてもらう必要があります。 メール、電話番号、UID2/EUID を送信する場合は、署名済みのUID2/EUID 契約を [!DNL The Trade Desk] と共有する必要があります。

## ID の一致要件 {#id-matching-requirements}

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件に従う必要があります。 詳しくは、[ID 名前空間の概要 &#x200B;](/help/identity-service/features/namespaces.md) を参照してください。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

Adobe Experience Platformでは、ハッシュ化されていないメールアドレスとハッシュ化されたメールアドレスの両方と電話番号がサポートされています。 ID の一致要件の節の手順に従って、プレーンテキストとハッシュ化されたメールアドレスに適切な名前空間をそれぞれ使用します。

| ターゲット ID | 説明 |
|---|---|
| メール | メールアドレス（クリアテキスト） |
| Email_LC_SHA256 | メールアドレスは、SHA256 を使用してハッシュ化し、小文字で区切る必要があります。 この設定は、後で変更することはできません。 |
| 電話（E.164） | E.164 形式で正規化する必要がある電話番号。 E.164 形式には、プラス記号（+）、国際通話コード、市外局番、および電話番号が含まれます。 例：（+）（国コード）（市外局番）（電話番号） この識別子は、Trade Desk - ファーストパーティデータ（EU）では使用できません。 |
| 電話（SHA256_E.164） | 既に E.164 形式に正規化され、SHA-256 を使用してハッシュ化された電話番号。結果として、ハッシュが Base64 でエンコードされます。 この識別子は、Trade Desk - ファーストパーティデータ（EU）では使用できません。 |
| TDID | Trade Desk の Cookie ID |
| GAID | GOOGLE ADVERTISING ID |
| IDFA | Apple の広告主 ID |
| UID2 | 生のUID2 値 |
| UID2Token | 暗号化されたUID2 トークン（広告トークンとも呼ばれます）。 |
| EUID | 生の欧州連合 ID 値 |
| EUIDToken | 暗号化された EUID トークン（広告トークンとも呼ばれます）。 |
| ランプ ID | 49 文字または 70 文字の RampID （旧称：IdentityLink または IDL）。 これは、Trade Desk 専用にマッピングされた LiveRamp の RampID である必要があります。 |
| netId | ユーザーの netID を base64 でエンコードされた 70 文字の文字列として指定します。 この ID はヨーロッパでのみサポートされています。 |
| FirstID | ユーザーの First-id （通常、フランスのパブリッシャーが設定するファーストパーティ cookie）。 この ID はヨーロッパでのみサポートされています。 |

{style="table-layout:auto"}

## メールハッシュ要件 {#email-hashing}

メールアドレスは、Adobe Experience Platformに取り込む前にハッシュ化したり、生のメールアドレスを使用したりできます。

Experience Platformでのメールアドレスの取り込みについては、[&#x200B; バッチ取り込みの概要 &#x200B;](/help/ingestion/batch-ingestion/overview.md) を参照してください。

メールアドレスを自分でハッシュ化することを選択する場合は、次の要件に必ず従ってください。

* 先頭と末尾のスペースを削除します。
* ASCII 文字をすべて小文字に変換します。
* メ `gmail.com` ルアドレスのユーザー名部分から、以下の文字を削除します。

      *期間（&#39;.&#39;） 文字（ASCII コード 46）。 例えば、「jane.doe@gmail.com」を「janedoe@gmail.com」に正規化します。
     * プラス記号（&#39;+&#39;）文字（ASCII コード 43）とそれに続くすべての文字。 例えば、「janedoe+home@gmail.com」を「janedoe@gmail.com」に正規化します。
  

## 電話番号の正規化とハッシュ化の要件 {#phone-hashing}

電話番号のアップロードに関して知っておくべき情報を以下に示します。

* リクエストでハッシュ化されているかハッシュ化されていないかに関係なく、リクエストで送信する前に電話番号を正規化する必要があります。
* 正規化、ハッシュ、エンコードされたデータをアップロードするには、電話番号を正規化された電話番号の Base64 でエンコードされた SHA-256 ハッシュとして送信する必要があります。

生の電話番号をアップロードする場合でも、ハッシュ化された電話番号をアップロードする場合でも、正規化する必要があります。

>[!IMPORTANT]
>
>ハッシュ前の正規化により、生成される ID 値が常に同じになり、データを正確に一致させることができます。

電話番号の正規化に関する要件について知っておく必要があるのは、次のとおりです。

* UID2 演算子は、グローバル一意性を確保する国際電話番号形式である E.164 形式の電話番号を受け入れます。
* E.164 の電話番号は最大 15 桁までです。
* 正規化された E.164 電話番号には、次の構文を使用します。`[+][country code][subscriber number including area code]` にスペース、ハイフン、括弧、その他の特殊文字を含めないでください。 以下に、いくつかの例を示します。

      * US: 1 （234） 567-8901 は+12345678901 に正規化されています。
     * シンガポール：65 1243 5678 は+6512345678 に正規化されています。
     * オーストラリア：携帯電話番号 0491 570 006 は、国コードを追加して先頭のゼロ（+61491570006）を削除するために正規化されています。
     * UK：国コードを追加して先頭の 0 を削除するために、345678 が正規化された携帯電話番号 07812 +447812345678.
  
正規化された電話番号が、UTF-16 などの別のエンコーディングシステムではなく、UTF-8 であることを確認します。

電話番号ハッシュは、正規化された電話番号の Base64 でエンコードされた SHA-256 ハッシュです。 電話番号は、最初に正規化され、次に SHA-256 ハッシュアルゴリズムを使用してハッシュされ、次に、ハッシュ値の結果のバイトが Base64 エンコーディングを使用してエンコードされます。 Base64 エンコーディングは、16 進数でエンコードされた文字列表現ではなく、ハッシュ値のバイトに適用されることに注意してください。
次の表に、単純な入力電話番号の例を示します。安全な不透明な値に到達するために、各手順を適用した結果です。

| タイプ | 例 | コメントと使用方法 |
|---|---|---|
| 生の電話番号 | 1 （234） 567-8901 | これが出発点です。 |
| 正規化された電話番号 | +12345678901 | 正規化は常に最初のステップです。 |
| 正規化された電話番号の SHA-256 ハッシュ | 10e6f0b47054a83359477dcb35231db6de5c69fb1816e1a6b98e192de9e5b9ee | この 64 文字の文字列は、32 バイトの SHA-256 を 16 進数でエンコードした表現です。 |
| 正規化されたハッシュ化された電話番号の 16 進数から Base64 SHA-256 エンコーディング | EObwtHBUqDNZR33LNSMdtt5cafsYFuGmuY4ZLenlue4 | この 44 文字の文字列は、32 バイトの SHA-256 を Base64 でエンコードした表現です。 SHA-256 ハッシュは 16 進数値です。 16 進値を入力として受け取る Base64 エンコーダーを使用する必要があります。 リクエスト本文で送信される phone_hash 値には、このエンコーディングを使用します。 |

>[!IMPORTANT]
>
>Base64 エンコーディングを適用する場合は、16 進値を入力として取る関数を使用してください。 テキストを入力として受け取る関数を使用すると、結果は、UID2 では無効な長い文字列になります。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | Trade Desk 宛先で使用される識別子（メールまたはハッシュ化されたメール）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Daily Batch]** | オーディエンスの評価に基づいてExperience Platform内でプロファイルを更新すると、プロファイル（ID）は 1 日 1 回ダウンストリームの宛先プラットフォームで更新されます。 詳しくは、[&#x200B; バッチエクスポート &#x200B;](/help/destinations/destination-types.md#file-based) を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

### 宛先に対する認証 {#authenticate}

CRM 宛先 [!DNL The Trade Desk]、毎日のバッチファイルアップロードであり、ユーザーによる認証を必要としません。

### 宛先の詳細の入力 {#fill-in-details}

オーディエンスデータを宛先に送信または有効化する前に、独自の宛先プラットフォームへの接続を設定する必要があります。 この宛先を[設定](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Account Type]**: **[!UICONTROL Existing Account]** オプションを選択してください。
* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Advertiser ID]**：お使いの [!DNL Trade Desk Advertiser ID]。[!DNL Trade Desk] アカウントマネージャーで共有するか、[!DNL Advertiser Preferences] UI の [!DNL Trade Desk] にあるアセットです。

![&#x200B; 宛先の詳細を入力する方法を示すExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/configuredestination2.png)

宛先に接続する場合、データガバナンスポリシーの設定は完全にオプションです。 詳しくは、Experience Platform[&#x200B; データガバナンスの概要 &#x200B;](/help/data-governance/policies/overview.md) を参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

宛先に対してオーディエンスをアクティブ化する手順については、[&#x200B; プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

**[!UICONTROL Scheduling]** ページでは、書き出す各オーディエンスのスケジュールとファイル名を設定できます。 スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

![&#x200B; オーディエンスのアクティベーションをスケジュールするためのExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment1.png)

>[!NOTE]
>
>CRM 宛先に対してアクティブ化 [!DNL The Trade Desk] れたすべてのオーディエンスは、毎日の頻度と完全なファイル書き出しに自動的に設定されます。

![&#x200B; オーディエンスのアクティベーションをスケジュールするためのExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment2.png)

**[!UICONTROL Mapping]** ページでは、ソース列から属性または ID 名前空間を選択し、ターゲット列にマッピングする必要があります。

![&#x200B; オーディエンスのアクティベーションをマッピングするためのExperience Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/mappingsegment1.png)

以下に、オーディエンスを CRM 宛先に対してアクティブ化する際の、正しい ID マッピング [!DNL The Trade Desk] 例を示します。

ソースフィールドとターゲットフィールドの選択：

| ソースフィールド | ターゲットフィールド |
|---|---|
| メール | メール |
| Email_LC_SHA256 | hashed_email |
| 電話（E.164） | 電話 |
| 電話（SHA256_E.164） | hashed_phone |
| TDID | tdid |
| GAID | daid |
| IDFA | idfa |
| UID2 | uid2 |
| UID2Token | uid2_token |
| EUID | euid |
| EUIDToken | euid_token |
| ランプ ID | idl |
| ID5 | id5 |
| netId | net_id |
| FirstID | first_id |


## データの書き出しを検証 {#validate}

データがExperience Platformから [!DNL The Trade Desk] に正しく書き出されていることを検証するには、「広告主データと ID」ライブラリ内の「Adobe 1PD」タブでオーディエンス [!DNL The Trade Desk] 見つけてください。 [!DNL Trade Desk] UI 内で対応する ID を見つける手順は次のとおりです。

1. まず、「**[!UICONTROL Libraries]**」タブを選択し、「**[!UICONTROL Advertiser data and identity]**」セクションを確認します。
2. **[!UICONTROL Adobe 1PD]** をクリックすると、アクティブ化されたすべてのオーディエンスが一覧表示さ [!DNL The Trade Desk] ます。
3. Experience Platformのセグメント名またはセグメント ID が、[!DNL Trade Desk] UI にセグメント名として表示されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

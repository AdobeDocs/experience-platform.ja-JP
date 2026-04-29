---
title: The Trade Desk - CRM接続
description: CRM データにもとづくオーディエンスのターゲティングと除外のために、Trade Desk アカウントにプロファイルをアクティベートします。
last-substantial-update: 2026-04-29T00:00:00Z
exl-id: e09eaede-5525-4a51-a0e6-00ed5fdc662b
source-git-commit: a052203dce4949bc795fe181821a8d890c341673
workflow-type: tm+mt
source-wordcount: '1861'
ht-degree: 9%

---

# [!DNL Trade Desk] - CRM接続

>[!IMPORTANT]
>
>[宛先カタログ &#x200B;](/help/destinations/catalog/overview.md)には、The Trade Desk - CRMの宛先が2つあります。
>
>* EU内でデータを取得する場合は、**[!DNL The Trade Desk - CRM (EU)]**&#x200B;宛先を使用します。
>* APACまたはNAMER地域でデータを取得する場合は、**[!DNL The Trade Desk - CRM (NAMER & APAC)]**&#x200B;の宛先を使用します。
>
>この宛先コネクタとドキュメント ページは、*[!DNL Trade Desk]* チームによって作成および管理されています。 問い合わせや更新のリクエストについては、[!DNL Trade Desk]担当者にお問い合わせください。

## 概要 {#overview}

CRM データに基づくオーディエンスのターゲティングと抑制のために、[!DNL Trade Desk] アカウントにプロファイルをアクティブ化する方法について説明します。

このコネクタは、ファーストパーティデータのアクティブ化のために[!DNL The Trade Desk]にデータを送信します。 [!DNL The Trade Desk]生の（ハッシュ化されていない）電子メールと電話番号を保存します。

>[!TIP]
>
>[!DNL The Trade Desk - CRM]の宛先を使用して、CRM データ （電子メールや電話番号など）およびCookieやデバイス IDなどの他のファーストパーティデータ IDを送信します。 引き続き、Experience Platform カタログの[Trade Deskの宛先](/help/destinations/catalog/advertising/tradedesk.md)をCookieとデバイス ID マッピングに使用できます。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>The Trade Deskにオーディエンスをアクティベートする前に、この機能を有効にするには、[!DNL Trade Desk] アカウントマネージャーに連絡する必要があります。 電子メール、電話番号、およびUID2/EUIDを送信する場合は、署名済みのUID2/EUID契約書を[!DNL The Trade Desk]と共有する必要があります。

## ID照合要件 {#id-matching-requirements}

[!DNL Adobe Experience Platform]に取り込むIDの種類に応じて、対応する要件を遵守する必要があります。 詳しくは、[ID名前空間の概要](/help/identity-service/features/namespaces.md)を参照してください。

## サポートされている ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明する ID のアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

[!DNL Adobe Experience Platform]では、ハッシュ化されていない電子メール アドレスとハッシュ化された電子メール アドレスおよび電話番号の両方をサポートしています。 「ID照合要件」セクションの手順に従い、プレーンテキストとハッシュ化されたメールアドレスにそれぞれ適切な名前空間を使用します。

| ターゲット ID | 説明 |
|---|---|
| メール | 電子メールアドレス（クリアテキスト） |
| Email_LC_SHA256 | メールアドレスは、SHA256と小文字を使用してハッシュ化する必要があります。 後でこの設定を変更することはできません。 |
| 電話（E.164） | E.164形式で正規化する必要がある電話番号。 E.164形式には、プラス記号（+）、国際電話コード、市外局番、電話番号が含まれます。 例：（+）（国コード）（市外局番）（電話番号）。 この識別子は、The Trade Desk - First-Party Data （EU）では使用できません。 |
| 電話（SHA256_E.164） | 既にE.164形式に正規化され、SHA-256を使用してハッシュ化され、結果として生じるハッシュ Base64 エンコードされた電話番号。 この識別子は、The Trade Desk - First-Party Data （EU）では使用できません。 |
| TDID | The Trade DeskでのCookie ID |
| GAID | GOOGLE ADVERTISING ID |
| IDFA | Apple の広告主 ID |
| UID2 | 生のUID2値 |
| UID2Token | 暗号化されたUID2 トークン（広告トークンとも呼ばれます）。 |
| EUID | 生の欧州連合ID値 |
| EUIDToken | 暗号化されたEUID トークン。広告トークンとも呼ばれます。 |
| RampID | 49文字または70文字のRampID （以前はIdentityLinkまたはIDLとして知られていました）。 これは、The Trade Desk専用にマッピングされたLiveRampIDである必要があります。 |
| netID | ユーザーのnetIDは、70文字のbase64でエンコードされた文字列です。 このIDはヨーロッパでのみサポートされています。 |
| FirstID | ユーザーのファーストパーティ Cookie。通常、フランスのメディア企業が設定します。 このIDはヨーロッパでのみサポートされています。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> Adobe Journey OptimizerやGoogle AnalyticsなどのExperience Platformアプリケーションで， </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## メールハッシュ要件 {#email-hashing}

メールアドレスを[!DNL Adobe Experience Platform]に取り込む前にハッシュ化するか、生のメールアドレスを使用できます。

Experience Platformでのメールアドレスの取り込みについて詳しくは、[&#x200B; バッチ取り込みの概要](/help/ingestion/batch-ingestion/overview.md)を参照してください。

自分でメールアドレスをハッシュ化することを選択した場合は、次の要件に必ず準拠してください。

* 先頭と末尾のスペースを削除します。
* すべてのASCII文字を小文字に変換します。
* `gmail.com`個の電子メールアドレスで、電子メールアドレスのユーザー名部分から次の文字を削除します。

      *ピリオド （&#39;.&#39;）文字（ASCII コード 46）。 例えば、「jane.doe@gmail.com」を「janedoe@gmail.com」に正規化します。
     * プラス記号（「+」）文字（ASCII コード 43）とその後のすべての文字。 例えば、「janedoe+home@gmail.com」を「janedoe@gmail.com」に標準化します。
  

## 電話番号の正規化とハッシュ化の要件 {#phone-hashing}

電話番号のアップロードについて知っておくべきことを以下に示します。

* 電話番号をリクエストで送信する前に、その電話番号をハッシュ化するかハッシュ化しないかを問わず、その電話番号をリクエストで送信する必要があります。
* 正規化、ハッシュ化、およびエンコード化されたデータをアップロードするには、正規化された電話番号のBase64 エンコードされたSHA-256 ハッシュとして電話番号を送信する必要があります。

生またはハッシュ化された電話番号をアップロードする場合は、それらを正規化する必要があります。

>[!IMPORTANT]
>
>ハッシュ前の正規化により、生成されるID値が常に同じになり、データを正確に照合できるようになります。

電話番号の正規化の要件について知っておくべきことを次に示します。

* UID2 オペレーターは、グローバルな一意性を保証する国際電話番号フォーマットであるE.164 フォーマットで電話番号を受け付けます。
* E.164の電話番号は、最大15桁までです。
* 正規化されたE.164の電話番号では、次の構文が使用されます：`[+][country code][subscriber number including area code]`。スペース、ハイフン、括弧、またはその他の特殊文字は使用されません。 以下に、いくつかの例を示します。

      * US: 1 （234） 567-8901が+12345678901に正規化されます。    * シンガポール：65 1243 5678が+6512345678に正規化されます。
     * オーストラリア：モバイル電話番号0491 570 0 006が正規化され、国コードが追加され、国コードが削除されます。+61491570006.
     *：携帯電話番号07812国コードコードが追加され、先頭先頭が開始され、先頭がされ、先頭0ががされ、国がが0に345678447812345678 0がり
。  

正規化された電話番号がUTF-16などの別のエンコーディングシステムではなく、UTF-8であることを確認します。

電話番号ハッシュは、正規化された電話番号のBase64でエンコードされたSHA-256 ハッシュです。 電話番号は最初に正規化され、次にSHA-256 ハッシュアルゴリズムを使用してハッシュ化され、その後ハッシュ値のバイトはBase64 エンコーディングを使用してエンコードされます。 Base64 エンコーディングは、16進エンコードされた文字列表現ではなく、ハッシュ値のバイトに適用されます。
次の表は、簡単な入力電話番号の例を示しており、各ステップの結果が適用されて、安全で不透明な値に到達します。

| タイプ | 例 | コメントと使用状況 |
|---|---|---|
| 未加工の電話番号 | 1 (234) 567-8901 | これが出発点です。 |
| 正規化された電話番号 | +12345678901 | 標準化は常に最初のステップです。 |
| 正規化された電話番号のSHA-256 ハッシュ | 10e6f0b47054a83359477dcb35231db6de5c69fb1816e1a6b98e192de9e5b9ee | この64文字の文字列は、32 バイトのSHA-256を16進数でエンコードした表現です。 |
| 正規化およびハッシュ化された電話番号の16進数からBase64へのSHA-256 エンコーディング | EObwtHBUqDNZR33LNSMdtt5cafsYFuGmuY4ZLenlue4 | この44文字の文字列は、32 バイトのSHA-256のBase64 エンコードされた表現です。 SHA-256 ハッシュは16進数値です。 16進値を入力として取り込むBase64 エンコーダーを使用する必要があります。 このエンコーディングは、リクエスト本文で送信されるphone_hash値に使用します。 |

>[!IMPORTANT]
>
>Base64 エンコーディングを適用する場合は、必ず16進値を入力として取り込む関数を使用してください。 テキストを入力として受け取る関数を使用する場合、結果は長い文字列になり、UID2では無効です。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディエンスのすべてのメンバーを、Trade Deskの宛先で使用されている識別子（メールまたはハッシュ化されたメール）で書き出します。 |
| 書き出し頻度 | **[!UICONTROL Daily Batch]** | オーディエンスの評価に基づいてExperience Platformでプロファイルが更新されると、プロファイル（ID）は1日1回下流の宛先プラットフォームに更新されます。 [&#x200B; バッチ書き出し](/help/destinations/destination-types.md#file-based)の詳細をご覧ください。 |

{style="table-layout:auto"}

>[!NOTE]
>
>現在&#x200B;**[ファイルの書き出し](/help/destinations/ui/export-file-now.md)**&#x200B;機能は、[!DNL The Trade Desk] CRM宛先では使用できません。 オーディエンスを書き出すには、[&#x200B; スケジュールされた毎日のバッチ書き出し](#activate)を使用します。

## 宛先への接続 {#connect}

### 宛先に対する認証 {#authenticate}

[!DNL The Trade Desk] CRMの宛先は、毎日のバッチ ファイルのアップロードであり、ユーザーによる認証は必要ありません。

### 宛先の詳細を入力 {#fill-in-details}

オーディエンスデータを宛先に送信またはアクティベートする前に、独自の宛先プラットフォームへの接続を設定する必要があります。 この宛先を[設定](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Account Type]**: **[!UICONTROL Existing Account]** オプションを選択してください。
* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Advertiser ID]**: [!DNL Trade Desk Advertiser ID]。これは、[!DNL Trade Desk] アカウントマネージャーが共有するか、[!DNL Trade Desk] UIの[!DNL Advertiser Preferences]で見つけることができます。

宛先の詳細を入力する方法を示す![Experience Platform UIのスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/configuredestination2.png)

宛先に接続する場合、データガバナンスポリシーの設定は完全にオプションです。 詳しくは、Experience Platform [&#x200B; データガバナンスの概要](/help/data-governance/policies/overview.md)を参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![&#x200B; ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

宛先に対するオーディエンスのアクティブ化に関する手順については、[&#x200B; バッチプロファイル書き出し宛先へのオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。

**[!UICONTROL Scheduling]** ページでは、書き出す各オーディエンスのスケジュールとファイル名を設定できます。 スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

オーディエンスのアクティブ化をスケジュールする![Experience Platform UIのスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment1.png)

>[!NOTE]
>
>[!DNL The Trade Desk] CRM宛先にアクティブ化されたすべてのオーディエンスは、自動的に日次の頻度と完全なファイル書き出しに設定されます。

オーディエンスのアクティブ化をスケジュールする![Experience Platform UIのスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment2.png)

**[!UICONTROL Mapping]** ページで、ソース列から属性またはID名前空間を選択し、ターゲット列にマッピングする必要があります。

オーディエンスのアクティブ化をマップする![Experience Platform UIのスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/mappingsegment1.png)

以下は、オーディエンスを[!DNL The Trade Desk] CRM宛先にアクティブ化する際の正しいID マッピングの例です。

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
| RampID | idl |
| ID5 | id5 |
| netID | net_id |
| FirstID | first_id |

{style="table-layout:auto"}

## データ書き出しの検証 {#validate}

データがExperience Platformから[!DNL The Trade Desk]に正しく書き出されていることを検証するには、[!DNL The Trade Desk] 「広告主データとID」ライブラリ内の「Adobe 1PD」タブでオーディエンスを見つけてください。 [!DNL Trade Desk] UI内で対応するIDを見つける手順は次のとおりです。

1. まず、**[!UICONTROL Libraries]** タブを選択し、**[!UICONTROL Advertiser data and identity]** セクションを確認します。
2. **[!UICONTROL Adobe 1PD]**&#x200B;を選択すると、[!DNL The Trade Desk]に対してアクティブ化されたすべてのオーディエンスが一覧表示されます。
3. Experience Platformのセグメント名またはセグメント IDは、[!DNL Trade Desk] UIにセグメント名として表示されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

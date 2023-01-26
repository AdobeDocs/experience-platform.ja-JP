---
title: （ベータ版）トレードデスク — CRM 接続
description: CRM データに基づいて、オーディエンスのターゲティングと抑制のために、トレードデスクアカウントにプロファイルをアクティブ化します。
last-substantial-update: 2023-01-25T00:00:00Z
exl-id: e09eaede-5525-4a51-a0e6-00ed5fdc662b
source-git-commit: 83778bc5d643f69e0393c0a7767fef8a4e8f66e9
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 16%

---

# （ベータ版）[!DNL Trade Desk]- CRM 接続

>[!IMPORTANT]
>
>[!DNL The Trade Desk - CRM] の宛先は、現在ベータ版です。 ドキュメントと機能は変更される場合があります。
>
>EUID（ヨーロッパの統合 ID）のリリースに伴い、次の 2 つが表示されるようになりました [!DNL The Trade Desk - CRM] の宛先 [宛先カタログ](/help/destinations/catalog/overview.md).
>* EU でデータをソースする場合は、 **[!DNL The Trade Desk - CRM (EU)]** 宛先。
>* APAC または NAMER 地域のデータをソースにする場合は、 **[!DNL The Trade Desk - CRM (NAMER & APAC)]** 宛先。
>
>現在、Experience Platformの両方の宛先はベータ版です。 このドキュメントページは、 *[!DNL Trade Desk]* チーム。 お問い合わせや更新のご依頼については、 [!DNL Trade Desk] 担当者は、ドキュメントと機能を変更する場合があります。

## 概要 {#overview}

このドキュメントは、 [!DNL Trade Desk] CRM データに基づくオーディエンスのターゲティングおよび抑制のアカウント。

[!DNL The Trade Desk(TTD)] は、いつでも電子メールアドレスのアップロードファイルを直接処理しません。また、 [!DNL The Trade Desk] 生の（ハッシュ化されていない）電子メールを保存します。

>[!TIP]
>
>用途 [!DNL The Trade Desk] CRM データマッピング用の CRM の宛先（電子メールやハッシュ化された電子メールアドレスなど）。 以下を使用： [その他のトレードデスクの宛先](/help/destinations/catalog/advertising/tradedesk.md) ( Adobe Experience Platformカタログの cookie とデバイス ID のマッピング )

## 前提条件 {#prerequisites}

セグメントをアクティブ化する前に [!DNL The Trade Desk]を使用する場合、 [!DNL The Trade Desk] CRM オンボーディング契約に署名するアカウントマネージャー。 [!DNL The Trade Desk] が権限を付与し、広告主 ID を共有して宛先を設定します。

## ID 一致の要件 {#id-matching-requirements}

Adobe Experience Platformに取り込む ID のタイプに応じて、対応する要件を満たす必要があります。 詳しくは、 [ID 名前空間の概要](/help/identity-service/namespaces.md) を参照してください。

## サポートされる ID {#supported-identities}

[!DNL The Trade Desk] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](/help/identity-service/namespaces.md) についての詳細情報。

プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 「 ID 一致要件」の節の手順に従い、プレーンテキストとハッシュ化された電子メールアドレスにそれぞれ適切な名前空間を使用します。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | 電子メールアドレス（クリアテキスト） | 入力 `email` ソース id が E メールの名前空間または属性の場合、をターゲット id として使用します。 |
| Email_LC_SHA256 | 電子メールアドレスは、SHA256 を使用してハッシュ化し、小文字にする必要があります。 必ず次の手順に従ってください。 [電子メールの正規化](https://github.com/UnifiedID2/uid2docs/tree/main/api#email-address-normalization) ルールが必要です。 後でこの設定を変更することはできません。 | 入力 `hashed_email` ソース id が Email_LC_SHA256 名前空間または属性の場合、をターゲット id として設定します。 |

{style=&quot;table-layout:auto&quot;}

## 電子メールのハッシュ要件 {#hashing-requirements}

電子メールアドレスをAdobe Experience Platformに取り込む前にハッシュ化したり、生の電子メールアドレスを使用したりできます。

E メールアドレスの取り込みについては、Experience Platform [バッチ取得の概要](/help/ingestion/batch-ingestion/overview.md).

電子メールアドレスを自分でハッシュ化する場合は、次の要件に従ってください。

* 先頭と末尾の空白を削除します。
* すべての ASCII 文字を小文字に変換します。
* In `gmail.com` 電子メールアドレスで、電子メールアドレスのユーザー名部分から次の文字を削除します。
   * ピリオド (. （ASCII コード 46）)。 例えば、normalize `jane.doe@gmail.com` から `janedoe@gmail.com`.
   * プラス記号 (+ （ASCII コード 43）) とそれ以降のすべての文字。 例えば、normalize `janedoe+home@gmail.com` から `janedoe@gmail.com`.

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | セグメント（オーディエンス）のすべてのメンバーを、トレードデスクの宛先で使用される識別子（電子メールまたはハッシュ化された電子メール）で書き出します。 |
| 書き出し頻度 | **[!UICONTROL 日別バッチ]** | セグメントの評価に基づいてExperience Platform内でプロファイルが更新されると、プロファイル (ID) は、宛先プラットフォームの下流にある 1 日 1 回更新されます。 詳細を表示 [バッチエクスポート](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## 宛先への接続 {#connect}

### 宛先に対する認証 {#authenticate}

[!DNL The Trade Desk] CRM 宛先は毎日のバッチファイルアップロードで、ユーザーによる認証は必要ありません。

### 宛先の詳細を入力 {#fill-in-details}

オーディエンスデータを宛先に送信またはアクティブ化する前に、独自の宛先プラットフォームへの接続を設定する必要があります。 この宛先を[設定](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=en)するとき、次の情報を指定する必要があります。

* **[!UICONTROL アカウントタイプ]**:を選択してください。 **[!UICONTROL 既存のアカウント]** オプション。
* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL 広告主 ID]**:あなたの [!DNL Trade Desk Advertiser ID]は、 [!DNL Trade Desk] アカウントマネージャーまたは次の場所にあります： [!DNL Advertiser Preferences] 内 [!DNL Trade Desk] UI

![宛先の詳細を入力する方法を示す Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/configuredestination2.png)

宛先に接続する場合、データガバナンスポリシーの設定は完全にオプションです。 Experience Platform [データガバナンスの概要](/help/data-governance/policies/overview.md) を参照してください。

## この宛先に対してセグメントをアクティブ化 {#activate}

読み取り [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md) 宛先に対するオーディエンスセグメントをアクティブ化する手順について説明します。

**[!UICONTROL スケジュール]**&#x200B;ページでは、書き出す各セグメントのスケジュールとファイル名を設定できます。スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

![セグメントのアクティベーションをスケジュールするための Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment1.png)

>[!NOTE]
>
>アクティブ化されたすべてのセグメント [!DNL The Trade Desk] CRM の宛先は、毎日の頻度とフルファイルエクスポートに自動的に設定されます。

![セグメントのアクティベーションをスケジュールするための Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/schedulesegment2.png)

内 **[!UICONTROL マッピング]** 」ページで、属性または id 名前空間をソース列から選択し、ターゲット列にマッピングする必要があります。

![セグメントのアクティベーションをマッピングするための Platform UI のスクリーンショット。](/help/destinations/assets/catalog/advertising/tradedesk/mappingsegment1.png)

セグメントをアクティブ化する際の正しい ID マッピングの例を以下に示します。 [!DNL The Trade Desk] CRM の宛先。

>[!IMPORTANT]
>
> [!DNL The Trade Desk] CRM 宛先は、同じアクティベーションフロー内の ID として、生の電子メールアドレスおよびハッシュ化された電子メールアドレスを受け取りません。 生の電子メールアドレスとハッシュ化された電子メールアドレスに対して、個別のアクティベーションフローを作成します。

ソースフィールドを選択しています。

* を選択します。 `Email` データ取り込み時に生の電子メールアドレスを使用する場合は、名前空間または属性をソース id として指定します。
* を選択します。 `Email_LC_SHA256` 名前空間または属性（Platform へのデータ取り込み時に顧客の電子メールアドレスをハッシュ化した場合）。

ターゲットフィールドの選択：

* 入力  `email` ソースの名前空間または属性がターゲット ID として使用される場合 `Email`.
* 入力  `hashed_email` ソースの名前空間または属性がターゲット ID として使用される場合 `Email_LC_SHA256`.

## データ書き出しの検証 {#validate}

データがExperience Platformから、およびに正しくエクスポートされたことを検証するには、以下を実行します。 [!DNL The Trade Desk]の下のAdobe1PD データタイルの下にセグメントがあります。 [!DNL The Trade Desk] データ管理プラットフォーム (DMP)。 次に、 [!DNL Trade Desk] UI:

1. まず、 **[!UICONTROL データ]** タブとレビュー **[!UICONTROL ファーストパーティ]**.
2. ページを下にスクロールし、の下に移動します。 **[!UICONTROL インポートされたデータ]**&#x200B;を検索すると、 **[!UICONTROL Adobe1PD タイル]**.
3. をクリックします**[!UICONTROL Adobe1PD]**タイルに表示され、 [!DNL Trade Desk] 広告主の宛先。 また、検索機能を使用することもできます。
4. セグメント ID # fromExperience Platformは、 [!DNL Trade Desk] UI

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

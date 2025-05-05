---
title: Linkedin Conversions API イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能を使用すると、Linkedin マーケティングキャンペーンのパフォーマンスを測定できます。
last-substantial-update: 2023-10-25T00:00:00Z
exl-id: 411e7b77-081e-4139-ba34-04468e519ea5
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 3%

---

# [!DNL LinkedIn] Conversions API 拡張機能

[[!DNL LinkedIn Conversions API]](https://learn.microsoft.com/en-us/linkedin/marketing/integrations/ads-reporting/conversions-api) は、広告主のサーバーと [!DNL LinkedIn] からのマーケティングデータの間に直接接続を作成するコンバージョントラッキングツールです。 これにより、広告主は、コンバージョンの場所に関係なく、[!DNL LinkedIn] ールマーケティングキャンペーンの有効性を評価し、その情報を利用してキャンペーンの最適化を推進できます。 [!DNL LinkedIn Conversions API] 拡張機能は、より完全なアトリビューション、データ信頼性の向上、最適化された配信により、パフォーマンスを強化し、アクションあたりのコストを削減するのに役立ちます。

## 前提条件 {#prerequisites}

[!DNL LinkedIn Campaign Manager] アカウントで [ コンバージョンルールを作成 ](https://www.linkedin.com/help/lms/answer/a1657171) する必要があります。 [!DNL Adobe] では、設定した他のコンバージョンルールタイプとは別に設定するために、会話ルール名の先頭に「CAPI」を含めることをお勧めします。

### 秘密鍵およびデータ要素の作成

新しい [!DNL LinkedIn][ イベント転送の秘密鍵 ](../../../ui/event-forwarding/secrets.md) を作成し、認証メンバーを示す一意の名前を指定します。 これは、値を保護しながら、アカウントへの接続を認証するために使用されます。

次に、[Core] 拡張機能と [!UICONTROL Secret] データ要素タイプを使用して (../../../ui/managing-resources/data-elements.md#create-a-data-element) データ要素を作成  し、作成した `LinkedIn` シークレットを参照します。

## [!DNL LinkedIn] 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[ イベント転送プロパティを作成 ](../../../ui/event-forwarding/overview.md#properties) するか、編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。「**[!UICONTROL カタログ]**」タブで、**[!UICONTROL LinkedIn]** 拡張機能を選択してから「**[!UICONTROL インストール]**」を選択します。

![ インストールを強調表示した [!DNL LinkedIn] 拡張機能カードを示す拡張機能カタログ ](../../../images/extensions/server/linkedin/install-extension.png)

次の画面で、前に作成したデータ要素の秘密鍵を「`Access Token`」フィールドに入力します。 データ要素の秘密鍵には、[!DNL LinkedIn] の OAuth 2 トークンが含まれます。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![ 「[!UICONTROL &#x200B; アクセストークン &#x200B;] フィールドと「[!UICONTROL &#x200B; 保存 &#x200B;] がハイライト表示された [!DNL LinkedIn] 拡張機能の設定ページ ](../../../images/extensions/server/linkedin/configure-extension.png)

## [!DNL Send Conversion] ルールの作成 {#tracking-rule}

すべてのデータ要素を設定したら、イベントを [!DNL LinkedIn] に送信するタイミングと方法を決定するイベント転送ルールの作成を開始できます。

イベント転送プロパティに新しいイベント転送 [ ルール ](../../../ui/managing-resources/rules.md) を作成します。 「**[!UICONTROL アクション]**」で、新しいアクションを追加し、拡張機能を「**[!UICONTROL LinkedIn]**」に設定します。 次に、「アクションタイプ **[!UICONTROL で「**&#x200B;[!UICONTROL &#x200B; コンバージョンを送信 &#x200B;]&#x200B;**」を選択]** ます。

![ イベント転送ルールのアクション設定を追加するために必要なフィールドがハイライト表示されたイベント転送プロパティルール ビュー。](../../../images/extensions/server/linkedin/linkedin-event-action.png)

選択後、追加のコントロールが表示され、イベントをさらに設定できます。 「**[!UICONTROL 変更を保持]**」を選択して、ルールを保存します。

**[!UICONTROL ユーザーデータ]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL メール] | コンバージョンイベントに関連付けられた連絡先のメールアドレス。 指定した値が既に SHA256 文字列でない限り、メール値は SHA256 の拡張機能コードでエンコードされます。 |
| [!UICONTROL LinkedInのファーストパーティ広告トラッキング UUID] | これはファーストパーティ cookie ID です。 クリック URL にクリック ID パラメーター `li_fat_id` を付加したファーストパーティ Cookie を有効化するには、広告主は [[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/help/lms/answer/a423304/enable-first-party-cookies-on-a-linkedin-insight-tag) からの拡張コンバージョントラッキングを有効にする必要があります。 |
| [!UICONTROL &#x200B; 顧客情報データ &#x200B;] | このフィールドには、メッセージと共に送信される追加の属性を含む JSON オブジェクトが含まれています。<br><br> 「**[!UICONTROL Raw]**」オプションで、JSON オブジェクトを指定されたテキストフィールドに直接貼り付けるか、データ要素アイコン（![ データセットアイコン ](/help/images/icons/database.png)）を選択して、データを表す既存のデータ要素のリストから選択できます。<br><br> また、「**[!UICONTROL JSON キーと値のペア エディター]**」オプションを使用し、UI エディターを使用して各キーと値のペアを手動で追加することもできます。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 使用できるキー値は、`firstName`、`lastName`、`companyName`、`title`、`country` です。 |

{style="table-layout:auto"}

![ フィールドへのデータ入力の例を示す [!DNL User Data] の節。](../../../images/extensions/server/linkedin/configure-extension-user-data.png)

**[!UICONTROL コンバージョンデータ]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; コンバージョン &#x200B;] | [LinkedIn Campaign Manager&rbrace; で作成されたコンバージョンルール ](https://www.linkedin.com/help/lms/answer/a1657171)ID。 コンバージョンルールを選択して ID を取得し、ブラウザーの URL から ID をコピーします（例：`/campaignmanager/accounts/508111232/conversions/15588877`）。`/conversions/<id>` |
| [!UICONTROL &#x200B; コンバージョン時間 &#x200B;] | コンバージョンイベントが発生した各タイムスタンプ（ミリ秒単位）。 <br><br> メモ：ソースがコンバージョンタイムスタンプを秒単位で記録する場合は、末尾に 000 を挿入してミリ秒に変換してください。 |
| [!UICONTROL 通貨] | ISO 形式の通貨コード。 |
| [!UICONTROL &#x200B; 量 &#x200B;] | 10 進文字列でのコンバージョンの値（例：「100.05」）。 |
| [!UICONTROL &#x200B; イベント ID] | 各イベントを示すために広告主によって生成される一意の ID。 これはオプションのフィールドで、[ 重複排除 ](https://learn.microsoft.com/en-us/linkedin/marketing/conversions/deduplication?view=li-lms-2024-02) に使用されます。 |

{style="table-layout:auto"}

![ フィールドにサンプルデータを表示する [!DNL Conversion Data] のセクション。](../../../images/extensions/server/linkedin/configure-extension-conversions-data.png)

**[!UICONTROL 設定の上書き]**

>メモ
>
>[!UICONTROL &#x200B; 設定の上書き &#x200B;] フィールドを使用すると、ユーザーはルールごとに異なる [!DNL LinkedIn] アクセストークンを設定でき、各ルールで、異なる [!DNL LinkedIn] ad アカウントへのアクセス権を持つアクセストークンを使用できます。

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; アクセストークン &#x200B;] | [!DNL LinkedIn] アクセストークン。 |

![ フィールドへのデータ入力の例を示す [!DNL Configuration Overrides] の節。](../../../images/extensions/server/linkedin/configure-extension-configuration-override.png)

## 次の手順

このガイドでは、[!DNL LinkedIn Conversions API] イベント転送拡張機能を使用して [!DNL LinkedIn] にデータを送信する方法について説明しました。 [!DNL Adobe Experience Platform] のイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。

イベント デバッガーとExperience Platform転送モニタリング ツールを使用して実装をデバッグする方法について詳しくは、[Adobe Experience Platform Debuggerの概要 ](../../../../debugger/home.md) および [ イベント転送でのアクティビティの監視 ](../../../ui/event-forwarding/monitoring.md) を参照してください。

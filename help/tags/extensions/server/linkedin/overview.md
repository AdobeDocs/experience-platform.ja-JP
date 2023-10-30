---
title: LinkedIn コンバージョン API イベント転送拡張機能
description: このAdobe Experience Platformイベント転送拡張機能を使用すると、Linkedin マーケティングキャンペーンのパフォーマンスを測定できます。
last-substantial-update: 2023-10-25T00:00:00Z
exl-id: 411e7b77-081e-4139-ba34-04468e519ea5
source-git-commit: 308d07cf0c3b4096ca934a9008a13bf425dc30b6
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 4%

---

# [!DNL LinkedIn] Conversions API 拡張機能

[[!DNL LinkedIn Conversions API]](https://learn.microsoft.com/en-us/linkedin/marketing/integrations/ads-reporting/conversions-api) は、広告主のサーバーから、 [!DNL LinkedIn]. これにより、広告主は、 [!DNL LinkedIn] コンバージョンの場所に関係なく、マーケティングキャンペーンを作成し、この情報を利用してキャンペーンの最適化を推進します。 The [!DNL LinkedIn Conversions API] 拡張機能は、より完全な属性を提供し、データの信頼性を向上し、配信を最適化して、パフォーマンスを強化し、アクションあたりのコストを削減するのに役立ちます。

## 前提条件 {#prerequisites}

変換ルールは、 [!DNL LinkedIn] キャンペーン広告アカウント。 [!DNL Adobe] では、会話ルール名の先頭に「CAPI」を含めて、設定した他のコンバージョンルールタイプと切り離すことをお勧めします。

### シークレットとデータ要素の作成

新規作成 [!DNL LinkedIn] [イベント転送秘密鍵](../../../ui/event-forwarding/secrets.md) 認証メンバーを示す一意の名前を指定します。 これは、値のセキュリティを維持しながら、アカウントへの接続を認証するために使用されます。

次に、 [データ要素の作成](../../../ui/managing-resources/data-elements.md#create-a-data-element) の使用 [!UICONTROL コア] 拡張機能と [!UICONTROL 秘密鍵] を参照するデータ要素タイプ `LinkedIn` 作成した秘密鍵。

## のインストールと設定 [!DNL LinkedIn] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。Adobe Analytics の **[!UICONTROL カタログ]** タブで、 **[!UICONTROL LinkedIn]** 拡張機能を選択し、「 **[!UICONTROL インストール]**.

![次を表示する拡張機能カタログ： [!DNL LinkedIn] 拡張機能カードのハイライト表示のインストール。](../../../images/extensions/server/linkedin/install-extension.png)

次の画面で、前の手順で作成したデータ要素秘密鍵を `Access Token` フィールドに入力します。 データ要素の秘密鍵には、 [!DNL LinkedIn] OAuth 2 トークン。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![The [!DNL LinkedIn] を含む拡張機能の設定ページ [!UICONTROL アクセストークン] フィールドと [!UICONTROL 保存] ハイライト表示されました。](../../../images/extensions/server/linkedin/configure-extension.png)

## の作成 [!DNL Send Conversion] ルール {#tracking-rule}

すべてのデータ要素を設定したら、イベントの送信先と送信方法を決定するイベント転送ルールの作成を開始できます。 [!DNL LinkedIn].

新しいイベント転送の作成 [ルール](../../../ui/managing-resources/rules.md) を設定します。 の下 **[!UICONTROL アクション]**、新しいアクションを追加し、拡張機能をに設定します。 **[!UICONTROL LinkedIn]**. 次に、「 **[!UICONTROL Web 変換を送信]** （の） **[!UICONTROL アクションタイプ]**.

![[Event Forwarding Property Rules] ビューで、イベント転送ルールのアクション設定を追加するために必要なフィールドがハイライト表示されています。](../../../images/extensions/server/linkedin/linkedin-event-action.png)

選択後、イベントをさらに設定するための追加のコントロールが表示されます。 選択 **[!UICONTROL 変更を保持]** 」と入力してルールを保存します。

**[!UICONTROL ユーザーデータ]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL メール] | コンバージョンイベントに関連付けられた連絡先の電子メールアドレス。 電子メール値は、指定された値が既に SHA256 文字列でない限り、SHA256 の拡張コードによってエンコードされます。 |
| [!UICONTROL LinkedInファーストパーティ広告トラッキング UUID] | これはファーストパーティ Cookie ID です。 広告主は、次の拡張コンバージョントラッキングを有効にする必要があります： [[!DNL LinkedIn Campaign Manager]](https://www.linkedin.com/help/lms/answer/a423304/enable-first-party-cookies-on-a-linkedin-insight-tag) クリック ID パラメーターを追加したファーストパーティ cookie をアクティブ化するために `li_fat_id` をクリックして、「URL」をクリックします。 |
| [!UICONTROL 顧客情報データ] | このフィールドには、メッセージと共に送信される追加の属性を含む JSON オブジェクトが含まれています。<br><br>の下 **[!UICONTROL 生データ]** 」オプションを使用する場合は、指定したテキストフィールドに JSON オブジェクトを直接貼り付けるか、データ要素アイコン (![データセットアイコン](../../../images/extensions/server/aws/data-element-icon.png)) をクリックして、データを表す既存のデータ要素のリストから選択します。<br><br>また、 **[!UICONTROL JSON キーと値のペアエディター]** UI エディターを使用して各キーと値のペアを手動で追加するオプションが追加されました。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 使用できるキー値は次のとおりです。 `firstName`, `lastName`, `companyName`, `title` および `country`. |

{style="table-layout:auto"}

![The [!DNL User Data] セクションに、フィールドへのデータ入力例を示します。](../../../images/extensions/server/linkedin/configure-extension-user-data.png)

**[!UICONTROL コンバージョンデータ]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL コンバージョン] | 作成されたコンバージョンルールの ID。 [LinkedIn Campaign Manager](https://www.linkedin.com/help/lms/answer/a1657171) または [!DNL LinkedIn Campaign Manager]. |
| [!UICONTROL コンバージョン時間] | コンバージョンイベントが発生した各タイムスタンプ（ミリ秒単位）。 <br><br> 注意：ソースが変換タイムスタンプを秒単位で記録している場合、最後に 000 を挿入して、ミリ秒に変換してください。 |
| [!UICONTROL 通貨] | ISO 形式の通貨コード。 |
| [!UICONTROL 量] | 10 進数文字列での変換の値（例：「100.05」）。 |
| [!UICONTROL イベント ID] | 各イベントを示す広告主によって生成される一意の ID。 これはオプションのフィールドで、重複排除に使用します。 |

{style="table-layout:auto"}

![The [!DNL Conversion Data] セクションに、フィールド内のサンプルデータが表示されます。](../../../images/extensions/server/linkedin/configure-extension-conversions-data.png)

**[!UICONTROL 設定の上書き]**

>メモ
>
>The [!UICONTROL 設定の上書き] フィールドを使用すると、ユーザーは [!DNL LinkedIn] 各ルールのアクセストークン（異なるルールへのアクセス権を持つアクセストークンを各ルールで使用可能） [!DNL LinkedIn] 広告アカウント。

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL アクセストークン] | The [!DNL LinkedIn] アクセストークン。 |

![The [!DNL Configuration Overrides] 「 」セクションに、フィールドに入力されたデータの例を示します。](../../../images/extensions/server/linkedin/configure-extension-configuration-override.png)

## 次の手順

このガイドでは、にデータを送信する方法について説明しました。 [!DNL LinkedIn] の使用 [!DNL LinkedIn Conversions API] イベント転送拡張機能。 でのイベント転送機能の詳細については、 [!DNL Adobe Experience Platform]（を参照） [イベント転送の概要](../../../ui/event-forwarding/overview.md).

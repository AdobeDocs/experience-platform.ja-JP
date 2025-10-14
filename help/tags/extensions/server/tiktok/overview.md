---
title: Adobe TikTok Web Events API 拡張機能の統合
description: このAdobe Experience Platform Web Events API を使用すると、web サイトとのインタラクションをTikTokと直接共有できます。
last-substantial-update: 2023-09-26T00:00:00Z
exl-id: 14b8e498-8ed5-4330-b1fa-43fd1687c201
source-git-commit: 7f3459f678c74ead1d733304702309522dd0018b
workflow-type: tm+mt
source-wordcount: '1105'
ht-degree: 4%

---

# [!DNL TikTok] web イベント API 拡張機能の概要

[!DNL TikTok] events API は、Web サイト上のユーザーアクションに関する情報を [!DNL TikTok] と直接共有できる、セキュアな [Edge Network API](https://developer.adobe.com/data-collection-apis/docs/) インターフェイスです。 イベント転送ルールを活用し、[!DNL TikTok] Web イベント API 拡張機能を使用して、[!DNL Adobe Experience Platform Edge Network] から [!DNL TikTok] にデータを送信できます。

## [!DNL TikTok] 前提条件 {#prerequisites}

[!DNL TikTok] Web イベント API を設定して [!DNL TikTok] イベント API を使用するには、[!DNL TikTok] ピクセルコードとアクセストークンを生成する必要があります。

パートナー設定を使用して [!DNL TikTok] ピクセルを作成するには、ビジネスアカウントの有効な [!DNL TikTok] が必要です。 アカウントをまだお持ちでない場合は、[[!DNL TikTok]  ビジネス登録ページ用 &#x200B;](https://www.tiktok.com/business/en-US/solutions/business-account) に移動し、アカウントを登録して作成してください。

パートナー設定を使用して [!DNL TikTok] Pixel を設定するには、ビジネスアカウントにログインする必要があります。 これを行うには、以下の手順に従います。

1. 「**[!UICONTROL Assets]**」タブに移動し、「**[!UICONTROL イベント]**」を選択します。
2. 「Web イベント」で、「**[!UICONTROL 管理]**」を選択します。
3. **[!UICONTROL Web イベントを設定]** を選択します。
4. 接続方法として **[!UICONTROL パートナー設定]** を選択します。

[!DNL TikTok] ピクセルのセットアップ方法について詳しくは、[&#x200B; ピクセルの基本を学ぶ &#x200B;](https://ads.tiktok.com/help/article/get-started-pixel) ガイドを参照してください。

ピクセルが正常に作成されたら、アクセストークンを生成できます。 これを行うには、ピクセルに移動し、「**[!UICONTROL 設定]**」タブを選択します。 イベント API で、「**[!UICONTROL アクセストークンを生成]**」を選択します。

ピクセルコードとアクセストークンの設定方法について詳しくは、[[!DNL TikTok]  はじめる前に &#x200B;](https://business-api.tiktok.com/portal/docs?id=1739584855420929) を参照してください。

## [!DNL TikTok] Web イベント API 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択します。 「**[!UICONTROL カタログ]**」タブで、「**[!UICONTROL TikTok Web Events API Extension」を選択し]** 「**[!UICONTROL インストール]**」を選択します。

![&#x200B; インストールを強調表示した [!DNL TikTok] 拡張機能カードを示す拡張機能カタログ &#x200B;](../../../images/extensions/server/tiktok/install-extension.png)

次の画面で、以前に [!DNL TikTok] Ads Manager から生成した次の設定値を入力します。

* **[!UICONTROL ピクセルコード]**
* **[!UICONTROL アクセストークン]**

完了したら「**[!UICONTROL 保存]**」を選択します。

[!DNL TikTok] Web ![[!DNL TikTok] API 拡張機能の設定画面 &#x200B;](../../../images/extensions/server/tiktok/configure.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントを [!DNL TikTok] に送信するタイミングと方法を決定するイベント転送ルールの作成を開始できます。

イベント転送プロパティに新しい [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成します。 **[!UICONTROL Actions]** の下に新しいアクションを追加し、拡張機能を **[!UICONTROL TikTok Web Events API 拡張機能]** に設定します。 Edge Network イベントを [!DNL TikTok] に送信するには、**[!UICONTROL アクションタイプ]** を **[!UICONTROL TikTok Web Events API イベントを送信 &#x200B;] に設定します。**

![Send TikTok Web Events API Event[!UICONTROL &#x200B; アクションタイプが &#x200B;]Data Collection UI の [!DNL TikTok] ルール用に選択されている。](../../../images/extensions/server/tiktok/select-action.png)

選択後、以下に示すように、追加のコントロールでイベントをさらに設定できます。 完了したら、「**[!UICONTROL 変更を保持]**」を選択してルールを保存します。

**[!UICONTROL Web イベントおよびパラメーター]**

Web イベントおよびパラメーターには、イベントに関する一般的な情報が含まれます。 標準イベントは [!DNL TikTok] 統合ツール全体でサポートされており、レポート、コンバージョンの最適化、オーディエンスの構築に使用できます。

| 入力 | 説明 |
| --- | --- |
| イベント名 | イベントの名前。 これらは、[!DNL TikTok] が作成した事前定義済みの名前を持つアクションで、必須フィールドです。 サポートされるイベントについて詳しくは、[[!DNL TikTok] Marketing API](https://business-api.tiktok.com/portal/docs?id=1741601162187777) ドキュメントを参照してください。 |
| イベント時間 | ISO 8601 形式または `yyyy-MM-dd'T'HH:mm:ss:SSSZ` 形式の文字列としての日時。 必須フィールドです。 |
| イベント ID | 各イベントを示すために広告主が生成した一意の ID。 これはオプションのフィールドで、重複排除に使用されます。 |

{style="table-layout:auto"}

![&#x200B; フィールドへのデータ入力の例を示す [!DNL Web Events and Parameters] の節。](../../../images/extensions/server/tiktok/configure-web-events-parameters.png)

**[!UICONTROL ユーザーコンテキストのパラメーター]**

ユーザーコンテキストパラメーターには、web 訪問者イベントを [!DNL TikTok] ユーザーと照合するために使用される顧客情報が含まれています。 複数のタイプのマッチングデータを含めると、ターゲティングと最適化モデルの精度を高めることができます。

| 入力 | 説明 |
| --- | --- |
| IP アドレス | ブラウザーのハッシュ化されていないパブリック IP アドレス。 IPv4 アドレスと IPv6 アドレスのサポートが提供されます。 IPv6 アドレスの完全な形式と圧縮された形式の両方が認識されます。 |
| ユーザーエージェント | ユーザーのデバイスからのハッシュ化されていないユーザーエージェント。 |
| メール | コンバージョンイベントに関連付けられた連絡先のメールアドレス。 |
| Phone | 電話番号は、ハッシュする前に E164 形式 [+][ 国コード ][ 市外局番 ][local phone number] である必要があります。 |
| Cookie ID | Pixel SDKを使用している場合、Cookie が有効であれば、`_ttp` Cookie に一意の ID が自動保存されます。 `_ttp` の値を抽出して、このフィールドに使用できます。 |
| 外部 ID | ユーザー ID、外部 Cookie ID などの一意の ID は、SHA256 でハッシュ化する必要があります。 |
| TikTok クリック ID | [!DNL TikTok] で広告が選択されるたびにランディングページの URL に追加される `ttclid`。 |
| ページ URL | イベント発生時のページ URL。 |
| ページリファラー URL | ページリファラーの URL。 |

{style="table-layout:auto"}

![&#x200B; フィールドへのデータ入力の例を示す [!DNL User Context Parameters] の節。](../../../images/extensions/server/tiktok/configure-user-context-parameters.png)

**[!UICONTROL プロパティパラメーター]**

プロパティパラメーターを使用して、サポートされる追加のプロパティを設定します。

| 入力 | 説明 |
| --- | --- |
| 価格 | 1 つの品目の原価。 |
| 数量 | イベントで購入されるアイテムの数。 これは 0 より大きい正の数にする必要があります。 |
| コンテンツタイプ | 製品カタログを設定する際にデータフィードをどのように設定するかに応じて、`product` または `product_group` の値を content_type オブジェクトプロパティに割り当てる必要があります。 |
| コンテンツ ID | 商品項目の一意の ID。 |
| コンテンツカテゴリ | ページ/製品のカテゴリ。 |
| コンテンツ名 | ページ/製品の名前。 |
| 通貨 | イベントで購入される品目の通貨。 これは ISO-4217 で表されます。 |
| 値 | 注文の合計価格。 この値は価格*数量と等しくなります。 |
| 説明 | アイテムまたはページの説明。 |
| クエリ | 商品の検索に使用されたテキスト文字列。 |
| ステータス | 注文、品目またはサービスのステータス。 例えば、「submitted」とします。 |

{style="table-layout:auto"}

![&#x200B; フィールドへのデータ入力の例を示す [!DNL Properties Parameters] の節。](../../../images/extensions/server/tiktok/configure-properties-parameters.png)

## イベントの重複排除 {#deduplication}

[!DNL TikTok] pixel SDKと [!DNL TikTok] web events API 拡張機能の両方を使用して同じイベントを [!DNL TikTok] に送信する場合、[!DNL TikTok] ピクセルを重複排除用に設定する必要があります。

重複せずにクライアントとサーバーから個別のイベントタイプが送信される場合は、重複排除は必要ありません。 レポートに悪影響が及ばないようにするには、[!DNL TikTok] pixel SDKと [!DNL TikTok] web events API 拡張機能で共有される単一のイベントについて、重複排除が行われるようにしてください。

共有イベントを送信する場合は、すべてのイベントにピクセル ID、イベント ID および名前が含まれていることを確認してください。 重複したイベントが 5 分以内に到着した場合は、結合されます。 データフィールドが最初のイベントにない場合は、後続のイベントと組み合わされます。 48 時間以内に受信した重複イベントは削除されます。

このプロセスについて詳しくは、[&#x200B; イベントの重複排除 &#x200B;](https://ads.tiktok.com/help/article/event-deduplication) に関する [!DNL TikTok] のドキュメントを参照してください。

## 次の手順

このガイドでは、[!DNL TikTok] web イベント API 拡張機能を使用してサーバーサイドイベントデータを [!DNL TikTok] に送信する方法について説明しました。 [!DNL Adobe Experience Platform] のイベント転送機能について詳しくは、[&#x200B; イベント転送の概要 &#x200B;](../../../ui/event-forwarding/overview.md) を参照してください。

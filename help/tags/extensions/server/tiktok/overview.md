---
title: AdobeTikTok Web イベント API 拡張機能の統合
description: このAdobe Experience Platform Web イベント API を使用すると、Web サイトでのインタラクションをTikTokと直接共有できます。
last-substantial-update: 2023-09-26T00:00:00Z
source-git-commit: d8b7006ade1dc82fdd79b7ed744c021bc304bca7
workflow-type: tm+mt
source-wordcount: '1126'
ht-degree: 5%

---

# [!DNL TikTok] web イベント API 拡張機能の概要

The [!DNL TikTok] events API はセキュア [Edge Network Server API](/help/server-api/overview.md) と情報を共有できるインターフェイス [!DNL TikTok] web サイト上のユーザーアクションに直接関する情報です。 イベント転送ルールを利用して、 [!DNL Adobe Experience Platform Edge Network] から [!DNL TikTok] を使用して、 [!DNL TikTok] Web イベント API 拡張機能。

## [!DNL TikTok] 前提条件 {#prerequisites}

次の手順で [!DNL TikTok] 使用する web イベント API [!DNL TikTok] イベント API を使用する場合は、 [!DNL TikTok] ピクセルコードとアクセストークン。

有効な [!DNL TikTok] を作成するためにビジネスアカウント用に [!DNL TikTok] partner 設定を使用するピクセル。 次に移動： [[!DNL TikTok] （ビジネス登録ページ）](https://www.tiktok.com/business/en-US/solutions/business-account) をクリックして、アカウントを作成します（まだ持っていない場合）。

を設定するには、ビジネスアカウントにログインする必要があります [!DNL TikTok] パートナー設定を使用するピクセル。 これを行うには、以下の手順に従います。

1. 次に移動： **[!UICONTROL Assets]** 「 」タブで「 」を選択します。 **[!UICONTROL イベント]**.
2. 「Web イベント」で、「 **[!UICONTROL 管理]**.
3. 選択 **[!UICONTROL ウェブイベントの設定]**.
4. 選択 **[!UICONTROL パートナーの設定]** を設定します。

詳しくは、 [ピクセルを使い始める](https://ads.tiktok.com/help/article/get-started-pixel) の設定方法の詳細については、ガイドを参照してください。 [!DNL TikTok] ピクセル。

ピクセルが正常に作成されたら、アクセストークンを生成できます。 これをおこなうには、ピクセルに移動し、「 **[!UICONTROL 設定]** タブをクリックします。 イベント API で、を選択します。 **[!UICONTROL アクセストークンの生成]**.

詳しくは、 [[!DNL TikTok] 入門ガイド](https://business-api.tiktok.com/portal/docs?id=1739584855420929) ピクセルコードとアクセストークンの設定方法について詳しくは、を参照してください。

## のインストールと設定 [!DNL TikTok] web イベント API 拡張機能 {#install}

拡張機能をインストールするには、「 」を選択します。 **[!UICONTROL 拡張機能]** をクリックします。 Adobe Analytics の **[!UICONTROL カタログ]** タブで、 **[!UICONTROL TikTok Web Events API 拡張機能]** 次に、「 **[!UICONTROL インストール]**.

![次を表示する拡張機能カタログ： [!DNL TikTok] 拡張機能カードのハイライト表示のインストール。](../../../images/extensions/server/tiktok/install-extension.png)

次の画面で、以前にから生成した次の設定値を入力します。 [!DNL TikTok] Ads Manager:

* **[!UICONTROL ピクセルコード]**
* **[!UICONTROL アクセストークン]**

完了したら「**[!UICONTROL 保存]**」を選択します。

![[!DNL TikTok] の設定画面 [!DNL TikTok] web イベント API 拡張機能。](../../../images/extensions/server/tiktok/configure.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントの送信先と送信方法を決定するイベント転送ルールの作成を開始できます。 [!DNL TikTok].

新規作成 [ルール](../../../ui/managing-resources/rules.md) を設定します。 の下 **[!UICONTROL アクション]**、新しいアクションを追加し、拡張機能をに設定します。 **[!UICONTROL TikTok Web Events API 拡張機能]**. Edge ネットワークイベントをに送信するには、以下を実行します。 [!DNL TikTok]を設定し、 **[!UICONTROL アクションタイプ]** から **[!UICONTROL TikTok Web イベント API イベントの送信].**

![The [!UICONTROL TikTok Web イベント API イベントの送信] アクションタイプが選択されています [!DNL TikTok] ルールを使用して、データ収集 UI に表示されます。](../../../images/extensions/server/tiktok/select-action.png)

選択後、次に示すように、イベントをさらに設定するための追加のコントロールが表示されます。 完了したら、「 」を選択します。 **[!UICONTROL 変更を保持]** 」と入力してルールを保存します。

**[!UICONTROL Web イベントとパラメーター]**

Web イベントとパラメーターには、イベントに関する一般的な情報が含まれています。 標準イベントは、 [!DNL TikTok] 統合ツールおよびを使用して、レポートを作成したり、コンバージョンを最適化したり、オーディエンスを構築したりできます。

| 入力 | 説明 |
| --- | --- |
| イベント名 | イベントの名前。 これらは、で作成された事前定義済みの名前を持つアクションです。 [!DNL TikTok] は必須フィールドです。 詳しくは、 [[!DNL TikTok] マーケティング API](https://business-api.tiktok.com/portal/docs?id=1741601162187777) サポートされるイベントの詳細については、ドキュメントを参照してください。 |
| イベント時刻 | ISO 8601 または yyyy-MM-dd&#39;T&#39;HH での文字列としての日時:mm:ss:SSSZ 形式。 これは必須フィールドです。 |
| イベント ID | 各イベントを示す広告主によって生成される一意の ID。 これはオプションのフィールドで、重複排除に使用します。 |

{style="table-layout:auto"}

![The [!DNL Web Events and Parameters] セクションに、フィールドへのデータ入力例を示します。](../../../images/extensions/server/tiktok/configure-web-events-parameters.png)

**[!UICONTROL ユーザーコンテキストパラメーター]**

ユーザーコンテキストパラメーターには、Web 訪問者イベントとの照合に使用される顧客情報が含まれます [!DNL TikTok] ユーザー。 複数のタイプの一致するデータを含めることで、ターゲティングと最適化モデルの精度を高めることができます。

| 入力 | 説明 |
| --- | --- |
| IP アドレス | ブラウザーのハッシュ化されていない公開 IP アドレス。 IPv4 および IPv6 アドレスに対するサポートが提供されます。 IPv6 アドレスの完全形式と圧縮形式の両方が認識されます。 |
| ユーザーエージェント | ユーザーのデバイスから取得される、ハッシュ化されていないユーザーエージェント。 |
| メール | コンバージョンイベントに関連付けられた連絡先の電子メールアドレス。 |
| Phone | 電話番号は、E164 形式 [+][ 国コード ] である必要があります[面積コード][local phone number] ハッシュする前に |
| Cookie ID | Pixel SDK を使用している場合、一意の ID が `_ttp` cookie（cookie が有効な場合） The `_ttp` の値を抽出して、このフィールドで使用できます。 |
| 外部 ID | ユーザー ID、外部 Cookie ID などの一意の識別子。SHA256 でハッシュ化する必要があります。 |
| TikTok Click ID | The `ttclid` 広告が選択されるたびにランディングページの URL に追加される [!DNL TikTok]. |
| ページ URL | イベント時のページ URL。 |
| ページリファラー URL | ページリファラーの URL。 |

{style="table-layout:auto"}

![The [!DNL User Context Parameters] セクションに、フィールドへのデータ入力例を示します。](../../../images/extensions/server/tiktok/configure-user-context-parameters.png)

**[!UICONTROL プロパティパラメーター]**

プロパティパラメーターを使用して、サポートされるその他のプロパティを設定します。

| 入力 | 説明 |
| --- | --- |
| 価格 | 1 つの項目のコスト。 |
| 数量 | イベントで購入された品目の数。 0 より大きい正の数を指定する必要があります。 |
| コンテンツタイプ | 値は、 `product` または `product_group` は、商品カタログの設定時にデータフィードを設定する方法に応じて、content_type オブジェクトプロパティに割り当てる必要があります。 |
| コンテンツ ID | 商品品目の一意の ID。 |
| コンテンツのカテゴリ | ページ/製品のカテゴリ。 |
| コンテンツ名 | ページ/製品の名前。 |
| 通貨 | イベントで購入される品目の通貨。 これは、ISO-4217 で表現されます。 |
| 値 | 注文の合計価格。 この値は price *数量と等しくなります。 |
| 説明 | 項目またはページの説明。 |
| クエリ | 商品の検索に使用された文字列。 |
| ステータス | 注文、品目またはサービスのステータス。 例えば、「submitted」と入力します。 |

{style="table-layout:auto"}

![The [!DNL Properties Parameters] セクションに、フィールドへのデータ入力例を示します。](../../../images/extensions/server/tiktok/configure-properties-parameters.png)

## イベントの重複排除 {#deduplication}

[!DNL TikTok] ピクセルは、重複排除の両方を使用する場合、重複排除用に設定する必要があります [!DNL TikTok] ピクセル SDK と [!DNL TikTok] 同じイベントをに送信する web イベント API 拡張機能 [!DNL TikTok].

重複のないクライアントおよびサーバーから異なるイベントタイプが送信される場合は、重複排除は必要ありません。 レポートが否定的な影響を受けないようにするには、 [!DNL TikTok] ピクセル SDK と [!DNL TikTok] web イベント API 拡張機能の重複は除外されます。

共有イベントを送信する場合は、すべてのイベントにピクセル ID、イベント ID、名前が含まれていることを確認します。 お互いから 5 分以内に届いた重複したイベントは結合されます。 最初のイベントにデータフィールドがない場合、後続のイベントと組み合わせられます。 48 時間以内に受信した重複イベントはすべて削除されます。

詳しくは、 [!DNL TikTok] に関するドキュメント [イベントの重複排除](https://ads.tiktok.com/help/article/event-deduplication) 詳しくは、このプロセスを参照してください。

## 次の手順

このガイドでは、サーバー側のイベントデータをに送信する方法について説明しました。 [!DNL TikTok] の使用 [!DNL TikTok] web イベント API 拡張機能。 でのイベント転送機能の詳細については、 [!DNL Adobe Experience Platform]（を参照） [イベント転送の概要](../../../ui/event-forwarding/overview.md).

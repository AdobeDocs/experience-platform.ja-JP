---
title: Snapchat Conversions API 拡張機能の概要
description: Snapchat 変換を使用して、サーバーサイドのイベントデータを Snap に送信します。
last-substantial-update: 2025-01-20T00:00:00Z
exl-id: 1c2d7243-5bcd-40a0-8515-9ab72613c5f3
source-git-commit: 7cece312a2f019d48653dbd208cc8b7593ab0ee3
workflow-type: tm+mt
source-wordcount: '964'
ht-degree: 4%

---

# [!DNL Snapchat] Conversions API 拡張機能の概要

[!DNL Snap] Conversion API Extension は、安全な [Edge Networkサーバー API](/help/server-api/overview.md) インターフェイスで、Web サイト上のユーザーアクションに関する情報を直接 [!DNL Snapchat] と共有できます。 イベント転送ルールを活用し、**[!DNL Snap]** Conversion API 拡張機能を使用して、**[!DNL Adobe Experience Platform Edge Network]** から **[!DNL Snapchat]** にデータを送信できます。

## [!DNL Snapchat] 前提条件 {#prerequisites}

[!DNL Snapchat] Conversions API を使用するには：

* Adobe Experience Platformで [ イベント転送プロパティ ](/help/tags/ui/event-forwarding/getting-started.md) を設定する必要があります。
* また、プロパティを編集するには [ 必要な権限 ](/help/collection/permissions.md) が必要です。

[ データストリーム ](/help/tags/ui/event-forwarding/getting-started.md) を作成し、[ イベント転送サービス ](/help/tags/ui/event-forwarding/getting-started.md#enable-event-forwarding) を追加します。

Conversions API を使用するには、**[!DNL Snapchat]** [Business Manager](https://business.snapchat.com/) アカウントが必要です。 Business Manager は、広告主が **[!DNL Snapchat]** のマーケティング活動をビジネス全体および外部パートナーと統合するのに役立ちます。 Business Manager アカウントがない場合は、作成に関する **[!DNL Snapchat]** [ ヘルプセンターの記事 ](https://businesshelp.snapchat.com/s/article/get-started?language=en_US) を参照してください。

Snapchat Ads Manager で [!DNL [Snap Pixel]](https://businesshelp.snapchat.com/s/article/pixel-website-install?language=en_US) を設定し、`Pixel ID` を表示するためのアクセス権を持っている必要があります。 `Pixel ID` の詳細については、[!UICONTROL [Events Manager]](https://businesshelp.snapchat.com/s/article/events-manager?language=en_US) の節を参照してください。

静的で長期間有効な API トークンが必要です。 このトークンを取得するには、[[!DNL Snapchat] Conversions API ドキュメント ](https://developers.snap.com/api/marketing-api/Conversions-API/GetStarted#access-token) を参照してください。

## [!DNL Snapchat] Web イベント API 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、**[!UICONTROL データ収集]**/**[!UICONTROL イベント転送]** に移動します。 拡張機能をインストールするプロパティを選択します。

目的のプロパティを選択したら、次の手順に従います。

1. 左側のナビゲーションパネルで、「**[!UICONTROL 拡張機能]**」を選択します。
2. **[!UICONTROL Snap Conversion API Extension]** を検索し、「**[!UICONTROL インストール]**」を選択します。

   ![ インストールボタンを示す画像 ](../../../images/extensions/server/snap/install.png)

3. 設定画面で、次の値を入力します。

* **[!UICONTROL ピクセル Id]**
* **[!UICONTROL API トークン]**

完了したら「**[!UICONTROL 保存]**」を選択します。

![ ピクセル ID と API トークンボタンを示す画像 ](../../../images/extensions/server/snap/configure.png)
<!-- 
![[!DNL Snap] configuration screen for the [!DNL Snap] conversion API extension.](../../../images/extensions/server/snap/configure.png) -->

## データ要素の作成 {#create-data-elements}

[!DNL Snapchat] Conversions API 拡張機能にデータを送信するには、データパラメーターごとに [ データ要素 ](https://experienceleague.adobe.com/en/docs/platform-learn/implement-web-sdk/event-forwarding/setup-event-forwarding#create-an-event-forwarding-data-element) を作成します。 次の手順に従います。

1. プロパティの **[!UICONTROL プロパティ情報]** 画面で **[!UICONTROL オーサリング]**>**[!UICONTROL データ要素]** に移動し、「**[!UICONTROL データ要素を追加]**」を選択します。

   ![ 「データ要素を追加」ボタンを示す画像 ](../../../images/extensions/server/snap/add_data_element.png)

2. データ要素の名前を入力します。

3. 拡張機能として **[!UICONTROL Core]** を選択し、データ要素タイプとして **[!UICONTROL Path]** を選択します。

4. ドロップダウンメニューから適切な項目を選択し、右側のパネルの [!UICONTROL  パス ] フィールドに入力して、スキーマ内の目的のデータを参照します。

   ![ データ要素の作成画面を示す画像 ](../../../images/extensions/server/snap/create_data_element.png)

例えば、以下に示すスキーマの `snapClickId` を参照するデータ要素を作成するとします。

![ スキーマ ](../../../images/extensions/server/snap/schema.png) を示す画像

データ要素は XDM スキーマの `_snap.inc.exchange` の下に `snapClickId` るため、設定する必要があります。

![ データ要素の編集画面を示す画像 ](../../../images/extensions/server/snap/edit_data_element.png)

データ要素の作成について詳しくは、[ イベント転送プロパティのドキュメント ](/help/tags/ui/event-forwarding/overview.md#data-elements) を参照してください。

## 変換イベントを Snap に送信するルールを作成する {#create-snap-rules}

[ ルール ](https://experienceleague.adobe.com/en/docs/platform-learn/implement-web-sdk/event-forwarding/setup-event-forwarding#create-an-event-forwarding-rule) は、Platform で拡張機能をトリガーするために使用されます。 この節では、イベント転送プロパティ内でルールを作成し、Conversions API 拡張機能を使用してコンバージョンイベントを Snap に送信する方法について概説します。

### 新しいルールの作成

1. イベント転送プロパティに移動し、オーサリングメニューから **[!UICONTROL ルール]** を選択します。 次に、「**[!UICONTROL 新規ルールを作成]**」をクリックします。

   ![ 左側のナビゲーションのルールを示す画像 ](../../../images/extensions/server/snap/create_new_rule.png)

2. ルールに名前を付け、スナップ・イベントをトリガーする条件を構成します。 例えば、イベントに注文番号が含まれる場合に `PURCHASE` イベントを送信するには、ユーザーインタラクションに有効な発注番号が含まれているかどうかを確認する条件を設定します。

   ![ 条件設定画面を示す画像 ](../../../images/extensions/server/snap/action_configuration.png)

3. 条件を保存した後、Snap Conversion API をトリガーするアクションを追加します。 左側のパネルで以下を行います。

   * [!UICONTROL  拡張機能 ] ドロップダウンメニューを [!UICONTROL Snap Conversions API 拡張機能 ] に設定します。

   * [!UICONTROL  アクションタイプ ] ドロップダウンメニューを [!UICONTROL Web コンバージョンをレポート ] に設定します。

   * ルールに適切な名前を付けます。

   ![ アクション設定画面を示す画像 ](../../../images/extensions/server/snap/action_configuration.png)

4. 右側のパネルの [ データバインディング ](https://developers.snap.com/api/marketing-api/Conversions-API/Parameters) セクションで、イベントに送信する **[!UICONTROL CAPI パラメーター値]** を設定します。 以下に示すように、拡張機能のフィールドは CAPI パラメーターにマッピングされます。 各パラメーターについて詳しくは、[Snapchat Conversions API ドキュメント ](https://developers.snap.com/api/marketing-api/Conversions-API/Parameters) を参照してください。

| データ連結フィールド | スナップ CAPI パラメータ |
| --- | --- |
| イベントタイプ （必須） | `event_name` |
| メール | `em` |
| 電話番号 | `ph` |
| ユーザーエージェント | `client_user_agent` |
| IP アドレス | `client_ip_address` |
| クリック ID | `sc_click_id` |
| Cookie1 | `so_cookie1` |
| 名 | `fn` |
| 姓 | `ln` |
| 性別 | `ge` |
| 市区町村 | `ph` |
| 都道府県 | `st` |
| 郵便番号 | `zp` |
| 国 | `country` |
| 外部 ID | `external_id` |
| パートナー Id | `partner_id` |
| 購読 ID | `subscription_id` |
| リード ID | `lead_id` |
| 品目またはカテゴリ | `content_category` |
| コンテンツ名 | `content_ids` |
| コンテンツタイプ | `content_name` |
| 目次 | `contents` |
| 説明 | `description` |
| イベントタグ | `event_tag` |
| 項目の数 | `num_items` |
| 価格 | `value` |
| 通貨 | `currency` |
| トランザクション ID | `order_id` （`client dedup idD` の代わりに `event_id` でも送信） |
| 予測される LTV | `predicted_ltv` |
| 検索文字列 | `search_string` |
| 新規登録メソッド | `sign_up_method` |
| クライアント重複排除 Id | `event_id` |
| データ使用の制限 | `data_processing_options` |
| ページ Url | `event_source_url` |

{style="table-layout:auto"}

### 必須フィールドとオプションフィールド

各イベントには `event_source` が必要で、常に `WEB.` に設定されます。一致させるには、次のフィールドまたは組み合わせのうち少なくとも 1 つが必要です。

* メール
* 電話番号
* IP アドレスとユーザーエージェント

**その他のメモ：**

* イベント `Purchase` 場合、「`Currency`」フィールドと「`Price`」フィールドは必須です。

* 「**[!UICONTROL テストモード]**」チェックボックスを有効にすると、イベントがテストイベントとして送信され、標準のレポートではなくテストイベントツールに表示されます。 詳しくは、こちらの [ ビジネスヘルプセンターの記事 ](https://businesshelp.snapchat.com/s/article/capi-event-testing?language=en_US#:~:text=Snap&#39;s%20Conversions%20API%20(CAPI)%20Test,being%20processed%20as%20production%20results。) を参照してください。

* `contents` パラメーターは、次のフィールドのうち少なくとも 1 つを含む JSON 文字列である必要があります。

   * `id`
   * `item_category`
   * `brand`
   * `delivery_category`
   * `item_price`
   * `quantity`

例：

```json
{
  "id": "id1",
  "brand": "brand1",
  "delivery_category": "c1",
  "item_price": 2.00,
  "quantity": 2
}
```

[ カスタムコンバージョン値と ROAS レポート ](https://businesshelp.snapchat.com/s/article/custom-conversions-value-roas?language=en_US) を使用するには、`contents` フィールドに関連するパラメーターを含めます。 購入イベントの設定例は、`brand`、`item_price`、`id` のようになります。

`Purchase` イベントの設定例：

![ データバインディングを示す画像 ](../../../images/extensions/server/snap/data_bindings.png)

オプションのフィールドは、次のように設定できます。

![ オプションフィールドを示す画像 ](../../../images/extensions/server/snap/optional_fields.png)

上記のようにルールの名前、条件、アクションを設定したら、ルールを保存して有効になっていることを確認します。

![ 有効なルールを示す画像 ](../../../images/extensions/server/snap/enabled_rule.png)

これで、これらの変更をプロパティに公開できます。 詳しくは、[ 公開フロー ](/help/tags/ui/publishing/overview.md) に関するドキュメントを参照してください。

## トラブルシューティング {#troubleshoot}

設定のトラブルシューティングと最適化については、[ イベント品質スコアの推奨事項 ](https://businesshelp.snapchat.com/s/article/event-quality-score) を確認し、イベントの一致率とパフォーマンス結果を可能な限り高くします。

**イベント品質スコア** に問題が発生した場合は、それを改善するための推奨事項の詳細を確認してください [ こちら ](https://businesshelp.snapchat.com/s/article/esq-issues-recommendations?language=en_US)。

## 次の手順 {#next-steps}

このガイドでは、**[!DNL Snap Conversions API]** 拡張機能を使用してサーバーサイドのイベントデータを **[!DNL Snap]** に送信する方法について説明しました。 Platform のイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。

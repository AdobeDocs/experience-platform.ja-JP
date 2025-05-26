---
title: Amazon Conversions API 拡張機能の概要
description: Adobe Experience Platform web events API を使用して、web サイトのインタラクションをAmazonと直接共有します
last-substantial-update: 2025-04-17T00:00:00Z
exl-id: 20339b6e-15e3-4d0e-8870-a3a85b7e66fd
source-git-commit: 306795c0fdd665b1813c70c41bd9b5d58f5507e6
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 3%

---

# [!DNL Amazon] web イベント API 拡張機能の概要

[!DNL Amazon] Conversions API 拡張機能は、広告主のサーバーと [!DNL Amazon] のマーケティングデータの間に直接接続を作成します。 これにより、広告主はコンバージョンの場所に関係なくキャンペーンの有効性を評価し、それに応じてキャンペーンを最適化できます。 この拡張機能により、完全なアトリビューション、データの信頼性、最適化された配信が実現します。

## [!DNL Amazon] 前提条件 {#prerequisites}

[!DNL Amazon] Conversions API 拡張機能をインストールして設定する前に、次の前提条件の手順を実行して、適切な認証とデータアクセスを確保します。

### 秘密鍵およびデータ要素の作成 {#secret}

新しい [!DNL Amazon][ イベント転送の秘密鍵 ](../../../ui/event-forwarding/secrets.md) を作成し、認証メンバーを示す一意の名前を指定します。 これは、値を保護しながら、アカウントへの接続を認証するために使用されます。

次に、[Core] 拡張機能と [!UICONTROL Secret] データ要素タイプを使用して ](../../../ui/managing-resources/data-elements.md#create-a-data-element) データ要素を作成 [!UICONTROL  し、作成した `Amazon` シークレットを参照します。

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Amazon] に接続するには、次の詳細を入力します。

| キータイプ | 説明 |
| --- | --- |
| アカウント ID | [!DNL Amazon] アカウントの一意のアカウント ID。 |
| エンティティ ID | 広告主アカウントに関連付けられたプロファイルの識別子。 これは Campaign Manager ポータル URL にあり、先頭に `entity` が付きます。 |
| アクセストークン | OAuth 経由で [!DNL Amazon] API への認証に使用される、アプリの有効期限が切れていないアクセストークン。 詳しくは、[ 認証に関するAmazon API ドキュメント ](https://developer.amazon.com/docs/app-porting/device-messaging-fit-obtain-api-key.html) を参照してください。 |

## [!DNL Amazon] 拡張機能のインストールと設定 {#install-configure}

次の手順に従って、[!DNL Amazon] Conversions API 拡張機能をインストールして設定します。

1. イベント転送プロパティを作成または編集します。
2. 左側のナビゲーションパネルで **拡張機能** に移動し、「カタログ」タブで [!DNL Amazon] 拡張機能を選択します。
3. **インストール** を選択します。

   ![Amazon拡張機能カタログでハイライト表示されたAdobe Experience Platform拡張機能カード。](../../../images/extensions/server/amazon/amazon-extension.png)

4. 拡張機能を次の詳細で設定します。
   - **アクセストークン**:OAuth 2 トークンを含むデータ要素の秘密鍵。

     ![OAuth 2 トークンのデータ要素の秘密鍵を入力するフィールドをハイライト表示した設定インターフェイス。](../../../images/extensions/server/amazon/amazon-oauth2-token-field.png)

   - **エンティティ ID**：エンティティ ID （「エンティティ」というプレフィックスを持つ Campaign Manager ポータル URL 内に見つかりました）。

     ![ 「エンティティ ID」フィールドがハイライト表示された Campaign Manager ポータルインターフェイス ](../../../images/extensions/server/amazon/campaign-manager-entity-id.png)

5. 「**保存**」を選択して、設定を完了します。

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベント転送ルールを作成して、イベントを [!DNL Amazon] に送信するタイミングと方法を決定します。

1. **ルール** に移動し、新しいイベント転送ルールを作成します。
2. 「**アクション**」で、「**Amazon Conversions API 拡張機能**」を選択します。
3. **アクションタイプ** を **コンバージョンイベントをインポート** に設定します。

   ![ 「アクションタイプ」が「コンバージョンイベントをインポート」に設定されているイベント転送ルール設定インターフェイス ](../../../images/extensions/server/amazon/amazon-import-conversion-events.png)

### コンバージョンイベントデータの設定 {#conversion-event-data}

コンバージョンイベントデータは、ユーザーのインタラクションを追跡し、キャンペーンの有効性を測定するために重要です。 このデータを [!DNL Amazon] に転送することで、ユーザーの行動に関するインサイトを得て、キャンペーンを最適化し、コンバージョンの正確なアトリビューションを確保できます。

次の表に、コンバージョンイベントデータの設定と転送に必要な主要なプロパティの概要を示します。

| 入力 | 説明 | 必須 | 例 |
| --- | --- | --- | --- |
| `name` | 読み込まれたイベントの名前。 | ○ | `My Event Name` |
| `eventType` | イベントに関連付けられ、レポートに使用される、標準のAmazon イベントタイプ。 | ○ | `Add to Shopping Cart` |
| `eventActionSource` | イベントのソースとなったプラットフォーム。 | ○ | `WEBSITE` |
| `clientDedupeId` | コンバージョンイベントに対して広告主が指定した `id`。 `clientDedupeId` が同じイベントの場合は、最初のイベントのみが保持され、後続のイベントはすべてドロップされます。 | オプション | `3234A398932` |
| `timestamp` | イベントが発生した際に報告されたタイムスタンプ。 タイムスタンプは、イベントを送信する前に最大 7 日間にすることができます。 7 日を超えるデータは処理されません。 | ○ | `2023-05-08T14:04:28Z` |
| `matchKeys` | トラフィックイベントへのアトリビューションに使用される顧客およびデバイスの識別子のタイプ/値を表す配列。 | ○ | --- |
| `matchKeys > type` | アトリビューションに使用される識別子タイプ。 | ○ | --- |
| `matchKeys > value` | アトリビューションに使用される識別子の値。 | ○ | イベントを実行した顧客の SHA-256 でハッシュ化された識別子の値のリスト。 |
| `value` | イベントの値。 | オプション | `5` または `0.99` |
| `currencyCode` | ISO-4217 形式のイベントの `value` に関連付けられた通貨コード。 オフAmazon購入イベントタイプにのみ適用されます。 指定しない場合、変換定義の通貨コード設定が使用されます。 | オプション | `USD`、`EUR`、`GBP` など |
| `unitsSold` | 購入した品目の数。 オフAmazon購入イベントタイプにのみ適用されます。 コンバージョンイベントで指定しなかった場合は、デフォルトの `1` が適用されます。 | オプション | --- |
| `countryCode` | この値は、国際標準化機構（ISO）が公開した ISO 3166 規格の一部である ISO 3166-1 で定義されている ISO 3166-1 alpha-2、2 文字の国コードに基づいて、国、依存地域、地理的に関心のある特別な領域を表します。 | ○ | --- |
| `dataProcessingOptions` | 広告データの使用に対するユーザーの同意を示します。 | オプション | LIMITED_DATA_USE |

- 「**[!UICONTROL 変更を保持]**」を選択して、ルールを保存します。

![ 「変更を保持」ボタンがハイライト表示されたイベントパラメーター設定インターフェイス ](../../../images/extensions/server/amazon/event-parameters.png)

![ 「変更を保持」ボタンがハイライト表示された追加のイベントパラメーター設定インターフェイス ](../../../images/extensions/server/amazon/additional-event-parameters.png)

## イベントの重複排除 {#deduplication}

重複排除は、[!DNL Amazon] Advertising タグ（AAT）と [!DNL Amazon] Conversions API 拡張機能の両方を使用して同じイベントをトラッキングする場合に、正確なレポートを確実に行い、コンバージョン数が水増しされるのを防ぐために不可欠です。

### 重複排除が必要な場合

- **必須**：クライアント（AAT）とサーバー（Conversions API）の両方から同じイベントが送信されている場合。
- **不要**：重複せずにクライアントとサーバーから個別のイベントタイプが送信される場合。

### 重複排除の有効化方法

重複排除を有効にするには、すべての共有イベントに `clientDedupeId` フィールドを含めます。 この一意の識別子を使用 [!DNL Amazon] ると、クライアントサイドのイベントとサーバーサイドのイベントを区別し、エントリの重複を防ぐことができます。

重複排除を適切に設定することで、最適化データが正確なままとなり、レポートに悪影響が及ばなくなります。

詳しくは、[Amazon イベントの重複排除ガイド ](https://advertising.amazon.com/) を参照してください。

## 次の手順 {#next-steps}

このガイドでは、[!DNL Amazon] Conversions API 拡張機能を使用してコンバージョンイベントを設定および [!DNL Amazon] に送信する方法について説明しました。 [!DNL Adobe Experience Platform] のイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。

Experience Platform Debugger とイベント転送の監視ツールを使用して実装をデバッグする方法について詳しくは、イベント転送の [Adobe Experience Platform Debuggerの概要 ](/help/debugger/home.md) および [ アクティビティの監視 ](../../../ui/event-forwarding/monitoring.md) を参照してください。

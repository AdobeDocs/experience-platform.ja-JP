---
keywords: イベント転送拡張機能；twitter;twitterイベント転送拡張機能
title: Twitterイベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能を使用すると、ビジネス要件に合わせてイベントをTwitterに取り込むことができます。
last-substantial-update: 2023-05-24T00:00:00Z
exl-id: 54c240e5-6160-4654-ac5b-6afa8d99a765
source-git-commit: 4ee895cb8371646fd2013e2a8f65c2ffdae95850
workflow-type: tm+mt
source-wordcount: '1048'
ht-degree: 7%

---

# [!DNL Twitter] イベント転送拡張機能

[[!DNL Twitter]](https://twitter.com/i/flow/login) は、ユーザーがツイートと呼ばれる 280 文字のメッセージを投稿してやり取りする、オンラインのソーシャルメディアおよびソーシャルネットワーキングサービスです。 ユーザーは、ブラウザー、モバイルフロントエンドソフトウェア、またはその [API](https://developer.twitter.com/en/docs/twitter-api) を介してプログラムを使用して、Twitterとやり取りできます

[!DNL Twitter] Web Conversions API [ イベント転送 ](../../../ui/event-forwarding/overview.md) 拡張機能を使用すると、Adobe Experience Platform Edge Networkで取得したデータを活用して、[!DNL Twitter] に送信できます。 このドキュメントでは、拡張機能のユースケース、インストール方法および機能をイベント転送 [ ルール ](../../../ui/managing-resources/rules.md) に統合する方法について説明します。

[!DNL Twitter] では、[!DNL Twitter] [!DNL Web Conversions] API を使用した認証に [OAuth 1.0](https://developer.twitter.com/en/docs/authentication/oauth-1-0a) が必要です。

## ユースケース

この拡張機能は、Edge Networkのデータを使用して、カスタマー分析とターゲティング機能を活用 [!DNL Twitter] る場合に使用します。

例えば、組織のマーケティングチームについて考えてみましょう。 チームは、Web サイトからユーザーインタラクションイベントデータをイベントデータとして取得し、このイベント転送拡張機能を使用して [!DNL Twitter] に読み込みます。

その後、マーケティングチームと分析チームは、[!DNL Twitter's] の機能を活用して追加の分析を実行し、ターゲットとなる広告キャンペーンに対してこれらのユーザーをターゲットに設定できます。

[!DNL Twitter] 固有のユースケースについて詳しくは、[[!DNL Twitter]  ユースケース ](https://developer.twitter.com/en/use-cases/build-for-businesses) ドキュメントを参照してください。

## [!DNL Twitter] の前提条件とガードレール {#prerequisites}

この拡張機能を使用するには、有効な [!DNL Twitter] アカウントが必要です。 アカウントをまだお持ちでない場合は、[[!DNL Twitter]  登録ページ ](https://help.twitter.com/en/using-twitter/create-twitter-account) に移動し、アカウントを登録して作成してください。

[!DNL Twitter] 開発者アカウントとしてアカウントを設定する必要があります。 開発者として新規登録する方法については、[[!DNL Twitter]  開発者アカウント ](https://developer.twitter.com/en/support/twitter-api/developer-account1) を参照してください。

### API ガードレール {#guardrails}

[!DNL Twitter] Web Conversions API には、15 分間隔あたり 60,000 リクエストのレート制限があります。各リクエストでは 500 イベントが許可されています。

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Twitter] に接続するには、次の入力が必要です。

| キータイプ | 説明 |
| --- | --- |
| 消費者キー | [!DNL Twitter] API にアクセスするためのアプリの API キーを&#x200B;します。 詳しくは、[api キーと秘密鍵 ](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/api-key-and-secret) に関する [!DNL Twitter] ドキュメントを参照してください。 | |
| 消費者秘密鍵 | API シークレットは、アプリに [!DNL Twitter] API へのアクセスを許可します。 詳しくは、[api キーと秘密鍵 ](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/api-key-and-secret) に関する [!DNL Twitter] ドキュメントを参照してください。 |
| トークンシークレット | OAuth 経由で [!DNL Twitter] API への認証に使用される、アプリの有効期限が切れていないトークンシークレット。 詳しくは、[ 使用アクセストークンの取得 ](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens) に関する [!DNL Twitter] のドキュメントを参照してください。 |
| アクセストークン | OAuth 経由で [!DNL Twitter] API への認証に使用される、アプリの有効期限が切れていないアクセストークン。 詳しくは、[ 使用アクセストークンの取得 ](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens) に関する [!DNL Twitter] のドキュメントを参照してください。 |
| ピクセル Id | [!DNL Twitter] Pixel は、サイトのアクションやコンバージョンを追跡するために web サイトに実装される web サイトタグです。 詳しくは、[Web サイトのコンバージョントラッキング ](https://business.twitter.com/en/help/campaign-measurement-and-analytics/conversion-tracking-for-websites.html) に関する [!DNL Twitter] ドキュメントを参照してください。 |

## [!DNL Twitter] 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[ イベント転送プロパティを作成 ](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。「**[!UICONTROL カタログ]**」タブで、[!DNL Twitter] 拡張機能のカードの **[!UICONTROL インストール]** を選択します。

![ インストールを強調表示した [!DNL Twitter] 拡張機能を示すカタログ ](../../../images/extensions/server/twitter/install.png)

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットの作成が必要になる場合があります。 ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

次の画面で、以前に [!DNL Twitter] から収集した次の [ 設定値 ](#configuration-details) を入力します。

* **[!UICONTROL ピクセル Id]**
* **[!UICONTROL コンシューマーキー]**
* **[!UICONTROL 消費者の秘密鍵]**
* **[!UICONTROL トークン]**
* **[!UICONTROL トークンシークレット]**

完了したら「**[!UICONTROL 保存]**」を選択します。

[!DNL Twitter] 拡張 ![[!DNL Twitter] 能の設定画面 ](../../../images/extensions/server/twitter/configure.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントを [!DNL Twitter] に送信するタイミングと方法を決定するイベント転送ルールの作成を開始できます。

イベント転送プロパティに新しい [ ルール ](../../../ui/managing-resources/rules.md) を作成します。 **[!UICONTROL Actions]** の下に新しいアクションを追加し、Twitterを **[!UICONTROL extension]** に設定します。 Edge Networkイベントを [!DNL Twitter] に送信するには、**[!UICONTROL アクションタイプ]** を **[!UICONTROL Web コンバージョンを送信 ].** に設定します。

選択後、追加のコントロールが表示され、イベントをさらに設定できます。 [!DNL Twitter] のイベントプロパティを、以前に作成したデータ要素にマッピングする必要があります。 詳しくは、[[!DNL Twitter] Web Conversions API](https://developer.twitter.com/en/docs/twitter-ads-api/measurement/api-reference/conversions) を参照してください。

![ コンバージョンイベントルールを作成する [!DNL Twitter]。](../../../images/extensions/server/twitter/action-configuration.png)

**[!UICONTROL ユーザーの識別]**

| フィールド名 | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| [!UICONTROL [!DNL Twitter] クリック ID] | [!DNL Twitter] クリックスルー URL から解析されたクリック ID」をクリックします。 | 26l6412g5p4iyj65a2oic2ayg2 | 他の識別子が追加されていない場合は必須です。 |
| [!UICONTROL メール] | SHA256 でハッシュ化されたメールアドレス。 テキストは小文字にし、末尾または先頭のスペースはハッシュ前に削除する必要があります。 | eventforwarding@example.com | 他の識別子が追加されていない場合は必須です。 |
| [!UICONTROL 電話] | 電話は、コンバージョンイベントに一致する識別子として機能します。 電話番号は、ハッシュする前に E164 形式 [+][ 国コード ][ 市外局番 ][local phone number] である必要があります。 | +911234567875 | 他の識別子が追加されていない場合は必須です。 |

**[!UICONTROL コンバージョンデータ]**

| フィールド名 | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| [!UICONTROL  コンバージョン時間 ] | ISO 8601 形式または `yyyy-MM-dd'T'HH:mm:ss:SSSZ` 形式の文字列としての日時。 | 2022-02-18T01:14:00.603Z | ○ |
| [!UICONTROL  イベント Id] | 特定のイベントの基本 36 ID。 この ID は、[!DNL Twitter] 広告アカウント内に含まれる事前設定済みのイベントと一致する必要があります。 これを、イベントマネージャーの対応するイベントの ID と呼びます。 | o87ne または tw-o8z6j-o87ne （tw-pixel_id-event-id） | ○ |
| [!UICONTROL  項目数 ] | イベントで購入されるアイテムの数。 これは 0 より大きい正の数にする必要があります。 | 4 | × |
| [!UICONTROL 通貨] | イベントで購入される品目の通貨。 これは ISO-4217 で表され、指定しない場合、デフォルトは USD になります。 | USD | × |
| [!UICONTROL 値] | イベントで購入される品目の価格値。 | 100.00 | × |
| [!UICONTROL  コンバージョン ID] | 同じイベントタグ内の web ピクセルとコンバージョン API 変換の間の重複排除に使用できるコンバージョンイベントの識別子。 | 23294827 | × |
| [!UICONTROL 説明] | コンバージョンに関する追加情報を含む説明。 | コンバージョンをテスト | × |

## [!DNL Twitter] 内のデータの検証

イベント転送ルールを作成して実行したら、[!DNL Twitter] API に送信されたイベントが [!DNL Twitter] UI で期待どおりに表示されるかどうかを検証します。

イベントの収集と [!DNL Experience Platform] の統合が成功すると、[!DNL Twitter] の [!UICONTROL  イベントマネージャー ] 内にイベントが表示されます。

![[!DNL Twitter] イベントマネージャー ](../../../images/extensions/server/twitter/event-manager.png)

## 次の手順

このガイドでは、イベント転送を使用してコンバージョンイベントを [!DNL Twitter] に送信する方法について説明しました。 これらの基盤となるテクノロジーについて詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Twitter] API](https://developer.twitter.com/en/docs/twitter-api)
* [[!DNL Twitter] Web 変換 API](https://developer.twitter.com/en/docs/twitter-ads-api/measurement/api-reference/conversions)
* [[!DNL Twitter]  ユーザアクセストークン ](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)
* [ ピクセル ID とコンバージョントラッキング ](https://business.twitter.com/en/help/campaign-measurement-and-analytics/conversion-tracking-for-websites.html)

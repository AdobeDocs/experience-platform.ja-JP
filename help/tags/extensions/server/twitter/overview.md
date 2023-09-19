---
keywords: イベント転送拡張機能；twitter;twitterイベント転送拡張機能
title: Twitterイベント転送拡張機能
description: このAdobe Experience Platformイベント転送拡張機能を使用すると、ビジネス要件に合わせてTwitterにイベントを取り込むことができます。
last-substantial-update: 2023-05-24T00:00:00Z
source-git-commit: 3272db15283d427eb4741708dffeb8141f61d5ff
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 7%

---

# [!DNL Twitter] イベント転送拡張機能

[[!DNL Twitter]](https://twitter.com/i/flow/login) は、ユーザーが投稿し、280 文字の長さのメッセージ（ツイートと呼ばれる）を操作する、オンラインのソーシャルメディアおよびソーシャルネットワーキングサービスです。 ユーザーは、ブラウザー、モバイルフロントエンドTwitterを使用して、またはプログラムを通じて、ソフトウェアとやり取りすることができます [API](https://developer.twitter.com/en/docs/twitter-api)

The [!DNL Twitter] Web コンバージョン API [イベント転送](../../../ui/event-forwarding/overview.md) 拡張機能を使用すると、 Adobe Experience Platform Edge Network で取得したデータを活用し、に送信できます。 [!DNL Twitter]. このドキュメントでは、拡張機能の使用例、拡張機能のインストール方法、イベント転送に拡張機能を統合する方法について説明します [ルール](../../../ui/managing-resources/rules.md).

[!DNL Twitter] が必要です [OAuth 1.0](https://developer.twitter.com/en/docs/authentication/oauth-1-0a) ( [!DNL Twitter] [!DNL Web Conversions] API.

## ユースケース

この拡張機能は、 [!DNL Twitter] を使用して、顧客分析およびターゲティング機能を活用します。

例えば、組織内のマーケティングチームについて考えてみましょう。 チームは、Web サイトのユーザーインタラクションイベントデータを Web サイトのイベントデータとしてキャプチャし、に読み込みます。 [!DNL Twitter] このイベント転送拡張機能を使用する。

マーケティングチームと分析チームは、 [!DNL Twitter's] 追加の分析を実行し、ターゲット広告キャンペーン用にこれらのユーザーをターゲットにする機能。

特有の使用例について詳しくは、 [!DNL Twitter]（を参照） [[!DNL Twitter] 使用例](https://developer.twitter.com/en/use-cases/build-for-businesses) ドキュメント。

## [!DNL Twitter] 前提条件とガードレール {#prerequisites}

有効な [!DNL Twitter] アカウントを使用して、この拡張機能を使用する必要があります。 次に移動： [[!DNL Twitter] 登録ページ](https://help.twitter.com/en/using-twitter/create-twitter-account) をクリックして、アカウントを作成します（まだ持っていない場合）。

アカウントは、 [!DNL Twitter] 開発者アカウント。 開発者としての新規登録方法については、 [[!DNL Twitter] 開発者アカウント](https://developer.twitter.com/en/support/twitter-api/developer-account1).

### API ガードレール {#guardrails}

The [!DNL Twitter] Web コンバージョン API には、15 分間隔あたり 60,000 個のリクエストのレート制限があります。各リクエストでは 500 個のイベントを許可しています。

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Twitter]の場合、次の入力が必要です。

| キータイプ | 説明 |
| --- | --- |
| 消費者キー | &#x200B;にアクセスするためのアプリの API キー [!DNL Twitter] API. 詳しくは、 [!DNL Twitter] に関するドキュメント [api キーと秘密鍵](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/api-key-and-secret) 指導のために | |
| 消費者の秘密鍵 | API Secret を使用すると、アプリが [!DNL Twitter] API. 詳しくは、 [!DNL Twitter] に関するドキュメント [api キーと秘密鍵](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/api-key-and-secret) 指導のために |
| トークン秘密鍵 | アプリの有効期限のないトークン秘密鍵。 [!DNL Twitter] OAuth 経由の API。 詳しくは、 [!DNL Twitter] に関するドキュメント [取得，アクセストークン](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens) 指導のために |
| アクセストークン | アプリの有効期限が切れないアクセストークン。 [!DNL Twitter] OAuth 経由の API。 詳しくは、 [!DNL Twitter] に関するドキュメント [取得，アクセストークン](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens) 指導のために |
| ピクセル ID | The [!DNL Twitter] ピクセルとは、サイトのアクションやコンバージョンを追跡するために Web サイト上に実装される Web サイトタグです。 詳しくは、 [!DNL Twitter] に関するドキュメント [web サイトのコンバージョントラッキング](https://business.twitter.com/en/help/campaign-measurement-and-analytics/conversion-tracking-for-websites.html) 指導のために |

## のインストールと設定 [!DNL Twitter] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。Adobe Analytics の **[!UICONTROL カタログ]** タブ、選択 **[!UICONTROL インストール]** ～のためのカードで [!DNL Twitter] 拡張子。

![を表示するカタログ [!DNL Twitter] 拡張機能ハイライトインストール。](../../../images/extensions/server/twitter/install.png)

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットの作成が必要になる場合があります。 ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

次の画面で、次の情報を入力します。 [設定値](#configuration-details) 以前に集めた [!DNL Twitter]:

* **[!UICONTROL ピクセル ID]**
* **[!UICONTROL 消費者キー]**
* **[!UICONTROL 消費者の秘密鍵]**
* **[!UICONTROL トークン]**
* **[!UICONTROL トークン秘密鍵]**

完了したら「**[!UICONTROL 保存]**」を選択します。

![[!DNL Twitter] の設定画面 [!DNL Twitter] 拡張子。](../../../images/extensions/server/twitter/configure.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントの送信先と送信方法を決定するイベント転送ルールの作成を開始できます。 [!DNL Twitter].

新規作成 [ルール](../../../ui/managing-resources/rules.md) を設定します。 の下 **[!UICONTROL アクション]**、新しいアクションを追加し、拡張機能をに設定します。 **[!UICONTROL Twitter]**. Edge ネットワークイベントをに送信するには、以下を実行します。 [!DNL Twitter]を設定し、 **[!UICONTROL アクションタイプ]** から **[!UICONTROL Web 変換を送信].**

選択後、イベントをさらに設定するための追加のコントロールが表示されます。 次をマッピングする必要があります。 [!DNL Twitter] イベントのプロパティを、以前に作成したデータ要素に追加します。 詳しくは、 [[!DNL Twitter] Web コンバージョン API](https://developer.twitter.com/en/docs/twitter-ads-api/measurement/api-reference/conversions).

![The [!DNL Twitter] コンバージョンイベントルールを作成しているとき。](../../../images/extensions/server/twitter/action-configuration.png)

**[!UICONTROL ユーザー ID]**

| フィールド名 | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| [!UICONTROL [!DNL Twitter] クリック ID] | [!DNL Twitter] クリックスルー URL から解析された ID をクリックします。 | 26l6412g5p4iyj65a2oic2ayg2 | 他の識別子が追加されていない場合は必須です。 |
| [!UICONTROL メール] | SHA256 でハッシュ化された電子メールアドレス。 テキストは小文字で、ハッシュ化の前に末尾または先頭のスペースを削除する必要があります。 | eventforwarding@example.com | 他の識別子が追加されていない場合は必須です。 |
| [!UICONTROL Phone] | 電話は、コンバージョンイベントに対応する識別子として機能します。 電話番号は、E164 形式 [+][ 国コード ] である必要があります[面積コード][local phone number] ハッシュする前に | +911234567875 | 他の識別子が追加されていない場合は必須です。 |

**[!UICONTROL コンバージョンデータ]**

| フィールド名 | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| [!UICONTROL コンバージョン時間] | ISO 8601 または yyyy-MM-dd&#39;T&#39;HH での文字列としての日時:mm:ss:SSSZ 形式。 | 2022-02-18T01:14:00.603Z | ○ |
| [!UICONTROL イベント ID] | 特定のイベントの base-36 ID。 この ID は、 [!DNL Twitter] 広告アカウント。 これは、イベントマネージャーで対応するイベントの ID と呼ばれます。 | o87ne または tw-o8z6j-o87ne (tw-pixel_id-event-id) | ○ |
| [!UICONTROL 項目数] | イベントで購入された品目の数。 0 より大きい正の数を指定する必要があります。 | 4 | × |
| [!UICONTROL 通貨] | イベントで購入される品目の通貨。 これは ISO-4217 で表現され、指定されていない場合、デフォルトは USD です。 | USD | × |
| [!UICONTROL 値] | イベントで購入される品目の価格値。 | 100.00 | × |
| [!UICONTROL コンバージョン ID] | 同じイベントタグ内の Web ピクセルとコンバージョン API のコンバージョンの重複排除に使用できるコンバージョンイベントの識別子。 | 23294827 | × |
| [!UICONTROL 説明] | コンバージョンに関する追加情報を含む説明。 | コンバージョンのテスト | × |

## 内のデータの検証 [!DNL Twitter]

イベント転送ルールを作成して実行したら、に送信されたイベントかどうかを検証します。 [!DNL Twitter] API は、 [!DNL Twitter] UI

イベント収集および [!DNL Experience Platform] 統合に成功した場合は、 [!DNL Twitter] [!UICONTROL イベントマネージャー].

![The [!DNL Twitter] イベントマネージャー](../../../images/extensions/server/twitter/event-manager.png)

## 次の手順

このガイドでは、コンバージョンイベントをに送信する方法について説明しました。 [!DNL Twitter] イベント転送を使用しています。 これらの基盤となるテクノロジーについて詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Twitter] API](https://developer.twitter.com/en/docs/twitter-api)
* [[!DNL Twitter] Web 変換 API](https://developer.twitter.com/en/docs/twitter-ads-api/measurement/api-reference/conversions)
* [[!DNL Twitter] ユーザーアクセストークン](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)
* [ピクセル ID とコンバージョントラッキング](https://business.twitter.com/en/help/campaign-measurement-and-analytics/conversion-tracking-for-websites.html)

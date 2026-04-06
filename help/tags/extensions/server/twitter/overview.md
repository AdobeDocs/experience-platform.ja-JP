---
keywords: イベント転送拡張機能；twitter;twitter イベント転送拡張機能
title: Twitter イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能を使用すると、ビジネス要件に合わせてTwitterにイベントを取り込むことができます。
last-substantial-update: 2023-05-24T00:00:00Z
exl-id: 54c240e5-6160-4654-ac5b-6afa8d99a765
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 6%

---

# [!DNL Twitter] イベント転送拡張機能

[[!DNL Twitter]](https://twitter.com/i/flow/login)は、ユーザーがツイートとして知られる280文字のメッセージを投稿して操作するオンライン ソーシャル メディアおよびソーシャル ネットワーキング サービスです。 ユーザーは、[API](https://developer.twitter.com/en/docs/twitter-api)を通じて、ブラウザー、モバイルフロントエンドソフトウェア、またはプログラムを使用してTwitterと対話できます

[!DNL Twitter] Web コンバージョン API [&#x200B; イベント転送](../../../ui/event-forwarding/overview.md)拡張機能を使用すると、Adobe Experience Platform Edge Networkでキャプチャしたデータを活用して、[!DNL Twitter]に送信できます。 このドキュメントでは、拡張機能の使用例、インストール方法、イベント転送[&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md)に拡張機能を統合する方法について説明します。

[!DNL Twitter]では、[&#x200B; &#x200B;](https://developer.twitter.com/en/docs/authentication/oauth-1-0a) APIを使用した認証に[!DNL Twitter]OAuth 1.0[!DNL Web Conversions]が必要です。

## ユースケース

この拡張機能は、[!DNL Twitter]のEdge Network データを使用して、Customer Analyticsおよびターゲティング機能を活用する場合に使用します。

例えば、組織内のあるマーケティングチームを考えてみましょう。 チームは、web サイトからユーザーインタラクションイベントデータをweb サイトからイベントデータとして取得し、このイベント転送拡張機能を使用して[!DNL Twitter]に読み込みます。

マーケティングおよび分析チームは、[!DNL Twitter's]機能を活用して追加分析を実行し、ターゲットを絞った広告キャンペーンにこれらのユーザーをターゲティングできます。

[!DNL Twitter]に固有のユースケースについて詳しくは、[[!DNL Twitter]  ユースケース &#x200B;](https://developer.twitter.com/en/use-cases/build-for-businesses)のドキュメントを参照してください。

## [!DNL Twitter]の前提条件とガードレール {#prerequisites}

この拡張機能を使用するには、有効な[!DNL Twitter] アカウントが必要です。 [[!DNL Twitter] 登録ページ &#x200B;](https://help.twitter.com/en/using-twitter/create-twitter-account)に移動してアカウントを登録し、まだアカウントをお持ちでない場合は作成します。

アカウントを[!DNL Twitter]開発者アカウントとして設定する必要があります。 開発者としてサインアップする方法については、[[!DNL Twitter] 開発者アカウント &#x200B;](https://developer.twitter.com/en/support/twitter-api/developer-account1)を参照してください。

### API ガードレール {#guardrails}

[!DNL Twitter] Web コンバージョン APIには、15分間隔ごとに60,000件のリクエストのレート制限があります。各リクエストで500件のイベントが許可されます。

### 必要な設定の詳細を収集する {#configuration-details}

Experience Platformを[!DNL Twitter]に接続するには、次の入力が必要です。

| キータイプ | 説明 |
| --- | --- |
| Consumer Key | &#x200B; [!DNL Twitter] APIにアクセスするためのアプリのAPI キー。 ガイダンスについては、[!DNL Twitter]api キーとシークレット [に関する](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/api-key-and-secret) ドキュメントを参照してください。 |
| Consumer Secret | API シークレットを使用すると、アプリは[!DNL Twitter] APIにアクセスできます。 ガイダンスについては、[!DNL Twitter]api キーとシークレット [に関する](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/api-key-and-secret) ドキュメントを参照してください。 |
| トークンシークレット | アプリの有効期限のないトークン秘密鍵。OAuth経由で[!DNL Twitter] APIに対する認証に使用されます。 ガイダンスについては、[!DNL Twitter]使用アクセストークンの取得[に関する](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens) ドキュメントを参照してください。 |
| アクセストークン | アプリの有効期限のないアクセストークン。OAuth経由で[!DNL Twitter] APIに対する認証に使用されます。 ガイダンスについては、[!DNL Twitter]使用アクセストークンの取得[に関する](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens) ドキュメントを参照してください。 |
| ピクセル Id | [!DNL Twitter] ピクセルは、サイトのアクションまたはコンバージョンを追跡するためにweb サイトに実装されるweb サイト タグです。 ガイダンスについては、[!DNL Twitter]web サイトのコンバージョン追跡[に関する](https://business.twitter.com/en/help/campaign-measurement-and-analytics/conversion-tracking-for-websites.html)のドキュメントを参照してください。 |

## [!DNL Twitter]拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティ &#x200B;](../../../ui/event-forwarding/overview.md#properties)を作成するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL Extensions]**」を選択します。 **[!UICONTROL Catalog]** タブで、**[!UICONTROL Install]**&#x200B;拡張機能のカードの[!DNL Twitter]を選択します。

インストールを強調表示する![拡張機能を示す[!DNL Twitter] カタログ。](../../../images/extensions/server/twitter/install.png)

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットを作成する必要がある場合があります。 ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

次の画面で、[から以前に収集した次の](#configuration-details)設定値[!DNL Twitter]を入力します。

* **[!UICONTROL Pixel Id]**
* **[!UICONTROL Consumer Key]**
* **[!UICONTROL Consumer Secret]**
* **[!UICONTROL Token]**
* **[!UICONTROL Token Secret]**

終了したら「**[!UICONTROL Save]**」を選択します。

![[!DNL Twitter]拡張機能[!DNL Twitter]の](../../../images/extensions/server/twitter/configure.png)設定画面

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントをいつ、どのように[!DNL Twitter]に送信するかを決定するイベント転送ルールの作成を開始できます。

イベント転送プロパティに新しい[&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md)を作成します。 **[!UICONTROL Actions]**&#x200B;で、新しいアクションを追加し、拡張機能を&#x200B;**[!UICONTROL Twitter]**&#x200B;に設定します。 Edge Network イベントを[!DNL Twitter]に送信するには、**[!UICONTROL Action Type]**&#x200B;を&#x200B;**[!UICONTROL Send Web Conversion]に設定します。**

選択後、追加のコントロールが表示され、イベントがさらに設定されます。 [!DNL Twitter] イベントプロパティを、以前に作成したデータ要素にマッピングする必要があります。 詳しくは、[[!DNL Twitter] Web コンバージョン API](https://developer.twitter.com/en/docs/twitter-ads-api/measurement/api-reference/conversions)を参照してください。

![&#x200B; コンバージョンイベントルールを作成する[!DNL Twitter]。](../../../images/extensions/server/twitter/action-configuration.png)

**[!UICONTROL User Identification]**

| フィールド名 | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| [!UICONTROL [!DNL Twitter] Click ID] | [!DNL Twitter] クリックスルーURLから解析されたクリック ID。 | `26l6412g5p4iyj65a2oic2ayg2` | 他の識別子が追加されていない場合は必須です。 |
| [!UICONTROL Email] | SHA256でハッシュ化されたメールアドレス。 テキストは小文字にする必要があり、ハッシュ化する前に末尾または先頭のスペースを削除する必要があります。 | `eventforwarding@example.com` | 他の識別子が追加されていない場合は必須です。 |
| [!UICONTROL Phone] | 電話は、コンバージョンイベントに一致する識別子として機能します。 電話番号は、ハッシュ化する前にE164形式`[+][country code][area code][local phone number]`である必要があります。 | `+911234567875` | 他の識別子が追加されていない場合は必須です。 |

**[!UICONTROL Conversion Data]**

| フィールド名 | 説明 | 例 | 必須 |
| --- | --- | --- | --- |
| [!UICONTROL Conversion Time] | 日時を文字列としてISO 8601または`yyyy-MM-dd'T'HH:mm:ss:SSSZ`形式で指定します。 | 2022-02-18T01:14:00.603Z | ○ |
| [!UICONTROL Event Id] | 特定のイベントのbase-36 ID。 このIDは、[!DNL Twitter]広告アカウント内に含まれる事前設定済みのイベントと一致する必要があります。 これは、イベントマネージャーの対応するイベントのIDとして知られています。 | o87neまたはtw-o8z6j-o87ne （tw-pixel_id-event-id） | ○ |
| [!UICONTROL Number of Items] | イベントで購入されたアイテムの数。 0より大きい正の数値を指定してください。 | 4 | × |
| [!UICONTROL Currency] | イベントで購入されるアイテムの通貨。 これはISO-4217で表され、指定されていない場合、デフォルトはUSDになります。 | USD | × |
| [!UICONTROL Value] | イベントで購入されるアイテムの価格値。 | 100.00 | × |
| [!UICONTROL Conversion ID] | 同じイベントタグ内のWeb ピクセルとコンバージョン API コンバージョン間の重複排除に使用できるコンバージョンイベントの識別子。 | 23294827 | × |
| [!UICONTROL Description] | コンバージョンに関する追加情報を含む説明。 | コンバージョンのテスト | × |

## [!DNL Twitter]内のデータを検証

イベント転送ルールを作成して実行したら、[!DNL Twitter] APIに送信されたイベントが[!DNL Twitter] UIで期待どおりに表示されているかどうかを検証します。

イベントの収集と[!DNL Experience Platform]統合が成功した場合、[!DNL Twitter] [!UICONTROL Events manager]内にイベントが表示されます。

![&#x200B; イベント マネージャー[!DNL Twitter]](../../../images/extensions/server/twitter/event-manager.png)

## 次の手順

このガイドでは、イベント転送を使用してコンバージョンイベントを[!DNL Twitter]に送信する方法について説明しました。 これらの基盤となるテクノロジーについて詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Twitter] API](https://developer.twitter.com/en/docs/twitter-api)
* [[!DNL Twitter] Web コンバージョン API](https://developer.twitter.com/en/docs/twitter-ads-api/measurement/api-reference/conversions)
* [[!DNL Twitter]  ユーザーアクセストークン &#x200B;](https://developer.twitter.com/en/docs/authentication/oauth-1-0a/obtaining-user-access-tokens)
* [&#x200B; ピクセル IDとコンバージョンの追跡](https://business.twitter.com/en/help/campaign-measurement-and-analytics/conversion-tracking-for-websites.html)

---
title: Google Ads Enhanced Conversions 拡張機能
description: Adobe Experience Platformのイベント転送用のGoogle Ads Enhanced Conversions 拡張機能について説明します。
exl-id: 65cdff40-276f-4481-9621-6c6861dbd412
last-substantial-update: 2022-11-23T00:00:00Z
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '1296'
ht-degree: 1%

---

# [!DNL Google Ads] Enhanced Conversions 拡張機能

[!DNL Google Ads] API を使用すると、コンバージョン調整の形式でファーストパーティ顧客データを送信することで、[ 拡張コンバージョン ](https://support.google.com/google-ads/answer/9888656) を活用できます。 Googleでは、この追加データを使用して、広告インタラクションに基づくオンラインコンバージョンのレポート機能を向上させます。

[[!DNL Google Ads]  拡張コンバージョンイベント転送拡張機能 ](https://exchange.adobe.com/apps/ec/108630/google-ads-enhanced-conversions) （以前の名称は [!DNL Enhanced Conversions] 拡張機能）は、[!DNL Google Ads] API の拡張コンバージョンを簡単に実装できる使いやすいテンプレートを提供します。

>[!IMPORTANT]
>
>エンハンスドコンバージョンは、購読、サインアップ、購入など、顧客データが存在するコンバージョンタイプでのみ機能します。 イベント転送ルールの [ コンバージョンアクションの設定 ](#conversion-action-event-forwarding) 時に必要になるので、次の顧客データを 1 つ以上を使用できる必要があります。
>
>* メールアドレス （推奨）
>* 名前と自宅住所（番地、市区町村、都道府県、郵便番号）
>* 電話番号（上記の他の 2 つの情報の 1 つに加えて指定する必要があります）

## 実装の概要

コンバージョンの強化機能 [!DNL Google Ads] API を活用して、クライアントデバイス（通常は web サイト）で発生したコンバージョンにファーストパーティデータを追加します。 つまり、エンハンスコンバージョンを実装するには、次の 2 つの手順があります。

1. クライアントからコンバージョンを送信します。
1. イベント転送を使用して、クライアントから送信されるコンバージョンデータを強化する追加のファーストパーティデータを送信します。

>[!TIP]
>
>イベント転送から送信されたファーストパーティデータにクライアントサイドのコンバージョンイベントを関連付けるには、両方の呼び出しで `transaction_ID` が同じである必要があります。 この値を各サービスに対して指定する必要がある場所について詳しくは、[ タグ ](#conversion-action-tags) と [ イベント転送 ](#conversion-action-event-forwarding) のコンバージョンアクションの設定の節を参照してください。

コンバージョンイベントの送信にはクライアントサイドとサーバーサイドの両方の実装が必要なので、このドキュメントでは、イベント転送用の [!DNL Enhanced Conversions] 拡張機能に加えて ](https://exchange.adobe.com/apps/ec/101437/google-global-site-tag-gtag) クライアントサイドの [[!DNL Google Global Site Tag]  （gtag）拡張機能を設定するための前提条件の手順について説明します。

次のビデオでは、[!DNL Enhanced Conversions] 拡張機能の概要と、実装手順の概要を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3411365?quality=12&learn=on)

## タグを使用してコンバージョンを送信

から web サイトにコンバージョンイベントを送信するには、[!DNL Google Global Site Tag] （gtag）をデプロイする必要があります。 これを行うには、タグを使用して、[!DNL Google Global Site Tag] （gtag）拡張機能を設定およびインストールします。

### [!DNL Google Global Site Tag] 拡張機能の設定とインストール

[!UICONTROL  データ収集 ] UI またはExperience PlatformUI に移動し、左側のナビゲーションで **[!UICONTROL タグ]** を選択します。 拡張機能をインストールするタグプロパティを選択し、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択します。 「**[!UICONTROL カタログ]**」タブで、[!UICONTROL Google グローバルサイトタグ（gtag） ] 拡張機能を探し、「**[!UICONTROL インストール]**」を選択します。

![[!UICONTROL Google グローバルサイトタグ（gtag） ] 拡張機能は、[!UICONTROL  データ収集 ] UI の [!UICONTROL  拡張機能 ] ビューで選択されています。](../../../images/extensions/server/google-ads-enhanced-conversions/install-gtag-extension.png)

インストールダイアログが表示されます。 ここから **[!UICONTROL アカウントを追加]** を選択し、プロンプトが表示されたら次の値を指定します。

| アカウントのプロパティ | 説明 |
| --- | --- |
| アカウント名 | アカウントの一意の名前。 この名前は、タグ UI 内でのみ使用されます。 |
| アカウント ID | [!DNL Google Ads] アカウント ID。 この値を見つけるには、[!DNL Google Ads] にログインし、**[!DNL Tools and Settings]**/**[!DNL Conversions]**/**[!DNL Select a conversion action]**/**[!DNL Tag Setup]**/**[!DNL Install the Tag yourself]** に移動します。 アカウント ID 文字列は、`AW-` または `d` で始まるコードスニペットウィンドウにあります。 |
| 製品 | 「**[!UICONTROL Google広告（AdWords）]**」を選択します。 |

{style="table-layout:auto"}

終了したら、「**[!UICONTROL アカウントを追加]**」を選択し、「**[!UICONTROL 保存]**」を選択します。

### コンバージョンを送信アクションを追加 {#conversion-action-tags}

拡張機能をインストールしたら、コンバージョンアクションをタグルールに含めることができます。 機能強化するコンバージョンをリッスンするルールを作成または編集する際に、「[!UICONTROL  アクション ] の下の **[!UICONTROL 追加]** を選択します。 次のダイアログで、「[!UICONTROL  拡張機能 ]」ドロップダウンから「**[!UICONTROL Google グローバルサイトタグ （gtag）]**」を選択し、「**[!UICONTROL アクションタイプ ]」の下の「[!UICONTROL  イベントを送信]**」を選択します。

![ ルール編集ワークフローのアクション設定ビュー内で選択されている [!UICONTROL  イベントを送信 ] アクションタイプ ](../../../images/extensions/server/google-ads-enhanced-conversions/select-client-action.png)

[!DNL gtag] イベントを設定できる追加のコントロールが表示されます。 少なくとも、次のフィールドに入力する必要があります。

1. **[!UICONTROL イベント名（アクション）]**：値として `conversion` を入力します。
1. キーが `transaction_id` で、値が [ トランザクション ID[ 値を含む ](../../../ui/managing-resources/data-elements.md) データ要素 ](https://support.google.com/google-ads/answer/6386790) である新しいフィールドを追加します。
1. **[!UICONTROL コンバージョンラベル]**:[!DNL Google Ads] アカウントから適切なコンバージョンラベルを入力します。 この値を見つけるには、Google Ads にログインし、**[!DNL Tools and Settings]**/**[!DNL Conversions]**/**[!DNL Select a conversion action]**/**[!DNL Tag Setup]**/**[!DNL Use Google Tag Manager]** に移動します。 コンバージョンラベルは [!DNL Instructions] の下にあります。
   >[!IMPORTANT]
   >
   >[!DNL Google Ads] アカウントのタグ設定エリアで、拡張コンバージョンが有効になっていることを確認します。 これを行うには、利用規約を確認して同意し、実装方法として **[!DNL Turn on enhanced conversions]** と **[!DNL API]** を選択します。

アクションを設定したら、「**[!UICONTROL 変更を保持]**」を選択して、ルール設定にアクションを追加します。 ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。

最後に、新しい [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## イベント転送を使用したファーストパーティデータの送信

クライアントサイドからコンバージョンイベントを送信できるようになったら、[!DNL Enhanced Conversions] イベント転送拡張機能を使用して、これらのコンバージョンを強化できます。

### Google OAuth2 シークレットとデータ要素を作成 {#create-secret-data-element}

拡張機能を設定する前に、API に対して認証するためのアクセストークンをイベント転送で作成する必要 [!DNL Google Ads] あります。

手順について詳しくは、[ イベント転送の秘密鍵の作成 ](../../../ui/event-forwarding/secrets.md) に関するガイドを参照してください。 必ず **[!UICONTROL Google OAuth 2]** を秘密鍵のタイプとして選択してください。 画面の指示に従い、Google アカウントプロファイルの選択を求められたら、設定中のコンバージョンアクションにアクセスできるアカウントを選択します。

秘密鍵を作成したら [ 新しいデータ要素を作成 ](../../../ui/managing-resources/data-elements.md#create-a-data-element)、データ要素タイプとして **[!UICONTROL 秘密鍵]** を選択します。 各環境に適したGoogle OAuth2 シークレットを選択し、「**[!UICONTROL ライブラリに保存]**」を選択します。

### [!DNL Enhanced Conversions] 拡張機能の設定とインストール {#install-enhanced-conversions}

イベント転送カタログで [!UICONTROL Google Ads Enhanced Conversions] 拡張機能を探し、「**[!UICONTROL インストール]**」を選択します。

![Data Collection] UI の {Extensions] ビューで選択されている [!UICONTROL 1}Google Ads Enhanced Conversions[!UICONTROL  拡張機能 ]](../../../images/extensions/server/google-ads-enhanced-conversions/install-enhanced-conversions.png)[!UICONTROL 

拡張機能を設定するには、次の 2 つの必須フィールドに入力する必要があります。

1. **[!UICONTROL 顧客 ID]**:[!DNL Google Ads] アカウントを一意に識別する ID。 この値を見つけるには、[!DNL Google Ads] にログインし、**[!DNL Help]**/**[!DNL Customer ID]** に移動します。
2. **[!UICONTROL アクセストークンデータ要素]**：データ要素アイコン（![ データ要素アイコン ](/help/images/icons/database.png)）を選択し、（前の手順で設定した [Google OAuth2 シークレットデータ要素をメニューから選択し ](#create-secret-data-element) す。

終了したら、「**[!UICONTROL 保存]**」を選択して拡張機能をインストールします。

### ルールに [!UICONTROL  コンバージョンを送信 ] アクションを追加する {#conversion-action-event-forwarding}

拡張機能がインストールされたら、イベント転送ルールに [!UICONTROL  コンバージョンを送信 ] アクションを含めることができます。 機能強化するコンバージョンをリッスンするルールを作成または編集する際に、「[!UICONTROL  アクション ] の下の **[!UICONTROL 追加]** を選択します。 次のダイアログで、「[!UICONTROL  拡張機能 ]」ドロップダウンから「**[!UICONTROL Google Ads Enhanced Conversions]**」を選択し、「[!UICONTROL  アクションタイプ ]」の「**[!UICONTROL コンバージョンを送信]**」を選択します。

![ ルール編集ワークフローのアクション設定ビュー内で選択されている [!UICONTROL  コンバージョンを送信 ] アクションタイプ ](../../../images/extensions/server/google-ads-enhanced-conversions/select-server-action.png)

新しいコントロールが右側のパネルに表示され、コンバージョンを設定できます。 少なくとも、次のフィールドに入力する必要があります。

**変換情報**

| 入力 | 説明 |
| --- | --- |
| 顧客 ID | [!DNL Google Ads] 顧客 ID。 デフォルトは、[ 拡張機能のインストール ](#install-enhanced-conversions) 時に入力した顧客 ID です。 |
| コンバージョン ID またはコンバージョンラベル | コンバージョントラッキングの設定時に [!DNL Google Ads] から取得したトラッキング値。 値は `AW-` で始まります。<br><br> これらの値の見つけ方について詳しくは、[[!DNL Google Ads]  ドキュメント ](https://support.google.com/tagmanager/answer/6105160?hl=en) を参照してください。 |
| トランザクション ID | [!DNL Google Global Site Tag] 拡張機能を使用して、[ クライアントサイドから送信された ](#conversion-action-tags) トランザクション ID 値が同じデータ要素を選択します。 |

**ユーザーの識別**

* 次の 3 つのユーザー識別子のうち、少なくとも 1 つを含める必要があります。
   * メール
   * 電話番号
   * フルアドレス

>[!TIP]
>
>ユーザー ID データは、Googleに送信する前にハッシュ化する必要があります。 イベント転送でデータを受け取ってもハッシュ化されていない場合は、特定のフィールドの **[!UICONTROL 正規化とハッシュ]** トグルを選択して、拡張機能に値をハッシュ化するように指示します。
>
>![[!UICONTROL  コンバージョンを送信 ] アクション設定フォーム内の [!UICONTROL  メール ] 入力に対して有効な [!UICONTROL  正規化とハッシュ ] トグル ](../../../images/extensions/server/google-ads-enhanced-conversions/hash-user-id-values.png)

完了したら、「**[!UICONTROL 変更を保持]**」を選択して、アクションをルール設定に追加します。 ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。

最後に、新しいイベント転送 [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## 次の手順

このガイドでは、[!DNL Enhanced Conversions] イベント転送拡張機能を使用してコンバージョンイベントを [!DNL Google Ads] に送信する方法について説明しました。 Experience Platformのイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。

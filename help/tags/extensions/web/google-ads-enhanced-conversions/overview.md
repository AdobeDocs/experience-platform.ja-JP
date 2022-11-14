---
title: Google Ads Enhanced Conversions 拡張機能
description: Adobe Experience Platformでのイベント転送に関するGoogle Ads 拡張コンバージョン拡張機能について説明します。
source-git-commit: a279c44ef9df3aa9bfc7763b153b87bde0015d57
workflow-type: tm+mt
source-wordcount: '1295'
ht-degree: 2%

---

# [!DNL Google Ads] 拡張コンバージョン拡張機能

の使用 [!DNL Google Ads] API、 [コンバージョンの強化](https://support.google.com/google-ads/answer/9888656) コンバージョン調整の形でファーストパーティの顧客データを送信する。 Googleは、この追加データを使用して、広告インタラクションによって駆動されるオンラインコンバージョンのレポートを改善します。

この [[!DNL Google Ads] コンバージョンイベント転送拡張機能の強化](https://exchange.adobe.com/apps/ec/108630/google-ads-enhanced-conversions) ( 従来、 [!DNL Enhanced Conversions] 拡張機能を参照 ) は、拡張コンバージョンを簡単に実装できる、使いやすいテンプレートを提供します。 [!DNL Google Ads] API

>[!IMPORTANT]
>
>変換機能の拡張は、購読、新規登録、購入など、顧客データが存在するコンバージョンタイプに対してのみ機能します。 次の場合に必要な、1 つ以上の顧客データを使用できる必要があります。 [変換アクションの設定](#conversion-action-event-forwarding) イベント転送ルールの場合：
>
>* 電子メールアドレス（推奨）
>* 氏名および住所（住所、市区町村、都道府県/地域、郵便番号）
>* 電話番号（上記の 2 つの情報のうち 1 つに加えて、もう 1 つも指定する必要があります）


## 実装の概要

コンバージョンの強化により、 [!DNL Google Ads] ファーストパーティデータをクライアントデバイス（通常は Web サイト）で発生したコンバージョンに追加する API。 つまり、拡張コンバージョンを実装する手順は次の 2 つです。

1. クライアントから変換を送信します。
1. イベント転送を使用して、クライアントから送信されるコンバージョンデータを強化する追加のファーストパーティデータを送信します。

>[!TIP]
>
>クライアント側コンバージョンイベントをイベント転送から送信されたファーストパーティデータに関連付けるには、 `transaction_ID` は、両方の呼び出しで同じである必要があります。 各サービスでこの値を指定する必要がある場所について詳しくは、 [タグ](#conversion-action-tags) および [イベント転送](#conversion-action-event-forwarding)、それぞれ。

コンバージョンイベントの送信には、クライアント側とサーバー側の両方の実装が必要なので、このドキュメントでは、クライアント側の設定の前提条件となる手順について説明します [[!DNL Google Global Site Tag] (gtag) 拡張](https://exchange.adobe.com/apps/ec/101437/google-global-site-tag-gtag) に加えて [!DNL Enhanced Conversions] イベント転送用の拡張機能。

## タグを使用してコンバージョンを送信する

Web サイト上からコンバージョンイベントを送信するには、次の手順を実行します。 [!DNL Google Global Site Tag] (gtag) をデプロイする必要があります。 タグを使用し、 [!DNL Google Global Site Tag] (gtag) 拡張機能。

### の設定とインストール [!DNL Google Global Site Tag] 拡張

次に移動： [!UICONTROL データ収集] UI またはExperience PlatformUI と選択 **[!UICONTROL タグ]** をクリックします。 拡張機能をインストールするタグプロパティを選択し、「 」を選択します。 **[!UICONTROL 拡張機能]** をクリックします。 以下 **[!UICONTROL カタログ]** タブで、 [!UICONTROL Google Global Site Tag (gtag)] 拡張機能と選択 **[!UICONTROL インストール]**.

![この [!UICONTROL Google Global Site Tag (gtag)] 以下の下で選択されている拡張 [!UICONTROL 拡張機能] 表示 [!UICONTROL データ収集] UI](../../../images/extensions/google-ads-enhanced-conversions/install-gtag-extension.png)

インストールダイアログが表示されます。 ここからを選択します。 **[!UICONTROL アカウントを追加]** プロンプトが表示されたら、次の値を指定します。

| Account プロパティ | 説明 |
| --- | --- |
| アカウント名 | アカウントの一意の名前。 この名前は、タグ UI 内でのみ使用されます。 |
| アカウント ID | お使いの [!DNL Google Ads] アカウント ID。 この値を見つけるには、次にログインします。 [!DNL Google Ads] に移動します。 **[!DNL Tools and Settings]** > **[!DNL Conversions]** > **[!DNL Select a conversion action]** > **[!DNL Tag Setup]** > **[!DNL Install the Tag yourself]**. アカウント ID の文字列は、で始まるコードスニペットウィンドウに表示されます。 `AW-` または `d`. |
| 製品 | 選択 **[!UICONTROL Google Ads (AdWords)]**. |

{style=&quot;table-layout:auto&quot;}

終了したら、「 」を選択します。 **[!UICONTROL アカウントを追加]**&#x200B;を選択し、「 **[!UICONTROL 保存]**.

### 変換アクションの送信の追加 {#conversion-action-tags}

拡張機能をインストールした後、タグルールにコンバージョンアクションを含めることができます。 拡張する変換をリッスンするルールを作成または編集する場合は、「 」を選択します **[!UICONTROL 追加]** under [!UICONTROL アクション]. 次のダイアログで、 **[!UICONTROL Google Global Site Tag (gtag)]** から [!UICONTROL 拡張] ドロップダウン、「選択」 **[!UICONTROL イベントの送信]** under [!UICONTROL アクションタイプ].

![この [!UICONTROL イベントの送信] ルール編集ワークフローの「アクション設定」ビュー内で選択されているアクションタイプ。](../../../images/extensions/google-ads-enhanced-conversions/select-client-action.png)

追加のコントロールが表示され、 [!DNL gtag] イベント。 少なくとも、次のフィールドには必ず入力する必要があります。

1. **[!UICONTROL イベント名（アクション）]**:入力 `conversion` を値として使用します。
1. キーが存在する新しいフィールドを追加 `transaction_id` 値は [データ要素](../../../ui/managing-resources/data-elements.md) を含む [トランザクション ID](https://support.google.com/google-ads/answer/6386790) の値です。
1. **[!UICONTROL コンバージョンラベル]**:適切な変換ラベルを [!DNL Google Ads] アカウント この値を見つけるには、Google Ads にログインし、に移動します。 **[!DNL Tools and Settings]** > **[!DNL Conversions]** > **[!DNL Select a conversion action]** > **[!DNL Tag Setup]** > **[!DNL Use Google Tag Manager]**. 変換ラベルは、の下にあります。 [!DNL Instructions].
   >[!IMPORTANT]
   >
   >をクリックします。 [!DNL Google Ads] アカウントで、拡張コンバージョンが有効になっていることを確認してください。 これをおこなうには、利用規約を確認して同意し、 **[!DNL Turn on enhanced conversions]** および **[!DNL API]** を使用します。

アクションを設定したら、「 **[!UICONTROL 変更を保持]** をクリックして、ルール設定にアクションを追加します。 ルールに問題がない場合は、「 」を選択します。 **[!UICONTROL ライブラリに保存]**.

最後に、新しい [ビルド](../../../ui/publishing/builds.md) ライブラリへの変更を有効にします。

## イベント転送を使用したファーストパーティデータの送信

クライアント側からコンバージョンイベントを送信できるようになったら、 [!DNL Enhanced Conversions] イベント転送拡張機能。

### Google OAuth 2 シークレットとデータ要素の作成 {#create-secret-data-element}

拡張機能を設定する前に、に対する認証をおこなうために、イベント転送でアクセストークンを作成する必要があります。 [!DNL Google Ads] API

詳しくは、 [イベント転送の秘密鍵の作成](../../../ui/event-forwarding/secrets.md) を参照してください。 必ず **[!UICONTROL Google OAuth 2]** を秘密の型として 引き続き画面の指示に従い、 Googleアカウントプロファイルの選択を求められたら、設定中の変換アクションにアクセスできるアカウントを選択します。

秘密鍵が作成されたら、 [新しいデータ要素の作成](../../../ui/managing-resources/data-elements.md#create-a-data-element) を選択し、 **[!UICONTROL 秘密鍵]** （データ要素タイプ用） 各環境で適切なGoogle OAuth 2 秘密鍵を選択し、「 」を選択します。 **[!UICONTROL ライブラリに保存]**.

### の設定とインストール [!DNL Enhanced Conversions] 拡張 {#install-enhanced-conversions}

次を検索： [!UICONTROL Google Ads 拡張コンバージョン] イベント転送カタログの拡張機能を選択し、 **[!UICONTROL インストール]**.

![この [!UICONTROL Google Ads 拡張コンバージョン] 以下の下で選択されている拡張 [!UICONTROL 拡張機能] 表示 [!UICONTROL データ収集] UI](../../../images/extensions/google-ads-enhanced-conversions/install-enhanced-conversions.png)

拡張機能を設定するには、2 つの必須フィールドに値を入力する必要があります。

1. **[!UICONTROL 顧客 ID]**:を一意に識別する ID [!DNL Google Ads] アカウント この値を見つけるには、次にログインします。 [!DNL Google Ads] をクリックし、 **[!DNL Help]** > **[!DNL Customer ID]**.
1. **[!UICONTROL トークンデータ要素にアクセス]**:データ要素アイコン (![データ要素アイコン](../../../images/extensions/google-ads-enhanced-conversions/data-element-icon.png)) をクリックし、目的のGoogle OAuth 2 secret データ要素を選択します。 [前の手順で設定済み](#create-secret-data-element) を選択します。

終了したら、「 」を選択します。 **[!UICONTROL 保存]** をクリックして、拡張機能をインストールします。

### を追加します。 [!UICONTROL 変換を送信] ルールに対するアクション {#conversion-action-event-forwarding}

拡張機能がインストールされたら、 [!UICONTROL 変換を送信] イベント転送ルールのアクションを参照してください。 拡張する変換をリッスンするルールを作成または編集する際に、「 」を選択します **[!UICONTROL 追加]** under [!UICONTROL アクション]. 次のダイアログで、 **[!UICONTROL Google Ads 拡張コンバージョン]** から [!UICONTROL 拡張] ドロップダウン、「選択」 **[!UICONTROL 変換を送信]** under [!UICONTROL アクションタイプ].

![この [!UICONTROL 変換を送信] ルール編集ワークフローの「アクション設定」ビュー内で選択されているアクションタイプ。](../../../images/extensions/google-ads-enhanced-conversions/select-server-action.png)

新しいコントロールが右側のパネルに表示され、コンバージョンを設定できます。 少なくとも、次のフィールドに入力する必要があります。

**コンバージョン情報**

| 必要情報 | 説明 |
| --- | --- |
| 顧客 ID | お使いの [!DNL Google Ads] 顧客 ID。 デフォルトでは、 [拡張機能のインストール](#install-enhanced-conversions). |
| コンバージョン ID またはコンバージョンラベル | から取得した追跡値 [!DNL Google Ads] コンバージョントラッキングを設定する際に使用します。 値は次の値で始まります。 `AW-`.<br><br>これらの値の検索方法の詳細については、 [[!DNL Google Ads] ドキュメント](https://support.google.com/tagmanager/answer/6105160?hl=ja). |
| トランザクション ID | 同じトランザクション ID 値を持つデータ要素を選択します。 [クライアント側から送信](#conversion-action-tags) の使用 [!DNL Google Global Site Tag] 拡張子。 |

**ユーザー ID**

* 次の 3 つのユーザー識別子のうち少なくとも 1 つを含める必要があります。
   * メール
   * 電話番号
   * 住所全体

>[!TIP]
>
>ユーザー ID データをGoogleに送信する前に、ハッシュ化する必要があります。 イベント転送がデータを受信した際にデータがハッシュ化されない場合は、 **[!UICONTROL Normalize &amp; Hash]** 特定のフィールドを切り替えて、拡張機能に値をハッシュ化するように指示します。
>
>![この [!UICONTROL Normalize &amp; Hash] 有効にする [!UICONTROL 電子メール] 内の入力 [!UICONTROL 変換を送信] アクション設定フォーム。](../../../images/extensions/google-ads-enhanced-conversions/hash-user-id-values.png)

終了したら、「 」を選択します。 **[!UICONTROL 変更を保持]** をクリックして、ルール設定にアクションを追加します。 ルールに問題がない場合は、「 」を選択します。 **[!UICONTROL ライブラリに保存]**.

最後に、新しいイベント転送を公開します [ビルド](../../../ui/publishing/builds.md) ライブラリへの変更を有効にします。

## 次の手順

このガイドでは、コンバージョンイベントをに送信する方法を説明しました。 [!DNL Google Ads] の使用 [!DNL Enhanced Conversions] イベント転送拡張機能。 Experience Platformのイベント転送機能について詳しくは、 [イベント転送の概要](../../../ui/event-forwarding/overview.md).

---
keywords: イベント転送拡張機能；mixpanel;mixpanel イベント転送拡張機能
title: Mixpanel Track Events API イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能は、Edge Network イベントを Mixpanel に送信します。
last-substantial-update: 2023-03-29T00:00:00Z
exl-id: 21e2e0fa-4949-4be4-859f-d449d21d8f41
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 2%

---

# [!DNL Mixpanel Track Events] API イベント転送拡張機能

[[!DNL Mixpanel]](https://www.mixpanel.com) は、ユーザーによるデジタル製品とのやり取りに関するデータを取得できる製品分析ツールです。 数回クリックするだけでデータをクエリして視覚化できるシンプルでインタラクティブなレポートを使用して、製品データを分析できます。 すべてのユーザー [!DNL Mixpanel] ユーザーデータをリアルタイムで分析し、傾向を特定し、ユーザーの行動を把握し、製品に関する意思決定を行えるようにすることで、チームの効率を高めるように設計されています。

[!DNL Mixpanel] には、各インタラクションを 1 人のユーザーに接続する、イベントベースのユーザー中心型モデルが採用されています。 [!DNL Mixpanel] データモデルは、ユーザー、イベント、プロパティの概念に基づいて構築されています。

>[!NOTE]
>
>イベントを結合して ID クラスターを作成する方法については、[ID 管理 &#x200B;](https://help.mixpanel.com/hc/en-us/articles/360041039771-Getting-Started-with-Identity-Management) に関する [!DNL Mixpanel] のドキュメント [!DNL Mixpanel] 参照してください。 また、[&#x200B; 個別の ID](https://help.mixpanel.com/hc/en-us/articles/115004509426-Distinct-ID-Creation-JavaScript-iOS-Android-) に関するドキュメントを確認して、イベントデータでユーザーを識別する際にどのように使用されるかを理解することをお勧めします。

## ユースケース

この拡張機能は、Edge Networkのデータを使用して製品分析機能を活用す [!DNL Mixpanel] 場合に使用します。

例えば、マルチチャネルプレゼンス（web サイトとモバイル）を持つ小売組織について考えてみます。 組織は、プラットフォームからイベントデータとしてトランザクション入力や会話入力を取得し、イベント転送拡張機能を使用して [!DNL Mixpanel] に読み込みます。

その後、分析チームは [!DNL Mixpanel's] の機能を活用してデータセットを処理し、ビジネスインサイトを得ることができます。これを使用して、グラフ、ダッシュボード、その他のビジュアライゼーションを生成し、ビジネス関係者に情報を提供できます。

[!DNL Mixpanel] 固有のユースケースについて詳しくは、次のドキュメントを参照してください。

* [&#x200B; 新規  [!DNL Mixpanel]](https://docs.mixpanel.com/docs)
* [&#x200B; [!DNL Mixpanel] について](https://developer.mixpanel.com/docs)
* [12 [!DNL Mixpanel]  必見 &#x200B;](https://mixpanel.com/blog/12-things-you-probably-didnt-know-you-could-do-with-mixpanel/)

## [!DNL Mixpanel] 前提条件 {#prerequisites-mixpanel}

この拡張機能を使用するには、有効な [!DNL Mixpanel] アカウントが必要です。 アカウントをまだお持ちでない場合は、[[!DNL Mixpanel]  登録ページ &#x200B;](https://mixpanel.com/register/) に移動し、アカウントを登録して作成してください。

[[!DNL Identity Merge]](https://help.mixpanel.com/hc/en-us/articles/9648680824852-ID-Merge-Implementation-Best-Practices) 設定がプロジェクトに対して有効になっていることを確認します。 **[!DNL Settings]**/**[!DNL Project Setting]**/**[!DNL Identity Merge]** に移動し、設定を切り替えます。

### [!DNL Mixpanel] の ID クラスターについて

ま [!DNL Mixpanel]、ID クラスターには、個々のユーザーに接続する `distinct_id` 値のコレクションが含まれます。 [!DNL Mixpanel] はユーザーごとに ID のクラスターを処理し、レポートで使用される各クラスターから 1 つの正規 `distinct_id` を解決します。 ユーザー ID イベントの前に発生する匿名イベントに、独自の ID （ローカル `distinct_id` と呼ばれます）を含めることもできます。

[!DNL Mixpanel] は、次の 2 つの方法で id クラスターを解決します。

* **ID**：選択した ID[!DNL Mixpanel] 匿名 `distinct_id` に接続します。 Web サイトで [!DNL Mixpanel] SDKが有効になっている場合、Experience Platformは、現在ログインしているユーザーに割り当てられている `distinct_id` を使用します。
* **エイリアス**：追加の結合条件 [!DNL Mixpanel] 満たされた場合に、2 つの非匿名 `distinct id` を組み合わせます。

>[!NOTE]
>
>これらの方法について詳しくは、[identity management](https://help.mixpanel.com/hc/en-us/articles/360041039771-Getting-Started-with-Identity-Management#user-identification) に関する [!DNL Mixpanel] ドキュメントを参照してください。
>
>[[!DNL Mixpanel] ID 結合機能 &#x200B;](#prerequisites-mixpanel) が有効になっていることを確認して、ID クラスターが適切に解決されていることを確認します。

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Mixpanel] に接続するには、次の入力が必要です。

| キータイプ | 説明 | 例 |
| --- | --- | --- |
| プロジェクトトークン | [!DNL Mixpanel] アカウントに関連付けられたプロジェクトトークン。 詳しくは、[&#x200B; プロジェクトトークンの検索 &#x200B;](https://help.mixpanel.com/hc/en-us/articles/115004502806-Find-Project-Token-) に関する [!DNL Mixpanel] ドキュメントを参照してください。 | `25470xxxxxxxxxxxxxxxxxxx1289` |

## [!DNL Mixpanel] 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティを作成 &#x200B;](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。「**[!UICONTROL カタログ]**」タブで、[!DNL Mixpanel] 拡張機能のカードの **[!UICONTROL インストール]** を選択します。

![[!DNL Mixpanel] 拡張機能のインストール &#x200B;](../../../images/extensions/server/mixpanel/install-extension.png)

## [!DNL Send Event] ルールの作成

イベント転送プロパティで新しいルールの作成を開始します。 **[!UICONTROL アクション]** の下に新しいアクションを追加し、拡張機能を **[!UICONTROL Mixpanel]** に設定します。 次に、アクションタイプを **[!UICONTROL トラックイベント]** に設定して、Edge Network イベントを [!DNL Mixpanel] に送信します。

| 入力 | 説明 | 必須 |
| --- | --- | --- |
| [!UICONTROL &#x200B; プロジェクトトークン &#x200B;] | このフィールドは、[!DNL Mixpanel] アカウントに関連付けられたプロジェクトトークンにマッピングする必要があります。 | ○ |
| [!UICONTROL イベントタイプ] | イベント名。 | ○ |
| [!UICONTROL &#x200B; イベント時間 &#x200B;] | イベント時間。 | |
| [!UICONTROL Mixpanel の個別 ID] | イベントを実行したユーザーの一意の ID。 | |
| [!UICONTROL ID を挿入 &#x200B;] | 重複排除に使用される、イベントの一意の ID。 | |
| [!UICONTROL &#x200B; イベントのプロパティ &#x200B;] | イベントのカスタムプロパティを含む JSON オブジェクト。 生の JSON を提供するか、キー値の入力のシンプルなセットを使用するかを選択します。 | |

>[!NOTE]
>
>[!DNL Mixpanel] イベントの標準フィールドについて詳しくは、[&#x200B; 公式ドキュメント &#x200B;](https://developer.mixpanel.com/reference/import-events#event) を参照してください。

![&#x200B; イベント転送ルールアクション設定を追加します &#x200B;](../../../images/extensions/server/mixpanel/track-event-action.png)。

[!UICONTROL &#x200B; イベントの追跡 &#x200B;] アクションがルールに追加されたら、特定のイベントに対してのみ発生するようにルールの条件を設定したり、条件セクションを空のままにして、すべてのイベントに対してルールを発生させることができます。

>[!IMPORTANT]
>
>Web サイトで [!DNL Mixpanel] SDKを使用している場合は、次の手順 [&#x200B; 内でのデータの検証  [!DNL Mixpanel]](#validate) に進むことができます。 [!DNL Mixpanel] SDKを使用していない場合は、[&#x200B; 個別の ID トラッキングルールを作成 &#x200B;](#create-an-identity-tracking-rule) して、ユーザー ID イベントが発生したときに適切なイベントと `distinct_id` 値が [!DNL Mixpanel] に送信されるようにする必要があります。

## [!DNL Mixpanel] 内のデータの検証 {#validate}

実装が成功し、イベントが収集された場合は、[[!DNL Mixpanel] console](https://help.mixpanel.com/hc/en-us/articles/4402837164948) 内にイベントが表示されます。

メール値 [!DNL Mixpanel] 入力されたログイン後イベントと、**[!UICONTROL イベントを送信]** を使用する際に作成されたイベントを結合したかどうかを確認します。 正しく実装され [!DNL Mixpanel] 場合は、それらを単一の [&#x200B; ユーザープロファイル &#x200B;](https://help.mixpanel.com/hc/en-us/articles/115004501966) に関連付けます。

## 次の手順

このガイドでは、イベント転送を使用してコンバージョンイベントを [!DNL Mixpanel] に送信する方法について説明しました。 このイベント転送拡張機能は、[!DNL Mixpanel] SDK API とJavaScript API を活用します。 これらの基盤となるテクノロジーについて詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Mixpanel] SDK](https://developer.mixpanel.com/docs/nodejs)
* [[!DNL Mixpanel] JavaScript API](https://developer.mixpanel.com/docs/javascript-full-api-reference#mixpanelidentify)

Experience Platformのイベント転送機能について詳しくは、[&#x200B; イベント転送の概要 &#x200B;](../../../ui/event-forwarding/overview.md) を参照してください。

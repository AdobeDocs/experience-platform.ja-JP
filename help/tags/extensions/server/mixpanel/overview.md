---
keywords: イベント転送拡張機能；mixpanel;mixpanel イベント転送拡張機能
title: Mixpanel Track Events API イベント転送拡張機能
description: このAdobe Experience Platformイベント転送拡張機能は、Adobe Experience Edge ネットワークイベントを Mixpanel に送信します。
source-git-commit: 54aa55fbafb5e9603b109dbf015e009efd8f5e1d
workflow-type: tm+mt
source-wordcount: '2343'
ht-degree: 2%

---

# [!DNL Mixpanel Track Events] API イベント転送拡張機能

[[!DNL Mixpanel]](https://www.mixpanel.com) は、ユーザーがデジタル製品とどのようにやり取りするかに関するデータをキャプチャできる、製品分析ツールです。 数回のクリックでデータをクエリし視覚化できる、シンプルでインタラクティブなレポートを使用して製品データを分析できます。 [!DNL Mixpanel] 誰もがリアルタイムでユーザーデータを分析し、傾向を特定し、ユーザー行動を把握し、製品に関する意思決定をおこなうことで、チームをより効率的にします。

[!DNL Mixpanel] は、各インタラクションを 1 人のユーザーに接続する、イベントベースのユーザー中心モデルを採用しています。 この [!DNL Mixpanel] データモデルは、ユーザー、イベント、プロパティの概念に基づいて構築されます。

>[!NOTE]
>
>詳しくは、 [!DNL Mixpanel] ドキュメント [id 管理](https://help.mixpanel.com/hc/en-us/articles/360041039771-Getting-Started-with-Identity-Management) どうやって理解するか [!DNL Mixpanel] イベントを結合して id クラスターを作成します。 また、 [ユニーク ID](https://help.mixpanel.com/hc/en-us/articles/115004509426-Distinct-ID-Creation-JavaScript-iOS-Android-) を参照して、イベントデータでユーザーを識別するためにユーザーがどのように使用されているかを理解します。

この [!DNL Mixpanel Track Events] API 拡張機能を使用すると、 [イベント転送](../../../ui/event-forwarding/overview.md) および [タグ](../../../home.md) を使用してAdobe Experience Platform Edge Network でイベント情報をキャプチャし、に送信できます。 [!DNL Mixpanel] の使用 [[!DNL Track Events] API](https://developer.mixpanel.com/reference/track-event). このドキュメントでは、拡張機能の使用例、拡張機能のインストール方法、イベント転送に拡張機能を統合する方法について説明します [ルール](../../../ui/managing-resources/rules.md).

## ユースケース

この拡張機能は、 [!DNL Mixpanel] を使用して、製品分析機能を活用できます。

例えば、マルチチャネルの存在（Web サイトおよびモバイル）を持つ小売組織について考えてみましょう。 組織は、プラットフォームからトランザクション入力または会話入力をイベントデータとしてキャプチャし、 [!DNL Mixpanel] イベント転送拡張機能の使用

分析チームは、 [!DNL Mixpanel's] データセットを処理し、ビジネスインサイトを導き出す機能。グラフ、ダッシュボード、その他のビジュアライゼーションを生成して、ビジネス関係者に知らせることができます。

特有の使用例の詳細 [!DNL Mixpanel]（次のドキュメントを参照）。

* [新規ユーザ [!DNL Mixpanel]](https://help.mixpanel.com/hc/en-us/sections/360008533532-New-to-Mixpanel)
* [ [!DNL Mixpanel] とは？](https://developer.mixpanel.com/docs)
* [12 の必須 [!DNL Mixpanel] 機能](https://mixpanel.com/blog/12-things-you-probably-didnt-know-you-could-do-with-mixpanel/)

## [!DNL Mixpanel] 前提条件 {#prerequisites-mixpanel}

有効な [!DNL Mixpanel] アカウントを使用して、この拡張機能を使用する必要があります。 次に移動： [[!DNL Mixpanel] 登録ページ](https://mixpanel.com/register/) をクリックして、アカウントを作成します（まだ持っていない場合）。

次を確認します。 [[!DNL Identity Merge]](https://help.mixpanel.com/hc/en-us/articles/9648680824852-ID-Merge-Implementation-Best-Practices) の設定がプロジェクトで有効になっている。 に移動します。 **[!DNL Settings]** > **[!DNL Project Setting]** > **[!DNL Identity Merge]** 設定を切り替えます。

<!-- (If these don't apply, do we need to include here at all?)
### API guardrails {#guardrails}

Refer to the [[!DNL Mixpanel] documentation](https://developer.mixpanel.com/reference/import-events#rate-limits) for limits and response codes. As [!DNL Mixpanel] only sends live events these limits should not apply.
-->

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Mixpanel] 次の入力が必要です。

| キータイプ | 説明 | 例 |
| --- | --- | --- |
| プロジェクトトークン | に関連付けられたプロジェクトトークン [!DNL Mixpanel] アカウント 詳しくは、 [!DNL Mixpanel] ドキュメント [プロジェクトトークンの検索](https://help.mixpanel.com/hc/en-us/articles/115004502806-Find-Project-Token-) 指導のために | `25470xxxxxxxxxxxxxxxxxxx1289` |

## Experience Cloudの前提条件

この節では、すべての実装のExperience Cloudの前提条件の手順を説明します。 個々の実装のニーズに応じて、拡張機能を設定する前に、次の構成を設定すると便利です。

1. A [スキーマ](../../../../xdm/schema/composition.md) Experience Cloudに取り込むデータの構造を記述する
1. A [datastream](https://experienceleague.adobe.com/docs/platform-learn/data-collection/event-forwarding/set-up-a-datastream.html) 受信データを適切なAdobe Experience Cloudアプリケーションにルーティングする
1. A [データセット](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja) 収集したデータを保存するには

すべての実装で、Experience Cloud側で次が必要です。

1. [秘密鍵の作成](#create-a-secret)
1. [タグプロパティの設定](#set-up-tag-properties)
1. [タグプロパティ内にデータ要素を追加する](#add-data-elements-within-tag-properties)
1. [タグプロパティ内にルールを追加する](#add-rules-within-tag-properties)

### 秘密鍵の作成

新しい [イベント転送秘密鍵](../../../ui/event-forwarding/secrets.md) の値を [[!DNL Mixpanel] プロジェクトトークン](#configuration-details). これは、値のセキュリティを維持しながら、アカウントへの接続を認証するために使用されます。

### タグプロパティの設定

[タグプロパティの作成](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=en) または、代わりに編集する既存のプロパティを選択します。 このプロパティは、 [!DNL Mixpanel] イベント転送を使用して送信される前に Edge ネットワークに取り込まれるためです。

### タグプロパティ内にデータ要素を追加する

Web サイトが [[!DNL Mixpanel] SDK](https://developer.mixpanel.com/docs/nodejs)を [データ要素の作成](../../../ui/managing-resources/data-elements.md) は **[!UICONTROL Cookie]** タイプ ( [[!UICONTROL コア] タグ拡張](../../client/core/overview.md)) [!DNL Mixpanel] `distinct_id` は Cookie から読み取ることができます。

この **[!UICONTROL Cookie 名]** 値は [!DNL Mixpanel] web サイトの cookie 名。 名前は、次のような形式にする必要があります `mp_{MIXPANEL_PROJECT_TOKEN_FOR_WEBSITE}_mixpanel`. 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![タグプロパティのユニーク ID データ要素。](../../../images/extensions/server/mixpanel/distinctId-data-element.png)

>[!IMPORTANT]
>
>上記のデータ要素の名前 (`distinctId` （この例では）は、スキーマ内の同じフィールドに使用される名前と一致する必要があります。 これは、後で作成するイベント転送データ要素にも当てはまります。

2 番目のデータ要素の場合、タイプをに設定します。 **[!UICONTROL XDM オブジェクト]** ( [Adobe Experience Platform Web SDK 拡張機能](../../client/sdk/overview.md)) にマッピングし、前に作成したスキーマにマッピングします。 データをマッピングする際は、必ず `distinct_id` データ要素 ( [!DNL Mixpanel] `distinct_id` の値がスキーマフィールドの 1 つ内の値として参照される )。

![タグプロパティの cookie からスキーマに distinct_id をバインドする XDM オブジェクトのデータ要素。](../../../images/extensions/server/mixpanel/xdm-data-element.png)

>[!NOTE]
>
>Web サイトが [!DNL Mixpanel] SDK、Adobe Experience Cloud ID(ECID) はフォールバックとして使用されます `distinct_id` に送信されるイベントで渡される値 [!DNL Mixpanel].

シナリオに応じて、スキーマ内のイベント名にマッピングするために使用できる別のデータ要素を作成する必要が生じる場合があります。 これは、 **[!UICONTROL DOM 属性]** 提供されるタイプ [!UICONTROL コア] 拡張子。

![タグプロパティでのデータ要素 eventnamesign。](../../../images/extensions/server/mixpanel/eventname-signin-data-element.png)

### タグプロパティ内にルールを追加する

データ要素を設定したら、どのイベントによってデータの送信先になるかを決定するルールの作成を開始できます。 [!DNL Mixpanel].

まず、ユーザー識別イベントに対してトリガーされるルールを作成します。 これは、ユーザーの識別に使用するログイン、サインアップ、登録、その他のイベントを表すことができます。

![タグのプロパティのログインルール。](../../../images/extensions/server/mixpanel/tag-rule-complete.png)

の下 **[!UICONTROL イベント]**&#x200B;に設定し、識別イベントをトリガーする条件（Web サイト固有）を追加します。 ユーザーのクリックでログインルールをトリガーする例を次に示します。

![タグプロパティのログインルールのイベント設定。](../../../images/extensions/server/mixpanel/tag-rule-event-config.png)

「**[!UICONTROL 変更を保持]**」を選択して、ルールにイベントを追加します。

次の、以下 **[!UICONTROL アクション]**&#x200B;を追加し、ルールが起動したときに実行するアクションを追加します。 これらのアクションの中で、 **[!UICONTROL イベントを送信]** は Platform Web SDK 拡張機能によって提供され、Edge ネットワークにイベントを送信します。Edge ネットワークでは、次のようなイベント転送拡張機能でイベントを取得できます。 [!DNL Mixpanel].

アクションを設定する場合は、 **[!UICONTROL XDM データ]** を選択します。 [前に作成したデータ要素](#add-data-elements-within-tag-properties) を含む `distinct_id` の値です。

![タグプロパティのログインルールのアクション設定。](../../../images/extensions/server/mixpanel/tag-rule-action-config.png)

選択 **[!UICONTROL 変更を保持]** イベントをルールに追加するには、「 **[!UICONTROL 保存]** をクリックして、タグライブラリにルールを追加します。 ここから、次の操作が可能です。 [新しいビルドを作成し、Web サイトにデプロイします。](../../../ui/publishing/overview.md).

## のインストールと設定 [!DNL Mixpanel] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。内 **[!UICONTROL カタログ]** タブ、選択 **[!UICONTROL インストール]** ～のためのカードで [!DNL Mixpanel] 拡張子。

![のインストール [!DNL Mixpanel] 拡張子。](../../../images/extensions/server/mixpanel/install-extension.png)

## イベント転送データ要素の設定

拡張機能のインストール後、次の手順では、に送信される必要なデータ構成をキャプチャするイベント転送データ要素を作成します。 [!DNL Mixpanel].

### の作成 `distinctId` データ要素

イベント転送の下にデータ要素を追加します。 サイトが [[!DNL Mixpanel] SDK](https://developer.mixpanel.com/docs/nodejs) の [タグプロパティデータ要素](#setup-tag-properties-data-element) が定義されたとします。 イベント転送データ要素に対して、 **[!UICONTROL パス]** 代わりに、

![イベント転送にデータ要素を追加します。](../../../images/extensions/server/mixpanel/distinctId-ef-data-element.png)

### の作成 `event_type` データ要素

イベントタイプ用に定義されたデータ要素の例を以下に示します。

![イベント転送のイベントタイプ。](../../../images/extensions/server/mixpanel/eventname-ef-data-element.png)

### 追加のデータ要素マッピングの作成

この `distinctId` および `event_type` データ要素は、どちらもにデータを送信するために必要です。 [!DNL Mixpanel]を使用する場合もありますが、既知のユーザー ID とカスタムデータオブジェクトを、使用可能な場合は各イベントと共に含めることをお勧めします。 詳しくは、 [[!DNL Mixpanel Track Events] REST API](https://developer.mixpanel.com/reference/track-event) を参照してください。

推奨されるデータ要素のマッピングの概要を以下に示します。

>[!IMPORTANT]
>
>以下に示すすべてのデータ要素で、 **[!UICONTROL パス]** タイプを入力し、 **スキーマパス** 列。
>
>スキーマパスの場合、 `{TENANT_ID}` 固有の [テナント ID](../../../../xdm/api/getting-started.md#know-your-tenant_id)：組織で定義されたカスタムフィールドの名前空間として機能します。

| [!DNL Mixpanel] キー | スキーマパス | 説明 | 必須 |
| --- | --- | --- | --- |
| [!DNL Mixpanel Distinct ID] | `arc.event.xdm._{TENANT_ID}.distinct_id` | `distinct_id` イベントを実行したユーザーを識別します。 `distinct_id` は、次のために非常に重要なので、すべてのイベントで指定する必要があります。 [!DNL Mixpanel] ユニークユーザ、ファネル、リテンション、コホートなどを含む行動分析を正確かつ効率的に行う。 | ○ |
| [!DNL Event Type] | `arc.event.xdm._{TENANT_ID}.event_type` | イベントの名前です。 [!DNL Mixpanel] では、一意のイベント名の数を比較的少なく保ち、イベントに関連付けられた任意の変数コンテキストのプロパティを使用することをお勧めします。<br><br>例えば、「有料サインアップ」や「無料サインアップ」などの名前でイベントを追跡する代わりに、「Signup」と呼ばれるイベントを追跡し、「Account Type」というプロパティの潜在的な値が「paid」と「free」になるようにすることをお勧めします。 | ○ |
| [!DNL Known User ID] | `arc.event.xdm._{TENANT_ID}.LoginID` | ユーザーの電子メールまたはログイン ID（使用可能な場合）。 | いいえ |
| [!DNL Data] | `arc.event.xdm._{TENANT_ID}.properties` | イベントに関するすべてのプロパティを表す JSON オブジェクト。 データは 255 文字に切り捨てられます。 | いいえ |

{style="table-layout:auto"}

## イベント転送ルールの設定

すべてのデータ要素を設定したら、イベントの送信先と送信方法を決定するイベント転送ルールの作成を開始できます [!DNL Mixpanel]. ただし、ルールを設定する前に、ID クラスターがでどのように機能するかを理解しておくことが重要です [!DNL Mixpanel] 送信したイベントは、個々のユーザーに正しく関連付けられます。

### での ID クラスターについて [!DNL Mixpanel]

In [!DNL Mixpanel]の場合、id クラスターには `distinct_id` 個々のユーザーに接続する値。 [!DNL Mixpanel] は、各ユーザーの ID のクラスタリングを処理し、1 つの標準の `distinct_id` レポートで使用する各クラスターから。 また、独自の識別子（ローカルと呼ばれる）を含めることもできます `distinct_id`) を使用します。

[!DNL Mixpanel] 次の 2 つの方法で id クラスターを解決します。

* **特定** : [!DNL Mixpanel] 選択した識別子を匿名に接続します `distinct_id`. この [!DNL Mixpanel] SDK が Web サイト上で設定されている場合、Platform は `distinct_id` 現在サインインしているユーザーに割り当てられました。
* **エイリアス**: [!DNL Mixpanel] は、2 つの非匿名のを結合します。 `distinct_id`が結合されます。

>[!NOTE]
>
>詳しくは、 [!DNL Mixpanel] 文書 [id 管理](https://help.mixpanel.com/hc/en-us/articles/360041039771-Getting-Started-with-Identity-Management#user-identification) を参照してください。
>
>を有効にしていることを確認します。 [[!DNL Mixpanel] ID 結合機能](#prerequisites-mixpanel) id クラスタが適切に解決されるようにします。

したがって、 [!DNL Mixpanel] イベント転送拡張機能は、をサポートしています。 **[!UICONTROL イベントの追跡]** ルール設定のアクションタイプ。

>[!IMPORTANT]
>
>各ルールでは、使用する ID クラスターの解決方法に関係なく、いずれかのアクションで **[!UICONTROL イベントの追跡]** タイプ。 このアクションタイプがない場合、ルールは Adobe Experience Edge ネットワークイベントをに送信しません。 [!DNL Mixpanel].

### イベント追跡ルールの作成

イベント転送プロパティで新しいルールの作成を開始します。 の下 **[!UICONTROL アクション]**、新しいアクションを追加し、拡張機能をに設定します。 **[!UICONTROL Mixpanel]**. 次に、アクションタイプをに設定します。 **[!UICONTROL イベントの追跡]** Adobe Experience Edge ネットワークイベントの送信先 [!DNL Mixpanel].

| 必要情報 | 説明 |
| --- | --- |
| [!UICONTROL プロジェクトトークン] | このフィールドは、 [!DNL Mixpanel] アカウント |
| [!UICONTROL イベントタイプ] | イベント名。 |
| [!UICONTROL イベント時刻] | イベント時間。  |
| [!UICONTROL Mixpanel Distinct ID] | このフィールドは、 `distinctId` 作成済みのデータ要素。 |
| [!UICONTROL ID を挿入] | このフィールドは、 `insertId` データ要素。 |
| [!UICONTROL イベントのプロパティ] | 生の JSON を提供するか、シンプル化されたキーと値の入力セットを使用するかを選択します。 |

>[!NOTE]
>
>の標準フィールドの詳細 [!DNL Mixpanel] イベント ( [公式ドキュメント](https://developer.mixpanel.com/reference/import-events#event).

![イベント転送ルールのアクション設定を追加します。](../../../images/extensions/server/mixpanel/track-event-action.png)

一度 [!UICONTROL イベントの追跡] アクションをルールに追加する場合は、特定のイベントに対してのみ実行するようにルールの条件を設定できます。また、「条件」セクションを空のままにして、すべてのイベントに対してルールを実行することもできます。

>[!IMPORTANT]
>
>Web サイトが [!DNL Mixpanel] SDK を使用する場合は、次の手順 ( [内のデータの検証 [!DNL Mixpanel]](#validate). を使用しない場合、 [!DNL Mixpanel] SDK の場合は、 [個別の id トラッキングルールの作成](#create-an-identity-tracking-rule) 適切なイベントと `distinct_id` の値が [!DNL Mixpanel] ユーザー識別イベントが発生したとき。

### ID トラッキングルールの作成

を使用しない場合、 [!DNL Mixpanel SDK]を使用する場合、次に別のルールを作成します。 このルールにより、Web サイトでユーザー ID イベント（ログイン、サインアップ、登録など）が発生した場合に常に、適切なイベントと `distinct_id` の値が [!DNL Mixpanel].

新しいルールを作成するプロセスを開始します。 の [!UICONTROL 条件] 「 」セクションで、イベントがユーザー識別イベントであるかどうかを確認する条件を追加します。 次の例では、条件に [!UICONTROL 値の比較] ( [!UICONTROL コア] 拡張 ) を使用して、受信イベントの名前が `signin`：ユーザーのログインイベントを示します。

![次のアクション設定を表示： [!DNL Mixpanel] アクションタイプ Alias および Identify。](../../../images/extensions/server/mixpanel/ef-rule-condition.png)

ルールに適切な条件を追加したら、 [!UICONTROL イベントの送信] Adobe Experience Edge ネットワークイベントの送信先 [!DNL Mixpanel].

![イベント転送ルールのアクション設定を追加します。](../../../images/extensions/server/mixpanel/track-event-action.png)

>[!NOTE]
>
>ID の詳細については、 [!DNL Mixpanel]（を参照） [公式ドキュメント](https://developer.mixpanel.com/reference/create-identity).

アクションをルールに追加したら、「 」を選択します。 **[!UICONTROL 保存]** をクリックして、イベント転送ライブラリにルールを追加します。 ここから、次の操作が可能です。 [新しいビルドを作成し、変更をアクティブ化する](../../../ui/publishing/overview.md).

![のイベント転送ルールを追加 [!DNL Mixpanel] アクションタイプ Alias および Identify。](../../../images/extensions/server/mixpanel/ef-rule-complete.png)

## 内のデータの検証 [!DNL Mixpanel] {#validate}

実装が成功し、イベントが収集された場合、 [[!DNL Mixpanel] コンソール](https://help.mixpanel.com/hc/en-us/articles/4402837164948).

次の場合に確認 [!DNL Mixpanel] は、電子メールの値と、 **[!UICONTROL イベントの送信]**. 正しく実装されている場合、 [!DNL Mixpanel] 彼らを単一の [ユーザープロファイル](https://help.mixpanel.com/hc/en-us/articles/115004501966).

## 次の手順

このガイドでは、コンバージョンイベントをに送信する方法について説明しました。 [!DNL Mixpanel] イベント転送を使用しています。 このイベント転送拡張機能では、 [!DNL Mixpanel] SDK および JavaScript API これらの基盤となるテクノロジーについて詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Mixpanel] SDK](https://developer.mixpanel.com/docs/nodejs)
* [[!DNL Mixpanel] JavaScript API](https://developer.mixpanel.com/docs/javascript-full-api-reference#mixpanelidentify)

Experience Platformのイベント転送機能について詳しくは、 [イベント転送の概要](../../../ui/event-forwarding/overview.md).

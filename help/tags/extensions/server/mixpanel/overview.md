---
keywords: イベント転送拡張機能；mixpanel;mixpanel イベント転送拡張機能
title: Mixpanel Track Events API イベント転送拡張機能
description: このAdobe Experience Platformイベント転送拡張機能は、Adobe Experience Edge ネットワークイベントを Mixpanel に送信します。
source-git-commit: 8538e3a2899c3e2451519996cabeffc4b42d706c
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 2%

---

# [!DNL Mixpanel Track Events] API イベント転送拡張機能

[[!DNL Mixpanel]](https://www.mixpanel.com) は、ユーザーがデジタル製品とどのようにやり取りするかに関するデータをキャプチャできる、製品分析ツールです。 数回のクリックでデータをクエリし視覚化できる、シンプルでインタラクティブなレポートを使用して製品データを分析できます。 [!DNL Mixpanel] 誰もがリアルタイムでユーザーデータを分析し、傾向を特定し、ユーザー行動を把握し、製品に関する意思決定をおこなうことで、チームをより効率的にします。

[!DNL Mixpanel] は、各インタラクションを 1 人のユーザーに接続する、イベントベースのユーザー中心モデルを採用しています。 この [!DNL Mixpanel] データモデルは、ユーザー、イベント、プロパティの概念に基づいて構築されます。

>[!NOTE]
>
>詳しくは、 [!DNL Mixpanel] ドキュメント [id 管理](https://help.mixpanel.com/hc/en-us/articles/360041039771-Getting-Started-with-Identity-Management) どうやって理解するか [!DNL Mixpanel] イベントを結合して id クラスターを作成します。 また、 [ユニーク ID](https://help.mixpanel.com/hc/en-us/articles/115004509426-Distinct-ID-Creation-JavaScript-iOS-Android-) を参照して、イベントデータでユーザーを識別するためにユーザーがどのように使用されているかを理解します。

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

### での ID クラスターについて [!DNL Mixpanel]

In [!DNL Mixpanel]の場合、id クラスターには `distinct_id` 個々のユーザーに接続する値。 [!DNL Mixpanel] は、各ユーザーの ID のクラスターを処理し、1 つの標準の `distinct_id` レポートで使用する各クラスターから。 また、独自の識別子（ローカルと呼ばれる）を含めることもできます `distinct_id`) を使用します。

[!DNL Mixpanel] 次の 2 つの方法で id クラスターを解決します。

* **特定** : [!DNL Mixpanel] 選択した識別子を匿名に接続します `distinct_id`. Web サイトが [!DNL Mixpanel] SDK が有効な場合、Platform は `distinct_id` 現在ログインしているユーザーに割り当てられました。
* **エイリアス**: [!DNL Mixpanel] 2 つの非匿名を組み合わせる `distinct id`を組み合わせます。

>[!NOTE]
>
>詳しくは、 [!DNL Mixpanel] 文書 [id 管理](https://help.mixpanel.com/hc/en-us/articles/360041039771-Getting-Started-with-Identity-Management#user-identification) を参照してください。
>
>を有効にしていることを確認します。 [[!DNL Mixpanel] ID 結合機能](#prerequisites-mixpanel) id クラスタが適切に解決されるようにします。

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Mixpanel] 次の入力が必要です。

| キータイプ | 説明 | 例 |
| --- | --- | --- |
| プロジェクトトークン | に関連付けられたプロジェクトトークン [!DNL Mixpanel] アカウント 詳しくは、 [!DNL Mixpanel] ドキュメント [プロジェクトトークンの検索](https://help.mixpanel.com/hc/en-us/articles/115004502806-Find-Project-Token-) 指導のために | `25470xxxxxxxxxxxxxxxxxxx1289` |

## のインストールと設定 [!DNL Mixpanel] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。内 **[!UICONTROL カタログ]** タブ、選択 **[!UICONTROL インストール]** ～のためのカードで [!DNL Mixpanel] 拡張子。

![のインストール [!DNL Mixpanel] 拡張子。](../../../images/extensions/server/mixpanel/install-extension.png)

## の作成 [!DNL Send Event] ルール

イベント転送プロパティで新しいルールの作成を開始します。 の下 **[!UICONTROL アクション]**、新しいアクションを追加し、拡張機能をに設定します。 **[!UICONTROL Mixpanel]**. 次に、アクションタイプをに設定します。 **[!UICONTROL イベントの追跡]** Adobe Experience Edge ネットワークイベントの送信先 [!DNL Mixpanel].

| 必要情報 | 説明 | 必須 |
| --- | --- | --- |
| [!UICONTROL プロジェクトトークン] | このフィールドは、 [!DNL Mixpanel] アカウント | ○ |
| [!UICONTROL イベントタイプ] | イベント名。 | ○ |
| [!UICONTROL イベント時刻] | イベント時間。  |  |
| [!UICONTROL Mixpanel Distinct ID] | イベントを実行したユーザーの一意の識別子。 |  |
| [!UICONTROL ID を挿入] | 重複排除に使用される、イベントの一意の識別子。 |  |
| [!UICONTROL イベントのプロパティ] | イベントのカスタムプロパティを含む JSON オブジェクト。 生の JSON を提供するか、シンプル化されたキーと値の入力セットを使用するかを選択します。 |  |

>[!NOTE]
>
>の標準フィールドの詳細 [!DNL Mixpanel] イベント ( [公式ドキュメント](https://developer.mixpanel.com/reference/import-events#event).

![イベント転送ルールのアクション設定を追加します。](../../../images/extensions/server/mixpanel/track-event-action.png)

一度 [!UICONTROL イベントの追跡] アクションをルールに追加する場合は、特定のイベントに対してのみ実行するようにルールの条件を設定できます。また、「条件」セクションを空のままにして、すべてのイベントに対してルールを実行することもできます。

>[!IMPORTANT]
>
>Web サイトが [!DNL Mixpanel] SDK を使用する場合は、次の手順 ( [内のデータの検証 [!DNL Mixpanel]](#validate). を使用しない場合、 [!DNL Mixpanel] SDK の場合は、 [個別の id トラッキングルールの作成](#create-an-identity-tracking-rule) 適切なイベントと `distinct_id` の値が [!DNL Mixpanel] ユーザー識別イベントが発生したとき。

## 内のデータの検証 [!DNL Mixpanel] {#validate}

実装が成功し、イベントが収集された場合、 [[!DNL Mixpanel] コンソール](https://help.mixpanel.com/hc/en-us/articles/4402837164948).

次の場合に確認 [!DNL Mixpanel] は、電子メールの値と、 **[!UICONTROL イベントの送信]**. 正しく実装されている場合、 [!DNL Mixpanel] 彼らを単一の [ユーザープロファイル](https://help.mixpanel.com/hc/en-us/articles/115004501966).

## 次の手順

このガイドでは、コンバージョンイベントをに送信する方法について説明しました。 [!DNL Mixpanel] イベント転送を使用しています。 このイベント転送拡張機能では、 [!DNL Mixpanel] SDK および JavaScript API これらの基盤となるテクノロジーについて詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Mixpanel] SDK](https://developer.mixpanel.com/docs/nodejs)
* [[!DNL Mixpanel] JavaScript API](https://developer.mixpanel.com/docs/javascript-full-api-reference#mixpanelidentify)

Experience Platformのイベント転送機能について詳しくは、 [イベント転送の概要](../../../ui/event-forwarding/overview.md).

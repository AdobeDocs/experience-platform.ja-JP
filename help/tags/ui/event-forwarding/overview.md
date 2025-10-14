---
title: イベント転送の概要
description: タグの実装を変更せずに Experience Platform Edge Network を使用してタスクを実行できる、Adobe Experience Platform のイベント転送について説明します。
feature: Event Forwarding
exl-id: 18e76b9c-4fdd-4eff-a515-a681bc78d37b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1190'
ht-degree: 54%

---

# イベント転送の概要

>[!NOTE]
>
>イベント転送は、Adobe Real-time Customer Data Platform Connections、Prime または Ultimate の一部として提供される有料機能です。

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

Adobe Experience Platform のイベント転送を使用すると、収集したイベントデータをサーバーサイドで処理するために宛先に送信できます。イベント転送は、通常クライアントで行なわれるタスクを、Adobe Experience Platform Edge ネットワークを使用して実行することにより、web ページやアプリの負担を軽減します。タグと同様の方法で実装されたイベント転送ルールは、データを変換して新しい宛先に送信できますが、このデータを web ブラウザーなどのクライアントアプリケーションから送信する代わりに、アドビのサーバーから送信します。

このドキュメントでは、Experience Platformのイベント転送の概要を説明します。

![&#x200B; データ収集エコシステムでのイベント転送 &#x200B;](../../../collection/images/home/event-forwarding.png)

>[!NOTE]
>
>イベント転送がExperience Platformのデータ収集エコシステム内でどのように適合するかについての詳細は、[&#x200B; データ収集の概要 &#x200B;](../../../collection/home.md) を参照してください。

イベント転送を Adobe Experience Platform [Web SDK](/help/web-sdk/home.md) および [Mobile SDK](https://experienceleague.adobe.com/docs/platform-learn/data-collection/mobile-sdk/overview.html?lang=ja) と組み合わせると、次のような利点があります。

**パフォーマンス**：

* データのペイロードを含むページから 1 回の呼び出しを行い、このデータをサーバーサイドで統合して、クライアントサイドのネットワークトラフィックを減らし、より高速なエクスペリエンスを顧客に提供します。
* Web ページの読み込みにかかる時間を短縮し、サイトのパフォーマンスを向上させます。
* 多くの宛先へのエクスペリエンスの配信やデータの送信に必要なクライアントサイドテクノロジーの数を減らします。

**データガバナンス**：

* 透明性を高め、すべてのプロパティを通して、どのテータをどこに送信するかを制御します。

## イベント転送とタグの違い {#differences-from-tags}

設定に関しては、イベント転送は[ルール](../managing-resources/rules.md)、[データ要素](../managing-resources/data-elements.md)、[拡張](../managing-resources/extensions/overview.md)など、タグと同じ概念の多くを使用します。この 2 つの主な違いを要約すると、次のようになります。

* タグは、イベントデータを web サイトまたはネイティブモバイルアプリケーションから **収集** し、Experience Platform Edge Networkに送信します。
* イベント転送 **Experience Platform Edge Networkから受信したイベントデータを** 最終的な宛先を表すエンドポイントまたは元のペイロードを強化するデータを提供するエンドポイントに送信）します。

タグは、Experience Platform Web および Mobile SDK を使用して、サイトまたはネイティブモバイルアプリケーションから直接イベントデータを収集しますが、イベント転送でイベントデータを宛先に転送するには、Experience Platform Edge Network経由でイベントデータが既に送信されている必要があります。 つまり、イベント転送を使用するには、デジタルプロパティにExperience Platform web またはモバイル SDKを（タグを通じてまたは生のコードを使用して）実装する必要があります。

### プロパティ {#properties}

イベント転送は、タグとは別に独自のプロパティのストアを保持しており、左側のナビゲーションから **[!UICONTROL イベント転送]** を選択して、Experience Platform UI またはデータ収集 UI で表示できます。

>[!TIP]
>
>右側のパネルの製品ヘルプのを使用して、イベント転送の詳細を確認し、その他の使用可能なリソースを表示します。

![&#x200B; データ収集 UI のイベント転送プロパティ。](../../images/ui/event-forwarding/overview/properties.png)

すべてのイベント転送プロパティには、プラットフォームとして **[!UICONTROL Edge]** がリストされています。Web とモバイルは区別されません。Experience Platform Edge Networkから受信したデータのみを処理し、それ自体が web とモバイルの両方のプラットフォームからイベントデータを受信できるからです。

### 拡張機能 {#extensions}

イベント転送には、[Core](../../extensions/server/core/overview.md) 拡張機能や [Adobe Cloud Connector](../../extensions/server/cloud-connector/overview.md) 拡張機能など、互換性のある拡張機能の独自のカタログがあります。左側のナビゲーションで「**[!UICONTROL 拡張機能]**」、「**[!UICONTROL カタログ]**」の順に選択すると、UI にイベント転送プロパティで使用可能な拡張機能を表示できます。

右側のパネルから ![&#x200B; 説明 &#x200B;](../../images/ui/event-forwarding/overview/about.png) を選択すると、この機能の詳細を確認できる追加のリソースを表示できます。

![&#x200B; データ収集 UI のイベント転送拡張機能。](../../images/ui/event-forwarding/overview/extensions.png)

### データ要素 {#data-elements}

イベント転送で利用できるデータ要素のタイプは、それらを提供する互換性のある[拡張機能](#extensions)のカタログに限定されます。

イベント転送では、データ要素自体はタグと同じように作成および設定されますが、Experience Platform Edge Networkのデータを参照する方法については、いくつかの重要な構文上の違いがあります。

#### Experience Platform Edge Networkからのデータの参照 {#data-element-path}

Experience Platform Edge Networkからデータを参照するには、そのデータへの有効なパスを提供するデータ要素を作成する必要があります。 UI でデータ要素を作成するときは、拡張機能として&#x200B;**[!UICONTROL コア]**&#x200B;を選択し、タイプとして&#x200B;**[!UICONTROL パス]**&#x200B;を選択します。

データ要素の&#x200B;**[!UICONTROL パス]**&#x200B;値は、パターン `arc.event.{ELEMENT}` に従う必要があります（例：`arc.event.xdm.web.webPageDetails.URL`）。データを送信するには、このパスを正しく指定する必要があります。

右側のパネルから ![&#x200B; 説明 &#x200B;](../../images/ui/event-forwarding/overview/about.png) を選択すると、この機能の詳細を確認できる追加のリソースを表示できます。

![&#x200B; イベント転送のパスタイプデータ要素の例。](../../images/ui/event-forwarding/overview/data-reference.png)

### ルール {#rules}

イベント転送プロパティでのルールの作成はタグと同様に機能しますが、重要な違いは、イベントをルールコンポーネントとして選択できないことです。代わりに、イベント転送ルールは[データストリーム](../../../datastreams/overview.md)から受け取ったすべてのイベントを処理し、特定の条件を満たした場合にそれらのイベントを宛先に転送します。

さらに、イベント転送プロパティ内のすべてのルール（およびすべてのアクション）で処理されるので、1 つのイベントに適用される 30 秒のタイムアウトがあります。 つまり、単一のイベントのすべてのルールとすべてのアクションは、この期間内に完了する必要があります。

右側のパネルから ![&#x200B; 説明 &#x200B;](../../images/ui/event-forwarding/overview/about.png) を選択すると、この機能の詳細を確認できる追加のリソースを表示できます。

![&#x200B; データ収集 UI のイベント転送ルール &#x200B;](../../images/ui/event-forwarding/overview/rules.png)

#### データ要素のトークン化 {#tokenization}

タグルールでは、データ要素名の先頭と末尾に `%` を付けてデータ要素をトークン化します（例：`%viewportHeight%`）。イベント転送ルールでは、データ要素は代わりにデータ要素名の先頭に `{{`、末尾に `}}` を付けてトークン化します（例：`{{viewportHeight}}`）。

右側のパネルから ![&#x200B; 説明 &#x200B;](../../images/ui/event-forwarding/overview/about.png) を選択すると、この機能の詳細を確認できる追加のリソースを表示できます。

![&#x200B; イベント転送のパスタイプデータ要素の例。](../../images/ui/event-forwarding/overview/tokenization.png)

#### ルールアクションのシーケンス {#action-sequencing}

イベント転送ルールの[!UICONTROL アクション]セクションは常に順番に実行されます。例えば、ルールに 2 つのアクションがある場合、2 番目のアクションは、前のアクションが完了するまで（およびエンドポイントから応答が期待される場合は、そのエンドポイントが応答するまで）実行を開始しません。 ルールを保存する際に、アクションの順序が正しいことを確認します。この実行シーケンスは、タグルールの場合のように非同期で実行することはできません。

## 秘密鍵 {#secrets}

イベント転送を使用すると、データの送信先のサーバーへの認証に使用できる秘密鍵を作成、管理、保存できます。 利用可能な秘密鍵のタイプと UI での実装方法については、[秘密鍵](./secrets.md)のガイドを参照してください。

## ビデオの概要 {#video}

次のビデオは、イベント転送とReal-Time CDP接続をより深く理解できるようにすることを目的としています。

>[!VIDEO](https://video.tv.adobe.com/v/3429308)

## 次の手順

このドキュメントでは、イベント転送の概要を説明しました。組織用にこの機能を設定する方法について詳しくは、[はじめる前に](./getting-started.md)を参照してください。

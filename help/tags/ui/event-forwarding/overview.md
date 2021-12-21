---
title: イベント転送の概要
description: Platform Edge ネットワークを使用して、タグの実装を変更せずにタスクを実行できる、Adobe Experience Platform のイベント転送について説明します。
feature: Event Forwarding
exl-id: 18e76b9c-4fdd-4eff-a515-a681bc78d37b
source-git-commit: 82ce288d55e57f05910fd8290c38f44b1846f48e
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 12%

---

# イベント転送の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

Adobe Experience Platformでのイベント転送を使用すると、収集したイベントデータをサーバー側で処理するために宛先に送信できます。 イベント転送は、通常クライアントでおこなわれるタスクをAdobe Experience Platform Edge Network を使用して実行することで、Web ページやアプリの重みを軽減します。 イベント転送ルールはタグと同様に実装され、データを変換して新しい宛先に送信できますが、Web ブラウザーなどのクライアントアプリケーションからデータを送信する代わりに、Adobeのサーバーから送信されます。

このドキュメントでは、Platform でのイベント転送の概要を説明します。

![データ収集エコシステムでのイベント転送](../../../collection/images/home/event-forwarding.png)

>[!NOTE]
>
>Platform のデータ収集エコシステム内でのイベント転送の適合について詳しくは、 [データ収集の概要](../../../collection/home.md).

Adobe Experience Platformと組み合わされたイベント転送 [Web SDK](../../../edge/home.md) および [モバイル SDK](https://aep-sdks.gitbook.io/docs/) には次のような利点があります。

**パフォーマンス**:

* データのペイロードを含むページから 1 回の呼び出しをおこない、サーバー側で統合してクライアント側のネットワークトラフィックを減らし、顧客に対するより高速なエクスペリエンスを提供します。
* サイトのパフォーマンスを向上させるために、Web ページの読み込みに要する時間を短縮します。
* エクスペリエンスを配信し、多くの宛先にデータを送信するために必要なクライアント側テクノロジーの数を減らします。

**データガバナンス**:

* 透明性を高め、どのデータをどこに送信するかをすべてのプロパティにわたって制御します。

## イベント転送とタグの違い

設定に関して、イベント転送ではタグと同じ概念の多くを使用します。例えば、 [ルール](../managing-resources/rules.md), [データ要素](../managing-resources/data-elements.md)、および [拡張機能](../managing-resources/extensions/overview.md). この 2 つの主な違いは、次のように要約できます。

* タグ **収集** イベントデータを Web サイトまたはネイティブモバイルアプリケーションから取得し、Platform Edge Network に送信します。
* イベント転送 **送信** Platform Edge Network から、最終的な宛先を表すエンドポイント、または元のペイロードをエンリッチメントするデータを提供するエンドポイントに、イベントデータを受信します。

タグは Platform Web および Mobile SDK を使用してサイトまたはネイティブモバイルアプリケーションから直接イベントデータを収集しますが、イベント転送は、イベントデータを宛先に転送するために、Platform Edge Network を介して既に送信されている必要があります。 つまり、イベント転送を使用するには、デジタルプロパティに Platform Web または Mobile SDK を（タグを通じてまたは生のコードを使用して）実装する必要があります。

### プロパティ

イベント転送は、タグとは別に独自のプロパティストアを維持します。このプロパティは、データ収集 UI で「 **[!UICONTROL イベント転送]** をクリックします。

![データ収集 UI のイベント転送プロパティ](../../images/ui/event-forwarding/overview/properties.png)

すべてのイベント転送プロパティリスト **[!UICONTROL Edge]** プラットフォームとして。 Web とモバイルは区別されません。Platform Edge Network から受信したデータは、Web とモバイルの両方のプラットフォームからイベントデータを受け取ることができる Platform Edge Network から受信したデータのみを処理するからです。

### 拡張機能 {#extensions}

イベント転送には、互換性のある拡張機能の独自のカタログがあります ( 例： [コア](../../extensions/web/core/event-forwarding.md) 拡張と [Adobeクラウドコネクタ](../../extensions/web/cloud-connector/overview.md) 拡張子。 UI で、イベント転送プロパティに使用可能な拡張機能を表示するには、次を選択します。 **[!UICONTROL 拡張機能]** 左のナビゲーションで、その後に **[!UICONTROL カタログ]**.

![データ収集 UI のイベント転送拡張機能](../../images/ui/event-forwarding/overview/extensions.png)

### データ要素

イベント転送で使用できるデータ要素のタイプは、互換性のあるのカタログに制限されています [拡張機能](#extensions) それが彼らを提供する

データ要素自体は、タグの場合と同様にイベント転送で作成および設定されますが、Platform Edge Network からのデータの参照方法に関しては、いくつかの重要な構文上の違いがあります。

#### Platform Edge Network からのデータの参照

Platform Edge Network からデータを参照するには、そのデータへの有効なパスを提供するデータ要素を作成する必要があります。 UI でデータ要素を作成する場合は、「 **[!UICONTROL コア]** 拡張機能と **[!UICONTROL パス]** の値を指定します。

この **[!UICONTROL パス]** データ要素の値は、パターンに従う必要があります `arc.event.{ELEMENT}` ( 例： `arc.event.xdm.web.webPageDetails.URL`) をクリックします。 データを送信するには、このパスを正しく指定する必要があります。

![イベント転送のパスタイプデータ要素の例](../../images/ui/event-forwarding/overview/data-reference.png)

### ルール

イベント転送プロパティでのルールの作成は、タグと同様の方法で動作します。重要な違いは、イベントをルールコンポーネントとして選択できない点です。 代わりに、イベント転送ルールは、 [datastream](../../../edge/fundamentals/datastreams.md) およびは、特定の条件が満たされた場合、これらのイベントを宛先に転送します。

![データ収集 UI のイベント転送ルール](../../images/ui/event-forwarding/overview/rules.png)

#### データ要素のトークン化

タグルールでは、データ要素は `%` データ要素名の先頭と末尾（例： ） `%viewportHeight%`) をクリックします。 イベント転送ルールでは、データ要素は、代わりに `{{` 最初に `}}` データ要素名の末尾に配置する ( 例： `{{viewportHeight}}`) をクリックします。

![イベント転送のパスタイプデータ要素の例](../../images/ui/event-forwarding/overview/tokenization.png)

#### 一連のルールアクション

この [!UICONTROL アクション] イベント転送ルールの「 」セクションは、常に順番に実行されます。 ルールを保存する際に、アクションの順序が正しいことを確認します。この実行シーケンスは、タグを使用できるのと同様に、非同期で実行することはできません。

## 秘密鍵

イベント転送を使用すると、データの送信先のサーバーへの認証に使用できる秘密鍵を作成、管理、保存できます。 詳しくは、 [秘密](./secrets.md) 様々な種類の使用可能な秘密鍵のタイプと、UI での実装方法について説明します。

## 次の手順

このドキュメントでは、イベント転送の概要を説明しました。 この機能を組織に対して設定する方法の詳細については、 [入門ガイド](./getting-started.md).

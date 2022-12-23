---
keywords: Experience Platform;ホーム;人気のトピック;エッジセグメント化;セグメント化;セグメント化サービス;UI ガイド;ストリーミングエッジ;
solution: Experience Platform
title: エッジセグメント化 UI ガイド
topic-legacy: ui guide
description: エッジセグメント化は、Platform 内のセグメントをエッジ上で即座に評価する機能で、これにより、同じページや次のページのパーソナライゼーションのユースケースが可能になります。
exl-id: eae948e6-741c-45ce-8e40-73d10d5a88f1
source-git-commit: 8c7c1273feb2033bf338f7669a9b30d9459509f7
workflow-type: tm+mt
source-wordcount: '939'
ht-degree: 94%

---

# エッジセグメント化 UI ガイド

>[!NOTE]
>
>エッジセグメント化は、すべての Platform ユーザーが一般に使用できるようになりました。ベータ版でエッジセグメントを作成した場合、これらのセグメントは引き続き動作します。

エッジセグメント化は、Adobe Experience Platform 内のセグメントを[エッジ上で](../../edge/home.md)即座に評価する機能で、これにより、同じページや次のページのパーソナライゼーションのユースケースが可能になります。

>[!IMPORTANT]
>
> エッジデータは、収集された場所に最も近いエッジサーバーの場所に保存されるので、ハブ（またはプリンシパル）Adobe Experience Platform データセンターとして指定された場所以外に保存される可能性があります。
>
> さらに、エッジセグメント化エンジンは、プライマリとしてマークされた&#x200B;**単一**&#x200B;の ID（エッジベース以外のプライマリ ID と一致するもの）があるエッジ上のリクエストのみに従います。

## エッジセグメント化のクエリタイプ {#query-types}

現在、エッジセグメント化で評価できるクエリタイプは、選ばれたクエリタイプのみです。 次の節では、エッジセグメント化で評価できるクエリのタイプと、現在サポートされていないクエリのタイプのリストを示します。

次の表に示す条件のいずれかを満たす場合、クエリはエッジセグメント化で評価できます。

>[!NOTE]
>
>クエリが次の表のいずれかのクエリタイプと一致する場合、エッジセグメント化を使用して自動的に評価されます。クエリ式に基づいて、システムがこの機能を自動的に判断します。

| クエリタイプ | 詳細 | 例 | PQL の例 |
| ---------- | ------- | ------- | ----------- |
| 単一イベント | 時間制限のない 1 つの受信イベントを参照するセグメント定義。 | 買い物かごにアイテムを追加した人物。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart")])` |
| 単一プロファイル | 単一プロファイルのみの属性を参照するセグメント定義 | 米国在住の人物。 | `homeAddress.countryCode = "US"` |
| プロファイルを参照する単一のイベント | 1 つ以上のプロファイル属性と、時間制限のない単一の受信イベントを参照する任意のセグメント定義。 | ホームページを訪問した米国在住の人物。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView")])` |
| プロファイル属性を持つ単一イベントの否定 | 単一の受信イベントの否定と 1 つ以上のプロファイル属性を参照する任意のセグメント定義 | ホームページを訪問&#x200B;**したことがない**&#x200B;米国在住の人物。 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView")]))` |
| 時間枠内での単一のイベント | 設定された期間内の単一の受信イベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを訪問した人物。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 8 days before now)])` |
| 時間枠内でのプロファイル属性を持つ単一イベント | 1 つ以上のプロファイル属性と、設定された期間内の単一の受信イベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを訪問した米国在住の人物。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 8 days before now)])` |
| 時間枠内でのプロファイル属性を持つ単一イベントの否定 | 1 つ以上のプロファイル属性と、期間内の単一受信イベントの否定を参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを訪問&#x200B;**していない**&#x200B;米国在住の人物。 | `homeAddress.countryCode = "US" and not(chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 8 days before now)]))` |
| 24 時間の時間枠内での頻度イベント | 24 時間の時間枠内に特定の回数だけ発生するイベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを&#x200B;**少なくとも** 5 回訪問した人物。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間の時間枠内でのプロファイル属性を持つ頻度イベント | 1 つ以上のプロファイル属性と、24 時間の時間枠内に一定の回数だけ発生するイベントを参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを&#x200B;**少なくとも** 5 回訪問した米国在住の人物。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間の時間枠内でのプロファイルを持つ頻度イベントの否定 | 1 つ以上のプロファイル属性と、24 時間の時間枠内に特定の回数だけ発生するイベントの否定を参照する任意のセグメント定義。 | 過去 24 時間以内にホームページを 6 回&#x200B;**以上**&#x200B;訪問していない人物。 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] ))` |
| 24 時間のタイムプロファイル以内での複数の受信ヒット | 24 時間の時間枠内に発生する複数のイベントを参照する任意のセグメント定義。 | ホームページを訪問した人、**または**&#x200B;過去 24 時間以内にチェックアウトページを訪問した人。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| 24 時間の時間枠内にプロファイルを持つ複数のイベント | 1 つ以上のプロファイル属性と、24 時間以内に発生する複数のイベントを参照するセグメント定義。 | ホームページ&#x200B;**および**&#x200B;チェックアウトページにアクセスした、米国在住の人。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 | 米国在住で、「既存のセグメント」のセグメントに属しているユーザー。 | `homeAddress.countryCode = "US" and inSegment("existing segment")` |
| マップを参照するクエリ | プロパティのマップを参照するセグメント定義。 | 外部セグメントデータに基づいて買い物かごに追加したユーザー。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart") WHERE(externalSegmentMapProperty.values().exists(stringProperty="active"))])` |

セグメント定義は次のようになります。 **not** は、次のシナリオでエッジセグメント化に対して有効になっています。

- セグメント定義には、単一のイベントと `inSegment` イベント。
   - ただし、セグメントが `inSegment` イベントはプロファイルのみ、セグメント定義 **遺言** をエッジセグメント化に対して有効にする。

## 次の手順

このガイドでは、Adobe Experience Platform でエッジセグメント化を使用してセグメントを評価する方法について説明します。 Experience Platform ユーザーインターフェイスの使用方法について詳しくは、[セグメント化ユーザーガイド](./overview.md)を参照してください。 Experience Platform API を使用して同様のアクションを実行し、セグメントを操作する方法については、[エッジセグメント化 API ガイド](../api/edge-segmentation.md)を参照してください。

## 付録

次の節では、エッジセグメント化に関するよくある質問を一覧表示します。

### Edge Network 上でセグメントが使用可能になるまで、どれくらいかかりますか？

Edge Network 上でセグメントが使用可能になるまで、最大 1 時間かかります。
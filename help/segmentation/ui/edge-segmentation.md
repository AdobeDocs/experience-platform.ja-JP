---
keywords: Experience Platform；ホーム；人気の高いトピック；エッジのセグメント化；セグメント化；セグメント化サービス；セグメント化サービス；ui ガイド；ストリーミングエッジ；
solution: Experience Platform
title: エッジセグメント UI ガイド
topic-legacy: ui guide
description: エッジのセグメント化は、Platform 内のセグメントを即座にエッジ上で評価する機能で、同じページや次のページのパーソナライゼーションの使用例を可能にします。
exl-id: eae948e6-741c-45ce-8e40-73d10d5a88f1
source-git-commit: 8c7c1273feb2033bf338f7669a9b30d9459509f7
workflow-type: tm+mt
source-wordcount: '939'
ht-degree: 1%

---

# エッジセグメント化 UI ガイド

>[!NOTE]
>
>エッジセグメント化は、すべての Platform ユーザーが一般に使用できるようになりました。 ベータ版でエッジセグメントを作成した場合、これらのセグメントは引き続き動作します。

エッジのセグメント化は、Adobe Experience Platformでセグメントを瞬時に評価する機能です [端に](../../edge/home.md)を使用して、同じページおよび次のページのパーソナライゼーションの使用例を有効にします。

>[!IMPORTANT]
>
> エッジデータは、収集された場所に最も近いエッジサーバーの場所に保存され、ハブ（またはプリンシパル）Adobe Experience Platformデータセンターとして指定された場所以外の場所に保存される場合があります。
>
> さらに、エッジセグメントエンジンは、があるエッジでのリクエストにのみ従います **1 つ** エッジベース以外のプライマリ ID と一致する、プライマリマーク ID。

## エッジセグメント化のクエリタイプ {#query-types}

現在、エッジセグメント化で評価できるクエリタイプは、選択したクエリタイプのみです。 次の節では、エッジセグメント化で評価できるクエリの種類と、現在サポートされていないクエリの種類のリストを示します。

次の表に示す条件のいずれかを満たす場合、クエリはエッジセグメント化を使用して評価できます。

>[!NOTE]
>
>クエリが次の表のいずれかのクエリタイプと一致する場合、エッジセグメント化を使用して自動的に評価されます。 クエリ式に基づいて、システムがこの機能を自動的に決定します。

| クエリタイプ | 詳細 | 例 | PQL の例 |
| ---------- | ------- | ------- | ----------- |
| 単一イベント | 時間制限のない単一の受信イベントを参照するセグメント定義。 | 買い物かごに項目を追加した担当者。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart")])` |
| 単一のプロファイル | 単一のプロファイルのみの属性を参照するセグメント定義 | 米国に住む人々。 | `homeAddress.countryCode = "US"` |
| プロファイルを参照する単一イベント | 時間制限のない 1 つ以上のプロファイル属性と 1 つの受信イベントを参照するセグメント定義。 | ホームページを訪問した米国在住の人。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView")])` |
| プロファイル属性を持つ否定された単一イベント | 否定された単一の受信イベントと 1 つ以上のプロファイル属性を参照するセグメント定義 | 米国に住み、 **not** ホームページにアクセスしました。 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView")]))` |
| 1 つの時間枠内の単一イベント | 設定された期間内の単一の受信イベントを参照するセグメント定義。 | 過去 24 時間にホームページを訪問した人。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 8 days before now)])` |
| 時間枠内のプロファイル属性を持つ単一イベント | 1 つ以上のプロファイル属性と、設定された期間内の単一の受信イベントを参照するセグメント定義。 | 過去 24 時間にホームページを訪問した米国に住む人。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 8 days before now)])` |
| 時間枠内のプロファイル属性を持つ 1 つのイベントを無効化 | 1 つ以上のプロファイル属性と、一定期間内の無効な単一の受信イベントを参照するセグメント定義。 | 米国に住み、 **not** 過去 24 時間にホームページにアクセスした。 | `homeAddress.countryCode = "US" and not(chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 8 days before now)]))` |
| 24 時間以内の頻度イベント | 24 時間の期間内に特定の回数だけ発生したイベントを参照するセグメント定義。 | ホームページを訪問した人 **少なくとも** 過去 24 時間で 5 回 | `chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間の時間枠内にプロファイル属性を持つ頻度イベント | 1 つ以上のプロファイル属性と、24 時間の期間内に一定の回数だけ発生したイベントを参照するセグメント定義。 | ホームページを訪問した米国出身の人 **少なくとも** 過去 24 時間で 5 回 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] )` |
| 24 時間以内のプロファイルを含む無効な頻度イベント | 1 つ以上のプロファイル属性と、24 時間の期間内に特定の回数だけ発生する無効なイベントを参照するセグメント定義。 | ホームページを訪問していない人 **詳細** 過去 24 時間で 5 回以上 | `not(chain(xEvent, timestamp, [A: WHAT(eventType = "homePageView") WHEN(< 24 hours before now) COUNT(5) ] ))` |
| 24 時間以内に複数の受信ヒット | 24 時間以内に発生した複数のイベントを参照するセグメント定義。 | ホームページを訪問した人 **または** 過去 24 時間以内にチェックアウトページにアクセスしました。 | `chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| 24 時間の期間内にプロファイルを持つ複数のイベント | 24 時間以内に発生する 1 つ以上のプロファイル属性と複数のイベントを参照するセグメント定義。 | ホームページを訪問した米国出身の人 **および** 過去 24 時間以内にチェックアウトページにアクセスしました。 | `homeAddress.countryCode = "US" and chain(xEvent, timestamp, [X: WHAT(eventType = "homePageView") WHEN(< 24 hours before now)]) and chain(xEvent, timestamp, [X: WHAT(eventType = "checkoutPageView") WHEN(< 24 hours before now)])` |
| セグメントのセグメント | 1 つ以上のバッチセグメントまたはストリーミングセグメントを含むセグメント定義。 | 米国に住んでいて、セグメント「既存のセグメント」に属している人。 | `homeAddress.countryCode = "US" and inSegment("existing segment")` |
| マップを参照するクエリ | プロパティのマップを参照するセグメント定義。 | 外部セグメントデータに基づいて買い物かごに追加した人。 | `chain(xEvent, timestamp, [A: WHAT(eventType = "addToCart") WHERE(externalSegmentMapProperty.values().exists(stringProperty="active"))])` |

セグメント定義は次のようになります。 **not** は、次のシナリオでエッジセグメント化に対して有効になっています。

- セグメント定義には、単一のイベントと `inSegment` イベント。
   - ただし、セグメントが `inSegment` イベントはプロファイルのみ、セグメント定義 **遺言** をエッジセグメント化に対して有効にする。

## 次の手順

このガイドでは、Adobe Experience Platformでエッジセグメント化を使用してセグメントを評価する方法について説明します。 Experience Platformユーザーインターフェイスの使用方法の詳細については、 [セグメント化ユーザーガイド](./overview.md). Experience PlatformAPI を使用して同様のアクションを実行し、セグメントを操作する方法については、 [エッジセグメント化 API ガイド](../api/edge-segmentation.md).

## 付録

次の節では、エッジセグメント化に関するよくある質問を示します。

### Edge ネットワーク上でセグメントが使用可能になるまで、どれくらいかかりますか？

Edge ネットワーク上でセグメントが使用可能になるまで、最大 1 時間かかります。
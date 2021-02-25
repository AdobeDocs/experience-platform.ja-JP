---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API
title: 計算済み属性のPQL式の例
topic: ガイド
type: ドキュメント
description: 計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 これらの関数では、有効なプロファイルクエリ言語(PQL)式を使用する必要があります。 このガイドでは、計算済み属性に対して最も一般的に使用されるPQL式の一部について説明します。
translation-type: tm+mt
source-git-commit: 92533f732cc14b57d2a0a34ce9afe99554f9af04
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 2%

---


# (Alpha)計算済み属性のPQL式のサンプル

>[!IMPORTANT]
>
>計算済み属性機能は現在アルファベットで表示されており、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformでは、計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 これらの関数は自動的に計算され、セグメント化、アクティベーションおよびパーソナライゼーションで使用できます。 計算済みの各属性は、名前と説明、値が保持されるフィールドのスキーマクラスとパス、計算済みの属性に格納する値を計算済みの値とする式など、基本情報を使用して定義されます。

計算済みの属性で使用される式は、[!DNL Profile Query Language] (PQL)というExperience Data Model(XDM)準拠のクエリ言語を使用して作成されます。この言語は、リアルタイム顧客プロファイルデータのクエリの定義と実行をサポートするように設計されています。

計算済み属性は、現在、次の関数をサポートしています。sum、count、min、maxおよびboolean。 このガイドでは、組織に独自の計算済み属性を定義する際に使用できる、最も一般的に使用されるPQL式の一部について説明します。 PQLの詳細、およびサポートされるクエリのその他のフォーマットガイドラインやサンプルへのリンクについては、[PQLの概要](../../segmentation/pql/overview.md)を参照してください。

## ストリーミング式

次の表に、ストリーミングモードでサポートされる、一般的に使用されるクエリ式の詳細を示します。

| 説明 | PQL式 | 入力タイプ：<br/>プロファイルまたはエクスペリエンスのイベント(EE[]) | 結果のタイプ |
|---|---|---|---|
| 過去7日間の画像のダウンロード数です。 | xEvent[（タイムスタンプは今後7日以内に発生）およびeventType=&quot;download&quot;とcontentType = &quot;image&quot;].count() | プロファイル&amp;EE[] | 整数 |
| 過去7日間のスポーツ用品に対する顧客支出の合計。 | xEvent[(timestamp occurs &lt; 7 days before now) and eventType=&quot;transaction&quot; andカテゴリ= &quot;sportgoods&quot;].sum(commerce.order.priceTotal) | プロファイル&amp;EE[] | 整数または重複 |
| 過去7日間のスポーツ用品に対する平均顧客支出。<br/><br/>**注意：計算済みの属性を3つ** 作成する必要があります。 | **ca1:** xEvent[(timestamp occurs  &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>.sum(commerce.order.priceTotal)<br/><br/>**2:** xEvent[(timestamp occurs ca3 ca &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;><br/><br/>**** : ca / ca2)]] | プロファイル&amp;EE[] | Double |
| 顧客は過去7日間にスポーツ用品に100ドル以上費やしたことがありますか。<br/><br/>**注意：計算済みの属性を2つ作成する** 必要があります。 | **ca1:** xEvent[(timestamp occurs  &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>.sum(commerce.order.priceTotal)<br/><br/>**ca2:** ca1 > 100] | プロファイル&amp;EE[] | Boolean |
| 顧客は過去7日間に購入したか。 | chain(xEvent, timestamp, [A:WHAT(eventType = &quot;transaction&quot;) WHEN（&lt;7日前）]) | プロファイル&amp;EE[] | ブール値 |
| 過去7日間で最も低いユーザーがスポーツ用品に費やした時間。 | xEvent[(timestamp occurs &lt; 7 days before now) and eventType=&quot;transaction&quot; andカテゴリ= &quot;sportgoods&quot;].min(commerce.order.priceTotal) | プロファイル&amp;EE[] | 整数または重複 |
| 過去7日間で最も多くのユーザーがスポーツ用品に費やした。 | xEvent[(timestamp occurs &lt; 7 days before now) and eventType=&quot;transaction&quot; andカテゴリ= &quot;sportgoods&quot;].max(commerce.order.priceTotal) | プロファイル&amp;EE[] | 整数または重複 |
| 過去7日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロード数です。 | xEvent[（タイムスタンプは今後7日以内に発生）およびeventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.count())) | プロファイル&amp;EE[] | Map[String, Integer] |
| 過去7日間の、ダウンロードされた各製品のダウンロード数に対する数値プロパティの合計で、製品ごとにインデックスが作成されます。 | xEvent[（タイムスタンプは今後7日前に発生）およびeventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal))) | プロファイル&amp;EE[] | Map[String、Integer]またはMap[String、重複] |
| 過去7日間の、ダウンロードされた各製品のダウンロード数に対する数値プロパティのインデックスが製品別に作成された平均です。<br/><br/>**注意：計算済みの属性を3つ** 作成する必要があります。 | **ca1:** xEvent[(timestamp occurs.groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal))) &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>ca2:<br/><br/>**xEvent** (timestamp occurs.groupBy(product).map[ &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;><br/><br/>**** (((K, G) entry => map)(K, K, G.count())g.ca3:ca1]] | プロファイル&amp;EE[] | Map[文字列、重複] |
| 過去7日間の、ダウンロードされた各製品のダウンロードに対する数値プロパティの最小値で、製品ごとにインデックスが作成されています。 | xEvent[（タイムスタンプは今後7日前に発生）およびeventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.min(commerce.order.priceTotal))) | プロファイル&amp;EE[] | Map[String、Integer]またはMap[String、重複] |
| 過去7日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードに対する数値プロパティの最大数。 | xEvent[（タイムスタンプは今後7日前に発生）およびeventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.max(commerce.order.priceTotal))) | プロファイル&amp;EE[] | Map[String、Integer]またはMap[String、重複] |
| イベントを参照せず、プロファイルに対する数値式。 | if(person.gender = &quot;female&quot;, 60, 65) | プロファイル | 整数または重複 |
| イベントを参照せず、プロファイルに対するブール式。 | person.birthYear >= 2000 | プロファイル | ブール値 |
| プロファイルに対する文字列式(イベントを参照しない)。 | if(homeAddress.countryCode in [&quot;US&quot;,&quot;MX&quot;,&quot;CA&quot;], &quot;NA&quot;, &quot;ROW&quot;) | プロファイル | 文字列 |

## バッチ式

次の表に、バッチモードでのみ使用可能なクエリ式（現在ストリーミングでは使用できないもの）の詳細を示します。

| 説明 | PQL式 | 入力タイプ：<br/>プロファイルまたはエクスペリエンスのイベント(EE[]) | 結果のタイプ |
|---|---|---|---|
| 過去7日間の製品のダウンロード数に対する数値式の合計が100を超えるかどうか（製品ごとにインデックス付け）。 | xEvent[（タイムスタンプは今後7日以内に発生）およびeventType=&quot;download&quot;].groupBy(product).map(((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal) > 100)) | プロファイル&amp;EE[] | Map[String, Boolean] |
| 過去7日間の製品のダウンロード数に対する数値式の平均が100を超えるかどうか（インデックスは製品別）。 | xEvent[（タイムスタンプは今後7日前に発生）およびeventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.average(commerce.order.priceTotal) > 100)) | プロファイル&amp;EE[] | Map[String, Boolean] |
| 過去7日間の、ダウンロードした各製品に対する様々な指標の累積的なインデックスが製品別に作成されたもの。 | xEvent[（タイムスタンプは今後7日前に発生）およびeventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, {&quot;count&quot;:G.count(), &quot;sum&quot;:G.sum(commerce.order.priceTotal)}) | プロファイル&amp;EE[] | Map[String, Object]。ObjectはカスタムXDM型です。 |
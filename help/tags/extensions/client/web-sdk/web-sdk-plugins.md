---
title: Common Web SDK Plugins 拡張機能の概要
description: Adobe Experience Platformの Common Web SDK Plugins タグ拡張機能について説明します。
exl-id: 6052603b-1537-4dc7-9278-969d892ca15b
source-git-commit: 12bd4c6c1993afc438b75a3e5163ebe2fe8a8dd0
workflow-type: tm+mt
source-wordcount: '2064'
ht-degree: 0%

---

# Common Web SDK Plugins 拡張機能の概要

>[!IMPORTANT]
>
>拡張機能は、Adobe Experience Platform Web SDK 拡張機能と共に使用することを目的としています。 Plugins と共に使用するためのバージョンについては、[Common Analytics AppMeasurement拡張機能 &#x200B;](../plugins/overview.md) の概要を参照してください。

このドキュメントでは、Web SDK Plugins タグ拡張機能を設定し、それを使用して [Adobe Experience Platform Web SDK 拡張機能 &#x200B;](../web-sdk/overview.md) を拡張する方法について説明します。

## Common Web SDK Plugins 拡張機能の設定

この節では、Web SDK プラグイン拡張機能の設定時に使用できるオプションのリファレンスを提供します。

>[!IMPORTANT]
>
>Common Web SDK Plugins 拡張機能は、Adobe Experience Platform Web SDK 拡張機能を拡張することを目的としていますが、拡張機能が期待どおりに動作するためにインストールする必要はありません。

## Adobe Experience Platform Web SDK 拡張機能へのプラグインの追加

Common Web SDK Plugins 拡張機能で提供される次のネイティブデータ要素を使用する以外では、プラグインを初期化またはライブラリに追加するための設定は必要ありません。

* [&#39;getAndPersistValue&#39;](#getAndPersistValue)
* [&#39;getGeoCoordinates&#39;](#getGeoCoordinates)
* [&#39;getNewRepeat&#39;](#getNewRepeat)
* [&#39;getPagename&#39;](#getPagename)
* [&#39;getPreviousValue&#39;](#getPreviousValue)
* [&#39;getQueryParam&#39;](#getQueryParam)
* [&#39;getTimeParting&#39;](#getTimeParting)
* [&#39;getTimeSinceLastVisit&#39;](#getTimeSInceLastVisit)
* [『 getValOnce 』](#getValOnce)
* [&#39;getVisitDuration&#39;](#getVisitDuration)
* [&#39;getVisitNum&#39;](#getVisitNum)
* [&#39;pFo&#39;](#pFo)

[//]: # (- [ ] Add links to plugin pages within the data elements below)

### `getAndPersistValue`

>[!IMPORTANT]
>
>このデータ要素は、Cookie を設定することと、ユーザーが生成した値を Cookie に保存することを可能にします。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getAndPersistValue` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getandpersistvalue.html?lang=ja) をセットアップおよび設定できます。 `getAndPersistValue` データ要素は、値を cookie に保存し、後で訪問中に取得することができます。

`getAndPersistValue` データ要素は、次の引数を提供します。

* `vtp` （必須）：ページ間で保持する値
* `cn` （任意）：値を保存する Cookie の名前です。 この引数を設定しない場合、cookie の名前は `"s_gapv"` になります
* `ex` （任意）:Cookie の有効期限が切れるまでの日数です。 この引数が `0` である場合、または設定されていない場合、cookie は訪問の最後（無操作状態が 30 分間続いた時点）に有効期限が切れます。

`vtp` 引数の変数が設定されている場合、データ要素は cookie を設定し、cookie の値を返します。 `vtp` 引数の変数が設定されていない場合、データ要素は cookie の値のみを返します。

### `getGeoCoordinates`

>[!IMPORTANT]
>
>このプラグインは、クライアント上で場所へのアクセスを必要としますが、アクセスできない場合は例外をスローしません。

[`getGeoCoordinates` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getgeocoordinates.html?lang=ja) をセットアップおよび設定できます。 `getGeoCoordinates` データ要素は、訪問者のデバイスの緯度と経度をキャプチャします。

`getGeoCoordinates` データ要素では引数を使用しません。 次のいずれかの値を返します。

* `"geo coordinates not available"`：プラグインの実行時に位置情報データを利用できないデバイスの場合。 この値は、訪問の最初のヒットで一般的です。特に、訪問者が最初に場所の追跡に同意する必要がある場合に重要です。
* `"error retrieving geo coordinates"`：デバイスの場所を取得しようとしたときにエラーが発生した場合
* `"latitude=[LATITUDE] | longtitude=[LONGITUDE]"`：ここで [ 緯度 ]/[ 経度 ] は、それぞれ緯度と経度です

### `getNewRepeat`

>[!IMPORTANT]
>
>このデータ要素は cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getNewRepeat` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getnewrepeat.html?lang=ja) をセットアップおよび設定できます。 `getNewRepeat` データ要素は、サイトへの訪問者が新規訪問者か、希望日数以内のリピート訪問者かを特定します。

`getNewRepeat` データ要素では、次の引数を使用します。

* `d` （整数、オプション）：訪問者を `"New"` にリセットする訪問の間に必要な最小日数。 この引数を設定しない場合は、デフォルトで 30 日になります。

このデータ要素は、データ要素によって設定された Cookie が存在しないか、有効期限が切れている場合、`"New"` の値を返します。 データ要素によって設定された Cookie が存在する場合、および現在のヒットからの経過時間と Cookie に設定された時間が 30 分を超える場合、`"Repeat"` の値を返します。 このメソッドは、訪問全体に対して同じ値を返します。

### `getPageName`

[`getPageName` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getpagename.html?lang=ja) をセットアップおよび設定できます。 `getPageName` データ要素は、現在の URL の読みやすい、わかりやすい形式のバージョンを作成します。

`getPageName` データ要素では、次の引数を使用します。

* `si` （オプション、文字列）：サイトの ID を表す文字列の先頭に挿入される ID。 この値には、数値 ID またはわかりやすい名前を指定できます。 設定しない場合は、現在のドメインがデフォルトで使用されます。
* `qv` （オプション、文字列）:URL 内に見つかった場合に、文字列に追加されるクエリ文字列パラメーターのコンマ区切りリスト
* `hv` （オプション、文字列）:URL ハッシュで見つかったパラメーターのコンマ区切りリスト。URL で見つかった場合に、文字列に追加されます
* `de` （オプション、文字列）：文字列の個々の部分を分割する区切り文字。 既定はパイプ（`|`）です。

データ要素は、わかりやすい形式の URL を含む文字列を返します。 この文字列は通常 `pageName` 変数に割り当てられますが、他の変数でも使用できます。

### `getPreviousValue`

>[!IMPORTANT]
>
>このデータ要素は、Cookie を設定することと、ユーザーが生成した値を Cookie に保存することを可能にします。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getPreviousValue` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getpreviousvalue.html?lang=ja) をセットアップおよび設定できます。 `getPreviousValue` データ要素は、以前のヒットで設定された値に変数を設定します。

`getPreviousValue` データ要素では、次の引数を使用します。

* `v` （string、必須）：次のイメージリクエストに渡す値を持つ変数です。 使用される一般的な変数は、以前のページ値を取得する `s.pageName` めに使用されます。
* `c` （string、オプション）：値を保存する Cookie の名前。  この引数が設定されていない場合は、デフォルトで `"s_gpv"` になります。

このデータ要素を呼び出すと、cookie に含まれる文字列値が返されます。 その後、プラグインは cookie の有効期限をリセットし、`v` 引数の変数値を割り当てます。 cookie は、30 分間無操作状態が続くと有効期限が切れます。

### `getQueryParam`

[`getQueryParam` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getqueryparam.html?lang=ja) をセットアップおよび設定できます。 `getQueryParam` データ要素は、URL に含まれる任意のクエリ文字列パラメーターの値を抽出します。 これは、ランディングページの URL から内部および外部の両方のキャンペーンコードを抽出する場合に役立ちます。 また、検索語句やその他のクエリ文字列パラメーターを抽出する場合にも役立ちます。 このデータ要素は、ハッシュや複数のクエリ文字列パラメーターを含んだ URL など、複雑な URL を解析する堅牢な機能を提供します。

`getQueryParam` データ要素では、次の引数を使用します。

* `qsp` （必須）:URL 内で検索するクエリ文字列パラメーターのコンマ区切りリスト。 大文字と小文字は区別されません。
* `de` （任意）：複数のクエリ文字列パラメーターが一致する場合に使用する区切り文字です。 デフォルトは空の文字列です。
* `url` （任意）：クエリ文字列パラメーターの値を抽出するカスタム URL、文字列、または変数です。 デフォルト値は `window.location` です。

このデータ要素を呼び出すと、上記の引数と URL に応じた値が返されます。

* 一致するクエリ文字列パラメーターが見つからない場合、メソッドは空の文字列を返します。
* 一致するクエリ文字列パラメーターが見つかった場合、メソッドはクエリ文字列パラメーター値を返します。
* 一致するクエリ文字列パラメーターが見つかったが値が空の場合、メソッドは `true` を返します。
* 一致するクエリ文字列パラメーターが複数見つかった場合、メソッドは `de` 引数内の文字列で区切られた各パラメーター値を持つ文字列を返します。

### `getTimeParting`

[`getTimeParting` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/gettimeparting.html?lang=ja) をセットアップおよび設定できます。 `getTimeParting` データ要素は、サイトで測定可能なアクティビティが発生した時間の詳細を取得します。 このデータ要素は、特定の日付範囲の繰り返し可能な時間分割で指標を分類する場合に役立ちます。 例えば、すべての日曜日とすべての木曜日など、2 つの異なる曜日のコンバージョン率を比較できます。 また、すべての朝とすべての夜など、1 日の期間を比較することもできます。

`getTimeParting` データ要素では、次の引数を使用します。

`t` （オプションですが、推奨される文字列）：訪問者のローカル時間を変換するタイムゾーンの名前です。  デフォルトは UTC/GMT 時間です。 有効な値の完全なリストについては、Wikipedia の [List of TZ database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) を参照してください。

一般的な有効な値は次のとおりです。

* 東部時間の `"America/New_York"`
* 中央標準時の `"America/Chicago"`
* 山岳標準時の `"America/Denver"`
* 太平洋時間の `"America/Los_Angeles"`

このデータ要素を呼び出すと、パイプ（`|`）で区切られた次を含む文字列が返されます。

* 今年
* 今月
* 月の日
* 曜日
* 現在の時刻（午前/午後）

### `getTimeSinceLastVisit`

>[!IMPORTANT]
>
>このデータ要素は cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getTimeSinceLastVisit` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/gettimesincelastvisit.html?lang=ja) をセットアップおよび設定できます。 `getTimeSinceLastVisit` データ要素は、訪問者が最後の訪問からサイトに戻るまでの時間を追跡します。

`getTimeSinceLastVisit` データ要素では引数を使用しません。 訪問者が最後にサイトに訪問してからの経過時間を次の形式でバケット化して返します。

* 最後の訪問から 30 分から 1 時間の間の時間が、最も近い 30 分のベンチマークに設定されます。 例：`"30.5 minutes"`、`"53 minutes"`
* 1 時間から 1 日の間の時間は、最も近い四半期/時のベンチマークに丸められます。 例：`"2.25 hours"`、`"7.5 hours"`
* 1 日を超える時間は、最も近い日のベンチマークに四捨五入されます。 例：`"1 day"`、`"3 days"`、`"9 days"`、`"372 days"`
* 以前に訪問者が訪問したことがない場合、または経過時間が 2 年を超える場合、値は `"New Visitor"` に設定されます。

### `getValOnce`

>[!IMPORTANT]
>
>このデータ要素は、Cookie を設定することと、ユーザーが生成した値を Cookie に保存することを可能にします。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getValOnce` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getvalonce.html?lang=ja) をセットアップおよび設定できます。 `getValOnce` データ要素は、変数が同じ値に複数回設定されるのを防ぎます。

`getValOnce` データ要素では、次の引数を使用します。

* `vtc` （必須、文字列）：以前に同じ値に設定されたかどうかを確認する変数です
* `cn` （オプション、文字列）：チェックする値を保持する cookie の名前。 デフォルトは `"s_gvo"`
* `et` （オプション、整数）:cookie の有効期限を日（または `ep` 引数に応じて分）で指定します。 デフォルトは `0` です。これはブラウザーセッションの終了時に期限切れになります
* `ep` （オプション、文字列）:`et` 引数も設定されている場合にのみ、この引数を設定します。 `et` 引数の有効期限を日ではなく分にする場合は、この引数を `"m"` に設定します。 デフォルトは `"d"` で、`et` 引数を日数で設定します。

`vtc` 引数と Cookie の値が一致する場合、このメソッドは空の文字列を返します。 `vtc` 引数と cookie の値が一致しない場合、メソッドは `vtc` 引数を文字列として返します。

### `getVisitDuration`

>[!IMPORTANT]
>
>このデータ要素は cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getVisitDuration` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getvisitduration.html?lang=ja) をセットアップおよび設定できます。 `getVisitDuration` のデータ要素は、訪問者がその時点までサイトを訪問した時間を分単位で追跡します。

`getVisitDuration` データ要素では引数を使用しません。 次のいずれかの値を返します。

* `"first hit of visit"`
* `"less than a minute"`
* `"1 minute"`
* `"[x] minutes"` （`[x]` は、訪問者がサイトに着陸してから経過した分数）

### `getVisitNum`

>[!IMPORTANT]
>
>このデータ要素は cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

[`getVisitNum` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getvisitnum.html?lang=ja) をセットアップおよび設定できます。 `getVisitNum` データ要素は、目的の日数内にサイトを訪問したすべての訪問者の訪問回数を返します。

`getVisitNum` データ要素では、次の引数を使用します。

* `rp` （オプション、整数または文字列）：訪問回数カウンターがリセットされるまでの日数です。  未設定の場合のデフォルト値は `365` です。
   * この引数を `"w"` にすると、カウンターは週末（この土曜日の午後 11:59）にリセットされます
   * この引数を `"m"` にすると、カウンターは月の終わり（今月の最終日）にリセットされます
   * この引数を `"y"` にすると、カウンターは年末（12 月 31 日（PT））にリセットされます
* `erp` （オプション、ブール値）:`rp` 引数が数値の場合、この引数は、訪問番号の有効期限を延長する必要があるかどうかを決定します。 `true` に設定すると、その後のサイトへのヒットでは、訪問回数カウンターがリセットされます。 `false` に設定した場合、訪問回数カウンターがリセットされても、サイトへの後続のヒットは拡張されません。 デフォルト値は `true` です。`rp` 引数が文字列の場合、この引数は無効です。

訪問回数は、訪問者が 30 分間無操作状態になった後にサイトに戻るたびに 1 増加します。 このメソッドを呼び出すと、訪問者の現在の訪問回数を表す整数が返されます。

### `p_fo` （ページ先頭のみ）

[`p_fo` Analytics プラグイン &#x200B;](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/p-fo.html?lang=ja) をセットアップおよび設定できます。 `p_fo` データ要素は、特定のJavaScript オブジェクトが存在するかどうかを確認するユーティリティです。 オブジェクトが存在しない場合、プラグインはオブジェクトを作成して `true` を返します。 JavaScript オブジェクトがページ上に既に存在する場合は、`false` を返します。 このデータ要素は、ページ上で 1 回だけコードを実行する場合に役立ちます。

`p_fo` データ要素では、次の引数を使用します。

* `on` （必須、文字列）：データ要素が作成するJavaScript オブジェクトの名前です（オブジェクトがまだページに存在しない場合）。

オブジェクトが存在しない場合、このメソッドは `true` を返してオブジェクトを作成します。 オブジェクトが既に存在する場合、このメソッドは `false` を返します。

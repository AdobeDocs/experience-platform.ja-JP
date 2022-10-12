---
title: Common Web SDK Plugins 拡張機能の概要
description: Adobe Experience Platformの Common Web SDK Plugins タグ拡張機能について説明します。
source-git-commit: 4edc811f6306cd66f0ebef0a21896624f6c5e181
workflow-type: tm+mt
source-wordcount: '2150'
ht-degree: 49%

---

# Common Web SDK Plugins 拡張機能の概要

>[!IMPORTANT]
>
>拡張機能は、Adobe Experience Platform Web SDK 拡張機能で使用されることを意図しています。 AppMeasurement で使用する予定のバージョンに関する情報を確認するには、 [Common Analytics Plugins 拡張機能](../plugins/overview.md).

このドキュメントでは、Web SDK Plugins タグ拡張機能を設定し、それを使用して [Adobe Experience Platform Web SDK 拡張機能](../sdk/overview.md).

## Common Web SDK Plugins 拡張機能の設定

この節では、Web SDK Plugins 拡張機能を設定する際に使用できるオプションについて説明します。

>[!IMPORTANT]
>
>Common Web SDK Plugins 拡張機能は、Adobe Experience Platform Web SDK 拡張機能を拡張することを目的としていますが、拡張機能が期待どおりに動作するように、拡張機能をインストールする必要はありません。

## Adobe Experience Platform Web SDK 拡張機能へのプラグインの追加

Common Web SDK Plugins 拡張機能で提供される次のネイティブデータ要素を使用する以外に、ライブラリを初期化または追加するための設定は必要ありません。

* [`getAndPersistValue`](#getAndPersistValue)
* [`getGeoCoordinates`](#getGeoCoordinates)
* [`getNewRepeat`](#getNewRepeat)
* [&#39;getPagename&#39;](#getPagename)
* [`getPreviousValue`](#getPreviousValue)
* [`getQueryParam`](#getQueryParam)
* [`getTimeParting`](#getTimeParting)
* [`getTimeSinceLastVisit`](#getTimeSInceLastVisit)
* [`getValOnce`](#getValOnce)
* [`getVisitDuration`](#getVisitDuration)
* [`getVisitNum`](#getVisitNum)
* [&#39;pFo&#39;](#pFo)

[//]: # (- [ ] Add links to plugin pages within the data elements below)

### `getAndPersistValue`

>[!IMPORTANT]
>
>このデータ要素は、Cookie を設定し、ユーザー生成値を Cookie に保存できるようにします。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getAndPersistValue` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getandpersistvalue.html). この `getAndPersistValue` データ要素は、cookie に値を保存します。この値は、後で訪問中に取得できます。

この `getAndPersistValue` データ要素には次の引数があります。

* `vtp`（必須）：ページ間で保持する値です。
* `cn`（オプション）：値を保存する Cookie の名前です。この引数が設定されていない場合、Cookie の名前は `"s_gapv"` です。
* `ex`（オプション）：Cookie の有効期限が切れるまでの日数です。この引数が `0` であるまたは設定されていない場合、Cookie は訪問の終了時（無操作状態が 30 分間続く）に期限切れになります。

変数が `vtp` 引数が設定されている場合、データ要素は cookie を設定し、その cookie の値を返します。 変数が `vtp` 引数が設定されていない場合、データ要素は cookie の値のみを返します。

### `getGeoCoordinates`

>[!IMPORTANT]
>
>このプラグインは、クライアント上の場所へのアクセスを必要としますが、取得されない場合は例外をスローしません。

を設定し、 [`getGeoCoordinates` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getgeocoordinates.html). この `getGeoCoordinates` データ要素は、訪問者のデバイスの緯度と経度を取り込みます。

この `getGeoCoordinates` データ要素は引数を使用しません。 以下のどちらかの値を返します。

* `"geo coordinates not available"`：プラグインの実行時に利用できる地域データのないデバイスの場合。この値は、訪問の最初のヒットで一般的です。特に、訪問者が場所を追跡する際に、最初に同意する必要がある場合に使用されます。
* `"error retrieving geo coordinates"`：プラグインがデバイスの場所を取得しようとしたときにエラーが発生した場合。
* `"latitude=[LATITUDE] | longtitude=[LONGITUDE]"`：ここで、[LATITUDE] と [LONGITUDE] は、それぞれ緯度と経度です。

### `getNewRepeat`

>[!IMPORTANT]
>
>このデータ要素は Cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getNewRepeat` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getnewrepeat.html). この `getNewRepeat` データ要素は、サイトの訪問者が新しい訪問者か、再訪問者かを希望の日数以内に判断します。

この `getNewRepeat` データ要素では、次の引数を使用します。

* `d`（整数、オプション）：訪問者を再び `"New"` にリセットする間隔の最小日数です。この引数を設定しない場合、デフォルトで 30 日に設定されます。

このデータ要素は、 `"New"` データ要素によって設定された cookie が存在しないか期限切れの場合。 次の値を返します。 `"Repeat"` データ要素によって設定された cookie が存在し、現在のヒットからの経過時間と cookie に設定された時間が 30 分を超える場合。 このメソッドは、訪問全体に対して同じ値を返します。

### `getPageName`

を設定し、 [`getPageName` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getpagename.html). この `getPageName` データ要素は、読みやすく、わかりやすい形式の現在の URL を作成します。

この `getPageName` データ要素では、次の引数を使用します。

* `si`（オプション、文字列）：サイトの ID を表す文字列の先頭に挿入される ID。この値は、数値 ID またはわかりやすい名前にすることができます。設定しない場合は、デフォルトで現在のドメインが使用されます。
* `qv`（オプション、文字列）：URL 内にある場合に文字列に追加されるクエリー文字列パラメーターのコンマ区切りリストです。
* `hv`（オプション、文字列）：URL ハッシュに含まれるパラメーターのコンマ区切りリストです。URL 内に含まれる場合は文字列に追加されます。
* `de`（オプション、文字列）：文字列の個々の部分を分割する区切り文字です。デフォルトはパイプ（`|`）です。

データ要素は、わかりやすい形式の URL を含む文字列を返します。 この文字列は通常 `pageName` 変数に割り当てられますが、他の変数でも使用できます。

### `getPreviousValue`

>[!IMPORTANT]
>
>このデータ要素は、Cookie を設定し、ユーザー生成値を Cookie に保存できるようにします。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getPreviousValue` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getpreviousvalue.html). この `getPreviousValue` データ要素は、変数を以前のヒットに設定された値に設定します。

この `getPreviousValue` データ要素では、次の引数を使用します。

* `v`（文字列、必須）：次のイメージリクエストに渡す値を持つ変数。一般的な変数は、前のページ値を取得するために使用される `s.pageName` です。
* `c`（文字列、オプション）：値を格納する Cookie の名前。この引数を設定しない場合、デフォルトは `"s_gpv"` になります。

このデータ要素を呼び出すと、Cookie に含まれる文字列値が返されます。 その後、プラグインは Cookie の有効期限をリセットし、`v` 引数から変数値を割り当てます。Cookie は、無操作状態が 30 分間続くと有効期限が切れます。

### `getQueryParam`

を設定し、 [`getQueryParam` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getqueryparam.html). この `getQueryParam` データ要素は、URL に含まれるクエリー文字列パラメーターの値を抽出します。 これは、ランディングページの URL から内部および外部の両方のキャンペーンコードを抽出する場合に役立ちます。また、検索用語や他のクエリー文字列パラメーターを抽出する場合にも役立ちます。このデータ要素は、複数のクエリー文字列パラメーターを含むハッシュや URL を含む複雑な URL を解析する際に堅牢な機能を提供します。

この `getQueryParam` データ要素では、次の引数を使用します。

* `qsp`（必須）：URL 内で検索するクエリー文字列パラメーターのコンマ区切りリストです。大文字と小文字は区別されません。
* `de`（オプション）：複数のクエリー文字列パラメーターが一致する場合に使用する区切り文字です。デフォルトでは空の文字列です。
* `url`（オプション）：クエリー文字列パラメーターの値を抽出するカスタム URL、文字列、または変数です。デフォルト値は `window.location` です。

このデータ要素を呼び出すと、上記の引数と URL に応じた値が返されます。

* 一致するクエリー文字列パラメーターが見つからない場合は、空の文字列を返します。
* 一致するクエリー文字列パラメーターが見つかった場合、このメソッドはクエリー文字列パラメーター値を返します。
* 一致するクエリー文字列パラメーターが見つかったが値が空の場合、このメソッドは `true` を返します。
* 一致するクエリー文字列パラメーターが複数見つかった場合、このメソッドは `de` 引数内の文字列で区切られた各パラメーター値を持つ文字列を返します。

### `getTimeParting`

を設定し、 [`getTimeParting` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/gettimeparting.html). この `getTimeParting` データ要素は、サイトで測定可能なアクティビティが発生した時間の詳細をキャプチャします。 このデータ要素は、指定した日付範囲で繰り返し可能な時間の除算で指標を分類する場合に役立ちます。 例えば、すべての日曜日とすべての木曜日など、2 つの異なる曜日間のコンバージョン率を比較できます。また、すべての朝とすべての晩など、1 日の時間を比較することもできます。

この `getTimeParting` データ要素では、次の引数を使用します。

`t`（オプション、推奨、文字列）：訪問者のローカル時間を変換するタイムゾーンの名前。デフォルトは UTC／GMT 時間です。詳しくは、 [TZ データベースのタイムゾーンのリスト](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 有効な値の完全なリストについては、Wikipedia のを参照してください。

一般的な有効値は次のとおりです。

* `"America/New_York"`（東部時間）
* `"America/Chicago"`（中央時間）
* `"America/Denver"`（山岳地帯）
* `"America/Los_Angeles"`（太平洋標準時間）

このデータ要素を呼び出すと、次のように区切られたパイプ (`|`):

* 現在の年
* 現在の月
* 日
* 曜日
* 現在の時刻（AM/PM）

### `getTimeSinceLastVisit`

>[!IMPORTANT]
>
>このデータ要素は Cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getTimeSinceLastVisit` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/gettimesincelastvisit.html). この `getTimeSinceLastVisit` データ要素は、訪問者が最後の訪問後にサイトに戻ってきた時間を追跡します。

この `getTimeSinceLastVisit` データ要素は引数を使用しません。 訪問者が最後にサイトに来てから経過した時間を次の形式でグループ化して返します。

* 最後の訪問から 30 分～1 時間の時間は、最も近い 0.5 分のベンチマークに設定されます。例：`"30.5 minutes"`、`"53 minutes"`。
* 1 時間～1 日の時間は、最も近い 0.25 時間刻みのベンチマーク値に丸められます。例：`"2.25 hours"`、`"7.5 hours"`。
* 1 日を超える時間は、最も近い 1 日刻みのベンチマーク値に丸められます。例：`"1 day"`、`"3 days"`、`"9 days"`、`"372 days"`。
* 訪問者が訪問しなかった場合、または経過時間が 2 年を超える場合、値は `"New Visitor"` に設定されます。

### `getValOnce`

>[!IMPORTANT]
>
>このデータ要素は、Cookie を設定し、ユーザー生成値を Cookie に保存できるようにします。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getValOnce` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getvalonce.html). この `getValOnce` データ要素は、変数が同じ値に複数回設定されるのを防ぎます。

この `getValOnce` データ要素では、次の引数を使用します。

* `vtc`（必須、文字列）：以前に同じ値に設定されていたかどうかを確認する変数です。
* `cn`（オプション、文字列）：確認する値を保持する Cookie の名前です。デフォルトは です。`"s_gvo"`
* `et`（オプション、整数）：Cookie の有効期限（引数に応じて日単位または分単位）`ep`です。デフォルト値は `0` で、ブラウザーセッションの終わりに有効期限が切れます。
* `ep`（オプション、文字列）：この引数は、`et` 引数も設定されている場合にのみ設定します。`et` 引数で有効期限を日数ではなく分単位で指定する場合は `"m"` に設定します。デフォルト値は `"d"` で、日単位で `et` 引数を設定します。

`vtc` 引数と Cookie の値が一致する場合、このメソッドは空の文字列を返します。`vtc` 引数と Cookie の値が一致しない場合、このメソッドは `vtc` 引数を文字列として返します。

### `getVisitDuration`

>[!IMPORTANT]
>
>このデータ要素は Cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getVisitDuration` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getvisitduration.html). この `getVisitDuration` データ要素は、訪問者がその時点までサイトに滞在した時間を分単位で追跡します。

この `getVisitDuration` データ要素は引数を使用しません。 以下のどちらかの値を返します。

* `"first hit of visit"`
* `"less than a minute"`
* `"1 minute"`
* `"[x] minutes"` （`[x]` は訪問者がサイトにランディングしてから経過した分数）

### `getVisitNum`

>[!IMPORTANT]
>
>このデータ要素は Cookie を設定します。 詳しくは、プラグイン固有のドキュメントを参照してください。

を設定し、 [`getVisitNum` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/getvisitnum.html). この `getVisitNum` データ要素は、目的の日数内にサイトに来訪したすべての訪問者の訪問回数を返します。

この `getVisitNum` データ要素では、次の引数を使用します。

* `rp`（オプション、整数または文字列）：訪問回数カウンターがリセットされるまでの日数です。未設定の場合のデフォルト値は `365` です。
   * この引数が `"w"` に指定されている場合、カウンターは週の終わり（この土曜日の午後 11:59）にリセットされます。
   * この引数が `"m"` に指定されている場合、カウンターは月の終わり（今月の最終日）にリセットされます。
   * この引数が `"y"` に指定されている場合、カウンターは年の終わり（12 月 31 日）にリセットされます。
* `erp`（オプション、ブール値）：`rp` 引数が数値の場合、この引数は訪問回数の有効期限を延長する必要があるかどうかを指定します。`true` に設定した場合、サイトへの後続のヒットによって訪問回数カウンターがリセットされます。`false` に設定した場合、サイトへの後続のヒットは、訪問回数カウンターがリセットされたときに延長されません。デフォルト値は `true` です。この引数は、`rp` 引数が文字列の場合は無効です。

無操作状態が 30 分間続いたあとで訪問者がサイトに戻るたびに、訪問回数が増加します。このメソッドを呼び出すと、訪問者の現在の訪問回数を表す整数が返されます。

### `p_fo` （最初のページのみ）

を設定し、 [`p_fo` Analytics プラグイン](https://experienceleague.adobe.com/docs/analytics/implementation/vars/plugins/p-fo.html). この `p_fo` データ要素は、特定の JavaScript オブジェクトが存在するかどうかを確認するユーティリティです。 オブジェクトが存在しない場合は、オブジェクトを作成して `true` を返します。JavaScript オブジェクトが既にページ上に存在する場合は、`false` を返します。このデータ要素は、ページ上でコードを 1 回だけ実行する場合に役立ちます。

この `p_fo` データ要素では、次の引数を使用します。

* `on` （必須、文字列）:データ要素が作成する JavaScript オブジェクトの名前（オブジェクトがまだページ上に存在しない場合）。

オブジェクトが存在しない場合、このメソッドは `true` を返してオブジェクトを作成します。オブジェクトが既に存在する場合、このメソッドは `false` を返します。

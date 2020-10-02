---
keywords: Experience Platform;home;popular topics;map csv;map csv file;map csv file to xdm;map csv to xdm;ui guide;mapper;mapping;mapping fields;mapping functions;
solution: Experience Platform
title: データ準備関数
topic: overview
description: このドキュメントでは、データ準備で使用するマッピング関数を紹介します。
translation-type: tm+mt
source-git-commit: d47410106a6d3955cc9af78e605c893f08185ffa
workflow-type: tm+mt
source-wordcount: '3432'
ht-degree: 20%

---


# データ準備関数

データ準備関数を使用して、ソースフィールドに入力した値に基づいて値を計算および計算できます。

## フィールド

フィールド名には、任意の正当な識別子を使用できます。Unicodeの文字と数字を無制限に長く並べたもので、文字、ドル記号(`$`)またはアンダースコア文字(`_`)で始まります。 変数名では大文字と小文字が区別されます。

フィールド名がこの規則に従わない場合は、フィールド名を含める必要があり `${}`ます。 したがって、例えば、フィールド名が「First Name」または「First.Name」の場合、名前は `${First Name}` それぞれまたはのように囲む必要があり `${First.Name}` ます。

また、フィールド名は **、次の予約キーワードのい** ずれかです。これを含める必要があり `${}`ます。

```console
new, mod, or, break, var, lt, for, false, while, eq, gt, div, not, null, continue, else, and, ne, true, le, if, ge, return
```

サブフィールド内のデータは、ドット表記を使用してアクセスできます。 例えば、オブジェクトがある場合は、を使用して `name` フィールドにアクセスし `firstName` ま `name.firstName`す。

## 関数のリスト

次の表に、サンプル式とその結果生成される出力を含む、サポートされるすべてのマッピング関数を示します。

### 文字列関数

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| concat | 渡された文字列を連結します。 | <ul><li>文字列：連結する文字列です。</li></ul> | concat(STRING_1, STRING_2) | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| explode | 正規表現に基づいて文字列を分割し、部分の配列を返します。文字列を分割するregexをオプションで含めることができます。 デフォルトでは、分割は「,」に解決されます。 | <ul><li>文字列： **必ず指定します** 。分割する必要のある文字列です。</li><li>REGEX: *オプション* ：文字列の分割に使用できる正規式です。</li></ul> | explode(STRING, REGEX) | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の場所/インデックスを返します。 | <ul><li>入力： **必須** ：検索する文字列です。</li><li>SUBSTRING: **必須** ：文字列内で検索するサブ文字列です。</li><li>開始位置： *オプション* ：文字列内で開始を検索する場所です。</li><li>オカレンス： *オプション* :開始位置から検索するn番目の値。 デフォルトの重み付けは 1 です。 </li></ul> | instr(INPUT, SUBSTRING,開始_位置，オカレンス) | instr(&quot;adobe`<span>`.com&quot;, &quot;com&quot;) | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合は、その文字列を置き換えます。 | <ul><li>入力： **必須** ：入力文字列。</li><li>TO_FIND: **必須** ：入力内で検索する文字列です。</li><li>TO_REPLACE: **必須** : &quot;TO_FIND&quot;内の値を置き換える文字列です。</li></ul> | replacestr(INPUT, TO_FIND, TO_REPLACE) | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 指定された長さのサブ文字列を返します。 | <ul><li>入力： **必須** ：入力文字列。</li><li>開始インデックス： **必須** ：サブ文字列開始ーが含まれる入力文字列のインデックスです。</li><li>LENGTH: **Required** The length of the substring.</li></ul> | substr(INPUT,開始_INDEX, LENGTH) | substr(&quot;This is a substring test&quot;, 7, 8) | &quot;サブセット&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | <ul><li>入力： **必須** ：小文字に変換する文字列です。</li></ul> | lower(INPUT) | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | <ul><li>入力： **必須** ：大文字に変換する文字列です。</li></ul> | upper(INPUT) | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り記号で入力文字列を分割します。 | <ul><li>入力： **必須** ：分割する入力文字列です。</li><li>区切り文字： **必須** ：入力を分割するために使用する文字列です。</li></ul> | split(INPUT, SEPARATOR) | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | 区切り記号を使用してオブジェクトのリストを結合します。 | <ul><li>区切り文字： **必須** ：オブジェクトを結合するために使用する文字列です。</li><li>オブジェクト： **必須** ：結合する文字列の配列です。</li></ul> | `join(SEPARATOR, [OBJECTS])` | `join(" ", ["Hello", "world"])` | &quot;Hello world&quot; |
| lpad | 文字列の左側を、もう1つの指定した文字列で埋めます。 | <ul><li>入力： **必須** ：埋め込む文字列です。 この文字列はnullにできます。</li><li>カウント： **必須** ：埋め込む文字列のサイズです。</li><li>パディング： **必須** ：入力に埋め込む文字列です。 nullまたは空の場合は、1つのスペースとして扱われます。</li></ul> | lpad(INPUT, COUNT, PADDING) | lpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;yzybat&quot; |
| rpad | 文字列の右側をもう1つの指定された文字列で埋めます。 | <ul><li>入力： **必須** ：埋め込む文字列です。 この文字列はnullにできます。</li><li>カウント： **必須** ：埋め込む文字列のサイズです。</li><li>パディング： **必須** ：入力に埋め込む文字列です。 nullまたは空の場合は、1つのスペースとして扱われます。</li></ul> | rpad(INPUT, COUNT, PADDING) | rpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;batyzyzy&quot; |
| left | 渡された文字列の最初の&quot;n&quot;文字を取得します。 | <ul><li>文字列： **必須** ：最初の&quot;n&quot;文字を取得する文字列。</li><li>カウント： ****&#x200B;必須：文字列から取得する&quot;n&quot;文字。</li></ul> | left(STRING, COUNT) | left(&quot;abcde&quot;, 2) | &quot;ab&quot; |
| right | 渡された文字列の最後の&quot;n&quot;文字を取得します。 | <ul><li>文字列： **必須** ：最後の「n」文字を取得する文字列。</li><li>カウント： ****&#x200B;必須：文字列から取得する&quot;n&quot;文字。</li></ul> | right(STRING, COUNT) | right(&quot;abcde&quot;, 2) | &quot;de” |
| ltrim | 文字列の先頭から空白を削除します。 | <ul><li>文字列： **必須** ：空白を削除する文字列です。</li></ul> | ltrim(STRING) | ltrim(&quot; hello&quot;) | &quot;hello&quot; |
| rtrim | 文字列の末尾の空白を削除します。 | <ul><li>文字列： **必須** ：空白を削除する文字列です。</li></ul> | rtrim(STRING) | rtrim(&quot;hello &quot;) | &quot;hello&quot; |
| trim | 文字列の先頭と末尾の空白を削除します。 | <ul><li>文字列： **必須** ：空白を削除する文字列です。</li></ul> | trim(STRING) | trim(&quot; hello &quot;) | &quot;hello&quot; |
| 次と等しい | 2つの文字列を比較して、等しいかどうかを確認します。 この関数では大文字と小文字が区別されます。 | <ul><li>STRING1: **必須** ：比較する最初の文字列。</li><li>STRING2: **必須** 2番目に比較する文字列。</li></ul> | STRING1.equals(STRING2) | &quot;string1&quot;.equals(&quot;STRING1) | false |
| equalSignoreCase | 2つの文字列を比較して、等しいかどうかを確認します。 この関数では大文字と小文字が区別され **ません** 。 | <ul><li>STRING1: **必須** ：比較する最初の文字列。</li><li>STRING2: **必須** 2番目に比較する文字列。</li></ul> | STRING1.equalsIgnoreCase(STRING2) | &quot;string1&quot;.equalsIgnoreCase(&quot;STRING1) | true |

### ハッシュ関数

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| sha1 | 入力を受け取り、Secure Hash Algorithm 1(SHA-1)を使用してハッシュ値を生成します。 | <ul><li>入力： **必須** ：ハッシュするプレーンテキストです。</li><li>CHARSET: *オプション* ：文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | sha1(INPUT, CHARSET) | sha1(&quot;my text&quot;, &quot;UTF-8&quot;) | c3599c11e47719df18a24 &#x200B; 48690840c5dfcce3c80 |
| sha256 | 入力を受け取り、Secure Hash Algorithm 256(SHA-256)を使用してハッシュ値を生成します。 | <ul><li>入力： **必須** ：ハッシュするプレーンテキストです。</li><li>CHARSET: *オプション* ：文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | sha256(INPUT, CHARSET) | sha256(&quot;my text&quot;, &quot;UTF-8&quot;) | 7330d2b39ca35eaf4cb95fc846c21 &#x200B;ee6a39af698154a83a586ee270a0d372104 |
| sha512 | 入力を受け取り、Secure Hash Algorithm 512(SHA-512)を使用してハッシュ値を生成します。 | <ul><li>入力： **必須** ：ハッシュするプレーンテキストです。</li><li>CHARSET: *オプション* ：文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | sha512(INPUT, CHARSET) | sha512(&quot;my text&quot;, &quot;UTF-8&quot;) | a3d7e45a0d9be5fd4e4b9a3b8c9c2163c21ef &#x200B; 708bf11b4232bb21d2a8704ada2cd7b367dd078a89 a5c908cfe377aceb1072a7b386b7d4fd2ff68a8fd24d16 |
| md5 | 入力を受け取り、MD5を使用してハッシュ値を生成します。 | <ul><li>入力： **必須** ：ハッシュするプレーンテキストです。</li><li>CHARSET: *オプション* ：文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。 </li></ul> | md5(INPUT, CHARSET) | md5(&quot;my text&quot;, &quot;UTF-8&quot;) | d3b96ce8c9fb4 &#x200B; e9bd0198d03ba6852c7 |
| crc32 | 入力でCyclic Redundancy Check（CRC；巡回冗長検査）アルゴリズムを使用し、32ビットの循環コードを生成します。 | <ul><li>入力： **必須** ：ハッシュするプレーンテキストです。</li><li>CHARSET: *オプション* ：文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | crc32(INPUT, CHARSET) | crc32(&quot;my text&quot;, &quot;UTF-8&quot;) | 8df92e80 |

### URL関数

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| get_url_protocol | 渡されたURLからプロトコルを返します。 入力が無効な場合は、nullを返します。 | <ul><li>URL: **必須** ：プロトコルの抽出元URL。</li></ul> | get_url_protocol(URL) | get_url_protocol(&quot;https://platform.adobe.com/home&quot;) | https |
| get_url_host | 渡されたURLのホストを返します。 入力が無効な場合は、nullを返します。 | <ul><li>URL: **必須** ：ホストの抽出元のURL。</li></ul> | get_url_host(URL) | get_url_host(&quot;https://platform.adobe.com/home&quot;) | platform.adobe.com |
| get_url_port | 渡されたURLのポートを返します。 入力が無効な場合は、nullを返します。 | <ul><li>URL: **必須** ：ポートの抽出元URL。</li></ul> | get_url_port(URL) | get_url_port(&quot;sftp://example.com//home/joe/employee.csv&quot;) | 22 |
| get_url_path | 渡されたURLのパスを返します。 デフォルトでは、フルパスが返されます。 | <ul><li>URL: **必須** ：パスの抽出元のURL。</li><li>FULL_PATH: *オプション* 。フルパスを返すかどうかを指定するboolean値です。 falseに設定した場合は、パスの終わりのみが返されます。</li></ul> | get_url_path(URL, FULL_PATH) | get_url_path(&quot;sftp://example.com//home/joe/employee.csv&quot;) | &quot;//home/joe/employee.csv&quot; |
| get_url_クエリ_str | 渡されたURLのクエリ文字列を返します。 | <ul><li>URL: **必須** :クエリ文字列の取得元のURL。</li><li>アンカー： **必須** :クエリ文字列内のアンカーを使用して何を行うかを決定します。 次の3つの値のいずれかになります。&quot;retain&quot;、&quot;remove&quot;または&quot;append&quot;。<br><br>値が「retain」の場合、返される値にアンカーが割り当てられます。<br>値が「remove」の場合、返される値からアンカーが削除されます。<br>値が「append」の場合、アンカーは別の値として返されます。</li></ul> | get_url_クエリ_str(URL, ANCHOR) | get_url_クエリ_str(&quot;foo://example.com:8042/over/there?name=ferret#nose&quot;, &quot;retain&quot;)<br>get_url_クエリ_str(&quot;foo://example.com:8042/over/there?name=ferret#nose&quot;, &quot;remove&quot;)<br>get_url_クエリ_str(&quot;foo://example.com:8042/over/there?name=ferret#nose&quot;, &quot;append&quot;) | `{"name": "ferret#nose"}`<br>`{"name": "ferret"}`<br>`{"name": "ferret", "_anchor_": "nose"}` |

### 日付および時間関数

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| now | 現在の時刻を取得します。 |  | now() | now() | `2020-09-23T10:10:24.556-07:00[America/Los_Angeles]` |
| timestamp | 現在の Unix 時間を取得します。 |  | timestamp() | timestamp() | 1571850624571 |
| format | 指定された形式に従って入力日をフォーマットします。 | <ul><li>日付： **必須** ：書式を設定するZonedDateTimeオブジェクトとしての入力日付。</li><li>形式： **必須** ：日付を変更する形式を指定します。</li></ul> | format(DATE, FORMAT) | format(2019-10-23T11:24:00+00:00, &quot;yyyy-MM-dd HH:mm:ss&quot;) | &quot;2019-10-23 11:24:35&quot; |
| dformat | 指定された形式に従ってタイムスタンプを日付文字列に変換します。 | <ul><li>TIMESTAMP: **必須** ：形式を設定するタイムスタンプ。 これはミリ秒で書き込まれます。</li><li>形式： **必須** ：タイムスタンプを変更する形式です。</li></ul> | dformat(TIMESTAMP, FORMAT) | dformat(1571829875, &quot;dd-MMM-yyyy hh:mm&quot;) | &quot;23-Oct-2019 11:24&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付： **必須** ：日付を表す文字列です。</li><li>形式： **必須** ：日付の形式を表す文字列です。</li><li>DEFAULT_DATE: **必須** ：指定した日付がnullの場合にデフォルトで返される日付です。</li></ul> | date(DATE, FORMAT, DEFAULT_DATE) | date(&quot;23-Oct-2019 11:24&quot;, &quot;yyyy/MM/dd&quot;, now()) | &quot;2019-10-23T11:24:00+00:00&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付： **必須** ：日付を表す文字列です。</li><li>形式： **必須** ：日付の形式を表す文字列です。</li></ul> | date(DATE, FORMAT) | date(&quot;23-Oct-2019 11:24&quot;) | &quot;2019-10-23T11:24:00+00:00&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付： **必須** ：日付を表す文字列です。</li></ul> | date(DATE) | date(&quot;23-Oct-2019 11:24&quot;, &quot;yyyy/MM/dd&quot;) | &quot;2019-10-23T11:24:00+00:00&quot; |
| date_part | 日付の一部を取得します。次のコンポーネント値がサポートされています。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;quarter&quot;<br>&quot;qq&quot;<br>&quot;q&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;dayofyear&quot;<br>&quot;dy&quot;<br>&quot;y&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;week&quot;<br>&quot;ww&quot;<br>&quot;w&quot;<br><br>&quot;weekday&quot;<br>&quot;dw&quot;<br>&quot;w&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br>&quot;hh24&quot;<br>&quot;hh12&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot;<br><br>&quot;millisecond&quot;<br>&quot;ms&quot; | <ul><li>コンポーネント： **必ず指定します** 。日付の一部を表す文字列です。 </li><li>日付： **必須** ：標準形式の日付です。</li></ul> | date_part(COMPONENT, DATE) | date_part(&quot;MM&quot;, date(&quot;2019-10-17 11:55:12&quot;)) | 10 |
| set_date_part | 指定された日付のコンポーネントを置き換えます。次のコンポーネントが受け入れられます。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot; | <ul><li>コンポーネント： **必ず指定します** 。日付の一部を表す文字列です。 </li><li>値： **必須** ：指定した日付のコンポーネントに設定する値です。</li><li>日付： **必須** ：標準形式の日付です。</li></ul> | set_date_part(COMPONENT, VALUE, DATE) | set_date_part(&quot;m&quot;, 4, date(&quot;2016-11-09T11:44:44.797&quot;) | &quot;2016-04-09T11:44:44.797&quot; |
| make_date_time | 部分から日付を作成します。この関数は、make_timestampを使用して誘導することもできます。 | <ul><li>年： **必須** ：年を4桁で表します。</li><li>MONTH: **必須** ：月 指定できる値は1 ～ 12です。</li><li>DAY: **必須** ：日 指定できる値は1 ～ 31です。</li><li>HOUR: **必須** ：時間 指定できる値は0 ～ 23です。</li><li>分： **必須** ：分 指定できる値は0 ～ 59です。</li><li>ナノ秒： **必須** ：ナノ秒の値です。 指定できる値は0 ～ 99999999です。</li><li>TIMEZONE: **必須** ：日付時刻のタイムゾーンです。</li></ul> | make_date_time(YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, NANOSECOND, TIMEZONE) | make_date_time(2019, 10, 17, 11, 55, 12, 999, &quot;America/Los_Angeles&quot;) | `2019-10-17T11:55:12.0&#x200B;00000999-07:00[America/Los_Angeles]` |
| zone_date_to_utc | 任意のタイムゾーンの日付をUTCで表した日付に変換します。 | <ul><li>日付： **必須** ：変換しようとしている日付です。</li></ul> | zone_date_to_utc(DATE) | `zone_date_to_utc(2019-10-17T11:55:12.000000999-07:00[America/Los_Angeles])` | `2019-10-17T18:55:12.000000999Z[UTC]` |
| zone_date_to_zone | あるタイムゾーンの日付を別のタイムゾーンに変換します。 | <ul><li>日付： **必須** ：変換しようとしている日付です。</li><li>ゾーン： **必須** ：日付を変換しようとしているタイムゾーン。</li></ul> | zone_date_to_zone(DATE, ZONE) | `zone_date_to_utc(2019-10-17T11:55:12.000000999-07:00[America/Los_Angeles], "Europe/Paris")` | `2019-10-17T20:55:12.000000999+02:00[Europe/Paris]` |

### 階層 — オブジェクト

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| size_of | 入力のサイズを返します。 | <ul><li>入力： **必須** ：サイズを調べるオブジェクトです。</li></ul> | size_of(INPUT) | `size_of([1, 2, 3, 4])` | 4 |
| is_empty | オブジェクトが空かどうかをチェックします。 | <ul><li>入力： **必須** ：確認しようとしているオブジェクトが空です。</li></ul> | is_empty(INPUT) | `is_empty([1, 2, 3])` | false |
| arrays_to_object | オブジェクトのリストを作成します。 | <ul><li>入力： **必須** ：キーと配列のペアをグループ化します。</li></ul> | arrays_to_object(INPUT) | 必要なサンプル | 必要なサンプル |
| to_object | 指定されたフラットなキー/値のペアに基づいてオブジェクトを作成します。 | <ul><li>入力： **必須** ：キー/値のペアのフラットリスト。</li></ul> | to_object(INPUT) | to_object(&quot;firstName&quot;, &quot;John&quot;, &quot;lastName&quot;, &quot;Doe&quot;) | `{"firstName": "John", "lastName": "Doe"}` |
| str_to_object | 入力文字列からオブジェクトを作成します。 | <ul><li>文字列： **必須** ：オブジェクトを作成する際に解析する文字列です。</li><li>VALUE_DELIMITER: *オプション* ：フィールドと値を区切る区切り文字です。 The default delimiter is `:`.</li><li>FIELD_DELIMITER: *オプション* ：フィールド値のペアを区切る区切り文字です。 The default delimiter is `,`.</li></ul> | str_to_object(STRING, VALUE_DELIMITER, FIELD_DELIMITER) | str_to_object(&quot;firstName - John | lastName - | 電話 — 123 456 7890&quot;、&quot;-&quot;、&quot; | &quot;) | `{"firstName": "John", "lastName": "Doe", "phone": "123 456 7890"}` |
| is_set | オブジェクトがソースデータ内に存在するかどうかを確認します。 | <ul><li>入力： **必須** ：ソースデータ内に存在する場合に確認するパス。</li></ul> | is_set(INPUT) | is_set(&quot;evars.evar.field1&quot;) | true |

### 階層 — 配列

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| coalesce | 渡された配列の最初のnull以外のオブジェクトを返します。 | <ul><li>入力： **必須** :nullでない最初のオブジェクトを探す配列。</li></ul> | coalesce(INPUT) | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| first | 渡された配列の最初の要素を取得します。 | <ul><li>入力： **必須** ：の最初の要素を探す配列。</li></ul> | first(INPUT) | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | 渡された配列の最後の要素を取得します。 | <ul><li>入力： **必須** ：最後の要素を探す配列。</li></ul> | last(INPUT) | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| to_array | 入力のリストを取り、配列に変換します。 | <ul><li>INCLUDE_NULLS: **必須** ：応答配列にNULLを含めるかどうかを示すboolean値です。</li><li>値： **必須** ：配列に変換する要素。</li></ul> | to_array(INCLUDE_NULLS, VALUES) | to_array(false, 1, null, 2, 3) | `[1, 2, 3]` |

### 論理演算子

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| decode | キーと、キーと値のペアのリストが配列としてフラット化されている場合、この関数は、キーが見つかった場合は値を返し、デフォルト値が配列に存在する場合はデフォルト値を返します。 | <ul><li>キー： **必須** ：一致するキー。</li><li>OPTIONS: **必須** ：キー/値のペアのフラットな配列。 オプションで、デフォルト値を末尾に配置できます。</li></ul> | decode(KEY,OPTIONS) | decode(stateCode, &quot;ca&quot;, &quot;California&quot;, &quot;pa&quot;, &quot;Pennsylvania&quot;, &quot;N/A&quot;) | 渡されたstateCodeが「ca」、「California」の場合。<br>指定されたstateCodeが「pa」、「Pennsylvania」の場合。<br>stateCodeが次の値と一致しない場合は、「N/A」となります。 |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>BOOLEAN_式: **必須** ：評価対象のブール式です。</li><li>TRUE_VALUE: **必須** :式の値がtrueの場合に返す値です。</li><li>FALSE_VALUE: **必須** :式の評価結果がfalseの場合に返す値です。</li></ul> | iif(BOOLEAN_式, TRUE_VALUE, FALSE_VALUE) | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |

### 集計

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| min | 指定された引数の最小値を返します。自然な順序を使用します。 | <ul><li>OPTIONS: **必須** ：互いに比較できる1つ以上のオブジェクト。</li></ul> | min(OPTIONS) | min(3, 1, 4) | 1 |
| max | 指定された引数の最大値を返します。自然な順序を使用します。 | <ul><li>OPTIONS: **必須** ：互いに比較できる1つ以上のオブジェクト。</li></ul> | max(OPTIONS) | max(3, 1, 4) | 4 |

### 型変換

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| to_bigint | 文字列をBigIntegerに変換します。 | <ul><li>文字列： **必須** : BigIntegerに変換する文字列です。</li></ul> | to_bigint(STRING) | to_bigint(&quot;1000000.34&quot;) | 1000000.34 |
| to_decimal | 文字列を重複に変換します。 | <ul><li>文字列： **必須** :重複に変換する文字列です。</li></ul> | to_decimal(STRING) | to_decimal(&quot;20.5&quot;) | 20.5 |
| to_float | 文字列を浮動小数点数型(Float)に変換します。 | <ul><li>文字列： **必須** ：浮動小数点型(Float)に変換する文字列です。</li></ul> | to_float(STRING) | to_float(&quot;12.3456&quot;) | 12.34566 |
| to_integer | 文字列を整数型(Integer)に変換します。 | <ul><li>文字列： **必須** ：整数型(Integer)に変換する文字列です。</li></ul> | to_integer(STRING) | to_integer(&quot;12&quot;) | 12 |

### JSON関数

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| json_to_object | 渡された文字列からJSONコンテンツを逆シリアル化します。 | <ul><li>文字列： **必須** ：デシリアライズするJSON文字列。</li></ul> | json_to_object(STRING) | json_to_object({&quot;info&quot;:{&quot;firstName&quot;:&quot;John&quot;,&quot;lastName&quot;:&quot;Doe&quot;}}) | JSONを表すオブジェクト。 |

### 特別な操作

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| uuid /<br>guid | 擬似ランダム ID を生成します。 |  | uuid()<br>guid() | uuid()<br>guid() | 7c0267d2-bb74-4e1a-9275-3bf4fccda5f4<br>c7016dc7-3163-43f7-afc7-2e1c9c20633 |

### ユーザーエージェント機能

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| ua_os_name | ユーザーエージェント文字列からオペレーティングシステム名を抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_os_name(USER_AGENT) | ua_os_name(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS |
| ua_os_version_major | ユーザーエージェント文字列からオペレーティングシステムのメジャーバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_os_version_major(USER_AGENT) | ua_os_version_major(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5 |
| ua_os_version | ユーザーエージェント文字列からオペレーティングシステムのバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_os_version(USER_AGENT) | ua_os_version(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1.1 |
| ua_os_name_version | ユーザーエージェント文字列からオペレーティングシステムの名前とバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_os_name_version(USER_AGENT) | ua_os_name_version(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5.1.1 |
| ua_agent_version | ユーザーエージェント文字列からエージェントバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_agent_version(USER_AGENT) | ua_agent_version(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1 |
| ua_agent_version_major | ユーザーエージェント文字列からエージェント名とメジャーバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_agent_version_major(USER_AGENT) | ua_agent_version_major(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari 5 |
| ua_agent_name | ユーザーエージェント文字列からエージェント名を抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_agent_name(USER_AGENT) | ua_agent_name(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari |
| ua_device_class | ユーザーエージェント文字列からデバイスクラスを抽出します。 | <ul><li>USER_AGENT: **必須** ：ユーザーエージェント文字列。</li></ul> | ua_device_class(USER_AGENT) | ua_device_class(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Phone |
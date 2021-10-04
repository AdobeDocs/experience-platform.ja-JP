---
keywords: Experience Platform；ホーム；人気のあるトピック；CSV のマッピング；CSV ファイルの xdm へのマッピング；CSV の xdm へのマッピング；UI ガイド；マッピング；マッピング；マッピング；フィールド；マッピング関数；
solution: Experience Platform
title: データ準備マッピング関数
topic-legacy: overview
description: このドキュメントでは、データ準備で使用するマッピング関数を紹介します。
exl-id: e95d9329-9dac-4b54-b804-ab5744ea6289
source-git-commit: 1133580d6d4d8df352ab901d5106f0bb6c1f2a08
workflow-type: tm+mt
source-wordcount: '3933'
ht-degree: 18%

---

# データ準備マッピング関数

データ準備関数を使用して、ソースフィールドに入力された内容に基づいて値を計算できます。

## フィールド

フィールド名には、任意の正しい識別子を使用できます。長さに制限のない Unicode 文字と数字のシーケンスで、文字、ドル記号 (`$`)、アンダースコア文字 (`_`) で始まります。 変数名では大文字と小文字が区別されます。

フィールド名がこの規則に従わない場合、フィールド名は `${}` で囲む必要があります。 例えば、フィールド名が「First Name」または「First.Name」の場合、名前はそれぞれ `${First Name}` または `${First.Name}` のように囲む必要があります。

また、フィールド名が次の予約キーワードの **いずれか** の場合は、`${}` で囲む必要があります。

```console
new, mod, or, break, var, lt, for, false, while, eq, gt, div, not, null, continue, else, and, ne, true, le, if, ge, return
```

サブフィールド内のデータには、ドット表記を使用してアクセスできます。 例えば、`name` オブジェクトがある場合、`firstName` フィールドにアクセスするには、`name.firstName` を使用します。

## 関数のリスト

次の表に、サポートされるすべてのマッピング関数（サンプル式とその結果出力を含む）を示します。

### 文字列関数 {#string}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| concat | 指定された文字列を連結します。 | <ul><li>文字列：連結する文字列。</li></ul> | concat(STRING_1, STRING_2) | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| explode | 正規表現に基づいて文字列を分割し、部分の配列を返します。オプションで正規表現を含めて、文字列を分割できます。 デフォルトでは、分割は「,」に解決されます。 次の区切り文字 **は、`\` でエスケープする必要があります。**`+, ?, ^, |, ., [, (, {, ), *, $, \` 複数の文字を区切り文字として含める場合、区切り文字は複数文字の区切り文字として扱われます。 | <ul><li>文字列：**必須** 分割する文字列。</li><li>正規表現：*オプション* 文字列の分割に使用できる正規表現。</li></ul> | explode(STRING, REGEX) | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の場所/インデックスを返します。 | <ul><li>入力：**必須** 検索する文字列。</li><li>サブ文字列：**必須** 文字列内で検索されるサブ文字列。</li><li>開始位置：*オプション* 文字列内で検索を開始する場所。</li><li>発生件数：*オプション* 開始位置から探す n 番目の値。 デフォルトの重み付けは 1 です。 </li></ul> | instr(INPUT, SUBSTRING, START_POSITION, OCCURRENCE) | instr(&quot;adobe.com&quot;, &quot;com&quot;) | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合は、その文字列を置き換えます。 | <ul><li>入力：**必須** 入力文字列。</li><li>TO_FIND:**必須** 入力内で検索する文字列。</li><li>TO_REPLACE:**必須** 「TO_FIND」内の値を置き換える文字列。</li></ul> | replacestr(INPUT, TO_FIND, TO_REPLACE) | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 指定された長さのサブ文字列を返します。 | <ul><li>入力：**必須** 入力文字列。</li><li>START_INDEX:**必須** 部分文字列が始まる入力文字列のインデックス。</li><li>長さ：**必須** サブ文字列の長さ。</li></ul> | substr(INPUT, START_INDEX, LENGTH) | substr(&quot;This is a substring test&quot;, 7, 8) | &quot;サブセット&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | <ul><li>入力：**必須** 小文字に変換される文字列。</li></ul> | lower(INPUT) | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | <ul><li>入力：**必須** 大文字に変換する文字列。</li></ul> | upper(INPUT) | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り記号で入力文字列を分割します。次の区切り文字 **は、`\` でエスケープする必要があります。**`\`. 複数の区切り文字を含めると、文字列内に存在する区切り文字の **** に対して文字列が分割されます。 | <ul><li>入力：**必須** 分割する入力文字列。</li><li>区切り文字：**必須** 入力を分割するために使用する文字列。</li></ul> | split(INPUT, SEPARATOR) | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | 区切り記号を使用してオブジェクトのリストを結合します。 | <ul><li>区切り文字：**必須** オブジェクトの結合に使用する文字列。</li><li>オブジェクト：**必須** 結合される文字列の配列。</li></ul> | `join(SEPARATOR, [OBJECTS])` | `join(" ", to_array(true, "Hello", "world"))` | &quot;Hello world&quot; |
| lpad | 文字列の左側を他の指定した文字列で埋めます。 | <ul><li>入力：**必須** パッドアウトする文字列。 この文字列は null にできます。</li><li>カウント：**必須** パッドアウトする文字列のサイズ。</li><li>パディング：**必須** 入力を埋め込む文字列。 null または空の場合は、1 つのスペースとして扱われます。</li></ul> | lpad(INPUT, COUNT, PADDING) | lpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;yzybat&quot; |
| rpad | 文字列の右側を他の指定した文字列で埋めます。 | <ul><li>入力：**必須** パッドアウトする文字列。 この文字列は null にできます。</li><li>カウント：**必須** パッドアウトする文字列のサイズ。</li><li>パディング：**必須** 入力を埋め込む文字列。 null または空の場合は、1 つのスペースとして扱われます。</li></ul> | rpad(INPUT, COUNT, PADDING) | rpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;batyzy&quot; |
| left | 指定された文字列の最初の「n」文字を取得します。 | <ul><li>文字列：**必須** 最初の「n」文字を取得する文字列。</li><li>カウント：**必須** 文字列から取得する&quot;n&quot;文字。</li></ul> | left(STRING, COUNT) | left(&quot;abcde&quot;, 2) | &quot;ab&quot; |
| right | 指定された文字列の最後の「n」文字を取得します。 | <ul><li>文字列：**必須** 最後の「n」文字を取得する文字列。</li><li>カウント：**必須** 文字列から取得する&quot;n&quot;文字。</li></ul> | right(STRING, COUNT) | right(&quot;abcde&quot;, 2) | &quot;de&quot; |
| ltrim | 文字列の先頭から空白を削除します。 | <ul><li>文字列：**必須** 空白を削除する文字列。</li></ul> | ltrim(STRING) | ltrim(&quot;hello&quot;) | &quot;hello&quot; |
| rtrim | 文字列の末尾から空白を削除します。 | <ul><li>文字列：**必須** 空白を削除する文字列。</li></ul> | rtrim(STRING) | rtrim(&quot;hello &quot;) | &quot;hello&quot; |
| trim | 文字列の先頭と末尾から空白を削除します。 | <ul><li>文字列：**必須** 空白を削除する文字列。</li></ul> | trim(STRING) | trim(&quot; hello &quot;) | &quot;hello&quot; |
| 次と等しい | 2 つの文字列を比較して、等しいかどうかを確認します。 この関数では大文字と小文字が区別されます。 | <ul><li>文字列 1:**必須** 比較する最初の文字列。</li><li>STRING2:**必須** 比較する 2 番目の文字列。</li></ul> | STRING1&#x200B;.equals( &#x200B; STRING2) | &quot;string1&quot;&#x200B;。equals&#x200B;(&quot;STRING1&quot;) | false |
| equalSignoreCase | 2 つの文字列を比較して、等しいかどうかを確認します。 この関数は **大文字と小文字を区別しません。** | <ul><li>文字列 1:**必須** 比較する最初の文字列。</li><li>STRING2:**必須** 比較する 2 番目の文字列。</li></ul> | STRING1&#x200B;.equalsIgnoreCase(STRING2&#x200B;) | &quot;string1&quot;&#x200B;。equalsIgnoreCase(&#x200B;&quot;STRING1) | true |

{style=&quot;table-layout:auto&quot;}

### 正規表現関数

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| extract_regex | 正規表現に基づいて、入力文字列からグループを抽出します。 | <ul><li>文字列：**必須** グループの抽出元の文字列。</li><li>正規表現：**必須** グループを一致させる正規表現。</li></ul> | extract_regex(STRING, REGEX) | extract_regex&#x200B;(&quot;E259,E259B_009,1_1&quot;, &#x200B;&quot;([^,]+),[^,]*,([^,]+)&quot;) | [&quot;E259,E259B_009,1_1&quot;, &quot;E259&quot;, &quot;1_1&quot;] |
| matches_regex | 文字列が入力された正規表現と一致するかどうかを確認します。 | <ul><li>文字列：**必須** チェックする文字列は、正規表現と一致します。</li><li>正規表現：**必須** 比較する正規表現。</li></ul> | matches_regex(STRING, REGEX) | matches_regex(&quot;E259,E259B_009,1_1&quot;, &quot;([^,]+),[^,]*,([^,]+)&quot;) | true |

{style=&quot;table-layout:auto&quot;}

### ハッシュ関数 {#hashing}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| sha1 | 入力を受け取り、Secure Hash Algorithm 1(SHA-1) を使用してハッシュ値を生成します。 | <ul><li>入力：**必須** ハッシュ化するプレーンテキスト。</li><li>文字セット：*オプション* 文字セットの名前。 UTF-8、UTF-16、ISO-8859-1、US-ASCII などの値が使用できます。</li></ul> | sha1(INPUT, CHARSET) | sha1(&quot;my text&quot;, &quot;UTF-8&quot;) | c3599c11e47719df18a24 &#x200B; 48690840c5dfcce3c80 |
| sha256 | 入力を受け取り、Secure Hash Algorithm 256(SHA-256) を使用してハッシュ値を生成します。 | <ul><li>入力：**必須** ハッシュ化するプレーンテキスト。</li><li>文字セット：*オプション* 文字セットの名前。 UTF-8、UTF-16、ISO-8859-1、US-ASCII などの値が使用できます。</li></ul> | sha256(INPUT, CHARSET) | sha256(&quot;my text&quot;, &quot;UTF-8&quot;) | 7330d2b39ca35eaf4cb95fc846c21&#x200B;ee6a39af698154a83a586ee270a0d372104 |
| sha512 | 入力を受け取り、Secure Hash Algorithm 512(SHA-512) を使用してハッシュ値を生成します。 | <ul><li>入力：**必須** ハッシュ化するプレーンテキスト。</li><li>文字セット：*オプション* 文字セットの名前。 UTF-8、UTF-16、ISO-8859-1、US-ASCII などの値が使用できます。</li></ul> | sha512(INPUT, CHARSET) | sha512(&quot;my text&quot;, &quot;UTF-8&quot;) | a3d7e45a0d9be5fd4e4b9a3b8c9c2163c21&#x200B;ef 708bf11b4232bb21d2a8704ada2cdcd7b367dd078a89 &#x200B;a5c908cfe377aceb1072a7b386b7d4fd2ff68a8fd24d16 |
| md5 | 入力を受け取り、MD5 を使用してハッシュ値を生成します。 | <ul><li>入力：**必須** ハッシュ化するプレーンテキスト。</li><li>文字セット：*オプション* 文字セットの名前。 UTF-8、UTF-16、ISO-8859-1、US-ASCII などの値が使用できます。 </li></ul> | md5(INPUT, CHARSET) | md5(&quot;my text&quot;, &quot;UTF-8&quot;) | d3b96ce8c9fb4 &#x200B;e9bd0198d03ba6852c7 |
| crc32 | 入力では、Cyclic Redundancy Check（CRC；巡回冗長検査）アルゴリズムを使用して 32 ビットの巡回コードを生成します。 | <ul><li>入力：**必須** ハッシュ化するプレーンテキスト。</li><li>文字セット：*オプション* 文字セットの名前。 UTF-8、UTF-16、ISO-8859-1、US-ASCII などの値が使用できます。</li></ul> | crc32(INPUT, CHARSET) | crc32(&quot;my text&quot;, &quot;UTF-8&quot;) | 8df92e80 |

{style=&quot;table-layout:auto&quot;}

### URL 関数 {#url}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| get_url_protocol | 指定された URL からプロトコルを返します。 入力が無効な場合は null を返します。 | <ul><li>URL:**必須** プロトコルの抽出元の URL。</li></ul> | get_url_protocol(&#x200B;URL) | get_url_protocol(&quot;https://platform &#x200B; .adobe.com/home&quot;) | https |
| get_url_host | 指定された URL のホストを返します。 入力が無効な場合は null を返します。 | <ul><li>URL:**必須** ホストの抽出元の URL。</li></ul> | get_url_host(&#x200B;URL) | get_url_host(&quot;&#x200B;https://platform .adobe.com/home&#x200B;&quot;) | platform.adobe.com |
| get_url_port | 指定された URL のポートを返します。 入力が無効な場合は null を返します。 | <ul><li>URL:**必須** ポートの抽出元の URL。</li></ul> | get_url_port(URL) | get_url_port(&quot;&#x200B;sftp://example.com//home/ joe/employee.csv&#x200B;&quot;) | 22 |
| get_url_path | 指定された URL のパスを返します。 デフォルトでは、完全なパスが返されます。 | <ul><li>URL:**必須** パスの抽出元の URL。</li><li>FULL_PATH:*オプション* フルパスを返すかどうかを指定する boolean 値。 false に設定した場合は、パスの末尾のみが返されます。</li></ul> | get_url_path(&#x200B;URL, FULL_PATH) | get_url_path(&quot;&#x200B;sftp://example.com// home/joe/employee.csv&#x200B;&quot;) | &quot;//home/joe/ employee.&#x200B;csv&quot; |
| get_url_query_str | 指定された URL のクエリ文字列を返します。 | <ul><li>URL:**必須** クエリー文字列の取得元の URL。</li><li>アンカー：**必須** クエリ文字列内のアンカーの処理を決定します。 次の 3 つの値のいずれかを指定できます。&quot;retain&quot;、&quot;remove&quot;、または&quot;append&quot;<br><br>値が「retain」の場合、返される値にアンカーが付加されます。<br>値が「remove」の場合、返される値からアンカーが削除されます。<br>値が「append」の場合、アンカーは別の値として返されます。</li></ul> | get_url_query_str(&#x200B;URL, ANCHOR) | get_url_query_&#x200B;(&quot;foo://example.com:8042/over/there?name&#x200B;=ferret#nose&quot;, &quot;retain&quot;)<br>get_url_query_&#x200B;str(&quot;foo://example.com:8042?name/str/str/foo://example.com?name/str/get/nose、>get_rel>。#nose&quot;, &quot;append&quot;)<br> | `{"name": "ferret#nose"}`<br>`{"name": "ferret"}`<br>`{"name": "ferret", "_anchor_": "nose"}` |

{style=&quot;table-layout:auto&quot;}

### 日付および時間関数 {#date-and-time}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。`date` 関数の詳細については、[ データフォーマット処理ガイド ](./data-handling.md#dates) の dates の節を参照してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| now | 現在の時刻を取得します。 |  | now() | now() | `2020-09-23T10:10:24.556-07:00[America/Los_Angeles]` |
| timestamp | 現在の Unix 時間を取得します。 |  | timestamp() | timestamp() | 1571850624571 |
| format | 指定された形式に従って入力日をフォーマットします。 | <ul><li>日付：**必須** 書式設定する ZonedDateTime オブジェクトとしての入力日。</li><li>形式：**必須** 日付の変更先の形式。</li></ul> | format(DATE, FORMAT) | format(2019-10-23T11:24:00+00:00, &quot;yyyy-MM-dd HH:mm:ss&quot;) | &quot;2019-10-23 11:24:35&quot; |
| dformat | 指定された形式に従ってタイムスタンプを日付文字列に変換します。 | <ul><li>タイムスタンプ：**必須** 形式を設定するタイムスタンプ。 これはミリ秒単位で記述されます。</li><li>形式：**必須** タイムスタンプの変更先の形式。</li></ul> | dformat(&#x200B;TIMESTAMP, FORMAT) | dformat(1571829875000, &quot;yyyy-MM-dd&#39;T&#39;HH:mm:ss.SSX&quot;) | &quot;2019-10-23T11:24:35.000Z&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付：**必須** 日付を表す文字列。</li><li>形式：**必須** 日付の形式を表す文字列。</li><li>DEFAULT_DATE:**必須** 指定した日付が null の場合は、デフォルトの日付が返されます。</li></ul> | date(DATE, FORMAT, DEFAULT_DATE) | date(&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;, now()) | &quot;2019-10-23T11:24Z&quot; |
| 日付 | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付：**必須** 日付を表す文字列。</li><li>形式：**必須** 日付の形式を表す文字列。</li></ul> | date(DATE, FORMAT) | date(&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;) | &quot;2019-10-23T11:24Z&quot; |
| 日付 | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付：**必須** 日付を表す文字列。</li></ul> | date(DATE) | date(&quot;2019-10-23 11:24&quot;) | &quot;2019-10-23T11:24Z&quot; |
| date_part | 日付の一部を取得します。次のコンポーネント値がサポートされています。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;quarter&quot;<br>&quot;qq&quot;<br>&quot;q&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;dayofyear&quot;<br>&quot;dy&quot;<br>&quot;y&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;week&quot;<br>&quot;ww&quot;<br>&quot;w&quot;<br><br>&quot;weekday&quot;<br>&quot;dw&quot;<br>&quot;w&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br>&quot;hh24&quot;<br>&quot;hh12&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot;<br><br>&quot;millisecond&quot;<br>&quot;ms&quot; | <ul><li>コンポーネント：**必須** 日付の一部を表す文字列。 </li><li>日付：**必須** 標準形式の日付。</li></ul> | date_part(&#x200B;COMPONENT, DATE) | date_part(&quot;MM&quot;, date(&quot;2019-10-17 11:55:12&quot;)) | 10 |
| set_date_part | 指定された日付のコンポーネントを置き換えます。次のコンポーネントが受け入れられます。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot; | <ul><li>コンポーネント：**必須** 日付の一部を表す文字列。 </li><li>値：**必須** 指定した日付のコンポーネントに設定する値。</li><li>日付：**必須** 標準形式の日付。</li></ul> | set_date_part(&#x200B;COMPONENT, VALUE, DATE) | set_date_part(&quot;m&quot;, 4, date(&quot;2016-11-09T11:44:44.797&quot;) | &quot;2016-04-09T11:44:44.797&quot; |
| make_date_time | 部分から日付を作成します。この関数は、 make_timestamp を使用して誘導することもできます。 | <ul><li>年：**必須** 4 桁で書かれた年。</li><li>月：**必須** 月。 使用できる値は 1 ～ 12 です。</li><li>日：**必須** 日。 指定できる値は 1 ～ 31 です。</li><li>時間：**必須** 時間。 指定できる値は 0 ～ 23 です。</li><li>分：**必須** 分。 指定できる値は 0 ～ 59 です。</li><li>ナノ秒：**必須** ナノ秒値。 指定できる値は 0 ～ 999999999です。</li><li>タイムゾーン：**必須** 日時のタイムゾーン。</li></ul> | make_date_time(&#x200B;YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, NANOSECOND, TIMEZONE) | make_date_time(2019, 10, 17, 11, 55, 12, 999, &quot;America/Los_Angeles&quot;) | `2019-10-17T11:55:12.0&#x200B;00000999-07:00[America/Los_Angeles]` |
| zone_date_to_utc | 任意のタイムゾーンの日付を UTC での日付に変換します。 | <ul><li>日付：**必須** 変換しようとしている日付。</li></ul> | zone_date_to_utc(DATE&#x200B;) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:&#x200B;12.000000999-&#x200B;07:00[America/Los_Angeles])` | `2019-10-17T18:55:12.000000999Z[UTC]` |
| zone_date_to_zone | 日付をタイムゾーンから別のタイムゾーンに変換します。 | <ul><li>日付：**必須** 変換しようとしている日付。</li><li>ゾーン：**必須** 日付を変換しようとしているタイムゾーン。</li></ul> | zone_date_to_zone(&#x200B;DATE, ZONE) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:12&#x200B;.000000999-07:00&#x200B;[America/Los_Angeles], "Europe/Paris")` | `2019-10-17T20:55:12.000000999+02:00[Europe/Paris]` |

{style=&quot;table-layout:auto&quot;}
&#x200B;

### 階層 — オブジェクト {#objects}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| size_of | 入力のサイズを返します。 | <ul><li>入力：**必須** サイズを探すオブジェクト。</li></ul> | size_of(INPUT) | `size_of([1, 2, 3, 4])` | 4 |
| is_empty | オブジェクトが空かどうかを確認します。 | <ul><li>入力：**必須** 確認しようとしているオブジェクトが空です。</li></ul> | is_empty(INPUT) | `is_empty([1, 2, 3])` | false |
| arrays_to_object | オブジェクトのリストを作成します。 | <ul><li>入力：**必須** キーと配列のペアのグループ。</li></ul> | arrays_to_object(INPUT) | 必要なサンプル | 必要なサンプル |
| to_object | 指定されたフラットなキーと値のペアに基づいてオブジェクトを作成します。 | <ul><li>入力：**必須** キーと値のペアのフラットなリスト。</li></ul> | to_object(INPUT) | to_&#x200B;object(&quot;firstName&quot;, &quot;John&quot;, &quot;lastName&quot;, &quot;Doe&quot;) | `{"firstName": "John", "lastName": "Doe"}` |
| str_to_object | 入力文字列からオブジェクトを作成します。 | <ul><li>文字列：**必須** オブジェクトを作成するために解析する文字列。</li><li>値の区切り：*オプション* フィールドと値を区切る区切り文字。 デフォルトの区切り文字は `:` です。</li><li>FIELD_DELIMITER:*オプション* フィールド値のペアを区切る区切り文字。 デフォルトの区切り文字は `,` です。</li></ul> | str_to_object(&#x200B;STRING, VALUE_DELIMITER, FIELD_DELIMITER) | str_to_object(&quot;firstName - John | lastName - | phone - 123 456 7890&quot;, &quot;-&quot;, &quot; | &quot;) | `{"firstName": "John", "lastName": "Doe", "phone": "123 456 7890"}` |
| is_set | オブジェクトがソースデータ内に存在するかどうかを確認します。 | <ul><li>入力：**必須** ソースデータ内に存在する場合に確認するパス。</li></ul> | is_set(INPUT) | is_set&#x200B;(&quot;evars.evar.field1&quot;) | true |
| 無効にする | 属性の値を `null` に設定します。 これは、フィールドをターゲットスキーマにコピーしない場合に使用します。 |  | nullify() | nullify() | `null` |
| get_keys | キーと値のペアを解析し、すべてのキーを返します。 | <ul><li>オブジェクト：**必須** キーの抽出元のオブジェクト。</li></ul> | get_keys(OBJECT) | get_keys({&quot;book1&quot;):&quot;Pride and Paironds&quot;, &quot;book2&quot;:&quot;1984&quot;}) | `["book1", "book2"]` |
| get_values | キーと値のペアを解析し、指定されたキーに基づいて文字列の値を返します。 | <ul><li>文字列：**必須** 解析する文字列。</li><li>キー：**必須** 値の抽出対象となるキー。</li><li>値の区切り：**必須** フィールドと値を区切る区切り文字。 `null` または空の文字列が指定された場合、この値は `:` です。</li><li>FIELD_DELIMITER:*オプション* フィールドと値のペアを区切る区切り文字。 `null` または空の文字列が指定された場合、この値は `,` です。</li></ul> | get_values(STRING, KEY, VALUE_DELIMITER, FIELD_DELIMITER) | get_values(\&quot;firstName - John , lastName - Cena , phone - 555 420 8692\&quot;, \&quot;firstName\&quot;, \&quot;-\&quot;, \&quot;,\&quot;) | John |

{style=&quot;table-layout:auto&quot;}

### 階層 — 配列 {#arrays}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| coalesce | 渡された配列の最初の null 以外のオブジェクトを返します。 | <ul><li>入力：**必須** 最初の null 以外のオブジェクトを検索する配列。</li></ul> | coalesce(INPUT) | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| first | 指定された配列の最初の要素を取得します。 | <ul><li>入力：**必須** 最初の要素を探す配列。</li></ul> | first(INPUT) | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | 指定された配列の最後の要素を取得します。 | <ul><li>入力：**必須** 最後の要素を探す配列。</li></ul> | last(INPUT) | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| add_to_array | 配列の末尾に要素を追加します。 | <ul><li>配列：**必須** 要素を追加する配列。</li><li>値：配列に追加する要素。</li></ul> | add_to_array(ARRAY, VALUES&#x200B;) | add_to_array(&#x200B;[&#39;a&#39;, &#39;b&#39;], &#39;c&#39;, &#39;d&#39;) | [&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;] |
| join_arrays | 配列を相互に結合します。 | <ul><li>配列：**必須** 要素を追加する配列。</li><li>値：親配列に追加する配列。</li></ul> | join_arrays(&#x200B;ARRAY, VALUES) | join_arrays&#x200B;([&#39;a&#39;, &#39;b&#39;], [&#39;c&#39;], [&#39;d&#39;, &#39;e&#39;]) | [&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;, &#39;e&#39;] |
| to_array | 入力のリストを取得し、配列に変換します。 | <ul><li>INCLUDE_NULLS:**必須** 応答配列に NULL を含めるかどうかを示す boolean 値。</li><li>値：**必須** 配列に変換する要素。</li></ul> | to_array(&#x200B;INCLUDE_NULLS, VALUES) | to_array(false, 1, null, 2, 3) | `[1, 2, 3]` |

{style=&quot;table-layout:auto&quot;}

### 論理演算子 {#logical-operators}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| decode | キーと、キーと値のペアのリストが配列としてフラット化されている場合、この関数は、キーが見つかった場合は値を返し、デフォルト値が配列に存在する場合はデフォルト値を返します。 | <ul><li>キー：**必須** 一致するキー。</li><li>OPTIONS:**必須** キーと値のペアのフラット化された配列。 オプションで、デフォルト値を最後に付けることができます。</li></ul> | decode(KEY,OPTIONS) | decode(stateCode, &quot;ca&quot;, &quot;California&quot;, &quot;pa&quot;, &quot;Pennsylvania&quot;, &quot;N/A&quot;) | 指定された stateCode が「ca」、「California」の場合。<br>指定された stateCode が「pa」、「Pennsylvania」の場合。<br>stateCode が次の値と一致しない場合は、「該当なし」。 |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>式：**必須** 評価されるブール式。</li><li>TRUE_VALUE:**必須** 式の評価結果が true の場合に返される値。</li><li>FALSE_VALUE:**必須** 式の評価結果が false の場合に返される値。</li></ul> | iif(EXPRESSION, TRUE_VALUE, FALSE_VALUE) | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |

{style=&quot;table-layout:auto&quot;}

### 集計 {#aggregation}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| min | 指定された引数の最小値を返します。自然な順序を使用します。 | <ul><li>OPTIONS:**必須** 互いに比較できる 1 つ以上のオブジェクト。</li></ul> | min(OPTIONS) | min(3, 1, 4) | 1 |
| max | 指定された引数の最大値を返します。自然な順序を使用します。 | <ul><li>OPTIONS:**必須** 互いに比較できる 1 つ以上のオブジェクト。</li></ul> | max(OPTIONS) | max(3, 1, 4) | 4 |

{style=&quot;table-layout:auto&quot;}

### 型変換 {#type-conversions}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| to_bigint | 文字列を BigInteger に変換します。 | <ul><li>文字列：**必須** BigInteger に変換する文字列。</li></ul> | to_bigint(STRING) | to_bigint(&#x200B;&quot;1000000.34&quot;) | 1000000.34 |
| to_decimal | 文字列を倍精度浮動小数点型 (Double) に変換します。 | <ul><li>文字列：**必須** 倍精度浮動小数点型 (Double) に変換する文字列。</li></ul> | to_decimal(STRING) | to_decimal(&quot;20.5&quot;) | 20.5 |
| to_float | 文字列を浮動小数に変換します。 | <ul><li>文字列：**必須** 浮動小数点型 (Float) に変換する文字列。</li></ul> | to_float(STRING) | to_float(&quot;12.3456&quot;) | 12.34566 |
| to_integer | 文字列を整数に変換します。 | <ul><li>文字列：**必須** 整数に変換する文字列。</li></ul> | to_integer(STRING) | to_integer(&quot;12&quot;) | 12 |

{style=&quot;table-layout:auto&quot;}

### JSON 関数 {#json}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| json_to_object | 指定された文字列から JSON コンテンツを逆シリアル化します。 | <ul><li>文字列：**必須** デシリアライズする JSON 文字列。</li></ul> | json_to_object(&#x200B;STRING) | json_to_object&#x200B;({&quot;info&quot;:{&quot;firstName&quot;:&quot;John&quot;,&quot;lastName&quot; :&quot;Doe&quot;}}) | JSON を表すオブジェクト。 |

{style=&quot;table-layout:auto&quot;}

### 特別な操作 {#special-operations}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| uuid /<br>guid | 擬似ランダム ID を生成します。 |  | uuid()<br>guid() | uuid()<br>guid() | 7c0267d2-bb74-4e1a-9275-3bf4fccda5f4<br>c7016dc7-3163-43f7-afc7-2e1c9c206333 |

{style=&quot;table-layout:auto&quot;}

### ユーザーエージェントの関数 {#user-agent}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| ua_os_name | ユーザーエージェント文字列からオペレーティングシステム名を抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_os_name(&#x200B;USER_AGENT) | ua_os_name(&quot;&#x200B;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS |
| ua_os_version_major | ユーザーエージェント文字列からオペレーティングシステムのメジャーバージョンを抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_os_version_major(&#x200B;USER_AGENT) | ua_os_version_major &#x200B; s(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5 |
| ua_os_version | ユーザーエージェント文字列からオペレーティングシステムのバージョンを抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_os_version(&#x200B;USER_AGENT) | ua_os_version&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1.1 |
| ua_os_name_version | ユーザーエージェント文字列からオペレーティングシステムの名前とバージョンを抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_os_name_version(&#x200B;USER_AGENT) | ua_os_name_version&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5.1.1 |
| ua_agent_version | ユーザーエージェント文字列からエージェントのバージョンを抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_agent_version(&#x200B;USER_AGENT) | ua_agent_version&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1 |
| ua_agent_version_major | ユーザーエージェント文字列からエージェント名とメジャーバージョンを抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_agent_version_major(&#x200B;USER_AGENT) | ua_agent_version_major(&quot;&#x200B;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari 5 |
| ua_agent_name | ユーザーエージェント文字列からエージェント名を抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_agent_name(&#x200B;USER_AGENT) | ua_agent_name(&quot;&#x200B;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari |
| ua_device_class | ユーザーエージェント文字列からデバイスクラスを抽出します。 | <ul><li>USER_AGENT:**必須** ユーザーエージェント文字列。</li></ul> | ua_device_class(&#x200B;USER_AGENT) | ua_device_class&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS X など）AppleWebKit/534.46（Gecko など KHTML）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Phone |

{style=&quot;table-layout:auto&quot;}

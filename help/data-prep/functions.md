---
keywords: Experience Platform；ホーム；人気のあるトピック；CSVのマップ；CSVファイルのマップ；CSVファイルのxdmへのマップ；CSVのxdmへのマップ；ui guide；マッピング；マッピング；フィールドのマッピング；マッピング関数；
solution: Experience Platform
title: データ準備マッピング関数
topic: overview
description: このドキュメントでは、データ準備で使用するマッピング関数を紹介します。
exl-id: e95d9329-9dac-4b54-b804-ab5744ea6289
translation-type: tm+mt
source-git-commit: c3939d4ce30bf12748b898f461a166f28f010abf
workflow-type: tm+mt
source-wordcount: '3798'
ht-degree: 18%

---

# データ準備マッピング関数

データ準備関数を使用して、ソースフィールドに入力した値に基づいて値を計算および計算できます。

## フィールド

フィールド名には、任意の正当な識別子を使用できます。Unicode文字と数字の無制限のシーケンス（文字で始まる）、ドル記号(`$`)、アンダースコア文字(`_`)を使用できます。 変数名では大文字と小文字が区別されます。

フィールド名がこの規則に従わない場合は、フィールド名を`${}`で囲む必要があります。 したがって、例えば、フィールド名が「First Name」または「First.Name」の場合、名前はそれぞれ`${First Name}`または`${First.Name}`のように囲む必要があります。

また、フィールド名が次の予約キーワードの&#x200B;**いずれか**&#x200B;の場合は、`${}`で囲む必要があります。

```console
new, mod, or, break, var, lt, for, false, while, eq, gt, div, not, null, continue, else, and, ne, true, le, if, ge, return
```

サブフィールド内のデータは、ドット表記を使用してアクセスできます。 例えば、`name`オブジェクトがある場合、`firstName`フィールドにアクセスするには、`name.firstName`を使用します。

## 関数のリスト

次の表に、サンプル式とその結果生成される出力を含む、サポートされるすべてのマッピング関数を示します。

### 文字列関数 {#string}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| concat | 渡された文字列を連結します。 | <ul><li>文字列：連結する文字列です。</li></ul> | concat(STRING_1, STRING_2) | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| explode | 正規表現に基づいて文字列を分割し、部分の配列を返します。文字列を分割するregexをオプションで含めることができます。 デフォルトでは、分割は「,」に解決されます。 次の区切り文字&#x200B;**は、`\`でエスケープする必要があります。**`+, ?, ^, |, ., [, (, {, ), *, $, \`複数の文字を区切り文字として含める場合、区切り文字は複数文字の区切り文字として扱われます。 | <ul><li>文字列：**必須**&#x200B;分割する必要のある文字列。</li><li>REGEX:*オプション*&#x200B;文字列の分割に使用できる正規式。</li></ul> | explode(STRING, REGEX) | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の場所/インデックスを返します。 | <ul><li>入力：**必須**&#x200B;検索する文字列。</li><li>SUBSTRING:**必須**&#x200B;文字列内で検索されるサブ文字列。</li><li>開始位置：*オプション*&#x200B;文字列内の開始の位置。</li><li>オカレンス：*オプション*&#x200B;開始位置から探すn番目の値。 デフォルトの重み付けは 1 です。 </li></ul> | instr(INPUT, SUBSTRING,開始_位置，オカレンス) | instr(&quot;adobe.com&quot;, &quot;com&quot;) | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合は、その文字列を置き換えます。 | <ul><li>入力：**必須**&#x200B;入力文字列。</li><li>TO_FIND:**必須**&#x200B;入力内で検索する文字列。</li><li>TO_REPLACE:**必須** 「TO_FIND」内の値を置き換える文字列。</li></ul> | replacestr(INPUT, TO_FIND, TO_REPLACE) | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 指定された長さのサブ文字列を返します。 | <ul><li>入力：**必須**&#x200B;入力文字列。</li><li>開始インデックス：**必須**&#x200B;サブ文字列開始ーが含まれる入力文字列のインデックス。</li><li>長さ：**必須**&#x200B;サブ文字列の長さ。</li></ul> | substr(INPUT,開始_INDEX, LENGTH) | substr(&quot;This is a substring test&quot;, 7, 8) | &quot;サブセット&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | <ul><li>入力：**必須**&#x200B;小文字に変換される文字列。</li></ul> | lower(INPUT) | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | <ul><li>入力：**必須**&#x200B;大文字に変換される文字列。</li></ul> | upper(INPUT) | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り記号で入力文字列を分割します。次の区切り文字&#x200B;**は`\`でエスケープする必要があります：**`\`。 複数の区切り文字を含める場合、文字列は文字列内に存在する区切り文字の&#x200B;****&#x200B;に分割されます。 | <ul><li>入力：**必須**&#x200B;分割する入力文字列。</li><li>区切り文字：**必須**&#x200B;入力を分割するために使用する文字列。</li></ul> | split(INPUT, SEPARATOR) | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | 区切り記号を使用してオブジェクトのリストを結合します。 | <ul><li>区切り文字：**必須**&#x200B;オブジェクトの結合に使用する文字列。</li><li>オブジェクト：**必須**&#x200B;結合される文字列の配列。</li></ul> | `join(SEPARATOR, [OBJECTS])` | `join(" ", to_array(true, "Hello", "world"))` | &quot;Hello world&quot; |
| lpad | 文字列の左側を、もう1つの指定した文字列で埋めます。 | <ul><li>入力：**必須**&#x200B;パドアウトする文字列。 この文字列はnullにできます。</li><li>カウント：**必須**&#x200B;埋め込む文字列のサイズ。</li><li>パディング：**必須**&#x200B;入力を埋め込む文字列。 nullまたは空の場合は、1つのスペースとして扱われます。</li></ul> | lpad(INPUT, COUNT, PADDING) | lpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;yzybat&quot; |
| rpad | 文字列の右側をもう1つの指定された文字列で埋めます。 | <ul><li>入力：**必須**&#x200B;パドアウトする文字列。 この文字列はnullにできます。</li><li>カウント：**必須**&#x200B;埋め込む文字列のサイズ。</li><li>パディング：**必須**&#x200B;入力を埋め込む文字列。 nullまたは空の場合は、1つのスペースとして扱われます。</li></ul> | rpad(INPUT, COUNT, PADDING) | rpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;batyzyzy&quot; |
| left | 渡された文字列の最初の&quot;n&quot;文字を取得します。 | <ul><li>文字列：**必須**&#x200B;最初の&quot;n&quot;文字を取得する文字列。</li><li>カウント：**必須**&#x200B;文字列から取得する&quot;n&quot;文字。</li></ul> | left(STRING, COUNT) | left(&quot;abcde&quot;, 2) | &quot;ab&quot; |
| right | 渡された文字列の最後の&quot;n&quot;文字を取得します。 | <ul><li>文字列：**必須**&#x200B;最後のn文字を取得する文字列。</li><li>カウント：**必須**&#x200B;文字列から取得する&quot;n&quot;文字。</li></ul> | right(STRING, COUNT) | right(&quot;abcde&quot;, 2) | &quot;de” |
| ltrim | 文字列の先頭から空白を削除します。 | <ul><li>文字列：**必須**&#x200B;空白文字を削除する文字列。</li></ul> | ltrim(STRING) | ltrim(&quot; hello&quot;) | &quot;hello&quot; |
| rtrim | 文字列の末尾の空白を削除します。 | <ul><li>文字列：**必須**&#x200B;空白文字を削除する文字列。</li></ul> | rtrim(STRING) | rtrim(&quot;hello &quot;) | &quot;hello&quot; |
| trim | 文字列の先頭と末尾の空白を削除します。 | <ul><li>文字列：**必須**&#x200B;空白文字を削除する文字列。</li></ul> | trim(STRING) | trim(&quot; hello &quot;) | &quot;hello&quot; |
| 次と等しい | 2つの文字列を比較して、等しいかどうかを確認します。 この関数では大文字と小文字が区別されます。 | <ul><li>STRING1:**必須**&#x200B;比較する最初の文字列。</li><li>STRING2:**必須**&#x200B;比較する2番目の文字列。</li></ul> | STRING1&#x200B;.equals( &#x200B; STRING2) | &quot;string1&quot;&#x200B;に置き換えます。equals&#x200B;(&quot;STRING1&quot;) | false |
| equalSignoreCase | 2つの文字列を比較して、等しいかどうかを確認します。 この関数では、****&#x200B;では大文字と小文字が区別されません。 | <ul><li>STRING1:**必須**&#x200B;比較する最初の文字列。</li><li>STRING2:**必須**&#x200B;比較する2番目の文字列。</li></ul> | STRING1&#x200B;.equalsIgnoreCase&#x200B;(STRING2) | &quot;string1&quot;&#x200B;に置き換えます。equalsIgnoreCase&#x200B;(&quot;STRING1) | true |

{style=&quot;table-layout:auto&quot;}

### 正規式関数

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| extract_regex | 正規式に基づいて、入力文字列からグループを抽出します。 | <ul><li>文字列：**必須**&#x200B;グループの抽出元の文字列。</li><li>REGEX:**必須**&#x200B;グループに一致させる正規式。</li></ul> | extract_regex(STRING, REGEX) | extract_regex&#x200B;(&quot;E259,E259B_009,1_1&quot;, &quot;([^,]+), [^,]*,([^,]+)&quot;) | [&quot;E259,E259B_009,1_1&quot;, &quot;E259&quot;, &quot;1_1&quot;] |
| matches_regex | 文字列が入力された正規式と一致するかどうかを確認します。 | <ul><li>文字列：**必須**&#x200B;チェックする文字列は、正規式と一致します。</li><li>REGEX:**必須**&#x200B;比較する正規式。</li></ul> | matches_regex(STRING, REGEX) | matches_regex(&quot;E259,E259B_009,1_1&quot;, &quot;([^,]+), [^,]*,([^,]+)&quot;) | true |

{style=&quot;table-layout:auto&quot;}

### ハッシュ関数{#hashing}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| sha1 | 入力を受け取り、Secure Hash Algorithm 1(SHA-1)を使用してハッシュ値を生成します。 | <ul><li>入力：**必須**&#x200B;ハッシュするプレーンテキスト。</li><li>CHARSET:*オプション*&#x200B;文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | sha1(INPUT, CHARSET) | sha1(&quot;my text&quot;, &quot;UTF-8&quot;) | c3599c11e47719df18a24 &#x200B; 48690840c5dfcce3c80 |
| sha256 | 入力を受け取り、Secure Hash Algorithm 256(SHA-256)を使用してハッシュ値を生成します。 | <ul><li>入力：**必須**&#x200B;ハッシュするプレーンテキスト。</li><li>CHARSET:*オプション*&#x200B;文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | sha256(INPUT, CHARSET) | sha256(&quot;my text&quot;, &quot;UTF-8&quot;) | 7330d2b39ca35eaf4cb95fc846c21 &#x200B;ee6a39af698154a83a586ee270a0d372104 |
| sha512 | 入力を受け取り、Secure Hash Algorithm 512(SHA-512)を使用してハッシュ値を生成します。 | <ul><li>入力：**必須**&#x200B;ハッシュするプレーンテキスト。</li><li>CHARSET:*オプション*&#x200B;文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | sha512(INPUT, CHARSET) | sha512(&quot;my text&quot;, &quot;UTF-8&quot;) | a3d7e45a0d9be5fd4e4b9a3b8c9c2163c21ef &#x200B; 708bf11b4232bb21d2a8704ada2cd7b367dd078a89 a5c908cfe377aceb1072a7b386b7d4fd2ff68a8fd24d16 |
| md5 | 入力を受け取り、MD5を使用してハッシュ値を生成します。 | <ul><li>入力：**必須**&#x200B;ハッシュするプレーンテキスト。</li><li>CHARSET:*オプション*&#x200B;文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。 </li></ul> | md5(INPUT, CHARSET) | md5(&quot;my text&quot;, &quot;UTF-8&quot;) | d3b96ce8c9fb4 &#x200B; e9bd0198d03ba6852c7 |
| crc32 | 入力でCyclic Redundancy Check（CRC；巡回冗長検査）アルゴリズムを使用し、32ビットの循環コードを生成します。 | <ul><li>入力：**必須**&#x200B;ハッシュするプレーンテキスト。</li><li>CHARSET:*オプション*&#x200B;文字セットの名前。 使用できる値は、「UTF-8」、「UTF-16」、「ISO-8859-1」および「US-ASCII」です。</li></ul> | crc32(INPUT, CHARSET) | crc32(&quot;my text&quot;, &quot;UTF-8&quot;) | 8df92e80 |

{style=&quot;table-layout:auto&quot;}

### URL関数{#url}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| get_url_protocol | 渡されたURLからプロトコルを返します。 入力が無効な場合は、nullを返します。 | <ul><li>URL:**必須**&#x200B;プロトコルの抽出元URL。</li></ul> | get_url_&#x200B;protocol(URL) | get_url_protocol(&quot;https://platform &#x200B; .adobe.com/home&quot;) | https |
| get_url_host | 渡されたURLのホストを返します。 入力が無効な場合は、nullを返します。 | <ul><li>URL:**必須**&#x200B;ホストの抽出元URL。</li></ul> | get_url_&#x200B;host(URL) | get_url_&#x200B;host(&quot;https://platform &#x200B; .adobe.com/home&quot;) | platform.adobe.com |
| get_url_port | 渡されたURLのポートを返します。 入力が無効な場合は、nullを返します。 | <ul><li>URL:**必須**&#x200B;ポートの抽出元URL。</li></ul> | get_url_port(URL) | get_url_port&#x200B;(&quot;sftp://example.com//home/ &#x200B; joe/employee.csv&quot;) | 22 |
| get_url_path | 渡されたURLのパスを返します。 デフォルトでは、フルパスが返されます。 | <ul><li>URL:**必須**&#x200B;パスの抽出元URL。</li><li>FULL_PATH:*オプション*&#x200B;フルパスが返されるかどうかを指定するboolean値。 falseに設定した場合は、パスの終わりのみが返されます。</li></ul> | get_url_path&#x200B;(URL, FULL_PATH) | get_url_path&#x200B;(&quot;sftp://example.com// &#x200B; home/joe/employee.csv&quot;) | &quot;//home/joe/ &#x200B; employee.csv&quot; |
| get_url_クエリ_str | 渡されたURLのクエリ文字列を返します。 | <ul><li>URL:**必須**&#x200B;クエリ文字列の取得元のURL。</li><li>アンカー：**必須**&#x200B;クエリ文字列内のアンカーの処理を指定します。 次の3つの値のいずれかになります。&quot;retain&quot;、&quot;remove&quot;または&quot;append&quot;。<br><br>値が「retain」の場合、返される値にアンカーが割り当てられます。<br>値が「remove」の場合、返される値からアンカーが削除されます。<br>値が「append」の場合、アンカーは別の値として返されます。</li></ul> | get_url_&#x200B;クエリ_str(URL, ANCHOR) | get_url_&#x200B;クエリ_str(&quot;foo://example.com:8042/over/there?ferret#nose&#x200B;, &quot;retain&quot;&#x200B;?ferret#nose=<br>get_url_クエリ_str(&quot;foo://example.com:8042&#x200B;/there?name=nose>get_nose&lt;nose>get_get_noser_str_name foo://example.com?=ferret#nose&quot;, &quot;append&quot;)<br> | `{"name": "ferret#nose"}`<br>`{"name": "ferret"}`<br>`{"name": "ferret", "_anchor_": "nose"}` |

{style=&quot;table-layout:auto&quot;}

### 日付および時間関数 {#date-and-time}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。`date`関数の詳細については、[データ形式処理ガイド](./data-handling.md#dates)の日付セクションを参照してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| now | 現在の時刻を取得します。 |  | now() | now() | `2020-09-23T10:10:24.556-07:00[America/Los_Angeles]` |
| timestamp | 現在の Unix 時間を取得します。 |  | timestamp() | timestamp() | 1571850624571 |
| format | 指定された形式に従って入力日をフォーマットします。 | <ul><li>日付：**必須** ZonedDateTimeオブジェクトとしての、フォーマットする入力日。</li><li>形式：**必須**&#x200B;日付の変更先の形式。</li></ul> | format(DATE, FORMAT) | format(2019-10-23T11:24:00+00:00, &quot;yyyy-MM-dd HH:mm:ss&quot;) | &quot;2019-10-23 11:24:35&quot; |
| dformat | 指定された形式に従ってタイムスタンプを日付文字列に変換します。 | <ul><li>TIMESTAMP:**必須**&#x200B;形式を設定するタイムスタンプ。 これはミリ秒で書き込まれます。</li><li>形式：**必須**&#x200B;タイムスタンプを変更する形式。</li></ul> | dformat &#x200B;(TIMESTAMP, FORMAT) | dformat(1571829875, &quot;dd-MMM-yyyy hh:mm&quot;) | &quot;23-Oct-2019 11:24&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付：**必須**&#x200B;日付を表す文字列。</li><li>形式：**必須**&#x200B;日付の形式を表す文字列。</li><li>DEFAULT_DATE:**必須**&#x200B;指定された日付がnullの場合は、デフォルトの日付が返されます。</li></ul> | date(DATE, FORMAT, DEFAULT_DATE) | date(&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;, now()) | &quot;2019-10-23T11:24Z&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付：**必須**&#x200B;日付を表す文字列。</li><li>形式：**必須**&#x200B;日付の形式を表す文字列。</li></ul> | date(DATE, FORMAT) | date(&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;) | &quot;2019-10-23T11:24Z&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付：**必須**&#x200B;日付を表す文字列。</li></ul> | date(DATE) | date(&quot;2019-10-23 11:24&quot;) | &quot;2019-10-23T11:24Z&quot; |
| date_part | 日付の一部を取得します。次のコンポーネント値がサポートされています。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;quarter&quot;<br>&quot;qq&quot;<br>&quot;q&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;dayofyear&quot;<br>&quot;dy&quot;<br>&quot;y&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;week&quot;<br>&quot;ww&quot;<br>&quot;w&quot;<br><br>&quot;weekday&quot;<br>&quot;dw&quot;<br>&quot;w&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br>&quot;hh24&quot;<br>&quot;hh12&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot;<br><br>&quot;millisecond&quot;<br>&quot;ms&quot; | <ul><li>コンポーネント：**必須**&#x200B;日付の一部を表す文字列。 </li><li>日付：**必須**&#x200B;標準形式の日付。</li></ul> | date_part&#x200B;(COMPONENT, DATE) | date_part(&quot;MM&quot;, date(&quot;2019-10-17 11:55:12&quot;)) | 10 |
| set_date_part | 指定された日付のコンポーネントを置き換えます。次のコンポーネントが受け入れられます。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot; | <ul><li>コンポーネント：**必須**&#x200B;日付の一部を表す文字列。 </li><li>値：**必須**&#x200B;指定した日付のコンポーネントに設定する値。</li><li>日付：**必須**&#x200B;標準形式の日付。</li></ul> | set_date_part&#x200B;(COMPONENT, VALUE, DATE) | set_date_part(&quot;m&quot;, 4, date(&quot;2016-11-09T11:44:44.797&quot;) | &quot;2016-04-09T11:44:44.797&quot; |
| make_date_time | 部分から日付を作成します。この関数は、make_timestampを使用して誘導することもできます。 | <ul><li>年：**必須**&#x200B;年、4桁で表記。</li><li>MONTH:**必須**&#x200B;月。 指定できる値は1 ～ 12です。</li><li>DAY:**必須**&#x200B;日。 指定できる値は1 ～ 31です。</li><li>HOUR:**必須**&#x200B;時間。 指定できる値は0 ～ 23です。</li><li>分：**必須**&#x200B;分。 指定できる値は0 ～ 59です。</li><li>ナノ秒：**必須**&#x200B;ナノ秒値。 指定できる値は0 ～ 99999999です。</li><li>TIMEZONE:**必須**&#x200B;日付時間のタイムゾーン。</li></ul> | make_date_time&#x200B;(YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, NANOSECOND, TIMEZONE) | make_date_time&#x200B;(2019, 10, 17, 11, 55, 12, 999, &quot;America/Los_Angeles&quot;) | `2019-10-17T11:55:12.0&#x200B;00000999-07:00[America/Los_Angeles]` |
| zone_date_to_utc | 任意のタイムゾーンの日付をUTCで表した日付に変換します。 | <ul><li>日付：**必須**&#x200B;変換しようとしている日付。</li></ul> | zone_date_to_utc&#x200B;(DATE) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:&#x200B;12.000000999-&#x200B;07:00[America/Los_Angeles])` | `2019-10-17T18:55:12.000000999Z[UTC]` |
| zone_date_to_zone | あるタイムゾーンの日付を別のタイムゾーンに変換します。 | <ul><li>日付：**必須**&#x200B;変換しようとしている日付。</li><li>ゾーン：**必須**&#x200B;日付を変換しようとしているタイムゾーン。</li></ul> | zone_date_to_zone&#x200B;(DATE, ZONE) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:12&#x200B;.000000999-07:00&#x200B;[America/Los_Angeles], "Europe/Paris")` | `2019-10-17T20:55:12.000000999+02:00[Europe/Paris]` |

{style=&quot;table-layout:auto&quot;}
&#x200B;

### 階層 — オブジェクト{#objects}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| size_of | 入力のサイズを返します。 | <ul><li>入力：**必須**&#x200B;サイズを調べようとしているオブジェクト。</li></ul> | size_of(INPUT) | `size_of([1, 2, 3, 4])` | 4 |
| is_empty | オブジェクトが空かどうかをチェックします。 | <ul><li>入力：**必須**&#x200B;確認しようとしているオブジェクトが空です。</li></ul> | is_empty(INPUT) | `is_empty([1, 2, 3])` | false |
| arrays_to_object | オブジェクトのリストを作成します。 | <ul><li>入力：**必須**&#x200B;キーと配列のペアのグループ。</li></ul> | arrays_to_object(INPUT) | 必要なサンプル | 必要なサンプル |
| to_object | 指定されたフラットなキー/値のペアに基づいてオブジェクトを作成します。 | <ul><li>入力：**必須**&#x200B;キーと値のペアのフラットリスト。</li></ul> | to_object(INPUT) | to_&#x200B;object(&quot;firstName&quot;, &quot;John&quot;, &quot;lastName&quot;, &quot;Doe&quot;) | `{"firstName": "John", "lastName": "Doe"}` |
| str_to_object | 入力文字列からオブジェクトを作成します。 | <ul><li>文字列：**必須**&#x200B;オブジェクトを作成するために解析される文字列。</li><li>VALUE_DELIMITER:*オプション*&#x200B;フィールドと値を区切る区切り文字。 デフォルトの区切り文字は`:`です。</li><li>FIELD_DELIMITER:*オプション*&#x200B;フィールド値のペアを区切る区切り文字。 デフォルトの区切り文字は`,`です。</li></ul> | str_to_object&#x200B;(STRING, VALUE_DELIMITER, FIELD_DELIMITER) | str_to_object(&quot;firstName - John | lastName - | 電話 — 123 456 7890&quot;、&quot;-&quot;、&quot; | &quot;) | `{"firstName": "John", "lastName": "Doe", "phone": "123 456 7890"}` |
| is_set | オブジェクトがソースデータ内に存在するかどうかを確認します。 | <ul><li>入力：**必須**&#x200B;ソースデータ内に存在する場合に確認するパス。</li></ul> | is_set(INPUT) | is_set&#x200B;(&quot;evars.evar.field1&quot;) | true |
| 無効にする | 属性の値を`null`に設定します。 これは、フィールドをターゲットスキーマにコピーしない場合に使用します。 |  | nullify() | nullify() | `null` |

{style=&quot;table-layout:auto&quot;}

### 階層 — 配列{#arrays}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| coalesce | 渡された配列の最初のnull以外のオブジェクトを返します。 | <ul><li>入力：**必須** NULLでない最初のオブジェクトを探す配列。</li></ul> | coalesce(INPUT) | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| first | 渡された配列の最初の要素を取得します。 | <ul><li>入力：**必須**&#x200B;最初の要素を探す配列。</li></ul> | first(INPUT) | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | 渡された配列の最後の要素を取得します。 | <ul><li>入力：**必須**&#x200B;最後の要素を探す配列。</li></ul> | last(INPUT) | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| add_to_array | 配列の末尾に要素を追加します。 | <ul><li>配列：**必須**&#x200B;要素を追加する配列。</li><li>値：配列に追加する要素です。</li></ul> | add_to_array&#x200B;(ARRAY, VALUES) | add_to_array&#x200B;([&#39;a&#39;, &#39;b&#39;], &#39;c&#39;, &#39;d&#39;) | [&#39;a&#39;、&#39;b&#39;、&#39;c&#39;、&#39;d&#39;] |
| join_arrays | 配列を結合します。 | <ul><li>配列：**必須**&#x200B;要素を追加する配列。</li><li>値：親配列に追加する配列。</li></ul> | join_arrays&#x200B;(ARRAY, VALUES) | join_arrays&#x200B;([&#39;a&#39;, &#39;b&#39;], [&#39;c&#39;], [&#39;d&#39;, &#39;e&#39;]) | [「a」、「b」、「c」、「d」、「e」] |
| to_array | 入力のリストを取り、配列に変換します。 | <ul><li>INCLUDE_NULLS:**必須**&#x200B;応答配列にNULLを含めるかどうかを示すboolean値。</li><li>値：**必須**&#x200B;配列に変換する要素。</li></ul> | to_array&#x200B;(INCLUDE_NULLS, VALUES) | to_array(false, 1, null, 2, 3) | `[1, 2, 3]` |

{style=&quot;table-layout:auto&quot;}

### 論理演算子 {#logical-operators}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| decode | キーと、キーと値のペアのリストが配列としてフラット化されている場合、この関数は、キーが見つかった場合は値を返し、デフォルト値が配列に存在する場合はデフォルト値を返します。 | <ul><li>キー：**必須**&#x200B;一致するキー。</li><li>OPTIONS:**必須**&#x200B;キーと値のペアのフラットな配列。 オプションで、デフォルト値を末尾に配置できます。</li></ul> | decode(KEY,OPTIONS) | decode(stateCode, &quot;ca&quot;, &quot;California&quot;, &quot;pa&quot;, &quot;Pennsylvania&quot;, &quot;N/A&quot;) | 渡されたstateCodeが「ca」、「California」の場合。<br>指定されたstateCodeが「pa」、「Pennsylvania」の場合。<br>stateCodeが次の値と一致しない場合は、「N/A」となります。 |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>式:**必須**&#x200B;評価するブール値式。</li><li>TRUE_VALUE:**必須**&#x200B;式がtrueと評価された場合に返される値。</li><li>FALSE_VALUE:**必須**&#x200B;式の評価結果がfalseの場合に返される値。</li></ul> | iif(式, TRUE_VALUE, FALSE_VALUE) | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |

{style=&quot;table-layout:auto&quot;}

### 集計 {#aggregation}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| min | 指定された引数の最小値を返します。自然な順序を使用します。 | <ul><li>OPTIONS:**必須**&#x200B;互いに比較できる1つ以上のオブジェクト。</li></ul> | min(OPTIONS) | min(3, 1, 4) | 1 |
| max | 指定された引数の最大値を返します。自然な順序を使用します。 | <ul><li>OPTIONS:**必須**&#x200B;互いに比較できる1つ以上のオブジェクト。</li></ul> | max(OPTIONS) | max(3, 1, 4) | 4 |

{style=&quot;table-layout:auto&quot;}

### 型変換{#type-conversions}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| to_bigint | 文字列をBigIntegerに変換します。 | <ul><li>文字列：**必須** BigIntegerに変換する文字列。</li></ul> | to_bigint(STRING) | to_bigint&#x200B;(&quot;1000000.34&quot;) | 1000000.34 |
| to_decimal | 文字列を重複に変換します。 | <ul><li>文字列：**必須**&#x200B;重複に変換する文字列。</li></ul> | to_decimal(STRING) | to_decimal(&quot;20.5&quot;) | 20.5 |
| to_float | 文字列を浮動小数点数型(Float)に変換します。 | <ul><li>文字列：**必須**&#x200B;浮動小数点型に変換する文字列。</li></ul> | to_float(STRING) | to_float(&quot;12.3456&quot;) | 12.34566 |
| to_integer | 文字列を整数型(Integer)に変換します。 | <ul><li>文字列：**必須**&#x200B;整数に変換する文字列。</li></ul> | to_integer(STRING) | to_integer(&quot;12&quot;) | 12 |

{style=&quot;table-layout:auto&quot;}

### JSON関数{#json}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| json_to_object | 渡された文字列からJSONコンテンツを逆シリアル化します。 | <ul><li>文字列：**必須**&#x200B;デシリアライズするJSON文字列。</li></ul> | json_to_object &#x200B;(STRING) | json_to_object&#x200B;({&quot;info&quot;:{&quot;firstName&quot;:&quot;John&quot;,&quot;lastName&quot;:&quot;Doe&quot;}}) | JSONを表すオブジェクト。 |

{style=&quot;table-layout:auto&quot;}

### 特殊演算{#special-operations}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| uuid /<br>guid | 擬似ランダム ID を生成します。 |  | uuid()<br>guid() | uuid()<br>guid() | 7c0267d2-bb74-4e1a-9275-3bf4fccda5f4<br>c7016dc7-3163-43f7-afc7-2e1c9c206333 |

{style=&quot;table-layout:auto&quot;}

### ユーザーエージェント機能{#user-agent}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| ua_os_name | ユーザーエージェント文字列からオペレーティングシステム名を抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_os_name&#x200B;(USER_AGENT) | ua_os_name&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS |
| ua_os_version_major | ユーザーエージェント文字列からオペレーティングシステムのメジャーバージョンを抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_os_version_major &#x200B;(USER_AGENT) | ua_os_version_major &#x200B; s(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5 |
| ua_os_version | ユーザーエージェント文字列からオペレーティングシステムのバージョンを抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_os_version&#x200B;(USER_AGENT) | ua_os_version&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1.1 |
| ua_os_name_version | ユーザーエージェント文字列からオペレーティングシステムの名前とバージョンを抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_os_name_version&#x200B;(USER_AGENT) | ua_os_name_version&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5.1.1 |
| ua_agent_version | ユーザーエージェント文字列からエージェントバージョンを抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_agent_version&#x200B;(USER_AGENT) | ua_agent_version&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1 |
| ua_agent_version_major | ユーザーエージェント文字列からエージェント名とメジャーバージョンを抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_agent_version_major &#x200B;(USER_AGENT) | ua_agent_version_major&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari 5 |
| ua_agent_name | ユーザーエージェント文字列からエージェント名を抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_agent_name&#x200B;(USER_AGENT) | ua_agent_name&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari |
| ua_device_class | ユーザーエージェント文字列からデバイスクラスを抽出します。 | <ul><li>USER_AGENT:**必須**&#x200B;ユーザーエージェント文字列。</li></ul> | ua_device_class&#x200B;(USER_AGENT) | ua_device_class&#x200B;(&quot;Mozilla/5.0 (iPhone;CPU iPhone OS 5_1_1（Mac OS Xなど）AppleWebKit/534.46（KHTML、Geckoなど）バージョン/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Phone |

{style=&quot;table-layout:auto&quot;}

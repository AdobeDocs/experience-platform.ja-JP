---
keywords: Experience Platform；ホーム；人気のトピック；CSV のマップ；CSV ファイルのマップ；xdm への CSV ファイルのマップ；xdm への CSV のマップ；ui ガイド；マッパー；マッピング；マッピングフィールド；マッピング関数；
solution: Experience Platform
title: データ準備のマッピング機能
description: このドキュメントでは、Data Prep で使用されるマッピング機能について説明します。
exl-id: e95d9329-9dac-4b54-b804-ab5744ea6289
source-git-commit: 1e06fa2f8a5685cf5debcc3b5279d7efab9af0c8
workflow-type: tm+mt
source-wordcount: '6024'
ht-degree: 8%

---

# データ準備のマッピング機能

データ準備関数を使用すると、ソースフィールドに入力された値に基づいて値を計算および計算できます。

## フィールド

フィールド名には、任意の法的識別子（Unicode 文字および数字の無制限のシーケンス、文字、ドル記号（`$`）、アンダースコア文字（`_`）を使用できます。 変数名でも大文字と小文字が区別されます。

フィールド名がこの規則に従わない場合は、フィールド名を `${}` でラップする必要があります。 例えば、フィールド名が「名」または「First.Name」の場合、名前をそれぞれ `${First Name}` または `${First\.Name}` のようにラップする必要があります。

>[!TIP]
>
>階層を操作する際に、子属性にピリオド（`.`）がある場合は、バックスラッシュ（`\`）を使用して特殊文字をエスケープする必要があります。詳しくは、[ 特殊文字のエスケープ ](home.md#escape-special-characters) に関するガイドを参照してください。

フィールド名が次の予約キーワードの **any** である場合、`${}{}` でラップする必要があります。

```console
new, mod, or, break, var, lt, for, false, while, eq, gt, div, not, null, continue, else, and, ne, true, le, if, ge, return, _errors, do, function, empty, size
```

また、予約済みキーワードには、このページにリストされているマッパー関数も含まれます。

サブフィールド内のデータには、ドット表記を使用してアクセスできます。 例えば、`name` オブジェクトがある場合、`firstName` フィールドにアクセスするには、`name.firstName` を使用します。

## 関数のリスト

次の表に、サンプル式とその結果の出力を含む、サポートされるすべてのマッピング関数を示します。

### 文字列関数 {#string}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| concat | 指定された文字列を連結します。 | <ul><li>STRING：連結される文字列。</li></ul> | concat （STRING_1, STRING_2） | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| explode | 正規表現に基づいて文字列を分割し、部分の配列を返します。 オプションで、正規表現を含めて文字列を分割できます。 デフォルトでは、分割は「,」に解決されます。 次の区切り文字を `\` でエスケープする必要があります **必須**。`+, ?, ^, \|, ., [, (, {, ), *, $, \` 区切り文字として複数の文字を含めると、区切り文字は複数の文字の区切り文字として扱われます。 | <ul><li>文字列：**必須** 分割する必要がある文字列。</li><li>REGEX: *オプション* 文字列の分割に使用できる正規表現。</li></ul> | explode （STRING, REGEX） | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の場所/インデックスを返します。 | <ul><li>入力：**必須** 検索する文字列。</li><li>部分文字列：**必須** 文字列内で検索される部分文字列。</li><li>START_POSITION: *オプション* 文字列内で検索を開始する場所です。</li><li>OCCURRENCE: *オプション* 開始位置から検索する n 番目のオカレンスです。 デフォルトの重み付けは 1 です。 </li></ul> | instr （INPUT, SUBSTRING, START_POSITION, OCCURRENCE） | instr （&quot;adobe.com&quot;, &quot;com&quot;） | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合は、その文字列を置き換えます。 | <ul><li>入力：**必須** 入力文字列。</li><li>TO_FIND: **必須** 入力内で検索する文字列。</li><li>TO_REPLACE: **必須** 「TO_FIND」内の値を置換する文字列。</li></ul> | replacestr （INPUT, TO_FIND, TO_REPLACE） | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 指定された長さのサブ文字列を返します。 | <ul><li>入力：**必須** 入力文字列。</li><li>START_INDEX: **必須** 部分文字列が開始する入力文字列のインデックス。</li><li>LENGTH: **必須** 部分文字列の長さです。</li></ul> | substr （INPUT, START_INDEX, LENGTH） | substr(&quot;This is a substring test&quot;, 7, 8) | &quot;サブセット&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | <ul><li>入力：**必須** 小文字に変換される文字列。</li></ul> | lower （入力） | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | <ul><li>入力：**必須** 大文字に変換される文字列。</li></ul> | upper （INPUT） | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り文字で入力文字列を分割します。 次の区切り記号 **必須** は、`\` でエスケープする必要があります：`\`。 複数の区切り文字を含めると、文字列は文字列に存在する区切り文字の **任意** に分割されます。 **注意：** この関数は、区切り文字の存在に関係なく、文字列から null 以外のインデックスのみを返します。 結果の配列ですべてのインデックス（null を含む）が必要な場合は、「explode」関数を使用します。 | <ul><li>入力：**必須** 分割される入力文字列。</li><li>SEPARATOR: **必須** 入力を分割するために使用される文字列。</li></ul> | split （INPUT, SEPARATOR） | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | 区切り記号を使用してオブジェクトのリストを結合します。 | <ul><li>SEPARATOR: **必須** オブジェクトの結合に使用される文字列。</li><li>OBJECTS: **必須** 結合する文字列の配列。</li></ul> | `join(SEPARATOR, [OBJECTS])` | `join(" ", to_array(true, "Hello", "world"))` | &quot;Hello world&quot; |
| lpad | 文字列の左側を指定された他の文字列でパディングします。 | <ul><li>入力：**必須** パディングされる文字列。 この文字列は null にできます。</li><li>COUNT: **必須** 埋め込む文字列のサイズです。</li><li>PADDING: **必須** 入力を埋め込む文字列。 null または空の場合は、1 つのスペースとして扱われます。</li></ul> | lpad （INPUT, COUNT, PADDING） | lpad （&quot;bat&quot;, 8, &quot;yz&quot;） | 「yzyzybat」 |
| rpad | 文字列の右側を指定された他の文字列でパディングします。 | <ul><li>入力：**必須** パディングされる文字列。 この文字列は null にできます。</li><li>COUNT: **必須** 埋め込む文字列のサイズです。</li><li>PADDING: **必須** 入力を埋め込む文字列。 null または空の場合は、1 つのスペースとして扱われます。</li></ul> | rpad （INPUT, COUNT, PADDING） | rpad （&quot;bat&quot;, 8, &quot;yz&quot;） | 「バチジー」 |
| left | 指定された文字列の最初の「n」文字を取得します。 | <ul><li>文字列：**必須** 最初の「n」文字を取得する文字列。</li><li>COUNT: **必須** 文字列から取得する「n」文字。</li></ul> | left （STRING, COUNT） | left （&quot;abcde&quot;, 2） | &quot;ab&quot; |
| 右 | 指定された文字列の最後の「n」文字を取得します。 | <ul><li>文字列：**必須** 最後の「n」文字を取得する文字列。</li><li>COUNT: **必須** 文字列から取得する「n」文字。</li></ul> | right （STRING, COUNT） | right （&quot;abcde&quot;, 2） | &quot;de&quot; |
| ltrim | 文字列の先頭から空白を削除します。 | <ul><li>STRING: **必須** 空白を削除する文字列。</li></ul> | ltrim （STRING） | ltrim （「こんにちは」） | &quot;hello&quot; |
| rtrim | 文字列の末尾から空白を削除します。 | <ul><li>STRING: **必須** 空白を削除する文字列。</li></ul> | rtrim （STRING） | rtrim （&quot;hello &quot;） | &quot;hello&quot; |
| trim | 文字列の先頭と末尾から空白を削除します。 | <ul><li>STRING: **必須** 空白を削除する文字列。</li></ul> | trim （STRING） | trim （&quot; hello &quot;） | &quot;hello&quot; |
| 次と等しい | 2 つの文字列を比較して、等しいかどうかを確認します。 この関数では大文字と小文字が区別されます。 | <ul><li>STRING1: **必須** 比較する最初の文字列。</li><li>STRING2: **必須** 比較する 2 番目の文字列。</li></ul> | STRING1.&#x200B;equals （&#x200B;STRING2） | &quot;string1&quot;..&#x200B;equals&#x200B;（&quot;STRING1&quot;） | 偽 |
| equalsIgnoreCase | 2 つの文字列を比較して、等しいかどうかを確認します。 この関数では、大文字と小文字は区別され **せん**。 | <ul><li>STRING1: **必須** 比較する最初の文字列。</li><li>STRING2: **必須** 比較する 2 番目の文字列。</li></ul> | STRING1.&#x200B;equalsIgnoreCase&#x200B;（STRING2） | &quot;string1&quot;..&#x200B;equalsIgnoreCase&#x200B;（&quot;STRING1） | true |

{style="table-layout:auto"}

### 正規表現関数

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| extract_regex | 正規表現に基づいて、入力文字列からグループを抽出します。 | <ul><li>STRING: **必須** グループを抽出する文字列。</li><li>正規表現：**必須** グループを照合する正規表現。</li></ul> | extract_regex （STRING, REGEX） | extract_regex&#x200B;（&quot;E259,E259B_009,1_1&quot;&#x200B;, &quot;（[^,]+）,[^,]*,（[^,]+）&quot;） | [&quot;E259,E259B_009,1_1&quot;, &quot;E259&quot;, &quot;1_1&quot;] |
| matches_regex | 文字列が入力された正規表現と一致するかどうかを確認します。 | <ul><li>文字列：**必須** 確認する文字列が正規表現と一致します。</li><li>正規表現：**必須** 比較する正規表現。</li></ul> | matches_regex （STRING, REGEX） | matches_regex （&quot;E259,E259B_009,1_1&quot;, &quot;（[^,]+）,[^,]*,（[^,]+）&quot;） | true |

{style="table-layout:auto"}

### ハッシュ関数 {#hashing}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| sha1 | 入力を受け取り、セキュアハッシュアルゴリズム 1 （SHA-1）を使用してハッシュ値を生成します。 | <ul><li>入力：**必須** ハッシュ化されるプレーンテキスト。</li><li>CHARSET: *オプション* 文字セットの名前。 使用可能な値は、UTF-8、UTF-16、ISO-8859-1、US-ASCII です。</li></ul> | sha1 （入力，CHARSET） | sha1 （&quot;my text&quot;, &quot;UTF-8&quot;） | c3599c11e47719df18a24&#x200B;48690840c5dfcce3c80 |
| sha256 | 入力を受け取り、セキュアハッシュアルゴリズム 256 を使用してハッシュ値を生成します（SHA-256）。 | <ul><li>入力：**必須** ハッシュ化されるプレーンテキスト。</li><li>CHARSET: *オプション* 文字セットの名前。 使用可能な値は、UTF-8、UTF-16、ISO-8859-1、US-ASCII です。</li></ul> | sha256 （入力，CHARSET） | sha256 （&quot;my text&quot;, &quot;UTF-8&quot;） | 7330d2b39ca35eaf4cb95fc846c21&#x200B;ee6a39af698154a83a586ee270a0d372104 |
| sha512 | 入力を受け取り、セキュアハッシュアルゴリズム 512 を使用してハッシュ値を生成します（SHA-512）。 | <ul><li>入力：**必須** ハッシュ化されるプレーンテキスト。</li><li>CHARSET: *オプション* 文字セットの名前。 使用可能な値は、UTF-8、UTF-16、ISO-8859-1、US-ASCII です。</li></ul> | sha512 （入力，CHARSET） | sha512 （&quot;my text&quot;, &quot;UTF-8&quot;） | a3d7e45a0d9be5fd4e4b9a3b8c9c2163c21ef&#x200B;708bf11b4232bb21d2a8704ada2cdcd7b367dd0788a89&#x200B;a5c908cfe377aceb1072a7b386d4d 2ff68a8fd24d16 |
| md5 | 入力を受け取り、MD5 を使用してハッシュ値を生成します。 | <ul><li>入力：**必須** ハッシュ化されるプレーンテキスト。</li><li>CHARSET: *オプション* 文字セットの名前。 使用可能な値は、UTF-8、UTF-16、ISO-8859-1、US-ASCII です。 </li></ul> | md5 （入力，CHARSET） | md5 （&quot;my text&quot;, &quot;UTF-8&quot;） | d3b96ce8c9fb4&#x200B;e9bd0198d03ba6852c7 |
| crc32 | 入力に巡回冗長検査（CRC）アルゴリズムを使用して、32 ビットの巡回符号を生成します。 | <ul><li>入力：**必須** ハッシュ化されるプレーンテキスト。</li><li>CHARSET: *オプション* 文字セットの名前。 使用可能な値は、UTF-8、UTF-16、ISO-8859-1、US-ASCII です。</li></ul> | crc32 （入力、文字セット） | crc32 （&quot;my text&quot;, &quot;UTF-8&quot;） | 8df92e80 |

{style="table-layout:auto"}

### URL 関数 {#url}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| get_url_protocol | 指定された URL からプロトコルを返します。 入力が無効な場合は、null を返します。 | <ul><li>URL: **必須** プロトコルを抽出する必要がある URL です。</li></ul> | get_url_protocol&#x200B;（URL） | get_url_protocol （&quot;https://platform&#x200B;.adobe.com/home&quot;） | https |
| get_url_host | 指定された URL のホストを返します。 入力が無効な場合は、null を返します。 | <ul><li>URL: **必須** ホストを抽出する必要がある URL です。</li></ul> | get_url_host&#x200B;（URL） | get_url_host&#x200B;（&quot;https://platform&#x200B;.adobe.com/home&quot;） | platform.adobe.com |
| get_url_port | 指定された URL のポートを返します。 入力が無効な場合は、null を返します。 | <ul><li>URL: **必須** ポートを抽出する必要がある URL です。</li></ul> | get_url_port （URL） | get_url_port&#x200B;（&quot;sftp://example.com//home/&#x200B;joe/employee.csv&quot;） | 22 |
| get_url_path | 指定された URL のパスを返します。 デフォルトでは、完全なパスが返されます。 | <ul><li>URL: **必須** パスを抽出する必要がある URL です。</li><li>FULL_PATH: *任意* フルパスを返すかどうかを決定するブール値。 false に設定した場合は、パスの末尾のみが返されます。</li></ul> | get_url_path&#x200B;（URL, FULL_PATH） | get_url_path&#x200B;（&quot;sftp://example.com//&#x200B;home/joe/employee.csv&quot;） | &quot;//home/joe/&#x200B;employee.csv&quot; |
| get_url_query_str | 指定された URL のクエリ文字列を、クエリ文字列名とクエリ文字列値のマップとして返します。 | <ul><li>URL: **必須** クエリ文字列の取得元の URL。</li><li>ANCHOR: **必須** クエリ文字列内のアンカーでの処理を決定します。 「retain」、「remove」、「append」の 3 つの値のいずれかです。<br><br> 値が「retain」の場合、アンカーは返された値に添付されます。<br> 値が「削除」の場合、アンカーは返された値から削除されます。<br> 値が「追加」の場合、アンカーは別の値として返されます。</li></ul> | get_url_query_str&#x200B;（URL, ANCHOR） | get_url_query_str&#x200B;（&quot;foo://example.com:8042&#x200B;/over/there?name=&#x200B;ferret#nose&quot;, &quot;retain&quot;） <br>get_url_query_str&#x200B;（&quot;foo://example.com:8042&#x200B;/over/there?name=&#x200B;ferret#nose&quot;, &quot;remove&quot;） <br>get_url_query_str&#x200B; &#x200B; &#x200B;（&quot;foo://example.comは：8042/over/there で？name=ferret#nose&quot;, &quot;append&quot;） | `{"name": "ferret#nose"}`<br>`{"name": "ferret"}`<br>`{"name": "ferret", "_anchor_": "nose"}` |
| get_url_encoded | この関数は、URL を入力として受け取り、特殊文字を ASCII 文字に置換つまりエンコードします。 特殊文字の詳細については、このドキュメントの付録の [ 特殊文字のリスト ](#special-characters) を参照してください。 | <ul><li>URL: **必須** ASCII 文字で置き換えるかエンコードする特殊文字を含む入力 URL。</li></ul> | get_url_encoded （URL） | get_url_encoded （&quot;https</span>://example.com/partneralliance_asia-pacific_2022&quot;） | https%3A%2F%2Fexample.com%2Fpartneralliance_asia-pacific_2022 |
| get_url_decoded | この関数は、URL を入力として受け取り、ASCII 文字を特殊文字にデコードします。  特殊文字の詳細については、このドキュメントの付録の [ 特殊文字のリスト ](#special-characters) を参照してください。 | <ul><li>URL: **必須** 特殊文字にデコードする ASCII 文字を含む入力 URL。</li></ul> | get_url_decoded （URL） | get_url_decoded （&quot;https%3A%2F%2Fexample.com%2Fpartneralliance_asia-pacific_2022&quot;） | https</span>://example.com/partneralliance_asia-pacific_2022 |

{style="table-layout:auto"}

### 日付および時間関数 {#date-and-time}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。 `date` 関数の詳細については、『 [ データ形式処理ガイド ](./data-handling.md#dates) の日付の節を参照してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| now | 現在の時刻を取得します。 | | now() | now() | `2021-10-26T10:10:24Z` |
| timestamp | 現在の Unix 時間を取得します。 | | timestamp() | timestamp() | 1571850624571 |
| format | 指定された形式に従って入力日をフォーマットします。 | <ul><li>DATE: **必須** 書式設定する ZonedDateTime オブジェクトとしての入力日付。</li><li>形式：**必須** 日付の変更先となる形式。</li></ul> | format （DATE, FORMAT） | 形式（2019-10-23T11:24:00+00:00, &quot;`yyyy-MM-dd HH:mm:ss`&quot;） | `2019-10-23 11:24:35` |
| dformat | 指定された形式に従ってタイムスタンプを日付文字列に変換します。 | <ul><li>TIMESTAMP: **必須** 書式設定するタイムスタンプ。 これはミリ秒単位で記述されます。</li><li>FORMAT: **必須** タイムスタンプを作成する形式。</li></ul> | dformat （TIMESTAMP, FORMAT） | dformat （1571829875000, &quot;`yyyy-MM-dd'T'HH:mm:ss.SSSX`&quot;） | `2019-10-23T11:24:35.000Z` |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>DATE: **必須** 日付を表す文字列。</li><li>FORMAT: **必須** ソースの日付の形式を表す文字列。**メモ：** これは **日付文字列の変換先の形式を表** ていません。 </li><li>DEFAULT_DATE: **必須** 指定した日付が null の場合に返されるデフォルトの日付。</li></ul> | date （DATE, FORMAT, DEFAULT_DATE） | date （&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;, now （）） | `2019-10-23T11:24:00Z` |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>DATE: **必須** 日付を表す文字列。</li><li>FORMAT: **必須** ソースの日付の形式を表す文字列。**メモ：** これは **日付文字列の変換先の形式を表** ていません。 </li></ul> | date （DATE, FORMAT） | 日付（&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;） | `2019-10-23T11:24:00Z` |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>DATE: **必須** 日付を表す文字列。</li></ul> | date （DATE） | date （&quot;2019-10-23 11:24&quot;） | 「2019-10-23T11:24:00Z」 |
| date_part | 日付の一部を取得します。次のコンポーネント値がサポートされています。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;quarter&quot;<br>&quot;qq&quot;<br>&quot;q&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;<br><br>&quot;dayofyear&quot;<br>&quot;dy&quot;<br>&quot;y&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;week&quot;<br>&quot;ww&quot;<br>&quot;w&quot;<br><br>&quot;weekday&quot;<br> <br> <br><br> <br> <br> <br> <br><br> <br> <br> <br><br> <br> <br> <br><br> <br>&quot;dw&quot;w&quot;w&quot;w&quot;w&quot;w&quot;s 「ミリ秒」 SSS | <ul><li>COMPONENT: **必須** 日付の一部を表す文字列。 </li><li>日付：**必須** 標準形式の日付。</li></ul> | date_part&#x200B;（COMPONENT, DATE） | date_part （&quot;MM&quot;, date （&quot;2019-10-17 11:55:12&quot;）） | 10 |
| set_date_part | 指定された日付のコンポーネントを置き換えます。次のコンポーネントが受け入れられます。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot; | <ul><li>COMPONENT: **必須** 日付の一部を表す文字列。 </li><li>値：**必須** 指定された日付にコンポーネントに対して設定する値。</li><li>日付：**必須** 標準形式の日付。</li></ul> | set_date_part&#x200B;（COMPONENT, VALUE, DATE） | set_date_part （&quot;m&quot;, 4, date （&quot;2016-11-09T11:44:44.797&quot;） | &quot;2016-04-09T11:44:44Z&quot; |
| make_date_time | パーツから日付を作成します。 この関数は make_timestamp を使用して引き起こすこともできます。 | <ul><li>YEAR: **必須** 年（4 桁で記述）。</li><li>月：**必須** 月。 使用できる値は 1～12 です。</li><li>DAY: **必須** その日。 使用できる値は 1～31 です。</li><li>時間：**必須** 時間。 使用できる値は 0～23 です。</li><li>MINUTE: **必須** 分。 使用できる値は 0～59 です。</li><li>NANOSECOND: **必須** ナノ秒の値です。 使用できる値は 0～999999999 です。</li><li>TIMEZONE: **必須** 日時のタイムゾーン。</li></ul> | make_date_time&#x200B;（YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, NANOSECOND, TIMEZONE） | make_date_time&#x200B;（2019, 10, 17, 11, 55, 12, 999, &quot;アメリカ / ロサンゼルス&quot;） | `2019-10-17T11:55:12Z` |
| zone_date_to_utc | 任意のタイムゾーンの日付を UTC の日付に変換します。 | <ul><li>日付：**必須** 変換しようとしている日付。</li></ul> | zone_date_to_utc&#x200B;（DATE） | `zone_date_to_utc&#x200B;(2019-10-17T11:55:&#x200B;12 PST` | `2019-10-17T19:55:12Z` |
| zone_date_to_zone | あるタイムゾーンから別のタイムゾーンに日付を変換します。 | <ul><li>日付：**必須** 変換しようとしている日付。</li><li>ゾーン：**必須** 日付の変換先のタイムゾーン。</li></ul> | zone_date_to_zone&#x200B;（DATE, ZONE） | `zone_date_to_zone(now(), "Europe/Paris")` | `2021-10-26T15:43:59Z` |

{style="table-layout:auto"}

### 階層 – オブジェクト {#objects}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| is_empty | オブジェクトが空かどうかを確認します。 | <ul><li>入力：**必須** 確認しようとしているオブジェクトが空です。</li></ul> | is_empty （INPUT） | `is_empty([1, null, 2, 3])` | 偽 |
| arrays_to_object | オブジェクトのリストを作成します。 | <ul><li>入力：**必須** キーと配列のペアのグループ。</li></ul> | arrays_to_object （INPUT） | `arrays_to_objects('sku', explode("id1\|id2", '\\\|'), 'price', [22.5,14.35])` | ```[{ "sku": "id1", "price": 22.5 }, { "sku": "id2", "price": 14.35 }]``` |
| to_object | 指定されたフラットなキーと値のペアに基づいてオブジェクトを作成します。 | <ul><li>入力：**必須** キーと値のペアのフラットリスト。</li></ul> | to_object （INPUT） | to_object&#x200B;（&quot;firstName&quot;, &quot;John&quot;, &quot;lastName&quot;, &quot;Doe&quot;） | `{"firstName": "John", "lastName": "Doe"}` |
| str_to_object | 入力文字列からオブジェクトを作成します。 | <ul><li>文字列：**必須** オブジェクトを作成するために解析される文字列。</li><li>VALUE_DELIMITER: *任意* フィールドを値から区切る区切り文字です。 デフォルトの区切り文字は `:` です。</li><li>FIELD_DELIMITER: *任意* フィールド値のペアを区切る区切り文字です。 デフォルトの区切り文字は `,` です。</li></ul> | str_to_object&#x200B;（STRING, VALUE_DELIMITER, FIELD_DELIMITER） **注意**: `get()` 関数と `str_to_object()` を使用して、文字列内のキーの値を取得できます。 | <ul><li>例#1: str_to_object （&quot;firstName - John ; lastName - ; - 123 345 7890&quot;, &quot;-&quot;, &quot;;&quot;）</li><li>例#2: str_to_object （&quot;firstName - John ; lastName - ; phone - 123 456 7890&quot;, &quot;-&quot;, &quot;;&quot;）.get （&quot;firstName&quot;）</li></ul> | <ul><li>例#1:`{"firstName": "John", "lastName": "Doe", "phone": "123 456 7890"}`</li><li>例#2: 「John」</li></ul> |
| contains_key | オブジェクトがソースデータ内に存在するかどうかを確認します。 **注意：** この関数は、廃止された `is_set()` 関数に代わるものです。 | <ul><li>入力：**必須** ソースデータ内に存在する場合にチェックされるパス。</li></ul> | contains_key （INPUT） | contains_key （&quot;evars.evar.field1&quot;） | true |
| 無効にする | 属性の値を `null` に設定します。 これは、フィールドをターゲットスキーマにコピーしない場合に使用する必要があります。 | | nullify （） | nullify （） | `null` |
| get_keys | キーと値のペアを解析し、すべてのキーを返します。 | <ul><li>OBJECT: **必須** キーの抽出元のオブジェクト。</li></ul> | get_keys （OBJECT） | get_keys （{&quot;book1&quot;: &quot;Pride and Predial&quot;, &quot;book2&quot;: &quot;1984&quot;}） | `["book1", "book2"]` |
| get_values | キーと値のペアを解析し、指定されたキーに基づいて文字列の値を返します。 | <ul><li>STRING: **必須** 解析する文字列。</li><li>KEY: **必須** 値を抽出する必要があるキー。</li><li>VALUE_DELIMITER: **必須** フィールドと値を区切る区切り文字です。 `null` または空の文字列を指定した場合、この値は `:` になります。</li><li>FIELD_DELIMITER: *任意* フィールドと値のペアを区切る区切り文字です。 `null` または空の文字列を指定した場合、この値は `,` になります。</li></ul> | get_values （STRING, KEY, VALUE_DELIMITER, FIELD_DELIMITER） | get_values （\&quot;firstName - John , lastName - Cena , phone - 555 420 8692\&quot;, \&quot;firstName\&quot;, \&quot;-\&quot;, \&quot;,\&quot;） | John |
| map_get_values | マップとキー入力を受け取ります。 入力が 1 つのキーである場合、この関数はそのキーに関連付けられた値を返します。 入力が文字列配列の場合、この関数は指定されたキーに対応するすべての値を返します。 入力マップに重複したキーがある場合、戻り値はキーを重複排除し、一意の値を返す必要があります。 | <ul><li>MAP: **必須** 入力マップデータ。</li><li>KEY: **必須** キーは、単一の文字列または文字列配列にすることができます。 他のプリミティブ型（data / number）が指定された場合は、文字列として扱われます。</li></ul> | get_values （MAP, KEY） | コードサンプルについては、[ 付録 ](#map_get_values) を参照してください。 | |
| map_has_keys | 1 つ以上の入力キーが指定されている場合、この関数は true を返します。 文字列配列が入力として指定されている場合、この関数は、見つかった最初のキーに対して true を返します。 | <ul><li>MAP: **必須** 入力マップデータ</li><li>KEY: **必須** キーは、単一の文字列または文字列配列にすることができます。 他のプリミティブ型（data / number）が指定された場合は、文字列として扱われます。</li></ul> | map_has_keys （MAP, KEY） | コードサンプルについては、[ 付録 ](#map_has_keys) を参照してください。 | |
| add_to_map | 少なくとも 2 つの入力を受け付けます。 任意の数のマップを入力として指定できます。 データ準備は、すべての入力からのすべてのキーと値のペアを持つ単一のマップを返します。 1 つ以上のキーが（同じマップ内またはマップ間で）繰り返される場合、Data Prep ではキーの重複が排除されるので、最初のキーと値のペアは入力に渡された順序で保持されます。 | MAP: **必須** 入力マップデータ。 | add_to_map （MAP 1, MAP 2, MAP 3, ...） | コードサンプルについては、[ 付録 ](#add_to_map) を参照してください。 | |
| object_to_map （構文 1） | この関数を使用して、マップ データタイプを作成します。 | <ul><li>KEY: **必須** キーは文字列である必要があります。 整数や日付などの他のプリミティブ値が指定されている場合、それらは文字列に自動変換され、文字列として扱われます。</li><li>ANY_TYPE: **必須** マップを除く、サポートされる任意の XDM データタイプを参照します。</li></ul> | object_to_map （KEY, ANY_TYPE, KEY, ANY_TYPE, ...） | コードサンプルについては、[ 付録 ](#object_to_map) を参照してください。 | |
| object_to_map （構文 2） | この関数を使用して、マップ データタイプを作成します。 | <ul><li>OBJECT: **必須** 受信オブジェクトまたはオブジェクト配列を指定し、オブジェクト内の属性をキーとして指すことができます。</li></ul> | object_to_map （OBJECT） | コードサンプルについては、[ 付録 ](#object_to_map) を参照してください。 |
| object_to_map （構文 3） | この関数を使用して、マップ データタイプを作成します。 | <ul><li>OBJECT: **必須** 受信オブジェクトまたはオブジェクト配列を指定し、オブジェクト内の属性をキーとして指すことができます。</li></ul> | object_to_map （OBJECT_ARRAY, ATTRIBUTE_IN_OBJECT_TO_BE_USED_AS_A_KEY） | コードサンプルについては、[ 付録 ](#object_to_map) を参照してください。 |

{style="table-layout:auto"}

オブジェクトのコピーフィーチャーについては、以下の [ 節を参照してください ](#object-copy)。

### 階層 – 配列 {#arrays}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| coalesce | 指定された配列の最初の null 以外のオブジェクトを返します。 | <ul><li>入力：**必須** 最初の null 以外のオブジェクトを検索する配列。</li></ul> | coalesce （入力） | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| first | 指定された配列の最初の要素を取得します。 | <ul><li>入力：**必須** 最初の要素を検索する配列。</li></ul> | first （INPUT） | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | 指定された配列の最後の要素を取得します。 | <ul><li>入力：**必須** 最後の要素を検索する配列。</li></ul> | last （入力） | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| add_to_array | 配列の末尾に要素を追加します。 | <ul><li>配列：**必須** 要素を追加する配列。</li><li>VALUES：配列に追加する要素。</li></ul> | add_to_array&#x200B;（ARRAY, VALUES） | add_to_array&#x200B;（[&#39;a&#39;, &#39;b&#39;], &#39;c&#39;, &#39;d&#39;） | [&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;] |
| join_arrays | 配列を相互に組み合わせます。 | <ul><li>配列：**必須** 要素を追加する配列。</li><li>VALUES：親配列に追加する配列。</li></ul> | join_arrays&#x200B;（ARRAY, VALUES） | join_arrays&#x200B;（[&#39;a&#39;, &#39;b&#39;], [&#39;c&#39;], [&#39;d&#39;, &#39;e&#39;]） | [&#39;a&#39;, &#39;b&#39;, &#39;c&#39;, &#39;d&#39;, &#39;e&#39;] |
| to_array | 入力のリストを取得し、配列に変換します。 | <ul><li>INCLUDE_NULLS: **必須** 応答配列に NULLS を含めるかどうかを示すブール値。</li><li>値：**必須** 配列に変換される要素。</li></ul> | to_array&#x200B;（INCLUDE_NULLS, VALUES） | to_array （false, 1, null, 2, 3） | `[1, 2, 3]` |
| size_of | 入力サイズを返します。 | <ul><li>入力：**必須** サイズの検索しようとしているオブジェクト。</li></ul> | size_of （INPUT） | `size_of([1, 2, 3, 4])` | 4 |
| upsert_array_append | この関数は、入力配列全体のすべての要素を Profile の配列の末尾に追加するために使用されます。 この関数は、更新時に **のみ** 適用されます。 挿入のコンテキストで使用される場合、この関数は入力をそのまま返します。 | <ul><li>配列：**必須** プロファイルに配列を追加する配列。</li></ul> | upsert_array_append （ARRAY） | `upsert_array_append([123, 456])` | [123、456] |
| upsert_array_replace | この関数は、配列内の要素を置き換えるために使用されます。 この関数は、更新時に **のみ** 適用されます。 挿入のコンテキストで使用される場合、この関数は入力をそのまま返します。 | <ul><li>配列：**必須** プロファイル内の配列を置き換える配列。</li></li> | upsert_array_replace （ARRAY） | `upsert_array_replace([123, 456], 1)` | [123、456] |
| [!BADGE Beta]{type=Informative} array_to_string | 指定された区切り文字を使用して、配列内の要素の文字列表現を結合します。 配列が多次元の場合、結合する前にフラット化されます。 **メモ**：この関数は、宛先で使用されます。 詳しくは、[ ドキュメント ](../destinations/ui/export-arrays-calculated-fields.md) を参照してください。 | <ul><li>SEPARATOR: **必須** 配列の要素の結合に使用する区切り文字。</li><li>配列：**必須** 結合される配列（フラット化後）。</li></ul> | array_to_string （SEPARATOR, ARRAY） | `array_to_string(";", ["Hello", "world"])` | 「こんにちは；world」 |
| [!BADGE Beta]{type=Informative} filterArray* | 述語に基づいて指定された配列をフィルタリングします。 **メモ**：この関数は、宛先で使用されます。 詳しくは、[ ドキュメント ](../destinations/ui/export-arrays-calculated-fields.md) を参照してください。 | <ul><li>配列：**必須** フィルタリングされる配列</li><li>PREDICATE: **必須** 指定された配列の各要素に適用される述語。 | filterArray （ARRAY, PREDICATE） | `filterArray([5, -6, 0, 7], x -> x > 0)` | [5、7] |
| [!BADGE Beta]{type=Informative} transformArray* | 述語に基づいて指定された配列を変換します。 **メモ**：この関数は、宛先で使用されます。 詳しくは、[ ドキュメント ](../destinations/ui/export-arrays-calculated-fields.md) を参照してください。 | <ul><li>ARRAY: **必須** 変換する配列。</li><li>PREDICATE: **必須** 指定された配列の各要素に適用される述語。 | transformArray （ARRAY, PREDICATE） | ` transformArray([5, 6, 7], x -> x + 1)` | [6、7、8] |
| [!BADGE Beta]{type=Informative} flattenArray* | 指定された（多次元の）配列を 1 次元配列にフラット化します。 **メモ**：この関数は、宛先で使用されます。 詳しくは、[ ドキュメント ](../destinations/ui/export-arrays-calculated-fields.md) を参照してください。 | <ul><li>ARRAY: **必須** フラット化する配列。</li></ul> | flattenArray （ARRAY） | flattenArray （[[[&#39;a&#39;, &#39;b&#39;], [&#39;c&#39;, &#39;d&#39;]], [[&#39;e&#39;], [&#39;f&#39;]]]） | [&#39;a&#39;、&#39;b&#39;、&#39;c&#39;、&#39;d&#39;、&#39;e&#39;、&#39;f&#39;] |

{style="table-layout:auto"}

### 階層 – マップ {#map}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| array_to_map | この関数は、オブジェクト配列とキーを入力として受け取り、値をキー、配列要素を値としてキーのフィールドのマップを返します。 | <ul><li>入力：**必須** 最初の null 以外のオブジェクトを検索するオブジェクト配列</li><li>KEY: **必須** キーは、オブジェクト配列内のフィールド名で、値としてオブジェクトである必要があります。</li></ul> | array_to_map （OBJECT[]INPUTS, KEY） | コードサンプルについては、[ 付録 ](#object_to_map) を参照してください。 |
| object_to_map | この関数は、オブジェクトを引数として受け取り、キーと値のペアのマップを返します。 | <ul><li>入力：**必須** 最初の null 以外のオブジェクトを検索するオブジェクト配列</li></ul> | object_to_map （OBJECT_INPUT） | &quot;object_to_map （address）を指定します。入力内容は&quot; + &quot;address: {line1 : \&quot;345 park ave\&quot;,line2 : \&quot;bldg 2\&quot;,City : \&quot;san jose\&quot;,State : \&quot;CA\&quot;,type: \&quot;office\&quot;}&quot;です | 指定されたフィールド名と値のペアを持つマップ、または入力が null の場合は null を返します。 例：`"{line1 : \"345 park ave\",line2: \"bldg 2\",City : \"san jose\",State : \"CA\",type: \"office\"}"` |
| to_map | この関数は、キーと値のペアのリストを受け取り、キーと値のペアのマップを返します。 | | to_map （OBJECT_INPUT） | &quot;to_map （\&quot;firstName\&quot;, \&quot;John\&quot;, \&quot;lastName\&quot;, \&quot;Doe\&quot;）&quot; | 指定されたフィールド名と値のペアを持つマップ、または入力が null の場合は null を返します。 例：`"{\"firstName\" : \"John\", \"lastName\": \"Doe\"}"` |

{style="table-layout:auto"}

### 論理演算子 {#logical-operators}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| decode | キーと、キーと値のペアのリストが配列としてフラット化されている場合、この関数は、キーが見つかった場合は値を返し、デフォルト値が配列に存在する場合はデフォルト値を返します。 | <ul><li>KEY: **必須** 照合するキー。</li><li>OPTIONS: **必須** キーと値のペアをフラット化した配列。 オプションで、最後にデフォルト値を付けることができます。</li></ul> | decode （KEY, OPTIONS） | decode （stateCode, &quot;ca&quot;, &quot;California&quot;, &quot;pa&quot;, &quot;Pennsylvania&quot;, &quot;該当なし&quot;） | 指定された stateCode が「ca」の場合は、「California」です。<br> 指定された stateCode が「pa」の場合は、「Pennsylvania」です。<br>stateCode が次と一致しない場合、「なし」。 |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>EXPRESSION: **必須** 評価されるブール式。</li><li>TRUE_VALUE: **必須** 式が true と評価された場合に返される値。</li><li>FALSE_VALUE: **必須** 式が false と評価された場合に返される値。</li></ul> | iif （EXPRESSION, TRUE_VALUE, FALSE_VALUE） | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |

{style="table-layout:auto"}

### 集計 {#aggregation}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| min | 指定された引数の最小値を返します。自然な順序を使用します。 | <ul><li>OPTIONS: **必須** 相互に比較できる 1 つ以上のオブジェクト。</li></ul> | min （OPTIONS） | min(3, 1, 4) | 1 |
| max | 指定された引数の最大値を返します。自然な順序を使用します。 | <ul><li>OPTIONS: **必須** 相互に比較できる 1 つ以上のオブジェクト。</li></ul> | max （OPTIONS） | max(3, 1, 4) | 4 |

{style="table-layout:auto"}

### タイプ変換 {#type-conversions}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| to_bigint | 文字列を BigInteger に変換します。 | <ul><li>STRING: **必須** BigInteger に変換される文字列。</li></ul> | to_bigint （STRING） | to_bigint&#x200B;（&quot;1000000.34&quot;） | 1000000.34 |
| to_decimal | 文字列を倍精度浮動小数点数に変換します。 | <ul><li>STRING: **必須** Double に変換する文字列。</li></ul> | to_decimal （STRING） | to_decimal （&quot;20.5&quot;） | 20.5 |
| to_float | 文字列を Float に変換します。 | <ul><li>STRING: **必須** Float に変換される文字列。</li></ul> | to_float （STRING） | to_float （&quot;12.3456&quot;） | 12.34566 |
| to_integer | 文字列を整数に変換します。 | <ul><li>文字列：**必須** 整数に変換される文字列。</li></ul> | to_integer （STRING） | to_integer （&quot;12&quot;） | 12 |

{style="table-layout:auto"}

### JSON 関数 {#json}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| json_to_object | 指定された文字列から JSON コンテンツを逆シリアル化します。 | <ul><li>文字列：**必須** シリアル化を解除する JSON 文字列。</li></ul> | json_to_object&#x200B;（STRING） | json_to_object&#x200B;（{&quot;info&quot;:{&quot;firstName&quot;:&quot;John&quot;,&quot;lastName&quot;: &quot;Doe&quot;}}） | JSON を表すオブジェクト。 |

{style="table-layout:auto"}

### 特別業務 {#special-operations}

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| uuid /<br>guid | 擬似ランダム ID を生成します。 | | uuid()<br>guid() | uuid()<br>guid() | 7c0267d2-bb74-4e1a-9275-3bf4fccda5f4<br>c7016dc7-3163-43f7-afc7-2e1c9c206333 |
| `fpid_to_ecid ` | この関数は、FPID 文字列を受け取り、Adobe Experience PlatformおよびAdobe Experience Cloud アプリケーションで使用される ECID に変換します。 | <ul><li>文字列：**必須** ECID に変換される FPID 文字列。</li></ul> | `fpid_to_ecid(STRING)` | `fpid_to_ecid("4ed70bee-b654-420a-a3fd-b58b6b65e991")` | `"28880788470263023831040523038280731744"` |

{style="table-layout:auto"}

### ユーザーエージェント関数 {#user-agent}

次の表に示すユーザーエージェント関数は、次のいずれかの値を返します。

* 電話 – 画面が小さいモバイルデバイス（通常は 7&quot;未満）
* モバイル – まだ識別されていないモバイルデバイス。 このモバイルデバイスは、eReader、タブレット、電話、時計などであり得る。

デバイスフィールド値について詳しくは、このドキュメントの付録の [ デバイスフィールド値のリスト ](#device-field-values) を参照してください。

>[!NOTE]
>
>左右にスクロールして、テーブルの内容をすべて表示してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| ua_os_name | ユーザーエージェント文字列からオペレーティングシステム名を抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_os_name&#x200B;（USER_AGENT） | ua_os_name&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （KHTML、Gecko のような） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | iOS |
| ua_os_version_major | ユーザーエージェント文字列からオペレーティングシステムのメジャーバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_os_version_major&#x200B;（USER_AGENT） | ua_os_version_major&#x200B;s （&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （KHTML、Gecko のような） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | IOS5 |
| ua_os_version | ユーザーエージェント文字列からオペレーティングシステムのバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_os_version&#x200B;（USER_AGENT） | ua_os_version&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （Gecko のような KHTML） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | 5.1.1 |
| ua_os_name_version | ユーザーエージェント文字列からオペレーティングシステムの名前とバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_os_name_version&#x200B;（USER_AGENT） | ua_os_name_version&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （Gecko のような KHTML） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | iOS 5.1.1 |
| ua_agent_version | ユーザーエージェント文字列からエージェントのバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_agent_version&#x200B;（USER_AGENT） | ua_agent_version&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （Gecko のような KHTML） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | 5.1 |
| ua_agent_version_major | ユーザーエージェント文字列からエージェント名とメジャーバージョンを抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_agent_version_major&#x200B;（USER_AGENT） | ua_agent_version_major&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （Gecko のような KHTML） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | Safari 5 |
| ua_agent_name | ユーザーエージェント文字列からエージェント名を抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_agent_name&#x200B;（USER_AGENT） | ua_agent_name&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （KHTML、Gecko のような） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | Safari |
| ua_device_class | ユーザーエージェント文字列からデバイスクラスを抽出します。 | <ul><li>USER_AGENT: **必須** ユーザーエージェント文字列。</li></ul> | ua_device_class&#x200B;（USER_AGENT） | ua_device_class&#x200B;（&quot;Mozilla/5.0 （iPhone、Mac OS X のような CPU iPhone OS 5_1_1） AppleWebKit/534.46 （KHTML、Gecko のような） Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;） | Phone |

{style="table-layout:auto"}

### Analytics 関数 {#analytics}

>[!NOTE]
>
>WebSDK およびAdobe Analytics フローでは、次の分析関数のみを使用できます。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| aa_get_event_id | Analytics イベント文字列からイベント ID を抽出します。 | <ul><li>EVENT_STRING: **必須** Analytics イベント文字列のコンマ区切りです。</li><li>EVENT_NAME: **必須** 抽出するイベント名と ID。</li></ul> | aa_get_event_id （EVENT_STRING, EVENT_NAME） | aa_get_event_id （&quot;event101=5:123456,scOpen&quot;, &quot;event101&quot;） | 123456 |
| aa_get_event_value | Analytics イベント文字列からイベント値を抽出します。 イベントの値が指定されていない場合は、1 が返されます。 | <ul><li>EVENT_STRING: **必須** Analytics イベント文字列のコンマ区切りです。</li><li>EVENT_NAME: **必須** 値を抽出するイベント名。</li></ul> | aa_get_event_value （EVENT_STRING, EVENT_NAME） | aa_get_event_value （&quot;event101=5:123456,scOpen&quot;, &quot;event101&quot;） | 5 |
| aa_get_product_categories | Analytics 製品文字列から製品カテゴリを抽出します。 | <ul><li>PRODUCTS_STRING: **必須** Analytics 製品文字列。</li></ul> | aa_get_product_categories （PRODUCTS_STRING） | aa_get_product_categories （&quot;;Example product 1;1;3.50,Example category 2;Example product 2;1;5.99&quot;） | [null,&quot;カテゴリ 2 の例&quot;] |
| aa_get_product_names | Analytics 製品文字列から製品名を抽出します。 | <ul><li>PRODUCTS_STRING: **必須** Analytics 製品文字列。</li></ul> | aa_get_product_names （PRODUCTS_STRING） | aa_get_product_names （&quot;;Example product 1;1;3.50,Example category 2;Example product 2;1;5.99&quot;） | [ 「Example product 1」,「Example product 2」 ] |
| aa_get_product_quantities | Analytics 製品文字列から数量を抽出します。 | <ul><li>PRODUCTS_STRING: **必須** Analytics 製品文字列。</li></ul> | aa_get_product_quantities （PRODUCTS_STRING） | aa_get_product_quantities （&quot;;Example product 1;1;3.50,Example category 2;Example product 2&quot;） | [&quot;1&quot;, null] |
| aa_get_product_prices | Analytics 製品文字列から価格を抽出します。 | <ul><li>PRODUCTS_STRING: **必須** Analytics 製品文字列。</li></ul> | aa_get_product_prices （PRODUCTS_STRING） | aa_get_product_prices （&quot;;Example product 1;1;3.50,Example category 2;Example product 2&quot;） | [&quot;3.50&quot;、null] |
| aa_get_product_event_values | 名前付きイベントの値を文字列の配列として products 文字列から抽出します。 | <ul><li>PRODUCTS_STRING: **必須** Analytics 製品文字列。</li><li>EVENT_NAME: **必須** 値の抽出元のイベント名。</li></ul> | aa_get_product_event_values （PRODUCTS_STRING, EVENT_NAME） | aa_get_product_event_values （&quot;;Example product 1;1;4.20;event1=2.3\|event2=5:1,;Example product 2;1;4.20;event1=3\|event2=2:2&quot;, &quot;event1&quot;） | [ 「2.3」、「3」 ] |
| aa_get_product_evars | 製品文字列から、指定されたイベントの evar 値を文字列の配列として抽出します。 | <ul><li>PRODUCTS_STRING: **必須** Analytics 製品文字列。</li><li>EVAR名：**必須** 抽出するeVar名。</li></ul> | aa_get_product_evars （PRODUCTS_STRING, EVENT_NAME） | aa_get_product_evars （&quot;;Example product;1;6.69;;eVar1=マーチャンダイジング値&quot;, &quot;eVar1&quot;） | [ 「マーチャンダイジング値」 ] |

{style="table-layout:auto"}

<!-- | aa_get_product_events | Extracts a named event from the products string as an array of objects. | <ul><li>PRODUCTS_STRING: **Required** The Analytics products string.</li><li>EVENT_NAME: **Required** The event name to extract values from.</li></ul> | aa_get_product_events(PRODUCTS_STRING, EVENT_NAME) | aa_get_product_events(";Example product 1;1;4.20;event1=2.3\|event2=5:1,;Example product 2;1;4.20;event1=3\|event2=2:2", "event2") | [`{"id": "1","value", "5"}`, `{"id": "2","value", "1"}`] |
| aa_get_product_event_ids | Extracts the IDs for the named event from the products string as an array of strings. | <ul><li>PRODUCTS_STRING: **Required** The Analytics products string.</li><li>EVENT_NAME: **Required** The event name to extract values from.</li></ul> | aa_get_product_event_ids(PRODUCTS_STRING, EVENT_NAME) | aa_get_product_event_ids(";Example product 1;1;4.20;event1=2.3\|event2=5:1,;Example product 2;1;4.20;event1=3\|event2=2:2", "event2") | ["1", "2"] | -->

### オブジェクトのコピー {#object-copy}

>[!TIP]
>
>ソース内のオブジェクトが XDM 内のオブジェクトにマッピングされると、オブジェクトのコピー機能が自動的に適用されます。 ユーザーによる追加のアクションは必要ありません。

オブジェクトのコピー機能を使用すると、マッピングを変更せずにオブジェクトの属性を自動的にコピーできます。 例えば、ソースデータの構造が次のような場合：

```json
address{
        line1: 4191 Ridgebrook Way,
        city: San Jose,
        state: California
        }
```

および次の XDM 構造：

```json
addr{
    addrLine1: 4191 Ridgebrook Way,
    city: San Jose,
    state: California
    }
```

その後、マッピングは次のようになります。

```json
address -> addr
address.line1 -> addr.addrLine1
```

上記の例では、`address` オブジェクトが `addr` にマッピングされているので、`city` 属性と `state` 属性も実行時に自動的に取り込まれます。 XDM 構造で `line2` 属性を作成し、入力データにも `address` オブジェクトの `line2` が含まれている場合、マッピングを手動で変更しなくても、入力データも自動的に取り込まれます。

自動マッピングが機能するには、次の前提条件を満たす必要があります。

* 親レベルオブジェクトはマッピングする必要があります。
* XDM スキーマに新しい属性が作成されている必要があります。
* 新しい属性は、ソーススキーマと XDM スキーマで名前が一致している必要があります。

前提条件のいずれかが満たされない場合は、データ準備を使用して、ソーススキーマを XDM スキーマに手動でマッピングする必要があります。

## 付録

次に、データ準備のマッピング機能の使用に関する追加情報を示します

### 特殊文字 {#special-characters}

予約文字とそれらに対応するエンコードされた文字のリストを次の表に示します。

| 予約文字 | エンコードされた文字 |
| --- | --- |
| space | %20 |
| ! | %21 |
| 」 | %22 |
| # | %23 |
| $ | %24 |
| % | %25 |
| &amp; | %26 |
| &#39; | %27 |
| ( | %28 |
| ） | %29 |
| * | %2A |
| + | %2B |
| , | %2C |
| / | %2F |
| ： | %3A |
|   | %3B |
| &lt; | %3C |
| = | %3D |
| > | %3E |
| ? | %3F |
| @ | %40 |
| [ | %5B |
| | | %5C |
| ] | %5D |
| ^ | %5E |
| &#39; | %60 |
| ~ | %7E |

{style="table-layout:auto"}

### デバイスフィールドの値 {#device-field-values}

次の表に、デバイスフィールド値のリストと対応する説明を示します。

| デバイス | 説明 |
| --- | --- |
| Desktop | デスクトップまたはラップトップ型のデバイス。 |
| 匿名化 | 匿名デバイス。 場合によっては、これらは匿名化ソフトウェアによって変更された `useragents` です。 |
| 不明 | 不明なデバイスです。 これらは通常、デバイスに関する情報を含まない `useragents` です。 |
| Mobile | まだ識別されていないモバイルデバイス。 このモバイルデバイスは、eReader、タブレット、電話、時計などであり得る。 |
| タブレット | 大きな画面（通常は 7 インチを超える）を備えたモバイルデバイス。 |
| Phone | 画面が小さいモバイルデバイス（通常は 7&quot;未満）。 |
| ウォッチ | 画面が小さいモバイルデバイス（通常は 2 インチ未満）。 これらのデバイスは、通常、スマートフォンやタブレットタイプのデバイスの追加画面として動作します。 |
| 拡張現実 | AR 機能を備えたモバイルデバイス。 |
| バーチャルリアリティ | VR 機能を備えたモバイルデバイス。 |
| eReader | タブレットに似たデバイスですが、通常は [!DNL eInk] の画面が表示されます。 |
| セットトップボックス | テレビのサイズの画面を介した操作を可能にする接続されたデバイス。 |
| TV | セットトップボックスに似たデバイスですが、テレビに組み込まれています。 |
| 家電製品 | 冷蔵庫のような（通常は大きな）家電製品。 |
| ゲーム機 | [!DNL Playstation] や [!DNL XBox] などの固定ゲームシステムを提供する。 |
| 携帯型ゲーム機 | [!DNL Nintendo Switch] のようなモバイルゲームシステム。 |
| 音声 | [!DNL Amazon Alexa] や [!DNL Google Home] などの音声駆動デバイス。 |
| Car | 車両ベースのブラウザー。 |
| ロボット | Web サイトを訪問するロボット。 |
| ロボットモバイル | Web サイトを訪問し、モバイル訪問者として見られることを示すロボット。 |
| ロボットイミテータ | ウェブサイトを訪問するロボットは、[!DNL Google] のようなロボットであるふりをしますが、そうではありません。 **注意**：ほとんどの場合、ロボットイミテーターは実際にロボットです。 |
| Cloud | クラウドベースのアプリケーション。 これらはロボットでもハッカーでもなく、接続する必要のあるアプリケーションです。 これには、[!DNL Mastodon] サーバーが含まれます。 |
| ハッカー | このデバイス値は、`useragent` 文字列でスクリプトが検出された場合に使用されます。 |

{style="table-layout:auto"}

### コードサンプル {#code-samples}

#### map_get_values {#map-get-values}

+++選択すると例が表示されます

```json
 example = "map_get_values(book_details,\"author\") where input is : {\n" +
        "    \"book_details\":\n" +
        "    {\n" +
        "        \"author\": \"George R. R. Martin\",\n" +
        "        \"price\": 17.99,\n" +
        "        \"ISBN\": \"ISBN-978-0553801477\"\n" +
        "    }\n" +
        "}",
      result = "{\"author\": \"George R. R. Martin\"}"
```

+++

#### map_has_keys {#map_has_keys}

+++選択すると例が表示されます

```json
 example = "map_has_keys(book_details,\"author\")where input is : {\n" +
        "    \"book_details\":\n" +
        "    {\n" +
        "        \"author\": \"George R. R. Martin\",\n" +
        "        \"price\": 17.99,\n" +
        "        \"ISBN\": \"ISBN-978-0553801477\"\n" +
        "    }\n" +
        "}",
      result = "true"
```

+++

#### add_to_map {#add_to_map}

+++選択すると例が表示されます

```json
example = "add_to_map(book_details, book_details2) where input is {\n" +
        "    \"book_details\":\n" +
        "    {\n" +
        "        \"author\": \"George R. R. Martin\",\n" +
        "        \"price\": 17.99,\n" +
        "        \"ISBN\": \"ISBN-978-0553801477\"\n" +
        "    }\n" +
        "}" +
        "{\n" +
        "    \"book_details2\":\n" +
        "    {\n" +
        "        \"author\": \"Neil Gaiman\",\n" +
        "        \"price\": 17.99,\n" +
        "        \"ISBN\": \"ISBN-0-380-97365-0\"\n" +
        "        \"publisher\": \"William Morrow\"\n" +
        "    }\n" +
        "}",
      result = "{\n" +
        "    \"book_details\":\n" +
        "    {\n" +
        "        \"author\": \"George R. R. Martin\",\n" +
        "        \"price\": 17.99,\n" +
        "        \"ISBN\": \"ISBN-978-0553801477\"\n" +
        "        \"publisher\": \"William Morrow\"\n" +
        "    }\n" +
        "}",
      returns = "A new map with all elements from map and addends"
```

+++

#### object_to_map {#object_to_map}

**構文 1**

+++選択すると例が表示されます

```json
example = "object_to_map(\"firstName\", \"John\", \"lastName\", \"Doe\")",
result = "{\"firstName\" : \"John\", \"lastName\": \"Doe\"}"
```

+++

**構文 2**

+++選択すると例が表示されます

```json
example = "object_to_map(address) where input is " +
  "address: {line1 : \"345 park ave\",line2: \"bldg 2\",City : \"san jose\",State : \"CA\",type: \"office\"}",
result = "{line1 : \"345 park ave\",line2: \"bldg 2\",City : \"san jose\",State : \"CA\",type: \"office\"}"
```

+++

**構文 3**

+++選択すると例が表示されます

```json
example = "object_to_map(addresses,type)" +
        "\n" +
        "[\n" +
        "    {\n" +
        "        \"line1\": \"345 park ave\",\n" +
        "        \"line2\": \"bldg 2\",\n" +
        "        \"City\": \"san jose\",\n" +
        "        \"State\": \"CA\",\n" +
        "        \"type\": \"home\"\n" +
        "    },\n" +
        "    {\n" +
        "        \"line1\": \"345 park ave\",\n" +
        "        \"line2\": \"bldg 2\",\n" +
        "        \"City \": \"san jose\",\n" +
        "        \"State\": \"CA\",\n" +
        "        \"type\": \"work\"\n" +
        "    },\n" +
        "    {\n" +
        "        \"line1\": \"345 park ave\",\n" +
        "        \"line2\": \"bldg 2\",\n" +
        "        \"City \": \"san jose\",\n" +
        "        \"State\": \"CA\",\n" +
        "        \"type\": \"office\"\n" +
        "    }\n" +
        "]" ,
result = "{\n" +
        "    \"home\":\n" +
        "    {\n" +
        "        \"line1\": \"345 park ave\",\n" +
        "        \"line2\": \"bldg 2\",\n" +
        "        \"City\": \"san jose\",\n" +
        "        \"State\": \"CA\",\n" +
        "        \"type\": \"home\"\n" +
        "    },\n" +
        "    \"work\":\n" +
        "    {\n" +
        "        \"line1\": \"345 park ave\",\n" +
        "        \"line2\": \"bldg 2\",\n" +
        "        \"City \": \"san jose\",\n" +
        "        \"State\": \"CA\",\n" +
        "        \"type\": \"work\"\n" +
        "    },\n" +
        "    \"office\":\n" +
        "    {\n" +
        "        \"line1\": \"345 park ave\",\n" +
        "        \"line2\": \"bldg 2\",\n" +
        "        \"City \": \"san jose\",\n" +
        "        \"State\": \"CA\",\n" +
        "        \"type\": \"office\"\n" +
        "    }\n" +
        "}" 
```

+++

#### array_to_map {#array_to_map}

+++選択すると例が表示されます

```json
example = "array_to_map(addresses, \"type\") where addresses is\n" +
  "\n" +
  "[\n" +
  "    {\n" +
  "        \"line1\": \"345 park ave\",\n" +
  "        \"line2\": \"bldg 2\",\n" +
  "        \"City\": \"san jose\",\n" +
  "        \"State\": \"CA\",\n" +
  "        \"type\": \"home\"\n" +
  "    },\n" +
  "    {\n" +
  "        \"line1\": \"345 park ave\",\n" +
  "        \"line2\": \"bldg 2\",\n" +
  "        \"City \": \"san jose\",\n" +
  "        \"State\": \"CA\",\n" +
  "        \"type\": \"work\"\n" +
  "    },\n" +
  "    {\n" +
  "        \"line1\": \"345 park ave\",\n" +
  "        \"line2\": \"bldg 2\",\n" +
  "        \"City \": \"san jose\",\n" +
  "        \"State\": \"CA\",\n" +
  "        \"type\": \"office\"\n" +
  "    }\n" +
  "]" ,
result = "{\n" +
  "    \"home\":\n" +
  "    {\n" +
  "        \"line1\": \"345 park ave\",\n" +
  "        \"line2\": \"bldg 2\",\n" +
  "        \"City\": \"san jose\",\n" +
  "        \"State\": \"CA\",\n" +
  "        \"type\": \"home\"\n" +
  "    },\n" +
  "    \"work\":\n" +
  "    {\n" +
  "        \"line1\": \"345 park ave\",\n" +
  "        \"line2\": \"bldg 2\",\n" +
  "        \"City \": \"san jose\",\n" +
  "        \"State\": \"CA\",\n" +
  "        \"type\": \"work\"\n" +
  "    },\n" +
  "    \"office\":\n" +
  "    {\n" +
  "        \"line1\": \"345 park ave\",\n" +
  "        \"line2\": \"bldg 2\",\n" +
  "        \"City \": \"san jose\",\n" +
  "        \"State\": \"CA\",\n" +
  "        \"type\": \"office\"\n" +
  "    }\n" +
  "}",
returns = "Returns a map with given field name and value pairs or null if input is null"
```

+++


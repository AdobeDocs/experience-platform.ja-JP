---
keywords: エクスペリエンス Platform、home、ポピュラーなトピック、map .csv、map csv、xdm、xdm、xdm、、、ui ガイド、マッパー、マッピング、マッピング、マッピング、フィールド、マッピング、
solution: Experience Platform
title: データ準備マップ関数
topic-legacy: overview
description: このドキュメントでは、データ準備に使用されるマッピング関数について説明します。
exl-id: e95d9329-9dac-4b54-b804-ab5744ea6289
source-git-commit: c4fc370929000a5d34ce2696da7efe8c75abd07c
workflow-type: tm+mt
source-wordcount: '3940'
ht-degree: 18%

---

# データ準備マップ関数

データ準備関数を使用すると、ソースフィールドに入力した内容に基づいて値を計算および計算できます。

## フィールド

フィールド名として使用できる識別子は、Unicode 文字と数字で、文字、ドル記号 ( `$` )、またはアンダースコア文字 () のように制限されて `_` います。 変数名も大文字と小文字が区別されます。

この規則に準拠していないフィールド名を使用する場合は、フィールド名を囲む必要があり `${}` ます。 そのため、例えば、「姓」や「First.Name」などのフィールド名を指定する場合は、その名前を折り返して使用する必要があり `${First Name}` `${First.Name}` ます。

さらに、フィールド名が **** 次のような予約キーワードの場合は、それを囲む必要があり `${}` ます。

```console
new, mod, or, break, var, lt, for, false, while, eq, gt, div, not, null, continue, else, and, ne, true, le, if, ge, return
```

サブフィールド内のデータには、ドット表記を使用してアクセスできます。 例えば、フィールドにアクセスするオブジェクトがあった場合は `name` `firstName` 、を使用 `name.firstName` します。

## 関数のリスト

以下の表は、サポートされているマッピング関数をすべて示しています。これは、サンプル式とその出力結果を含みます。

### 文字列関数 {#string}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| concat | 指定したストリングが連結されます。 | <ul><li>ストリング: 連結されるストリング。</li></ul> | concat (STRING_1、STRING_2) | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| explode | 正規表現に基づいて文字列を分割し、部分の配列を返します。ストリングを分割するための regex を指定することもできます。 デフォルトでは、スプリットは「,」に解決されます。 次の区切り記号は、 **エスケープする必要があり** `\` ます。 `+, ?, ^, |, ., [, (, {, ), *, $, \` 区切り文字として複数の文字を指定した場合、その区切り文字は複数の区切り文字として扱われます。 | <ul><li>STRING: **必** split の対象となるストリングです。</li><li>REGEX: *省略可能です。* ストリングを分割するために使用できる正規表現です。</li></ul> | 分解 (STRING、REGEX) | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の場所/インデックスを返します。 | <ul><li>INPUT: **必須** 検索対象のストリングです。</li><li>SUBSTRING: **必要なのは、** ストリング内で検索されるサブストリングです。</li><li>START_POSITION: ** ストリング内での検索を開始する位置を指定します (オプション)。</li><li>出現回数 *:* 開始位置から、n 番目の検索を実行します (オプション)。 デフォルトの重み付けは 1 です。 </li></ul> | instr (INPUT、SUBSTRING、START_POSITION、出現) | instr (&quot;adobe .com&quot;, &quot;com&quot;) | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合は、その文字列を置き換えます。 | <ul><li>INPUT: **** 入力ストリングです。</li><li>TO_FIND: **** 入力値を参照するために必要なストリングです。</li><li>TO_REPLACE: **必要なのは** 、&quot;TO_FIND&quot; 内の値を置き換えるストリングです。</li></ul> | replacestr (インプット、TO_FIND、TO_REPLACE) | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 指定された長さのサブ文字列を返します。 | <ul><li>INPUT: **** 入力ストリングです。</li><li>START_INDEX: 必須ではあり **** ません。これは、部分文字列を開始する入力ストリングのインデックスです。</li><li>長さ: **** substring の長さを指定します。</li></ul> | substr (INPUT、START_INDEX、LENGTH) | substr(&quot;This is a substring test&quot;, 7, 8) | &quot;サブセット&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | <ul><li>INPUT: **必要な** のは、小文字に変換されるストリングです。</li></ul> | 下 (入力) | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | <ul><li>INPUT: **必要な** のは、大文字に変換されるストリングです。</li></ul> | 上側 (入力した場合) | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り記号で入力文字列を分割します。区切り記号には、次の文字を **使用してエスケープする必要があります** `\` `\` 。 複数の区切り記号を指定した場合、ストリング **内の任意の区切り記号に、その区切り文字が表示され** ます。 | <ul><li>INPUT: **** split される入力ストリングを指定する必要があります。</li><li>SEPARATOR: **必須** 入力を分割するために使用するストリングです。</li></ul> | 分割 (入力、区切り) | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | 区切り記号を使用してオブジェクトのリストを結合します。 | <ul><li>SEPARATOR: **必** objects に参加するために使用されるストリングです。</li><li>オブジェクト: **必須** 結合されるストリングの配列です。</li></ul> | `join(SEPARATOR, [OBJECTS])` | `join(" ", to_array(true, "Hello", "world"))` | &quot;Hello world&quot; |
| lpad | ストリングの左側に他の指定したストリングが埋め込まれます。 | <ul><li>INPUT: **Required** には、埋め込む対象のストリングを指定する必要があります。 このストリングは null でもかまいません。</li><li>COUNT: **** 埋め込みが可能なストリングのサイズです。</li><li>PADDING: **入力に埋め** 込むストリングを指定する必要があります。 Null または空の場合は、1つのスペースとして扱われます。</li></ul> | lpad (INPUT、COUNT、PADDING) | lpad (&quot;bat&quot;、8、&quot;yz&quot;) | &quot;yzyzybat&quot; |
| rpad | ストリングの右側に、その他の指定したストリングが埋め込まれます。 | <ul><li>INPUT: **Required** には、埋め込む対象のストリングを指定する必要があります。 このストリングは null でもかまいません。</li><li>COUNT: **** 埋め込みが可能なストリングのサイズです。</li><li>PADDING: **入力に埋め** 込むストリングを指定する必要があります。 Null または空の場合は、1つのスペースとして扱われます。</li></ul> | r pad (入力、回数、パディング) | r pad (&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;batyzyzy&quot; |
| left | 指定されたストリングの最初の「n」文字を取得します。 | <ul><li>STRING: **必須** 最初の「n」文字を取得するストリングです。</li><li>COUNT: **** ストリングから取得する「n」文字を指定する必要があります。</li></ul> | 左寄せ (文字列、文字数) | left (&quot;abcde&quot;, 2) | ab |
| そうです | 指定されたストリングの最後の &quot;n&quot; 文字を取得します。 | <ul><li>STRING: **必須** 最後の &quot;n&quot; 文字を取得するストリングを指定する必要があります。</li><li>COUNT: **** ストリングから取得する「n」文字を指定する必要があります。</li></ul> | right (文字列、文字数) | right (&quot;abcde&quot;, 2) | 防止 |
| ltrim | ストリングの先頭から空白が削除されます。 | <ul><li>STRING: **必須** 空白を削除する文字列です。</li></ul> | ltrim (STRING) | ltrim (&quot;こんにちは&quot;) | はじめ |
| rtrim | ストリングの末尾にあるホワイトスペースを削除します。 | <ul><li>STRING: **必須** 空白を削除する文字列です。</li></ul> | rtrim (STRING) | rtrim (&quot;hello&quot;) | はじめ |
| trim | ストリングの先頭と末尾からホワイトスペースを削除します。 | <ul><li>STRING: **必須** 空白を削除する文字列です。</li></ul> | trim (STRING) | trim (&quot;hello&quot;) | はじめ |
| 次と等しい | 2つのストリングが等しいかどうかを比較します。 この関数では、大文字と小文字が区別されます。 | <ul><li>STRING1: **** 1 番目のストリングは比較する必要があります。</li><li>STRING2: **必要な** 比較対象の2番目のストリング。</li></ul> | STRING1equals (STRING2) | 「string1」となります。equals (&quot;STRING1&quot;) | false |
| equalSignoreCase | 2つのストリングが等しいかどうかを比較します。 この関数で **は** 、大文字と小文字は区別されません。 | <ul><li>STRING1: **** 1 番目のストリングは比較する必要があります。</li><li>STRING2: **必要な** 比較対象の2番目のストリング。</li></ul> | STRING1equalsIgnoreCase (STRING2) | 「string1」となります。equalsIgnoreCase (&quot;STRING1) | true |

{style=&quot;table-layout:auto&quot;}

### 正規表現関数

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| extract_regex | 正規表現に基づいて、入力文字列からグループを抽出します。 | <ul><li>STRING: **必須** グループを抽出するストリングです。</li><li>REGEX: グループに一致させる正規表現を指定する **必要が** あります。</li></ul> | extract_regex (STRING、REGEX) | extract_regex (&quot;E259, E259B_009, 1_1&quot;, &quot;( [ ^, ] +), ^, [ ] *, ( [ ^, ] +)&quot;) | [&quot;E259、E259B_009、1_1&quot;、&quot;E259&quot;、&quot;1_1&quot;] |
| matches_regex | ストリングが inputted 正規表現に一致するかどうかをチェックします。 | <ul><li>STRING: **必須** チェックしようとしているストリングは、正規表現と一致しています。</li><li>REGEX: **** 比較対象となる正規表現を指定する必要があります。</li></ul> | matches_regex (STRING、REGEX) | matches_regex (&quot;E259, E259B_009, 1_1&quot;, &quot;( [ ^, ] +), ^, [ ] *, ( [ ^, ] +)&quot;) | 満たす |

{style = &quot;テーブル-layout: auto&quot;}

### ハッシュ関数 {#hashing}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| sha1 | 入力を取得し、セキュアハッシュアルゴリズム 1 (SHA-1) を使用してハッシュ値を生成します。 | <ul><li>INPUT: **** ハッシュされるプレーンテキストを指定する必要があります。</li><li>CHARSET: *オプション* 文字セットの名前を指定します。 指定可能な値には、UTF-8、UTF-16、ISO-8859-1 および US-ASCII が含まれます。</li></ul> | sha1 (入力、文字セット) | sha1 (&quot;my text&quot;、&quot;UTF-8&quot;) | c3599c11e47719df18a24&#x200B;48690840c5dfcce3c80 |
| sha256 | 入力を取得し、セキュアなハッシュアルゴリズム 256 (SHA-256) を使用してハッシュ値を生成します。 | <ul><li>INPUT: **** ハッシュされるプレーンテキストを指定する必要があります。</li><li>CHARSET: *オプション* 文字セットの名前を指定します。 指定可能な値には、UTF-8、UTF-16、ISO-8859-1 および US-ASCII が含まれます。</li></ul> | sha256 (入力、文字セット) | sha256 (&quot;my text&quot;、&quot;UTF-8&quot;) | 7330d2b39ca35eaf4cb95fc846c21&#x200B;ee6a39af698154a83a586ee270a0d372104 |
| sha512 | 入力を取得し、セキュアなハッシュアルゴリズム 512 (SHA-512) を使用してハッシュ値を生成します。 | <ul><li>INPUT: **** ハッシュされるプレーンテキストを指定する必要があります。</li><li>CHARSET: *オプション* 文字セットの名前を指定します。 指定可能な値には、UTF-8、UTF-16、ISO-8859-1 および US-ASCII が含まれます。</li></ul> | sha512 (入力、文字セット) | sha512 (&quot;my text&quot;、&quot;UTF-8&quot;) | a3d7e45a0d9be5fd4e4b9a3b8c9c2163c21ef&#x200B;708bf11b4232bb21d2a8704ada2cdcd7b367dd0788a89&#x200B;a5c908cfe377aceb1072a7b386b7d4fd2ff68a8fd24d16 |
| md5 | 入力を取得し、MD5 を使用してハッシュ値を生成します。 | <ul><li>INPUT: **** ハッシュされるプレーンテキストを指定する必要があります。</li><li>CHARSET: *オプション* 文字セットの名前を指定します。 指定可能な値には、UTF-8、UTF-16、ISO-8859-1 および US-ASCII が含まれます。 </li></ul> | md5 (入力、文字セット) | md5 (&quot;my text&quot;、&quot;UTF-8&quot;) | d3b96ce8c9fb4&#x200B;e9bd0198d03ba6852c7 |
| crc32 | 入力を取得するには、巡回冗長検査 (CRC) アルゴリズムを使用して32ビットの巡回コードを作成します。 | <ul><li>INPUT: **** ハッシュされるプレーンテキストを指定する必要があります。</li><li>CHARSET: *オプション* 文字セットの名前を指定します。 指定可能な値には、UTF-8、UTF-16、ISO-8859-1 および US-ASCII が含まれます。</li></ul> | crc32 (入力、文字セット) | crc32 (「my text」、「UTF-8」) | 8df92e80 |

{style = &quot;テーブル-layout: auto&quot;}

### URL 関数 {#url}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| get_url_protocol | 指定された URL からプロトコルを返します。 入力が無効な場合は、null が返されます。 | <ul><li>URL: **必要なのは、** プロトコルを抽出する必要がある url です。</li></ul> | get_url_protocol (URL) | get_url_protocol (&quot;https://platform.adobe.com/home&quot;) | https |
| get_url_host | 指定された URL のホストを返します。 入力が無効な場合は、null が返されます。 | <ul><li>URL: **必要なのは、ホストを展開する url です** 。</li></ul> | get_url_host (URL) | get_url_host (&quot;https://platform.adobe.com/home&quot;) | platform.adobe.com |
| get_url_port | 指定された URL のポートを返します。 入力が無効な場合は、null が返されます。 | <ul><li>URL: **必要なのは** 、ポートを展開する url です。</li></ul> | get_url_port (URL) | get_url_port (「sftp://」をお持ちの場合は、 | 22 |
| get_url_path | 指定した URL のパスを返します。 デフォルトでは、完全パスが返されます。 | <ul><li>URL: **必要なの** は、パスを抽出する url です。</li><li>FULL_PATH: ** フルパスを返すかどうかを決定するブール値です。 False に設定されている場合は、パスの末尾だけが返されます。</li></ul> | get_url_path (URL、FULL_PATH) | get_url_path (「sftp://」をお持ちの場合は、 | 「ホーム/アーカイブ」と「社員」 |
| get_url_query_str | 指定された URL のクエリストリングを返します。 | <ul><li>URL: **** クエリ文字列を取得しようとする url を指定する必要があります。</li><li>アンカー: **必須は、** クエリーストリング内のアンカーの処理方法を指定します。 指定できる値は、&quot;retain&quot;、&quot;remove&quot;、&quot;append&quot; のいずれかです。<br><br>値が &quot;retain&quot; に設定されている場合、アンカーは戻り値に割り当てられます。<br>値が &quot;remove&quot; の場合は、戻り値からアンカーが削除されます。<br>値が「append」の場合は、アンカーポイントは別の値として返されます。</li></ul> | get_url_query_str (URL、アンカー) | get_url_query_str (&quot;foo:///com: 8042/オーバー/ありますか? name = tlret # 鼻&quot;、&quot;retain&quot;) <br> get_url_query_str (&quot;foo:/8042) のようになります。 name = tlret # 鼻&quot;、&quot;remove&quot;) <br> get_url_query_str (&quot;foo://8042/) を指定します。 name = tlret # 鼻「、append」) | `{"name": "ferret#nose"}`<br>`{"name": "ferret"}`<br>`{"name": "ferret", "_anchor_": "nose"}` |

{style = &quot;テーブル-layout: auto&quot;}

### 日付および時間関数 {#date-and-time}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。この関数について詳しくは、 `date` データ形式の取り扱いガイドの「日付」の項を参照 [ ](./data-handling.md#dates) してください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| now | 現在の時刻を取得します。 |  | now() | now () | `2020-09-23T10:10:24.556-07:00[America/Los_Angeles]` |
| timestamp | 現在の Unix 時間を取得します。 |  | timestamp() | timestamp () | 1571850624571 |
| format | 指定された形式に従って入力日をフォーマットします。 | <ul><li>日付: **** 入力日付、つまり、フォーマットするのは、Zone eddatetime オブジェクトです。</li><li>フォーマット: **** 日付の変更後の形式を指定する必要があります。</li></ul> | フォーマット (日付、フォーマット) | format (2019-10-23T11 :24: 00 + 00:00、&quot;YYYY-MM-DD HH :mm: ss&quot;) | 「2019-10-23 11 :24: 35」 |
| dformat | 指定された形式に従ってタイムスタンプを日付文字列に変換します。 | <ul><li>タイムスタンプ **:** フォーマットするタイムスタンプを指定する必要があります。 これはミリ秒単位で作成されます。</li><li>フォーマット: **** タイムスタンプを変更するフォーマットを指定する必要があります。</li></ul> | dformat (タイムスタンプ、フォーマット) | dformat (1571829875000、「yyyy-mm-dd」をお持ちの場合は、「_」 HH ss ということに :mm: なります。SSSX &quot;) | &quot;2019-10-23T11 :24: 35.000 z&quot; |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付: **必須「日付」を** 表すストリングです。</li><li>フォーマット: **必要なのは** 、日付のフォーマットを表すストリングです。</li><li>DEFAULT_DATE: **** 指定された日付が null の場合は、戻り値としてデフォルトの日付を指定する必要があります。</li></ul> | 日付 (日付、形式、DEFAULT_DATE) | date (&quot;2019-10-23 11:24&quot;, &quot;yyyy-mm-dd HH: MM&quot;、now ()) | &quot;2019-10-23T11: 24Z&quot; |
| 古い | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付: **必須「日付」を** 表すストリングです。</li><li>フォーマット: **必要なのは** 、日付のフォーマットを表すストリングです。</li></ul> | 日付 (日付、フォーマット) | date (&quot;2019-10-23 11:24&quot;, &quot;yyyy-mm-dd HH: MM&quot;) | &quot;2019-10-23T11: 24Z&quot; |
| 古い | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>日付: **必須「日付」を** 表すストリングです。</li></ul> | 日付 (日付) | date (&quot;2019-10-23 11:24&quot;) | &quot;2019-10-23T11: 24Z&quot; |
| date_part | 日付の一部を取得します。次のコンポーネント値がサポートされています。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;quarter&quot;<br>&quot;qq&quot;<br>&quot;q&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;dayofyear&quot;<br>&quot;dy&quot;<br>&quot;y&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;week&quot;<br>&quot;ww&quot;<br>&quot;w&quot;<br><br>&quot;weekday&quot;<br>&quot;dw&quot;<br>&quot;w&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br>&quot;hh24&quot;<br>&quot;hh12&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot;<br><br>&quot;millisecond&quot;<br>&quot;ms&quot; | <ul><li>コンポーネント: **必須** 日付の部分を表すストリングです。 </li><li>日付: **** 日付を標準形式で指定します。</li></ul> | date_part (コンポーネント、日付) | date_part (&quot;MM&quot;、日付 (&quot;2019-10-17 11:&quot; :55: )) | 10 |
| set_date_part | 指定された日付のコンポーネントを置き換えます。次のコンポーネントが受け入れられます。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot; | <ul><li>コンポーネント: **必須** 日付の部分を表すストリングです。 </li><li>値: **必須** 指定された日付について、コンポーネントに設定する値です。</li><li>日付: **** 日付を標準形式で指定します。</li></ul> | set_date_part (コンポーネント、値、日付) | set_date_part (&quot;m&quot;、4、日付 (&quot;2016-11-09T11 :44: 44.797&quot;) | &quot;2016 年-04-09T11 :44: 44.797&quot; |
| make_date_time | 部分から日付を作成します。この関数は、make_timestamp を使用して実行することもできます。 | <ul><li>YEAR: **** 年は4桁で表記されています。</li><li>月: **** 月が必須です。 指定できる値は 1 ~ 12 です。</li><li>DAY: **** 日付を指定する必要があります。 指定できる値は 1 ~ 31 です。</li><li>時刻: **** 時刻を指定します。 指定できる値は 0 ~ 23 です。</li><li>分: **** 分を指定する必要があります。 指定できる値は、0 ~ 59 です。</li><li>ナノ秒 **:** ナノ秒の値が必要です。 指定できる値は、0 ~ 999999999 です。</li><li>TIMEZONE: **** 日付時刻のタイムゾーンを指定する必要があります。</li></ul> | make_date_time (YEAR、MONTH、DAY、HOUR、MINUTE、SECOND、ナノ秒、TIMEZONE) | make_date_time (2019、10、17、11、55、12、999、&quot;America/Los_Angeles&quot;) | `2019-10-17T11:55:12.0&#x200B;00000999-07:00[America/Los_Angeles]` |
| zone_date_to_utc | 任意のタイムゾーンの日付を UTC の日付に変換します。 | <ul><li>日付: **必須日付** 変換しようとしている日付を指定します。</li></ul> | zone_date_to_utc (日付) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:&#x200B;12.000000999-&#x200B;07:00[America/Los_Angeles])` | `2019-10-17T18:55:12.000000999Z[UTC]` |
| zone_date_to_zone | 日付をあるタイムゾーンから別のタイムゾーンに変換します。 | <ul><li>日付: **必須日付** 変換しようとしている日付を指定します。</li><li>ZONE: **** 日付の変換を試みたタイムゾーンを指定する必要がありました。</li></ul> | zone_date_to_zone (日付、ゾーン) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:12&#x200B;.000000999-07:00&#x200B;[America/Los_Angeles], "Europe/Paris")` | `2019-10-17T20:55:12.000000999+02:00[Europe/Paris]` |

{style=&quot;table-layout:auto&quot;}
&#x200B;

### 階層-オブジェクト {#objects}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| size_of | 入力のサイズを返します。 | <ul><li>インプット: **** サイズがわかるようなオブジェクトを指定する必要があります。</li></ul> | size_of (インプット) | `size_of([1, 2, 3, 4])` | 4 |
| is_empty | オブジェクトが空であるかどうかをチェックします。 | <ul><li>インプット: **必須** チェックしようとしているオブジェクトが空であることを示します。</li></ul> | is_empty (インプット) | `is_empty([1, 2, 3])` | オフ |
| arrays_to_object | オブジェクトのリストを作成します。 | <ul><li>インプット: **** キーと配列のペアをグループ化する必要があります。</li></ul> | arrays_to_object (インプット) | サンプルの必要性 | サンプルの必要性 |
| to_object | 指定されたフラットなキー/値のペアに基づいてオブジェクトを作成します。 | <ul><li>インプット: **** キーと値のペアを単純なリストにする必要があります。</li></ul> | to_object (インプット) | to_object (&quot;firstName&quot;、&quot;太郎&quot;、&quot;lastName&quot;、&quot;Doe&quot;) | `{"firstName": "John", "lastName": "Doe"}` |
| str_to_object | 入力ストリングからオブジェクトを作成します。 | <ul><li>STRING: **必要な** のは、オブジェクトを作成するために解析されるストリングです。</li><li>VALUE_DELIMITER: ** フィールドを値から区切る区切り文字。これはオプションです。 デフォルトの区切り記号は `:` です。</li><li>FIELD_DELIMITER: ** フィールドの値のペアを区切る区切り文字を指定します。 デフォルトの区切り記号は `,` です。</li></ul> | str_to_object (STRING、VALUE_DELIMITER、FIELD_DELIMITER) | str_to_object (「姓-太郎 | lastName - | 電話-123 456 7890 &quot;、&quot;-&quot;、&quot; | &quot;) | `{"firstName": "John", "lastName": "Doe", "phone": "123 456 7890"}` |
| contains_key | オブジェクトがソースデータ内に存在するかどうかをチェックします。 **注意:** この関数は旧式の `is_set()` 関数を置き換えます。 | <ul><li>INPUT: **** ソースデータ内に存在しているかどうかをチェックするためのパスを指定します。</li></ul> | contains_key (インプット) | contains_key (&quot;evevevar. field1&quot;) | 満たす |
| 修理 | 属性の値をに設定し `null` ます。 ターゲットスキーマにフィールドをコピーしない場合は、この方法を使用する必要があります。 |  | がありません。 | がありません。 | `null` |
| get_keys | キーと値のペアを解析し、すべてのキーを返します。 | <ul><li>オブジェクト: **** キーが抽出されるオブジェクトを指定する必要があります。</li></ul> | get_keys (オブジェクト) | get_keys ({&quot;book1&quot;: &quot;誇りと Prejudice&quot;、&quot;book2&quot;: &quot;1984&quot;}) | `["book1", "book2"]` |
| get_values | キーと値のペアを解析し、指定されたキーに基づいて、ストリングの値を返します。 | <ul><li>ストリング: **** 解析するストリングです。</li><li>キー: **必要な** 値を抽出するキーです。</li><li>VALUE_DELIMITER: **** フィールドと値を区切る区切り記号を指定する必要があります。 `null`または空白のストリングが指定されている場合は、この値が表示され `:` ます。</li><li>FIELD_DELIMITER: ** フィールドと値のペアを区切る区切り記号。 `null`または空白のストリングが指定されている場合は、この値が表示され `,` ます。</li></ul> | get_values (STRING、KEY、VALUE_DELIMITER、FIELD_DELIMITER) | get_values (\ &quot;555 420 8692 姓名&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、&quot;、\&quot;、\ &quot;、\&quot;) | John |

{style = &quot;テーブル-layout: auto&quot;}

### 階層-アレイ {#arrays}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| coalesce | 指定された配列内の最初の null 以外のオブジェクトを返します。 | <ul><li>INPUT: **** null ではない最初のオブジェクトを検索する配列です。</li></ul> | 結合 (入力) | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| first | 指定された配列の最初の要素を取得します。 | <ul><li>INPUT: **** 1 番目の要素の検索対象とする配列を指定する必要があります。</li></ul> | 最初の (インプット) | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | 指定された配列の最後の要素を取得します。 | <ul><li>INPUT: **必要な** のは、最後のエレメントを検索する配列です。</li></ul> | 最終 (入力した場合) | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| add_to_array | 配列の末尾にエレメントを追加します。 | <ul><li>配列: **** エレメントを追加する対象となるアレイを指定する必要があります。</li><li>値: 配列に追加するエレメントを指定します。</li></ul> | add_to_array (配列、値) | add_to_array ( [ &#39; a &#39;, &#39; b &#39;, &#39; c &#39;, ] d &#39;) | [「a」、「b」、「c」、d &#39;] |
| join_arrays | では、配列は互いに結合されます。 | <ul><li>配列: **** エレメントを追加する対象となるアレイを指定する必要があります。</li><li>値: 親の配列に追加する配列です。</li></ul> | join_arrays (配列、値) | join_arrays ( [ &#39; a &#39;、&#39; b &#39;、&#39; c &#39;、d &#39;、 ] [ ] [ 「e」 ] ) | [a、b、c、d、d &#39;、e &#39;] |
| to_array | 入力のリストを取得し、配列に変換します。 | <ul><li>INCLUDE_NULLS: **** response array に null を含めるかどうかを指定するブール値です。</li><li>値: **配列に変換するエレメントを指定する必要があり** ます。</li></ul> | to_array (INCLUDE_NULLS、値) | to_array (偽、1、null、2、3) | `[1, 2, 3]` |

{style = &quot;テーブル-layout: auto&quot;}

### 論理演算子 {#logical-operators}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| decode | キーと、キーと値のペアのリストが配列としてフラット化されている場合、この関数は、キーが見つかった場合は値を返し、デフォルト値が配列に存在する場合はデフォルト値を返します。 | <ul><li>KEY: **** 一致させるキーを指定する必要があります。</li><li>オプション: **** キーと値のペアが統合された配列であることが必要です。 必要に応じて、最後にデフォルト値を入力することもできます。</li></ul> | デコード (キー、オプション) | デコード (stateCode、&quot;ca&quot;、&quot;カリフォルニア&quot;、&quot;pa&quot;、&quot;ペンシルバニア&quot;、&quot;N/A&quot;) | 指定された stateCode が &quot;ca&quot;、&quot;カリフォルニア&quot; の場合、<br>指定された stateCode が &quot;pa&quot;、&quot;ペンシルバニア&quot; となるとします。<br>StateCode が次のようになっていない場合は、「N/A」となります。 |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>式: **必要な** のは、評価されるブール式です。</li><li>TRUE_VALUE: **必須** 式が true と評価された場合に返される値です。</li><li>FALSE_VALUE: **必須** 式が false に評価された場合に返される値です。</li></ul> | iif (式, TRUE_VALUE, FALSE_VALUE) | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |

{style = &quot;テーブル-layout: auto&quot;}

### 集計 {#aggregation}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| min | 指定された引数の最小値を返します。自然な順序を使用します。 | <ul><li>オプション: **互い** に比較可能な1つまたは複数のオブジェクトが必要です。</li></ul> | min (オプション) | min(3, 1, 4) | 1 |
| max | 指定された引数の最大値を返します。自然な順序を使用します。 | <ul><li>オプション: **互い** に比較可能な1つまたは複数のオブジェクトが必要です。</li></ul> | max (オプション) | max(3, 1, 4) | 4.0 |

{style = &quot;テーブル-layout: auto&quot;}

### 型変換 {#type-conversions}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| to_bigint | ストリングを Java.math.biginteger に変換します。 | <ul><li>STRING: **必要なのは、** java.math.biginteger に変換されるストリングです。</li></ul> | to_bigint (STRING) | to_bigint (&quot;1000000.34&quot;) | 1000000.34 |
| to_decimal | ストリングを倍精度浮動小数点型に変換します。 | <ul><li>STRING: **必須** 倍精度浮動小数点型に変換されるストリング。</li></ul> | to_decimal (STRING) | to_decimal (&quot;20.5&quot;) | 20.5 |
| to_float | ストリングを Float に変換します。 | <ul><li>STRING: **必** Float に変換されるストリングです。</li></ul> | to_float (STRING) | to_float (&quot;12.3456&quot;) | 12.34566 |
| to_integer | ストリングを整数に変換します。 | <ul><li>STRING: **必須** 整数に変換されるストリングです。</li></ul> | to_integer (STRING) | to_integer (&quot;12&quot;) | 12 |

{style = &quot;テーブル-layout: auto&quot;}

### JSON 関数 {#json}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| json_to_object | 指定したストリングから JSON コンテンツを逆シリアル化します。 | <ul><li>STRING: **** シリアル化を解除する JSON ストリングを指定する必要があります。</li></ul> | json_to_object (STRING) | json_to_object ({&quot;info&quot;: {&quot;firstName&quot;: &quot;太郎&quot;, &quot;姓&quot;: &quot;Doe&quot;}}) | JSON を表すオブジェクト。 |

{style = &quot;テーブル-layout: auto&quot;}

### 特殊操作 {#special-operations}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| uuid /<br>guid | 擬似ランダム ID を生成します。 |  | uuid()<br>guid() | uuid () <br> guid () | 7c0267d2-bb74-4e1a-9275-3bf4fccda5f4 <br> c7016dc7-3163-43f7-afc7-2e1c9c206333 |

{style = &quot;テーブル-layout: auto&quot;}

### ユーザーエージェント関数 {#user-agent}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| ua_os_name | ユーザーエージェントストリングからオペレーティングシステム名を抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_os_name (USER_AGENT) | ua_os_name (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | iOS |
| ua_os_version_major | ユーザーエージェントストリングからオペレーティングシステムのメジャーバージョンを抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_os_version_major (USER_AGENT) | ua_os_version_majors (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | iOS 5 |
| ua_os_version | ユーザーエージェントストリングからオペレーティングシステムのバージョンを抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_os_version (USER_AGENT) | ua_os_version (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | 5.1.1 |
| ua_os_name_version | ユーザーエージェントストリングからオペレーティングシステムの名前とバージョンを抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_os_name_version (USER_AGENT) | ua_os_name_version (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | iOS 5.1.1 |
| ua_agent_version | ユーザーエージェントストリングからエージェントのバージョンを抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_agent_version (USER_AGENT) | ua_agent_version (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | 5.1 |
| ua_agent_version_major | ユーザーエージェントストリングからエージェント名とメジャーバージョンを抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_agent_version_major (USER_AGENT) | ua_agent_version_major (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | Safari 5 |
| ua_agent_name | ユーザーエージェントストリングからエージェント名を抽出します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_agent_name (USER_AGENT) | ua_agent_name (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | Safari |
| ua_device_class | ユーザーエージェントストリングから device クラスを展開します。 | <ul><li>USER_AGENT: **** ユーザーエージェントストリングを指定する必要があります。</li></ul> | ua_device_class (USER_AGENT) | ua_device_class (&quot;Mozilla/5.0 [iPhone;CPU iPhone OS 5_1_1 Mac OS X と同様) AppleWebKit/534.46 (KHTML では Gecko) バージョン/5.1 Mobile/9B206 Safari/7534.48.3 &quot;) | Phone |

{style = &quot;テーブル-layout: auto&quot;}

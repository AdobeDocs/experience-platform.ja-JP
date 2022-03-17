---
keywords: Experience Platform;home;popular topics;map csv;map csv file;map csv file to xdm;map csv to xdm;ui guide;mapper;mapping;mapping fields;mapping functions;
solution: Experience Platform
title: Data Prep Mapping Functions
topic-legacy: overview
description: This document introduces the mapping functions used with Data Prep.
exl-id: e95d9329-9dac-4b54-b804-ab5744ea6289
source-git-commit: a50d903fe2765ec5153786c19eb767b695440eba
workflow-type: tm+mt
source-wordcount: '3965'
ht-degree: 17%

---

# Data Prep mapping functions

Data Prep functions can be used to compute and calculate values based on what is entered in source fields.

## フィールド

`$``_`Variable names are also case sensitive.

`${}``${First Name}``${First.Name}`

****`${}`

```console
new, mod, or, break, var, lt, for, false, while, eq, gt, div, not, null, continue, else, and, ne, true, le, if, ge, return
```

Data within sub-fields can be accessed by using the dot notation. `name``firstName``name.firstName`

## 関数のリスト

The following tables list all supported mapping functions, including sample expressions and their resulting outputs.

### 文字列関数 {#string}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| concat | Concatenates the given strings. | <ul><li>STRING: The strings that will be concatenated.</li></ul> | concat(STRING_1, STRING_2) | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| explode | 正規表現に基づいて文字列を分割し、部分の配列を返します。Can optionally include regex to split the string. By default, the splitting resolves to &quot;,&quot;. ****`\``+, ?, ^, \|, ., [, (, {, ), *, $, \` | <ul><li>****</li><li>**</li></ul> | explode(STRING, REGEX) | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の場所/インデックスを返します。 | <ul><li>****</li><li>****</li><li>**</li><li>**&#x200B;デフォルトの重み付けは 1 です。 </li></ul> | instr(INPUT, SUBSTRING, START_POSITION, OCCURRENCE) | instr(&quot;adobe.com&quot;, &quot;com&quot;) | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合は、その文字列を置き換えます。 | <ul><li>****</li><li>****</li><li>****</li></ul> | replacestr(INPUT, TO_FIND, TO_REPLACE) | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 指定された長さのサブ文字列を返します。 | <ul><li>****</li><li>****</li><li>****</li></ul> | substr(INPUT, START_INDEX, LENGTH) | substr(&quot;This is a substring test&quot;, 7, 8) | &quot;サブセット&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | <ul><li>****</li></ul> | lower(INPUT) | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | <ul><li>****</li></ul> | upper(INPUT) | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り記号で入力文字列を分割します。****`\``\`**** | <ul><li>****</li><li>****</li></ul> | split(INPUT, SEPARATOR) | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | 区切り記号を使用してオブジェクトのリストを結合します。 | <ul><li>****</li><li>****</li></ul> | `join(SEPARATOR, [OBJECTS])` | `join(" ", to_array(true, "Hello", "world"))` | &quot;Hello world&quot; |
| lpad | Pads the left side of a string with the other given string. | <ul><li>**** This string can be null.</li><li>****</li><li>**** If null or empty, it will be treated as a single space.</li></ul> | lpad(INPUT, COUNT, PADDING) | lpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;yzyzybat&quot; |
| rpad | Pads the right side of a string with the other given string. | <ul><li>**** This string can be null.</li><li>****</li><li>**** If null or empty, it will be treated as a single space.</li></ul> | rpad(INPUT, COUNT, PADDING) | rpad(&quot;bat&quot;, 8, &quot;yz&quot;) | &quot;batyzyzy&quot; |
| left | Gets the first &quot;n&quot; characters of the given string. | <ul><li>****</li><li>****</li></ul> | left(STRING, COUNT) | left(&quot;abcde&quot;, 2) | &quot;ab&quot; |
| right | Gets the last &quot;n&quot; characters of the given string. | <ul><li>****</li><li>****</li></ul> | right(STRING, COUNT) | right(&quot;abcde&quot;, 2) | &quot;de&quot; |
| ltrim | Removes the whitespace from the beginning of the string. | <ul><li>****</li></ul> | ltrim(STRING) | ltrim(&quot; hello&quot;) | &quot;hello&quot; |
| rtrim | Removes the whitespace from the end of the string. | <ul><li>****</li></ul> | rtrim(STRING) | rtrim(&quot;hello &quot;) | &quot;hello&quot; |
| trim | Removes the whitespace from the beginning and the end of the string. | <ul><li>****</li></ul> | trim(STRING) | trim(&quot; hello &quot;) | &quot;hello&quot; |
| 次と等しい | Compares two strings to confirm if they are equal. This function is case sensitive. | <ul><li>****</li><li>****</li></ul> | STRING1.&#x200B;equals(&#x200B;STRING2) | &quot;string1&quot;.&#x200B;equals&#x200B;(&quot;STRING1&quot;) | false |
| equalSignoreCase | Compares two strings to confirm if they are equal. **** | <ul><li>****</li><li>****</li></ul> | STRING1.&#x200B;equalsIgnoreCase&#x200B;(STRING2) | &quot;string1&quot;.&#x200B;equalsIgnoreCase&#x200B;(&quot;STRING1) | true |

{style=&quot;table-layout:auto&quot;}

### 正規表現関数

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| extract_regex | Extracts groups from the input string, based on a regular expression. | <ul><li>****</li><li>****</li></ul> | extract_regex(STRING, REGEX) | [][][] | [] |
| matches_regex | Checks to see if the string matches against the inputted regular expression. | <ul><li>****</li><li>****</li></ul> | matches_regex(STRING, REGEX) | [][][] | true |

{style=&quot;table-layout:auto&quot;}

### Hashing functions {#hashing}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| sha1 | Takes an input and produces a hash value using Secure Hash Algorithm 1 (SHA-1). | <ul><li>****</li><li>** Possible values include UTF-8, UTF-16, ISO-8859-1, and US-ASCII.</li></ul> | sha1(INPUT, CHARSET) | sha1(&quot;my text&quot;, &quot;UTF-8&quot;) | c3599c11e47719df18a24&#x200B;48690840c5dfcce3c80 |
| sha256 | Takes an input and produces a hash value using Secure Hash Algorithm 256 (SHA-256). | <ul><li>****</li><li>** Possible values include UTF-8, UTF-16, ISO-8859-1, and US-ASCII.</li></ul> | sha256(INPUT, CHARSET) | sha256(&quot;my text&quot;, &quot;UTF-8&quot;) | 7330d2b39ca35eaf4cb95fc846c21&#x200B;ee6a39af698154a83a586ee270a0d372104 |
| sha512 | Takes an input and produces a hash value using Secure Hash Algorithm 512 (SHA-512). | <ul><li>****</li><li>** Possible values include UTF-8, UTF-16, ISO-8859-1, and US-ASCII.</li></ul> | sha512(INPUT, CHARSET) | sha512(&quot;my text&quot;, &quot;UTF-8&quot;) | a3d7e45a0d9be5fd4e4b9a3b8c9c2163c21ef&#x200B;708bf11b4232bb21d2a8704ada2cdcd7b367dd0788a89&#x200B;a5c908cfe377aceb1072a7b386b7d4fd2ff68a8fd24d16 |
| md5 | Takes an input and produces a hash value using MD5. | <ul><li>****</li><li>** Possible values include UTF-8, UTF-16, ISO-8859-1, and US-ASCII. </li></ul> | md5(INPUT, CHARSET) | md5(&quot;my text&quot;, &quot;UTF-8&quot;) | d3b96ce8c9fb4&#x200B;e9bd0198d03ba6852c7 |
| crc32 | Takes an input uses a cyclic redundancy check (CRC) algorithm to produce a 32-bit cyclic code. | <ul><li>****</li><li>** Possible values include UTF-8, UTF-16, ISO-8859-1, and US-ASCII.</li></ul> | crc32(INPUT, CHARSET) | crc32(&quot;my text&quot;, &quot;UTF-8&quot;) | 8df92e80 |

{style=&quot;table-layout:auto&quot;}

### URL functions {#url}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| get_url_protocol | Returns the protocol from the given URL. If the input is invalid, it returns null. | <ul><li>****</li></ul> | get_url_protocol&#x200B;(URL) | get_url_protocol(&quot;https://platform&#x200B;.adobe.com/home&quot;) | https |
| get_url_host | Returns the host of the given URL. If the input is invalid, it returns null. | <ul><li>****</li></ul> | get_url_host&#x200B;(URL) | get_url_host&#x200B;(&quot;https://platform&#x200B;.adobe.com/home&quot;) | platform.adobe.com |
| get_url_port | Returns the port of the given URL. If the input is invalid, it returns null. | <ul><li>****</li></ul> | get_url_port(URL) | get_url_port&#x200B;(&quot;sftp://example.com//home/&#x200B;joe/employee.csv&quot;) | 22 |
| get_url_path | Returns the path of the given URL. By default, the full path is returned. | <ul><li>****</li><li>** If set to false, only the end of the path is returned.</li></ul> | get_url_path&#x200B;(URL, FULL_PATH) | get_url_path&#x200B;(&quot;sftp://example.com//&#x200B;home/joe/employee.csv&quot;) | &quot;//home/joe/&#x200B;employee.csv&quot; |
| get_url_query_str | Returns the query string of a given URL. | <ul><li>****</li><li>**** Can be one of three values: &quot;retain&quot;, &quot;remove&quot;, or &quot;append&quot;.<br><br><br><br></li></ul> | get_url_query_str&#x200B;(URL, ANCHOR) | <br><br> | `{"name": "ferret#nose"}`<br>`{"name": "ferret"}`<br>`{"name": "ferret", "_anchor_": "nose"}` |

{style=&quot;table-layout:auto&quot;}

### 日付および時間関数 {#date-and-time}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。`date`[](./data-handling.md#dates)

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| now | 現在の時刻を取得します。 |  | now() | now() | `2021-10-26T10:10:24Z` |
| timestamp | 現在の Unix 時間を取得します。 |  | timestamp() | timestamp() | 1571850624571 |
| format | 指定された形式に従って入力日をフォーマットします。 | <ul><li>****</li><li>****</li></ul> | format(DATE, FORMAT) | :24::mm: | `2019-10-23 11:24:35` |
| dformat | 指定された形式に従ってタイムスタンプを日付文字列に変換します。 | <ul><li>**** This is written in milliseconds.</li><li>****</li></ul> | dformat(TIMESTAMP, FORMAT) | :mm: | `2019-10-23T11:24:35.000Z` |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>****</li><li>************ </li><li>****</li></ul> | date(DATE, FORMAT, DEFAULT_DATE) | date(&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;, now()) | `2019-10-23T11:24:00Z` |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>****</li><li>************ </li></ul> | date(DATE, FORMAT) | date(&quot;2019-10-23 11:24&quot;, &quot;yyyy-MM-dd HH:mm&quot;) | `2019-10-23T11:24:00Z` |
| date | 日付文字列を ZonedDateTime オブジェクト（ISO 8601 形式）に変換します。 | <ul><li>****</li></ul> | date(DATE) | date(&quot;2019-10-23 11:24&quot;) | :24: |
| date_part | 日付の一部を取得します。次のコンポーネント値がサポートされています。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;quarter&quot;<br>&quot;qq&quot;<br>&quot;q&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;dayofyear&quot;<br>&quot;dy&quot;<br>&quot;y&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;week&quot;<br>&quot;ww&quot;<br>&quot;w&quot;<br><br>&quot;weekday&quot;<br>&quot;dw&quot;<br>&quot;w&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br>&quot;hh24&quot;<br>&quot;hh12&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot;<br><br>&quot;millisecond&quot;<br>&quot;ms&quot; | <ul><li>**** </li><li>****</li></ul> | date_part&#x200B;(COMPONENT, DATE) | :55: | 10 |
| set_date_part | 指定された日付のコンポーネントを置き換えます。次のコンポーネントが受け入れられます。<br><br>&quot;year&quot;<br>&quot;yyyy&quot;<br>&quot;yy&quot;<br><br>&quot;month&quot;<br>&quot;mm&quot;<br>&quot;m&quot;<br><br>&quot;day&quot;<br>&quot;dd&quot;<br>&quot;d&quot;<br><br>&quot;hour&quot;<br>&quot;hh&quot;<br><br>&quot;minute&quot;<br>&quot;mi&quot;<br>&quot;n&quot;<br><br>&quot;second&quot;<br>&quot;ss&quot;<br>&quot;s&quot; | <ul><li>**** </li><li>****</li><li>****</li></ul> | set_date_part&#x200B;(COMPONENT, VALUE, DATE) | :44: | :44: |
| make_date_time | 部分から日付を作成します。This function can also be induced using make_timestamp. | <ul><li>****</li><li>**** The allowed values are 1 to 12.</li><li>**** The allowed values are 1 to 31.</li><li>**** The allowed values are 0 to 23.</li><li>**** The allowed values are 0 to 59.</li><li>**** The allowed values are 0 to 999999999.</li><li>****</li></ul> | make_date_time&#x200B;(YEAR, MONTH, DAY, HOUR, MINUTE, SECOND, NANOSECOND, TIMEZONE) | make_date_time&#x200B;(2019, 10, 17, 11, 55, 12, 999, &quot;America/Los_Angeles&quot;) | `2019-10-17T11:55:12Z` |
| zone_date_to_utc | Converts a date in any timezone to a date in UTC. | <ul><li>****</li></ul> | zone_date_to_utc&#x200B;(DATE) | `zone_date_to_utc&#x200B;(2019-10-17T11:55:&#x200B;12 PST` | `2019-10-17T19:55:12Z` |
| zone_date_to_zone | Converts a date from one timezone to another timezone. | <ul><li>****</li><li>****</li></ul> | zone_date_to_zone&#x200B;(DATE, ZONE) | `zone_date_to_utc&#x200B;(now(), "Europe/Paris")` | `2021-10-26T15:43:59Z` |

{style=&quot;table-layout:auto&quot;}
&#x200B;

### Hierarchies - Objects {#objects}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| size_of | Returns the size of the input. | <ul><li>****</li></ul> | size_of(INPUT) | `size_of([1, 2, 3, 4])` | 4 |
| is_empty | Checks whether or not an object is empty. | <ul><li>****</li></ul> | is_empty(INPUT) | `is_empty([1, 2, 3])` | false |
| arrays_to_object | Creates a list of objects. | <ul><li>****</li></ul> | arrays_to_object(INPUT) | need sample | need sample |
| to_object | Creates an object based on the flat key/value pairs given. | <ul><li>****</li></ul> | to_object(INPUT) | to_object&#x200B;(&quot;firstName&quot;, &quot;John&quot;, &quot;lastName&quot;, &quot;Doe&quot;) | `{"firstName": "John", "lastName": "Doe"}` |
| str_to_object | Creates an object from the input string. | <ul><li>****</li><li>**`:`</li><li>**`,`</li></ul> | str_to_object&#x200B;(STRING, VALUE_DELIMITER, FIELD_DELIMITER) | str_to_object(&quot;firstName=John,lastName=Doe,phone=123 456 7890&quot;, &quot;=&quot;, &quot;,&quot;) | `{"firstName": "John", "lastName": "Doe", "phone": "123 456 7890"}` |
| contains_key | Checks if the object exists within the source data. ****`is_set()` | <ul><li>****</li></ul> | contains_key(INPUT) | contains_key(&quot;evars.evar.field1&quot;) | true |
| nullify | `null`This should be used when you do not want to copy the field to the target schema. |  | nullify() | nullify() | `null` |
| get_keys | Parses the key/value pairs and returns all the keys. | <ul><li>****</li></ul> | get_keys(OBJECT) | get_keys({&quot;book1&quot;: &quot;Pride and Prejudice&quot;, &quot;book2&quot;: &quot;1984&quot;}) | `["book1", "book2"]` |
| get_values | Parses the key/value pairs and returns the value of the string, based on the given key. | <ul><li>****</li><li>****</li><li>****`null``:`</li><li>**`null``,`</li></ul> | get_values(STRING, KEY, VALUE_DELIMITER, FIELD_DELIMITER) | get_values(\&quot;firstName - John , lastName - Cena , phone - 555 420 8692\&quot;, \&quot;firstName\&quot;, \&quot;-\&quot;, \&quot;,\&quot;) | John |

{style=&quot;table-layout:auto&quot;}

### Hierarchies - Arrays {#arrays}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| coalesce | Returns the first non-null object in a given array. | <ul><li>****</li></ul> | coalesce(INPUT) | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| first | Retrieves the first element of the given array. | <ul><li>****</li></ul> | first(INPUT) | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | Retrieves the last element of the given array. | <ul><li>****</li></ul> | last(INPUT) | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| add_to_array | Adds elements to the end of the array. | <ul><li>****</li><li>VALUES: The elements that you want to append to the array.</li></ul> | add_to_array&#x200B;(ARRAY, VALUES) | [] | [] |
| join_arrays | Combines the arrays with each other. | <ul><li>****</li><li>VALUES: The array(s) you want to append to the parent array.</li></ul> | join_arrays&#x200B;(ARRAY, VALUES) | [][][] | [] |
| to_array | Takes a list of inputs and converts it to an array. | <ul><li>****</li><li>****</li></ul> | to_array&#x200B;(INCLUDE_NULLS, VALUES) | to_array(false, 1, null, 2, 3) | `[1, 2, 3]` |

{style=&quot;table-layout:auto&quot;}

### 論理演算子 {#logical-operators}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| decode | キーと、キーと値のペアのリストが配列としてフラット化されている場合、この関数は、キーが見つかった場合は値を返し、デフォルト値が配列に存在する場合はデフォルト値を返します。 | <ul><li>****</li><li>**** Optionally, a default value can be put at the end.</li></ul> | decode(KEY, OPTIONS) | decode(stateCode, &quot;ca&quot;, &quot;California&quot;, &quot;pa&quot;, &quot;Pennsylvania&quot;, &quot;N/A&quot;) | If the stateCode given is &quot;ca&quot;, &quot;California&quot;.<br><br> |
| iif | 指定されたブール式を評価し、結果に基づいて指定された値を返します。 | <ul><li>****</li><li>****</li><li>****</li></ul> | iif(EXPRESSION, TRUE_VALUE, FALSE_VALUE) | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |

{style=&quot;table-layout:auto&quot;}

### 集計 {#aggregation}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| min | 指定された引数の最小値を返します。自然な順序を使用します。 | <ul><li>****</li></ul> | min(OPTIONS) | min(3, 1, 4) | 1 |
| max | 指定された引数の最大値を返します。自然な順序を使用します。 | <ul><li>****</li></ul> | max(OPTIONS) | max(3, 1, 4) | 4 |

{style=&quot;table-layout:auto&quot;}

### Type conversions {#type-conversions}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| to_bigint | Converts a string to a BigInteger. | <ul><li>****</li></ul> | to_bigint(STRING) | to_bigint&#x200B;(&quot;1000000.34&quot;) | 1000000.34 |
| to_decimal | Converts a string to a Double. | <ul><li>****</li></ul> | to_decimal(STRING) | to_decimal(&quot;20.5&quot;) | 20.5 |
| to_float | Converts a string to a Float. | <ul><li>****</li></ul> | to_float(STRING) | to_float(&quot;12.3456&quot;) | 12.34566 |
| to_integer | Converts a string to an Integer. | <ul><li>****</li></ul> | to_integer(STRING) | to_integer(&quot;12&quot;) | 12 |

{style=&quot;table-layout:auto&quot;}

### JSON functions {#json}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| json_to_object | Deserialize JSON content from the given string. | <ul><li>****</li></ul> | json_to_object&#x200B;(STRING) | json_to_object&#x200B;({&quot;info&quot;:{&quot;firstName&quot;:&quot;John&quot;,&quot;lastName&quot;: &quot;Doe&quot;}}) | An object representing the JSON. |

{style=&quot;table-layout:auto&quot;}

### Special operations {#special-operations}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| uuid /<br>guid | 擬似ランダム ID を生成します。 |  | uuid()<br>guid() | uuid()<br>guid() | <br> |

{style=&quot;table-layout:auto&quot;}

### User agent functions {#user-agent}

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| 関数 | 説明 | パラメーター | 構文 | 式 | サンプル出力 |
| -------- | ----------- | ---------- | -------| ---------- | ------------- |
| ua_os_name | Extracts the operating system name from the user agent string. | <ul><li>****</li></ul> | ua_os_name&#x200B;(USER_AGENT) | ua_os_name&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS |
| ua_os_version_major | Extracts the operating system&#39;s major version from the user agent string. | <ul><li>****</li></ul> | ua_os_version_major&#x200B;(USER_AGENT) | ua_os_version_major&#x200B;s(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5 |
| ua_os_version | Extracts the operating system&#39;s version from the user agent string. | <ul><li>****</li></ul> | ua_os_version&#x200B;(USER_AGENT) | ua_os_version&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1.1 |
| ua_os_name_version | Extracts the operating system&#39;s name and version from the user agent string. | <ul><li>****</li></ul> | ua_os_name_version&#x200B;(USER_AGENT) | ua_os_name_version&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | iOS 5.1.1 |
| ua_agent_version | Extracts the agent version from the user agent string. | <ul><li>****</li></ul> | ua_agent_version&#x200B;(USER_AGENT) | ua_agent_version&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | 5.1 |
| ua_agent_version_major | Extracts the agent name and major version from the user agent string. | <ul><li>****</li></ul> | ua_agent_version_major&#x200B;(USER_AGENT) | ua_agent_version_major&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari 5 |
| ua_agent_name | Extracts the agent name from the user agent string. | <ul><li>****</li></ul> | ua_agent_name&#x200B;(USER_AGENT) | ua_agent_name&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Safari |
| ua_device_class | Extracts the device class from the user agent string. | <ul><li>****</li></ul> | ua_device_class&#x200B;(USER_AGENT) | ua_device_class&#x200B;(&quot;Mozilla/5.0 (iPhone; CPU iPhone OS 5_1_1 like Mac OS X) AppleWebKit/534.46 (KHTML, like Gecko) Version/5.1 Mobile/9B206 Safari/7534.48.3&quot;) | Phone |

{style=&quot;table-layout:auto&quot;}

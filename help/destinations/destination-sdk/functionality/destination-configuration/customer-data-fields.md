---
description: Experience Platform UI で入力フィールドを作成する方法を説明します。これにより、ユーザーは、宛先への接続およびデータの書き出し方法に関連する様々な情報を指定できます。
title: 顧客データフィールド
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '1436'
ht-degree: 100%

---


# 顧客データフィールドを使用したユーザー入力の設定

Experience Platform UI で宛先に接続する際に、ユーザーに特定の設定の詳細を提供したり、特定のオプションを選択して使用できるようにしたりする必要がある可能性があります。Destination SDK では、これらのオプションは、顧客データフィールドと呼ばれます。

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、以下の宛先設定の概要ページを参照してください。

* [Destination SDK を使用したストリーミング宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-destination-configuration)

## 顧客データフィールドの使用例 {#use-cases}

Experience Platform UI でユーザーがデータを入力する必要がある様々なユースケースで、顧客データフィールドを使用します。例えば、ユーザーが以下を指定する必要がある場合に、顧客データフィールドを使用します。

* ファイルベースの宛先用の、クラウドストレージバケット名およびパス。
* 顧客データフィールドによって受け入れられる形式。
* ユーザーが選択できる、使用可能なファイル圧縮タイプ。
* リアルタイム（ストリーミング）統合に使用できるエンドポイントのリスト。

`/authoring/destinations` エンドポイントを介して顧客データフィールドを設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての顧客データフィールド設定タイプを説明し、Experience Platform UI で顧客に何が表示されるかを示します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○ |

## サポートされるパラメーター {#supported-parameters}

独自の顧客データフィールドを作成する際に、以下の表で説明されているパラメーターを使用して、その動作を設定できます。

| パラメーター | タイプ | 必須／オプション | 説明 |
|---------|----------|------|---|
| `name` | 文字列 | 必須 | 導入するカスタムフィールドの名前を指定します。この名前は、`title` フィールドが空か見つからない場合を除いて、Platform UI に表示されません。 |
| `type` | 文字列 | 必須 | 導入するカスタムフィールドのタイプを示します。使用できる値： <ul><li>`string`</li><li>`object`</li><li>`integer`</li></ul> |
| `title` | 文字列 | オプション | Platform UI で顧客に表示される、フィールドの名前を示します。このフィールドが空か見つからない場合、UI は、`name` 値からフィールド名を継承します。 |
| `description` | 文字列 | オプション | カスタムフィールドの説明を入力します。この説明は、Platform UI に表示されません。 |
| `isRequired` | ブール値 | オプション | ユーザーが宛先設定ワークフローでこのフィールドに値を指定する必要があるかどうかを示します。 |
| `pattern` | 文字列 | オプション | 必要に応じて、カスタムフィールドのパターンを適用します。正規表現を使用して、パターンを適用します。例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドで `^[A-Za-z]+$` を入力します。 |
| `enum` | 文字列 | オプション | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `default` | 文字列 | オプション | デフォルト値を `enum` リストから定義します。 |
| `hidden` | ブール値 | オプション | 顧客データフィールドが UI に表示されるかどうかを示します。 |
| `unique` | ブール値 | オプション | ユーザーの組織によって設定されたすべての宛先データフローで値が一意である必要がある顧客データフィールドを作成する必要がある場合は、このパラメーターを使用します。例えば、[カスタムパーソナライゼーション](../../../catalog/personalization/custom-personalization.md)宛先の「**[!UICONTROL 統合エイリアス]**」フィールドは、一意である必要があります。つまり、この宛先への 2 つの個別のデータフローがこのフィールドに対して同じ値を持つことはできません。 |
| `readOnly` | ブール値 | オプション | 顧客がフィールドの値を変更できるかどうかを示します。 |

{style="table-layout:auto"}

以下の例では、`customerDataFields` セクションで、宛先に接続する際にユーザーが Platform UI で入力する必要がある 2 つのフィールドを定義します。

* `Account ID`：宛先プラットフォームのユーザーアカウント ID。
* `Endpoint region`：ユーザーが接続する API の地域エンドポイント。`enum` セクションは、ユーザーが選択できる範囲内で定義された値を含むドロップダウンメニューを作成します。

```json
"customerDataFields":[
   {
      "name":"accountID",
      "title":"User account ID",
      "description":"User account ID for the destination platform.",
      "type":"string",
      "isRequired":true
   },
   {
      "name":"region",
      "title":"API endpoint region",
      "description":"The API endpoint region that the user should connect to.",
      "type":"string",
      "isRequired":true,
      "enum":[
         "EU"
         "US",
      ],
      "readOnly":false,
      "hidden":false
   }
]
```

以下の画像に、結果の UI エクスペリエンスを示します。

![顧客データフィールドの例を示す UI 画像。](../../assets/functionality/destination-configuration/customer-data-fields-example.png)

## 宛先接続名と説明 {#names-description}

新しい宛先を作成する際に、Destination SDK は、**[!UICONTROL 名前]**&#x200B;および&#x200B;**[!UICONTROL 説明]**&#x200B;フィールドを Platform UI の宛先接続画面に自動的に追加します。上記の例で確認できるように、**[!UICONTROL 名前]**&#x200B;および&#x200B;**[!UICONTROL 説明]**&#x200B;フィールドは、顧客データフィールド設定に含まれることなく、UI でレンダリングされます。

>[!IMPORTANT]
>
>**[!UICONTROL 名前]**&#x200B;および&#x200B;**[!UICONTROL 説明]**&#x200B;フィールドを顧客データフィールド設定に追加すると、ユーザーには、UI で重複して表示されます。

## 顧客データフィールドの順序付け {#ordering}

宛先設定で顧客データフィールドを追加した順序は、Platform UI に反映されます。

例えば、以下の設定は、UI にそのまま反映され、オプションは、**[!UICONTROL Name]**、**[!UICONTROL Description]**、**[!UICONTROL Bucket name]**、**[!UICONTROL Folder path]**、**[!UICONTROL File Type]**、**[!UICONTROL Compression format]** の順序で表示されます。

```json
"customerDataFields":[
{
   "name":"bucketName",
   "title":"Bucket name",
   "description":"Amazon S3 bucket name",
   "type":"string",
   "isRequired":true,
   "pattern":"(?=^.{3,63}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])\\.)*([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])$)",
   "readOnly":false,
   "hidden":false
},
{
   "name":"path",
   "title":"Folder path",
   "description":"Enter the path to your S3 bucket folder",
   "type":"string",
   "isRequired":true,
   "pattern":"^[0-9a-zA-Z\\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\\/?)+$",
   "readOnly":false,
   "hidden":false
},
{
   "name":"fileType",
   "title":"File Type",
   "description":"Select the exported file type.",
   "type":"string",
   "isRequired":true,
   "readOnly":false,
   "hidden":false,
   "enum":[
      "csv",
      "json",
      "parquet"
   ],
   "default":"csv"
},
{
   "name":"compression",
   "title":"Compression format",
   "description":"Select the desired file compression format.",
   "type":"string",
   "isRequired":true,
   "readOnly":false,
   "enum":[
      "SNAPPY",
      "GZIP",
      "DEFLATE",
      "NONE"
   ]
}
]
```

![Experience Platform UI でのファイル形式オプションの順序を示す画像。](../../assets/functionality/destination-configuration/customer-data-fields-order.png)

## 顧客データフィールドのグループ化 {#grouping}

いくつかの顧客データフィールドを 1 つのセクション内にグループ化できます。UI で宛先への接続を設定する際に、ユーザーは、類似したフィールドを視覚的にグループ化することで、メリットが得られます。

これを行うには、以下の画像に示すように、`"type": "object"` を使用してグループを作成し、`properties` オブジェクト内に目的の顧客データフィールドを収集します（**[!UICONTROL CSV オプション]**&#x200B;のグループ化がハイライト表示されています）。

```json {line-numbers="true" highlight="6-28"}
"customerDataFields":[
   {
      "name":"csvOptions",
      "title":"CSV Options",
      "description":"Select your CSV options",
      "type":"object",
      "properties":[
         {
            "name":"delimiter",
            "title":"Delimiter",
            "description":"Select your Delimiter",
            "type":"string",
            "isRequired":false,
            "default":",",
            "namedEnum":[
               {
                  "name":"Comma (,)",
                  "value":","
               },
               {
                  "name":"Tab (\\t)",
                  "value":"\t"
               }
            ],
            "readOnly":false,
            "hidden":false
         }
      ]
   }
]
```

![UI での顧客データフィールドグループを示す画像。](../../assets/functionality/destination-configuration/group-customer-data-fields.png)

## 顧客データフィールド用のドロップダウンセレクターの作成 {#dropdown-selectors}

ユーザーがいくつかのオプションから選択できるようにしたい状況（例えば、CSV ファイルのフィールドを区切るためにどの文字を使用するか）では、UI にドロップダウンフィールドを追加できます。

これを行うには、以下に示すように、`namedEnum` オブジェクトを使用して、ユーザーが選択できるオプションの `default` 値を設定します。

```json {line-numbers="true" highlight="15-24"}
"customerDataFields":[
   {
      "name":"csvOptions",
      "title":"CSV Options",
      "description":"Select your CSV options",
      "type":"object",
      "properties":[
         {
            "name":"delimiter",
            "title":"Delimiter",
            "description":"Select your Delimiter",
            "type":"string",
            "isRequired":false,
            "default":",",
            "namedEnum":[
               {
                  "name":"Comma (,)",
                  "value":","
               },
               {
                  "name":"Tab (\\t)",
                  "value":"\t"
               }
            ],
            "readOnly":false,
            "hidden":false
         }
      ]
   }
]
```

![上記の設定で作成されたドロップダウンセレクターの例を示す画面録画。](../../assets/functionality/destination-configuration/customer-data-fields-dropdown.gif)

## 条件付き顧客データフィールドの作成 {#conditional-options}

ユーザーが特定のオプションを選択する際にのみアクティベーションワークフローに表示される、条件付き顧客データフィールドを作成できます。

例えば、ユーザーが特定のファイル書き出しタイプを選択する場合にのみ表示される条件付きファイル形式オプションを作成できます。

以下の設定は、CSV ファイル形式オプション用の条件付きグループを作成します。CSV ファイルオプションは、ユーザーが CSV を書き出し用のファイルタイプとして選択する場合にのみ表示されます。

フィールドを条件付きとして設定するには、以下に示すように、`conditional` パラメーターを使用します。

```json
"conditional": {
   "field": "fileType",
   "operator": "EQUALS",
   "value": "CSV"
}
```

より広いコンテキストでは、以下の宛先設定で、`fileType` 文字列とそれが定義されている `csvOptions` オブジェクトと共に `conditional` フィールドが使用されているのを確認できます。

```json {line-numbers="true" highlight="3-15, 21-25"}
"customerDataFields":[
   {
      "name":"fileType",
      "title":"File Type",
      "description":"Select your file type",
      "type":"string",
      "isRequired":true,
      "enum":[
         "PARQUET",
         "CSV",
         "JSON"
      ],
      "readOnly":false,
      "hidden":false
   },
   {
      "name":"csvOptions",
      "title":"CSV Options",
      "description":"Select your CSV options",
      "type":"object",
      "conditional":{
         "field":"fileType",
         "operator":"EQUALS",
         "value":"CSV"
      },
      "properties":[
         {
            "name":"delimiter",
            "title":"Delimiter",
            "description":"Select your Delimiter",
            "type":"string",
            "isRequired":false,
            "default":",",
            "namedEnum":[
               {
                  "name":"Comma (,)",
                  "value":","
               },
               {
                  "name":"Tab (\\t)",
                  "value":"\t"
               }
            ],
            "readOnly":false,
            "hidden":false
         },
         {
            "name":"quote",
            "title":"Quote Character",
            "description":"Select your Quote character",
            "type":"string",
            "isRequired":false,
            "default":"",
            "namedEnum":[
               {
                  "name":"Double Quotes (\")",
                  "value":"\""
               },
               {
                  "name":"Null Character (\u0000)",
                  "value":"\u0000"
               }
            ],
            "readOnly":false,
            "hidden":false
         },
         {
            "name":"escape",
            "title":"Escape Character",
            "description":"Select your Escape character",
            "type":"string",
            "isRequired":false,
            "default":"\\",
            "namedEnum":[
               {
                  "name":"Back Slash (\\)",
                  "value":"\\"
               },
               {
                  "name":"Single Quote (')",
                  "value":"'"
               }
            ],
            "readOnly":false,
            "hidden":false
         },
         {
            "name":"emptyValue",
            "title":"Empty Value",
            "description":"Select the output value of blank fields",
            "type":"string",
            "isRequired":false,
            "default":"",
            "namedEnum":[
               {
                  "name":"Empty String",
                  "value":""
               },
               {
                  "name":"\"\"",
                  "value":"\"\""
               },
               {
                  "name":"null",
                  "value":"null"
               }
            ],
            "readOnly":false,
            "hidden":false
         },
         {
            "name":"nullValue",
            "title":"Null Value",
            "description":"Select the output value of 'null' fields",
            "type":"string",
            "isRequired":false,
            "default":"null",
            "namedEnum":[
               {
                  "name":"Empty String",
                  "value":""
               },
               {
                  "name":"\"\"",
                  "value":"\"\""
               },
               {
                  "name":"null",
                  "value":"null"
               }
            ],
            "readOnly":false,
            "hidden":false
         }
      ],
      "isRequired":false,
      "readOnly":false,
      "hidden":false
   }
]
```

以下に、上記の設定に基づいた結果の UI 画面を確認できます。ユーザーがファイルタイプで CSV を選択すると、CSV ファイルタイプを参照する追加のファイル形式オプションが UI に表示されます。

![CSV ファイルの条件付きファイル形式オプションを示す画面録画。](../../assets/functionality/destination-configuration/customer-data-fields-conditional.gif)

## テンプレート化された顧客データフィールドへのアクセス {#accessing-templatized-fields}

宛先がユーザー入力を必要とする場合、Platform UI を通じて入力できる、顧客データフィールドの選択肢をユーザーに提供する必要があります。次に、顧客データフィールドからユーザー入力を正しく読み取るために宛先サーバーを設定する必要があります。これは、テンプレート化されたフィールドを使用して行われます。

テンプレート化されたフィールドは、`{{customerData.fieldName}}` という形式を使用します（`fieldName` は、情報を読み取る顧客データフィールドの名前）。すべてのテンプレート化された顧客データフィールドは、先頭に `customerData.` が付き、二重中括弧 `{{ }}` で囲まれています。

例えば、以下の Amazon S3 宛先設定について見てみましょう。

```json
"customerDataFields":[
   {
      "name":"bucketName",
      "title":"Enter the name of your Amazon S3 bucket",
      "description":"Amazon S3 bucket name",
      "type":"string",
      "isRequired":true,
      "pattern":"(?=^.{3,63}$)(?!^(\\d+\\.)+\\d+$)(^(([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])\\.)*([a-z0-9]|[a-z0-9][a-z0-9\\-]*[a-z0-9])$)",
      "readOnly":false,
      "hidden":false
   },
   {
      "name":"path",
      "title":"Enter the path to your S3 bucket folder",
      "description":"Enter the path to your S3 bucket folder",
      "type":"string",
      "isRequired":true,
      "pattern":"^[0-9a-zA-Z\\/\\!\\-_\\.\\*\\''\\(\\)]*((\\%SEGMENT_(NAME|ID)\\%)?\\/?)+$",
      "readOnly":false,
      "hidden":false
   }
]
```

この設定では、[!DNL Amazon S3] バケット名およびフォルダーパスをそれぞれの顧客データフィールドに入力することを促すプロンプトをユーザーに表示します。

Experience Platform が [!DNL Amazon S3] に正しく接続するために、以下に示すように、これらの 2 つの顧客データフィールドから値を読み取るように宛先サーバーが設定されている必要があります。

```json
 "fileBasedS3Destination":{
      "bucketName":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.bucketName}}"
      },
      "path":{
         "templatingStrategy":"PEBBLE_V1",
         "value":"{{customerData.path}}"
      }
   }
```

テンプレート化された値 `{{customerData.bucketName}}` および `{{customerData.path}}` は、ユーザー指定の値を読み取るので、Experience Platform は、正常に宛先プラットフォームに接続できます。

テンプレート化されたフィールドを読み取るための宛先サーバーの設定方法について詳しくは、[ハードコーディングされたフィールドとテンプレート化されたフィールドの比較](../destination-server/server-specs.md#templatized-fields)のドキュメントを参照してください。

## 次の手順 {#next-steps}

この記事を読むことで、顧客データフィールドを使用して、Experience Platform UI でユーザーが情報を入力できるようにする方法について、理解を深めることができました。また、ユースケースに対して適切な顧客データフィールドを選択する方法や、Platform UI で顧客データフィールドを設定、順序付け、グループ化する方法についても理解しました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
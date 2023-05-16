---
description: Experience PlatformUI で入力フィールドを作成し、宛先へのデータの接続および書き出し方法に関連する様々な情報をユーザーが指定できるようにする方法を説明します。
title: 顧客データフィールド
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '1436'
ht-degree: 11%

---


# 顧客データフィールドを使用したユーザー入力の設定

Experience PlatformUI で宛先に接続する場合、特定の設定の詳細を提供したり、ユーザーが使用できる特定のオプションを選択したりする必要が生じる場合があります。 Destination SDKでは、これらのオプションを顧客データフィールドと呼びます。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、次の宛先設定の概要ページを参照してください。

* [Destination SDK を使用したストリーミングの宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)
* [Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-destination-configuration)

## 顧客データフィールドの使用例 {#use-cases}

ユーザーが顧客 UI にデータを入力する必要がある様々な使用例に対して、顧客データフィールドをExperience Platformします。 例えば、ユーザーが次の情報を提供する必要がある場合は、顧客データフィールドを使用します。

* ファイルベースの宛先のクラウドストレージバケット名とパス。
* 顧客データフィールドで受け入れられる形式。
* ユーザーが選択できる使用可能なファイル圧縮タイプ。
* リアルタイム（ストリーミング）統合に使用できるエンドポイントの一覧です。

顧客データフィールドは、 `/authoring/destinations` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての顧客データフィールド設定タイプについて説明し、Experience PlatformUI で顧客に表示される内容を示します。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | ○ |

## サポートされているパラメーター {#supported-parameters}

独自の顧客データフィールドを作成する場合、次の表で説明するパラメーターを使用して、その動作を設定できます。

| パラメーター | タイプ | 必須／オプション | 説明 |
|---------|----------|------|---|
| `name` | 文字列 | 必須 | 導入するカスタムフィールドの名前を記入します。この名前は、 `title` フィールドが空か、見つかりません。 |
| `type` | 文字列 | 必須 | 導入するカスタムフィールドのタイプを示します。 指定できる値： <ul><li>`string`</li><li>`object`</li><li>`integer`</li></ul> |
| `title` | 文字列 | オプション | Platform UI で顧客に表示されるフィールドの名前を示します。 このフィールドが空または見つからない場合、UI は `name` の値です。 |
| `description` | 文字列 | オプション | カスタムフィールドの説明を入力します。この説明は、Platform UI には表示されません。 |
| `isRequired` | ブール値 | オプション | ユーザーが宛先設定ワークフローでこのフィールドの値を指定する必要があるかどうかを示します。 |
| `pattern` | 文字列 | オプション | 必要に応じて、カスタムフィールドのパターンを適用します。正規表現を使用して、パターンを適用します。 例えば、顧客 ID に数字やアンダースコアが含まれない場合は、このフィールドで `^[A-Za-z]+$` を入力します。 |
| `enum` | 文字列 | オプション | カスタムフィールドをドロップダウンメニューとしてレンダリングし、ユーザーが使用できるオプションを一覧表示します。 |
| `default` | 文字列 | オプション | デフォルト値を `enum` リストから定義します。 |
| `hidden` | ブール値 | オプション | 顧客データフィールドが UI に表示されるかどうかを示します。 |
| `unique` | ブール値 | オプション | ユーザーの組織で設定されたすべての宛先データフローで一意の値を持つ顧客データフィールドを作成する必要がある場合は、このパラメータを使用します。  例えば、**[!UICONTROL カスタムパーソナライゼーション]**&#x200B;宛先の「[統合エイリアス](../../../catalog/personalization/custom-personalization.md)」フィールドは一意である必要があります。つまり、この宛先への 2 つの個別のデータフローがこのフィールドに同じ値を持つことはできません。 |
| `readOnly` | ブール値 | オプション | 顧客がフィールドの値を変更できるかどうかを示します。 |

{style="table-layout:auto"}

次の例では、 `customerDataFields` セクションでは、ユーザーが宛先に接続する際に Platform UI に入力する必要がある 2 つのフィールドを定義します。

* `Account ID`:宛先プラットフォームのユーザーアカウント ID。
* `Endpoint region`:接続先の API の地域エンドポイント。 この `enum` 「 」セクションには、で定義された値を含むドロップダウンメニューが作成され、ユーザーが選択できるようになります。

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

結果の UI エクスペリエンスは、次の画像に表示されます。

![顧客データフィールドの例を示す Ui 画像。](../../assets/functionality/destination-configuration/customer-data-fields-example.png)

## 宛先接続の名前と説明 {#names-description}

新しい宛先を作成すると、Destination SDKは自動的に追加します **[!UICONTROL 名前]** および **[!UICONTROL 説明]** フィールドを Platform UI の宛先接続画面に追加します。 上記の例で示すように、 **[!UICONTROL 名前]** および **[!UICONTROL 説明]** フィールドは、顧客データフィールド設定に含まれずに UI でレンダリングされます。

>[!IMPORTANT]
>
>次を追加する場合： **[!UICONTROL 名前]** および **[!UICONTROL 説明]** 顧客データフィールド設定のフィールドには、UI で重複して表示されます。

## 顧客データフィールドの注文 {#ordering}

宛先設定で顧客データフィールドを追加する順序は、Platform UI に反映されます。

例えば、以下の設定は UI に応じて反映され、オプションは順番に表示されます **[!UICONTROL 名前]**, **[!UICONTROL 説明]**, **[!UICONTROL バケット名]**, **[!UICONTROL フォルダーパス]**, **[!UICONTROL ファイルタイプ]**, **[!UICONTROL 圧縮形式]**.

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

![画像 UI のファイル形式設定オプションの順序を示すExperience Platform。](../../assets/functionality/destination-configuration/customer-data-fields-order.png)

## 顧客データフィールドをグループ化 {#grouping}

複数の顧客データフィールドを 1 つのセクション内でグループ化できます。 UI で宛先への接続を設定すると、類似したフィールドを視覚的にグループ化して、ユーザーに表示し、そのメリットをもたらすことができます。

これをおこなうには、 `"type": "object"` ：グループを作成し、 `properties` オブジェクト（以下の画像に示すように、グループ化の場所） **[!UICONTROL CSV オプション]** がハイライト表示されます。

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

![UI での顧客データフィールドのグループ化を示す画像。](../../assets/functionality/destination-configuration/group-customer-data-fields.png)

## 顧客データフィールド用のドロップダウンセレクターの作成 {#dropdown-selectors}

CSV ファイルのフィールドを区切る文字など、複数のオプションから選択できるようにする場合は、UI にドロップダウンフィールドを追加できます。

これをおこなうには、 `namedEnum` 以下に示すようにオブジェクトを選択し、 `default` ユーザーが選択できるオプションの値。

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

![上記の設定で作成されたドロップダウンセレクターの例を示す画面記録。](../../assets/functionality/destination-configuration/customer-data-fields-dropdown.gif)

## 条件付き顧客データフィールドの作成 {#conditional-options}

条件付き顧客データフィールドを作成できます。このフィールドは、ユーザーが特定のオプションを選択した場合にのみアクティベーションワークフローに表示されます。

例えば、ユーザーが特定のファイル書き出しタイプを選択した場合にのみ表示される、条件付きファイル書式オプションを作成できます。

以下の設定では、CSV ファイル形式設定オプションの条件付きグループが作成されます。 CSV ファイルオプションは、書き出し対象のファイルタイプとして CSV を選択した場合にのみ表示されます。

フィールドを条件付きとして設定するには、 `conditional` パラメーターの値を次に示します。

```json
"conditional": {
   "field": "fileType",
   "operator": "EQUALS",
   "value": "CSV"
}
```

より広いコンテキストでは、 `conditional` 以下の宛先設定で、 `fileType` 文字列と `csvOptions` オブジェクトを定義します。

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

以下に、上記の設定に基づいて、結果の UI 画面を示します。 ユーザーがファイルタイプ CSV を選択すると、CSV ファイルタイプを参照する追加のファイル形式設定オプションが UI に表示されます。

![CSV ファイルの条件付きファイル形式オプションを示す画面記録。](../../assets/functionality/destination-configuration/customer-data-fields-conditional.gif)

## テンプレート化された顧客データフィールドへのアクセス {#accessing-templatized-fields}

宛先でユーザー入力が必要な場合は、ユーザーに対して、選択した顧客データフィールドを指定し、Platform UI を使用して入力できるようにする必要があります。 次に、顧客データフィールドからユーザー入力が正しく読み取られるように宛先サーバーを設定する必要があります。 これは、テンプレート化されたフィールドを使用しておこなわれます。

テンプレート化されたフィールドは、形式を使用します `{{customerData.fieldName}}`で、 `fieldName` は、情報を読み取る元の顧客データフィールドの名前です。 テンプレート化されているすべての顧客データフィールドの前には、 `customerData.` 二重括弧で囲まれた `{{ }}`.

例えば、次のAmazon S3 の宛先設定について考えてみましょう。

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

この設定により、ユーザーに [!DNL Amazon S3] バケット名およびフォルダーパスを、それぞれの顧客データフィールドに挿入します。

Experience Platformが [!DNL Amazon S3]を設定する場合、次に示すように、宛先サーバーは、これら 2 つの顧客データフィールドから値を読み取るように設定する必要があります。

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

テンプレート化された値 `{{customerData.bucketName}}` および `{{customerData.path}}` ユーザーが宛先プラットフォームに正常に接続できるように、Experience Platformが指定した値を読み取ります。

テンプレート化されたフィールドを読み取るように宛先サーバーを設定する方法について詳しくは、 [ハードコードされたフィールドとテンプレート化されたフィールド](../destination-server/server-specs.md#templatized-fields).

## 次の手順 {#next-steps}

この記事を読むと、顧客データフィールドを使用してExperience PlatformUI に情報を入力できる方法をより深く理解できるようになります。 また、使用事例に適した顧客データフィールドを選択し、Platform UI で顧客データフィールドを設定、並べ替え、グループ化する方法についても説明します。

その他の宛先コンポーネントについて詳しくは、次の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間の設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータの設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
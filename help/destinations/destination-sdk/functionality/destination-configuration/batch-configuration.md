---
description: Destination SDK で作成された宛先に対するファイル書き出し設定の設定方法を説明します。
title: バッチ設定
source-git-commit: 3f31a54c0cf329d374808dacce3fac597a72aa11
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 91%

---


# バッチ設定 {#batch-configuration}

Destination SDK でバッチ設定オプションを使用して、ユーザーに、書き出されたファイル名のカスタマイズと、その環境設定に応じた書き出しスケジュールの設定を許可します。

Destination SDK でファイルベースの宛先を作成する場合、デフォルトのファイル名および書き出しスケジュールを設定したり、Platform UI からこれらの設定を行うオプションをユーザーに与えたりできます。例えば、以下のような動作を設定できます。

* ファイル名に特定の情報を含める（オーディエンス ID、宛先 ID、カスタム情報など）。
* ユーザーに Platform UI からのファイル名のカスタマイズを許可する。
* 設定した間隔でファイル書き出しが発生するように設定する。
* Platform UI でユーザーが表示できるファイル名および書き出しスケジュールのカスタマイズオプションを定義する。

バッチ設定は、ファイルベースの宛先に対する宛先設定の一部です。

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、[Destination SDK を使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration)方法に関するガイドを参照してください。

`/authoring/destinations` エンドポイントを介してファイル名および書き出しスケジュールを設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべてのバッチ設定オプションを説明し、Platform UI で顧客に何が表示されるかを示します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | × |
| ファイルベースの（バッチ）統合 | ○ |

## サポートされるパラメーター {#supported-parameters}

ここで設定した値は、 [オーディエンスの書き出しをスケジュール](../../../ui/activate-batch-profile-destinations.md#scheduling) ファイルベースの宛先のアクティベーションワークフローの手順です。

```json
"batchConfig":{
   "allowMandatoryFieldSelection":true,
   "allowDedupeKeyFieldSelection":true,
   "defaultExportMode":"DAILY_FULL_EXPORT",
   "allowedExportMode":[
      "DAILY_FULL_EXPORT",
      "FIRST_FULL_THEN_INCREMENTAL"
   ],
   "allowedScheduleFrequency":[
      "DAILY",
      "EVERY_3_HOURS",
      "EVERY_6_HOURS",
      "EVERY_8_HOURS",
      "EVERY_12_HOURS",
      "ONCE"
   ],
   "defaultFrequency":"DAILY",
   "defaultStartTime":"00:00",
   "filenameConfig":{
         "allowedFilenameAppendOptions":[
            "SEGMENT_NAME",
            "DESTINATION_INSTANCE_ID",
            "DESTINATION_INSTANCE_NAME",
            "ORGANIZATION_NAME",
            "SANDBOX_NAME",
            "DATETIME",
            "CUSTOM_TEXT"
         ],
         "defaultFilenameAppendOptions":[
            "DATETIME"
         ],
         "defaultFilename":"%DESTINATION%_%SEGMENT_ID%"
      },
   "segmentGroupingEnabled": true
   }
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `allowMandatoryFieldSelection` | ブール値 | `true` に設定すると、顧客は必須のプロファイル属性を指定できます。デフォルト値は `false` です。詳しくは、[必須の属性](../../../ui/activate-batch-profile-destinations.md#mandatory-attributes)を参照してください。 |
| `allowDedupeKeyFieldSelection` | ブール値 | `true` に設定すると、顧客は重複排除キーを指定できます。デフォルト値は `false` です。詳しくは、[重複排除キー](../../../ui/activate-batch-profile-destinations.md#deduplication-keys)を参照してください。 |
| `defaultExportMode` | 列挙 | デフォルトのファイルのエクスポートモードを定義します。サポートされている値：<ul><li>`DAILY_FULL_EXPORT`</li><li>`FIRST_FULL_THEN_INCREMENTAL`</li></ul> デフォルト値は `DAILY_FULL_EXPORT` です。ファイルエクスポートのスケジューリングについて詳しくは、[バッチ有効化ドキュメント](../../../ui/activate-batch-profile-destinations.md#scheduling)を参照してください。 |
| `allowedExportModes` | リスト | 顧客が使用できるファイルのエクスポートモードを定義します。サポートされている値：<ul><li>`DAILY_FULL_EXPORT`</li><li>`FIRST_FULL_THEN_INCREMENTAL`</li></ul> |
| `allowedScheduleFrequency` | リスト | 顧客が使用できるファイルエクスポートの頻度を定義します。サポートされている値：<ul><li>`ONCE`</li><li>`EVERY_3_HOURS`</li><li>`EVERY_6_HOURS`</li><li>`EVERY_8_HOURS`</li><li>`EVERY_12_HOURS`</li><li>`DAILY`</li></ul> |
| `defaultFrequency` | 列挙 | デフォルトのファイルエクスポートの頻度を定義します。サポートされている値を以下に示します。<ul><li>`ONCE`</li><li>`EVERY_3_HOURS`</li><li>`EVERY_6_HOURS`</li><li>`EVERY_8_HOURS`</li><li>`EVERY_12_HOURS`</li><li>`DAILY`</li></ul> デフォルト値は `DAILY` です。 |
| `defaultStartTime` | 文字列 | ファイルエクスポートのデフォルトの開始時間を定義します。24 時間のファイル形式を使用します。デフォルト値は「00:00」です。 |
| `filenameConfig.allowedFilenameAppendOptions` | 文字列 | *必須*。ユーザーが選択できる、使用可能なファイル名マクロのリスト。これにより、書き出されるファイル名（オーディエンス ID、組織名、書き出しの日時など）に追加される項目が決まります。 `defaultFilename` を設定する場合、必ずマクロが重複するのを避けてください。<br><br>サポートされている値： <ul><li>`DESTINATION`</li><li>`SEGMENT_ID`</li><li>`SEGMENT_NAME`</li><li>`DESTINATION_INSTANCE_ID`</li><li>`DESTINATION_INSTANCE_NAME`</li><li>`ORGANIZATION_NAME`</li><li>`SANDBOX_NAME`</li><li>`DATETIME`</li><li>`CUSTOM_TEXT`</li></ul>マクロを定義する順序にかかわらず、Experience Platform UI は、常に、ここに提示された順序で表示します。<br><br> `defaultFilename` が空の場合、`allowedFilenameAppendOptions` リストには、少なくとも 1 つのマクロが含まれている必要があります。 |
| `filenameConfig.defaultFilenameAppendOptions` | 文字列 | *必須*。ユーザーがチェックを外すことができる、事前に選択されたデフォルトのファイル名マクロ。<br><br> このリストのマクロは、`allowedFilenameAppendOptions` で定義されたもののサブセットです。 |
| `filenameConfig.defaultFilename` | 文字列 | *オプション*。書き出されたファイル用にデフォルトのファイル名マクロを定義します。ユーザーは、これらを上書きできません。<br><br> `allowedFilenameAppendOptions` で定義されたマクロは、`defaultFilename` マクロの後に追加されます。<br><br> `defaultFilename` が空の場合、`allowedFilenameAppendOptions` に少なくとも 1 つのマクロを定義する必要があります。 |
| `segmentGroupingEnabled` | ブール値 | オーディエンス[結合ポリシー](../../../../profile/merge-policies/overview.md)に基づいて、アクティブ化されたオーディエンスを単一のファイルで書き出すか複数のファイルで書き出すかを定義します。サポートされている値： <ul><li>`true`：結合ポリシーごとに 1 つのファイルを書き出します。</li><li>`false`：結合ポリシーに関係なく、オーディエンスごとに 1 つのファイルを書き出します。これはデフォルトの動作です。このパラメーターを完全に省略しても、同じ結果を得ることができます。</li></ul> |

{style="table-layout:auto"}

## ファイル名設定 {#file-name-configuration}

ファイル名設定マクロを使用して、書き出されたファイル名に含まれる必要があるものを定義します。以下の表にあるマクロは、[ファイル名設定](../../../ui/activate-batch-profile-destinations.md#file-names)画面の UI で見つかる要素を記述します。

>[!TIP]
> 
>ベストプラクティスとして、常に、書き出されたファイル名に `SEGMENT_ID` マクロを含める必要があります。セグメント ID は一意なので、ファイル名にそれらを含めることは、ファイル名も必ず一意にするための最適な方法です。

| マクロ | UI ラベル | 説明 | 例 |
|---|---|---|---|
| `DESTINATION` | [!UICONTROL 宛先] | UI の宛先名。 | Amazon S3 |
| `SEGMENT_ID` | [!UICONTROL セグメント ID] | プラットフォームで生成された一意のオーディエンス ID | ce5c5482-2813-4a80-99bc-57113f6acde2 |
| `SEGMENT_NAME` | [!UICONTROL セグメント名] | ユーザー定義のオーディエンス名 | VIP subscriber |
| `DESTINATION_INSTANCE_ID` | [!UICONTROL 宛先 ID] | 一意の、宛先インスタンスの Platform で生成された ID | 7b891e5f-025a-4f0d-9e73-1919e71da3b0 |
| `DESTINATION_INSTANCE_NAME` | [!UICONTROL 宛先名] | 宛先インスタンスのユーザー定義の名前。 | My 2022 Advertising Destination |
| `ORGANIZATION_NAME` | [!UICONTROL 組織名] | Adobe Experience Platform でのお客様の組織の名前。 | My Organization Name |
| `SANDBOX_NAME` | [!UICONTROL サンドボックス名] | 顧客によって使用されるサンドボックスの名前。 | prod |
| `DATETIME` または `TIMESTAMP` | [!UICONTROL 日時] | `DATETIME` と `TIMESTAMP` は両方とも、ファイルが生成されたタイミングを定義しますが、形式が異なります。<br><br><ul><li>`DATETIME` は、YYYYMMDD_HHMMSS の形式を使用します。</li><li>`TIMESTAMP` は、10 桁の Unix 形式を使用します。 </li></ul> `DATETIME` と `TIMESTAMP` は、相互に排他的で、同時に使用できません。 | <ul><li>`DATETIME`：20220509_210543</li><li>`TIMESTAMP`：1652131584</li></ul> |
| `CUSTOM_TEXT` | [!UICONTROL カスタムテキスト] | ファイル名に含まれる、ユーザー定義のカスタムテキスト。`defaultFilename` で使用することはできません。 | My_Custom_Text |
| `TIMESTAMP` | [!UICONTROL 日時] | ファイルが生成された時間の 10 桁のタイムスタンプ（Unix 形式）。 | 1652131584 |
| `MERGE_POLICY_ID` | [!UICONTROL 結合ポリシー ID] | 書き出されたオーディエンスを生成するために使用される[結合ポリシー](../../../../profile/merge-policies/overview.md)の ID。このマクロは、結合ポリシーに基づいて、ファイル内のエクスポートされたオーディエンスをグループ化する場合に使用します。 このマクロは `segmentGroupingEnabled:true` と併用してください。 | e8591fdb-2873-4b12-b63e-15275b1c1439 |
| `MERGE_POLICY_NAME` | [!UICONTROL 結合ポリシー名] | 書き出されたオーディエンスの生成に使用される[結合ポリシー](../../../../profile/merge-policies/overview.md)の名前。このマクロは、結合ポリシーに基づいて、ファイル内のエクスポートされたオーディエンスをグループ化する場合に使用します。 このマクロは `segmentGroupingEnabled:true` と併用してください。 | カスタム結合ポリシー |

{style="table-layout:auto"}

### ファイル名設定の例

以下の設定例に、API 呼び出しで使用される設定と UI に表示されるオプションの対応を示します。

```json
"filenameConfig":{
   "allowedFilenameAppendOptions":[
      "CUSTOM_TEXT",
      "SEGMENT_ID",
      "DATETIME"
   ],
   "defaultFilenameAppendOptions":[
      "SEGMENT_ID",
      "DATETIME"
   ],
   "defaultFilename": "%DESTINATION%"
}
```

![事前選択されたマクロを含むファイル名設定画面を示す UI 画像](../../assets/functionality/destination-configuration/file-name-configuration.png)

## 次の手順 {#next-steps}

この記事を読むことで、ファイルベースの宛先に対するファイル名および書き出しスケジュールの設定方法について、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証設定](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
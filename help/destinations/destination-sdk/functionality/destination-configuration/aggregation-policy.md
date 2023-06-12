---
description: 集計ポリシーを設定して、宛先に対する HTTP リクエストがどのようにグループ化およびバッチ化されるかを説明します。
title: 集計ポリシー
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 100%

---


# 集計ポリシー

API エンドポイントにデータを書き出す際に最大の効率を確保するために、様々な設定を使用して、書き出されたプロファイルをより大きいまたは小さいバッチに集計したり、ID でグループ化したりできます。また、これを使用すると、API エンドポイントでの任意のダウンストリーム制限（レート制限、API 呼び出しごとの ID 数など）に合わせてデータ書き出しをカスタマイズできます。

設定可能な集計を使用して、Destination SDK が提供する設定を深く掘り下げたり、ベストエフォート集計を使用して、API 呼び出しをできる限りバッチ化するように Destination SDK に指示したりできます。

Destination SDK を使用してリアルタイム（ストリーミング）宛先を作成する際に、書き出されたプロファイルを結果の書き出しにどのように組み合わせるかを設定できます。この動作は、集計ポリシー設定によって決まります。

このコンポーネントが Destination SDK で作成される統合のどこに適合するかを把握するには、[設定オプション](../configuration-options.md)ドキュメントの図を参照するか、[Destination SDK を使用したストリーミング宛先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration)方法に関するガイドを参照してください。

`/authoring/destinations` エンドポイントを介して集計ポリシー設定を設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての集計ポリシー設定について説明します。

このドキュメントを読み終えたら、[テンプレートの使用](../../functionality/destination-server/message-format.md#using-templating)および[集計キーの例](../../functionality/destination-server/message-format.md#template-aggregation-key)に関するドキュメントを参照して、選択した集計ポリシーに基づいてメッセージ変換テンプレートに集計ポリシーを含める方法を理解します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | × |

## ベストエフォート集計 {#best-effort-aggregation}

ベストエフォート集計は、リクエストあたりのプロファイル数が少ないほうが好まれ、多くのデータで少ない数のリクエストを受けるよりも、少ないデータで多くのリクエストを受けることを希望する宛先に最適です。

以下の設定例に、ベストエフォート集計設定を示します。設定可能な集計の例については、[設定可能な集計](#configurable-aggregation)の節を参照してください。ベストエフォート集計に適用されるパラメーターは、以下の表に記載されています。

```json
"aggregation":{
   "aggregationType":"BEST_EFFORT",
   "bestEffortAggregation":{
      "maxUsersPerRequest":10,
      "splitUserById":false
   }
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `aggregationType` | 文字列 | 宛先が使用する必要がある、集計ポリシーのタイプを示します。サポートされる集計タイプ： <ul><li>`BEST_EFFORT`</li><li>`CONFIGURABLE_AGGREGATION`</li></ul> |
| `bestEffortAggregation.maxUsersPerRequest` | 整数 | Adobe Experience Platform は、1 回の HTTP 呼び出しで書き出された複数のプロファイルを集計できます。<br><br>この値は、1 回の HTTP 呼び出しでエンドポイントが受け取るプロファイルの最大数を示します。これはベストエフォートの集計であることに注意してください。例えば、値 100 を指定した場合、Platform が 1 回の呼び出しで送信するプロファイルの数は、100 未満の任意の数になります。<br><br>サーバーが 1 回のリクエストで複数のユーザーを受け入れない場合、この値を `1` に設定します。 |
| `bestEffortAggregation.splitUserById` | ブール値 | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。サーバーが 1 回の呼び出しで 1 つの ID しか受け入れない場合、特定の ID 名前空間に対して、このフラグを `true` に設定します。 |

{style="table-layout:auto"}

>[!TIP]
>
>API エンドポイントが 1 回の API 呼び出しで受け入れるプロファイルが 100 個未満の場合、ベストエフォート集計を使用します。

## 設定可能な集計 {#configurable-aggregation}

設定可能な集計は、同じ呼び出しに何千ものプロファイルを含む、大きなバッチを取りたい場合に最適です。また、このオプションを使用すると、複雑な集計ルールに基づいて、書き出されたプロファイルを集計できます。

以下の設定例に、設定可能な集計設定を示します。ベストエフォート集計の例については、[ベストエフォート集計](#best-effort-aggregation)の節を参照してください。設定可能な集計に適用されるパラメーターは、以下の表に記載されています。

```json
"aggregation":{
   "aggregationType":"CONFIGURABLE_AGGREGATION",
   "configurableAggregation":{
      "splitUserById":true,
      "maxBatchAgeInSecs":2400,
      "maxNumEventsInBatch":5000,
      "aggregationKey":{
         "includeSegmentId":true,
         "includeSegmentStatus":true,
         "includeIdentity":true,
         "oneIdentityPerGroup":true,
         "groups":[
            {
               "namespaces":[
                  "IDFA",
                  "GAID"
               ]
            },
            {
               "namespaces":[
                  "EMAIL"
               ]
            }
         ]
      }
   }
}
```

| パラメーター | タイプ | 説明 |
|---------|----------|------|
| `aggregationType` | 文字列 | 宛先が使用する必要がある、集計ポリシーのタイプを示します。サポートされる集計タイプ： <ul><li>`BEST_EFFORT`</li><li>`CONFIGURABLE_AGGREGATION`</li></ul> |
| `configurableAggregation.splitUserById` | ブール値 | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。サーバーが 1 回の呼び出しで 1 つの ID しか受け入れない場合、特定の ID 名前空間に対して、このフラグを `true` に設定します。 |
| `configurableAggregation.maxBatchAgeInSecs` | 整数 | このパラメーターは、`maxNumEventsInBatch` と共に使用して、Experience Platform がエンドポイントに API 呼び出しを送信するまで待機する時間を決定します。 <ul><li>最小値（秒）：1800</li><li>最大値（秒）：3600</li></ul> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platform は、API 呼び出しを行う前に、3,600 秒か、認定済みプロファイルが 10,000 個になるまで待機します（いずれか早い方）。 |
| `configurableAggregation.maxNumEventsInBatch` | 整数 | このパラメーターは、`maxBatchAgeInSecs` と共に使用して API 呼び出しにいくつの認定プロファイルを集計するかを決定します。 <ul><li>最小値：1000</li><li>最大値：10000</li></ul> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platform は、API 呼び出しを行う前に、3,600 秒か、認定済みプロファイルが 10,000 個になるまで待機します（いずれか早い方）。 |
| `configurableAggregation.aggregationKey` | - | 以下に記述するパラメーターに基づいて、宛先にマッピングされた書き出し済みプロファイルを集計できます。 |
| `configurableAggregation.aggregationKey.includeSegmentId` | ブール値 | 宛先に書き出されたプロファイルをセグメント ID でグループ化する場合は、このパラメーターを `true` に設定します。 |
| `configurableAggregation.aggregationKey.includeSegmentStatus` | ブール値 | 宛先に書き出されたプロファイルをセグメント ID とセグメントステータスでグループ化する場合は、このパラメーターと `includeSegmentId` の両方を `true` に設定します。 |
| `configurableAggregation.aggregationKey.includeIdentity` | ブール値 | 宛先に書き出されたプロファイルを ID 名前空間でグループ化する場合は、このパラメーターを `true` に設定します。 |
| `configurableAggregation.aggregationKey.oneIdentityPerGroup` | ブール値 | 書き出されたプロファイルを単一の ID（GAID、IDFA、電話番号、メールなど）に基づいてグループに集計する場合は、このパラメーターを `true` に設定します。 |
| `configurableAggregation.aggregationKey.groups` | 配列 | 宛先に書き出されたプロファイルを ID 名前空間のグループでグループ化する場合は、ID グループのリストを作成します。例えば、上記の例に示した設定を使用して、IDFA および GAID モバイル識別子を含むプロファイルを宛先に対する 1 つの呼び出しに、メールを別の呼び出しに、それぞれ組み合わせることができます。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むことで、宛先に対する集計ポリシーの設定方法について、理解を深めることができました。

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
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
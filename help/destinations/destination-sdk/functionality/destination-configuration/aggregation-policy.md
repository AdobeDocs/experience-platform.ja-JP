---
description: 宛先への HTTP リクエストをグループ化およびバッチ化する方法を決定する、集計ポリシーを設定する方法について説明します。
title: 集計ポリシー
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 12%

---


# 集計ポリシー

API エンドポイントにデータを書き出す際の効率を最大限に高めるには、様々な設定を使用して、書き出されたプロファイルを大規模または小規模のバッチに集計し、ID 別にグループ化したり、その他の使用例を使用できます。 また、API エンドポイントに対するダウンストリームの制限（レート制限、API 呼び出しあたりの ID 数など）に応じて、データのエクスポートを調整できます。

設定可能な集計を使用して、Destination SDKが提供する設定の詳細を調べるか、ベストエフォート集計を使用して、API 呼び出しをできる限り最善の方法でバッチ処理するようDestination SDKに指示します。

Destination SDKを使用してリアルタイム（ストリーミング）の宛先を作成する場合、書き出したプロファイルを結合した書き出しで結合する方法を設定できます。 この動作は、集計ポリシーの設定によって決まります。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したストリーミング先の設定](../../guides/configure-destination-instructions.md#create-destination-configuration).

集計ポリシーの設定は、 `/authoring/destinations` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての集計ポリシー設定について説明します。

このドキュメントを読んだ後、 [テンプレートの使用](../../functionality/destination-server/message-format.md#using-templating) そして [集計の主な例](../../functionality/destination-server/message-format.md#template-aggregation-key) を参照して、選択した集計ポリシーに基づいて、メッセージ変換テンプレートに集計ポリシーを含める方法を理解してください。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | いいえ |

## ベストエフォート集計 {#best-effort-aggregation}

ベストエフォートの集計は、リクエストあたりのプロファイル数を少なくし、データ量の多いリクエストより少ないデータで多くのリクエストを取り込む宛先に最適です。

次の設定例は、ベストエフォートの集計設定を示しています。 設定可能な集計の例については、 [設定可能な集計](#configurable-aggregation) 」セクションに入力します。 ベストエフォートの集計に適用できるパラメーターを以下の表に示します。

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
| `aggregationType` | 文字列 | 宛先で使用する集計ポリシーのタイプを示します。 サポートされる集計タイプ： <ul><li>`BEST_EFFORT`</li><li>`CONFIGURABLE_AGGREGATION`</li></ul> |
| `bestEffortAggregation.maxUsersPerRequest` | 整数 | Adobe Experience Platformは、1 回の HTTP 呼び出しで書き出された複数のプロファイルを集計できます。 <br><br>この値は、1 回の HTTP 呼び出しでエンドポイントが受け取るプロファイルの最大数を示します。 これはベストエフォートの集計であることに注意してください。例えば、値 100 を指定した場合、Platform が 1 回の呼び出しで送信するプロファイルの数は、100 未満の任意の数になります。<br><br> サーバーが 1 回のリクエストで複数のユーザーを受け入れない場合、この値をに設定します。 `1`. |
| `bestEffortAggregation.splitUserById` | ブール値 | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。このフラグをに設定します。 `true` サーバーが呼び出しごとに 1 つの id のみを受け入れる場合（特定の id 名前空間に対して）。 |

{style="table-layout:auto"}

>[!TIP]
>
>API エンドポイントが API 呼び出しあたり 100 個未満のプロファイルを受け入れる場合は、ベストエフォートの集計を使用します。

## 構成可能な集計 {#configurable-aggregation}

多数のプロファイルを同じ呼び出しで扱い、大きなバッチを取る場合は、設定可能な集計が最適です。 また、このオプションを使用すると、複雑な集計ルールに基づいて、書き出されたプロファイルを集計できます。

次の設定例は、設定可能な集計設定を示しています。 ベストエフォート集計の例については、 [ベストエフォート集計](#best-effort-aggregation) 」セクションに入力します。 設定可能な集計に適用できるパラメーターを次の表に示します。

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
| `aggregationType` | 文字列 | 宛先で使用する集計ポリシーのタイプを示します。 サポートされる集計タイプ： <ul><li>`BEST_EFFORT`</li><li>`CONFIGURABLE_AGGREGATION`</li></ul> |
| `configurableAggregation.splitUserById` | ブール値 | 宛先への呼び出しを ID で分割する必要がある場合は、このフラグを使用します。このフラグをに設定します。 `true` サーバーが呼び出しごとに 1 つの id のみを受け入れる場合（特定の id 名前空間に対して）。 |
| `configurableAggregation.maxBatchAgeInSecs` | 整数 | ～と併用される `maxNumEventsInBatch`の値が 0 の場合、このパラメーターは、Experience Platformがエンドポイントに API 呼び出しを送信するまで待機する時間を決定します。 <ul><li>最小値（秒）:1800</li><li>最大値（秒）:3600</li></ul> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platformは、3600 秒または認定されたプロファイルが存在するまで API 呼び出しをおこなうまで、いずれか最初に発生するまで待ちま10000。 |
| `configurableAggregation.maxNumEventsInBatch` | 整数 | と共に使用されます。 `maxBatchAgeInSecs`の場合、このパラメーターは、API 呼び出しで集計する認定プロファイルの数を決定します。 <ul><li>最小値：1000</li><li>最大値：10000</li></ul> 例えば、両方のパラメーターに最大値を使用した場合、Experience Platformは、3600 秒または認定されたプロファイルが存在するまで API 呼び出しをおこなうまで、いずれか最初に発生するまで待ちま10000。 |
| `configurableAggregation.aggregationKey` | - | 以下に説明するパラメーターに基づいて、宛先にマッピングされたエクスポート済みプロファイルを集計できます。 |
| `configurableAggregation.aggregationKey.includeSegmentId` | ブール値 | このパラメーターをに設定します。 `true` を選択します。 |
| `configurableAggregation.aggregationKey.includeSegmentStatus` | ブール値 | このパラメーターと `includeSegmentId` から `true`を選択します。 |
| `configurableAggregation.aggregationKey.includeIdentity` | ブール値 | このパラメーターをに設定します。 `true` を追加します。 |
| `configurableAggregation.aggregationKey.oneIdentityPerGroup` | ブール値 | このパラメーターをに設定します。 `true` 書き出したプロファイルを、単一の id（GAID、IDFA、電話番号、電子メールなど）に基づいてグループに集計する場合。 |
| `configurableAggregation.aggregationKey.groups` | 配列 | ID 名前空間のグループで、宛先に書き出されたプロファイルをグループ化する場合は、ID グループのリストを作成します。 例えば、上記の例に示す設定を使用して、IDFA および GAID のモバイル識別子を含むプロファイルを、宛先への呼び出しと、別の電子メールへの呼び出しに組み合わせることができます。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

この記事を読むと、宛先の集計ポリシーを設定する方法をより深く理解できるはずです。

その他の宛先コンポーネントについて詳しくは、次の記事を参照してください。

* [顧客認証設定](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間の設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータの設定](audience-metadata-configuration.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
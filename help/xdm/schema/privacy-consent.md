---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;consent;Consent;preferences;Preferences;privacyOptOuts;marketingPreferences;optOutType;basisOfProcessing;
solution: Adobe Experience Platform
title: プライバシーミックスインの概要
description: プライバシー/マーケティング環境設定（同意）ミックスインは、CMPや他の顧客から生成されるユーザー権限や環境設定の収集をサポートするExperience Data Model(XDM)ミックスインです。 このドキュメントでは、ミックスインが提供する様々なフィールドの構造と使用方法を説明します。
topic: guide
translation-type: tm+mt
source-git-commit: 74a4a3cc713cc068be30379e8ee11572f8bb0c63
workflow-type: tm+mt
source-wordcount: '1827'
ht-degree: 1%

---


# [!DNL Privacy Consent] mixinの概要

ミッ [!DNL Privacy/Marketing Preferences (Consent)] クスイン（以下「ミックスイン」と呼ばれる）は、[!DNL Privacy Consent][!DNL Experience Data Model] (XDM)ミックスインで、お客様からCMPや他のソースによって生成されるユーザー権限や環境設定の収集をサポートすることを目的としています。 このドキュメントでは、ミックスインが提供する様々なフィールドの構造と使用方法を説明します。

## 前提条件 {#prerequisites}

このドキュメントでは、 [!DNL Experience Data Model] (XDM)とのスキーマの使用に関する十分な理解が必要 [!DNL Experience Platform]です。 先に進む前に、次のドキュメントを確認してください。

* [XDM システムの概要](http://www.adobe.com/go/xdm-home-en)
* [スキーマ構成の基本](http://www.adobe.com/go/xdm-schema-best-practices-en)

## スキーマの例 {#schema}

>[!NOTE]
>
>次の例は、Mixinが提供する主なフィールドについて説明するこのドキュメントの次の節にコンテキストを提供するために、Mixin [!DNL Platform][!DNL Privacy Consent] を介して送信されるデータの構造を示すためのものです。 スキーマの構造の完全な例は、 [付録](#full-schema) 「参照用」に記載されています。

次のJSONは、 [!DNL Privacy Consent] Mixinが処理できるデータのタイプの例を示しています。 これらの各フィールドの具体的な使用方法については、次の節で説明します。

```json
{
  "xdm:privacyOptOuts": [
     {
        "xdm:optOutType": "general_opt_out",
        "xdm:optOutValue": "in",
        "xdm:timestamp": "2019-01-01T15:52:25+00:00",
        "xdm:basisOfProcessing": "legitimate_interest"
     },
     {
        "xdm:optOutType": "device_linking",
        "xdm:optOutValue": "not_provided",
        "xdm:basisOfProcessing": "vital_interest"
     },
     {
        "xdm:optOutType": "anonymous_analysis",
        "xdm:optOutValue": "out"
     }
  ],
  "xdm:personalizationPreferences": {
     "xdm:default": {
        "xdm:choice": "unknown",
        "xdm:timestamp": "2019-01-01T15:52:25+00:00",
        "xdm:basisOfProcessing": "consent"
     },
     "xdm:details": [
        {
           "xdm:type": "email",
           "xdm:choice": "in"
        },
        {
           "xdm:type": "push_notifications",
           "xdm:choice": "out",
           "xdm:basisOfProcessing": "legitimate_interest",
           "xdm:timestamp": "2019-01-01T15:52:25+00:00"
        }
     ]
  },
  "xdm:marketingPreferences": {
     "xdm:default": {
        "xdm:choice": "unknown"
     },
     "xdm:details": [
        {
           "xdm:type": "email",
           "xdm:choice": "in",
           "xdm:subscriptions": {
              "weekly_mailer": {
                 "xdm:choice": "out",
                 "xdm:timestamp": "2019-02-03T15:52:25+00:00"
              },
              "daily_newsletter": {
                 "xdm:choice": "pending"
              }
           }
        },
        {
           "xdm:type": "iot",
           "xdm:choice": "out",
           "xdm:timestamp": "2019-01-01T15:52:25+00:00",
           "xdm:basisOfProcessing": "legitimate_interest",
           "xdm:subscriptions": {
              "out_of_milk": {
                 "xdm:choice": "in"
              }
           }
        }
     ]
  },
  "xdm:version": "1.0.0",
  "xdm:timestamp": "2019-01-01T15:52:25+00:00",
  "xdm:userLocale": "UK",
  "xdm:localeSource": "ip"
}
```

## フィールド {#fields}

以下の節では、 [!DNL Privacy Consent] Mixinが提供する主なフィールドの使用方法と、そのサブフィールドの構造を説明します。

### xdm:privacyOptOuts {#privacyOptOuts}

`xdm:privacyOptOuts` は、お客様が選択した一般的なオプトアウト設定を表す配列です。 この配列には複数のオブジェクトを含めることができます。各オブジェクトは、特定のオプトアウトタイプと、そのタイプに対してユーザーが選択した環境設定を表します。

```json
"xdm:privacyOptOuts": [
     {
        "xdm:optOutType": "general_opt_out",
        "xdm:optOutValue": "in",
        "xdm:timestamp": "2019-01-01T15:52:25+00:00",
        "xdm:basisOfProcessing": "legitimate_interest"
     },
     {
        "xdm:optOutType": "device_linking",
        "xdm:optOutValue": "not_provided",
        "xdm:basisOfProcessing": "vital_interest"
     },
     {
        "xdm:optOutType": "anonymous_analysis",
        "xdm:optOutValue": "out"
     }
  ]
```

| プロパティ | 説明 |
| --- | --- |
| `xdm:optOutType` | オプトアウトのタイプ。 使用できる値と定義については、 [付録](#optOutType-values) を参照してください。 |
| `xdm:optOutValue` | 顧客がこの特定のオプトアウトタイプに対して選択した優先順位。 使用できる値と定義については、 [付録](#choice-optOutValue-values) を参照してください。 |
| `xdm:timestamp` | オプトアウト設定が変更された場合のISO 8601タイムスタンプ（該当する場合）。 |
| `xdm:basisOfProcessing` | データを収集して処理するプライバシー関連の基準を示します。 デフォルトでは、このフィールドはに設定され `consent`ており、ユーザーが同意した場合（反映された場合）にのみデータが処理されることを示し `xdm:optOutValue`ます。<br><br>場合によっては、データ処理に関する同意を求めるメッセージが表示されないことがあります。 `xdm:basisOfProcessing` の値は、同意プロンプトが提供されなかった理由を示して、この場合はオプトアウトオブジェクトに含める必要があります。 使用できる値と定義については、 [付録](#basisOfProcessing-values) を参照してください。 |

### xdm:personalizationPreferences {#personalizationPreferences}

`xdm:personalizationPreferences` データをどのようにパーソナライズに使用できるかに関する顧客の環境設定を取得します。 ユーザーは、特定のパーソナライゼーションの使用例オプトアウト、またはパーソナライゼーション全体オプトアウトを使用できます。

>[!IMPORTANT]
>
>`xdm:personalizationPreferences` マーケティングの使用例は説明していません。 例えば、顧客が電子メールのパーソナライゼーションをオプトアウトした場合、電子メールの受信は停止されません。 その代わりに、受信した電子メールは一般的で、プロファイルに基づくものではありません。
>
>同じ例で、顧客が電子メールマーケティングを( `xdm:marketingPreferences`次の節で説明するを通して [](#marketingPreferences))オプトアウトした場合、電子メールのパーソナライゼーションが許可されていても、その顧客は電子メールを受け取りません。

```json
"xdm:personalizationPreferences": {
     "xdm:default": {
        "xdm:choice": "unknown",
        "xdm:timestamp": "2019-01-01T15:52:25+00:00",
        "xdm:basisOfProcessing": "consent"
     },
     "xdm:details": [
        {
           "xdm:type": "email",
           "xdm:choice": "in"
        },
        {
           "xdm:type": "push_notifications",
           "xdm:choice": "out",
           "xdm:basisOfProcessing": "legitimate_interest",
           "xdm:timestamp": "2019-01-01T15:52:25+00:00"
        }
     ]
  }
```

| プロパティ | 説明 |
| --- | --- |
| `xdm:default` | このオブジェクトに指定するデータは、顧客が個人設定を行う際に設定する環境設定を表します。 |
| `xdm:details` | オブジェクトの配列。ユーザーが設定した個別のパーソナライゼーションタイプごとに1つずつ。 |
| `xdm:choice` | 一般的なパーソナライゼーションに対する顧客提供の同意嗜好、または、それぞれの下に提供されるか、または下に提供されるかに応じて特定のパーソナライゼーションタイプ `xdm:default` を `xdm:details`設定する。 使用できる値と定義については、 [付録](#choice-optOutValue-values) を参照してください。 |
| `xdm:type` | アレイ内のオブジェクトは、顧客が同意データを提供する特定のパーソナライゼーションの使用事例を示すために、このフィールドを指定する必要があります。 `xdm:details` 使用できる値と定義については、 [付録](#type-values) を参照してください。 |
| `xdm:timestamp` | オプトアウト設定が変更された場合のISO 8601タイムスタンプ（該当する場合）。 |
| `xdm:basisOfProcessing` | データを収集して処理するプライバシー関連の基準を示します。 デフォルトでは、このフィールドはに設定され `consent`ており、ユーザーが同意した場合（反映された場合）にのみデータが処理されることを示し `xdm:choice`ます。<br><br>場合によっては、顧客に対してパーソナライゼーションの同意を求めるメッセージが表示されないことがあります。 `xdm:basisOfProcessing` の値は、同意プロンプトが提供されなかった理由を示して、この場合はオプトアウトオブジェクトに含める必要があります。 使用できる値と定義については、 [付録](#basisOfProcessing-values) を参照してください。 |


### xdm:marketingPreferences {#marketingPreferences}

`xdm:marketingPreferences` データを使用できるマーケティング目的に関する顧客の判断基準を取り込みます。 特定オプトアウトのマーケティングの使用例や、完全なダイレクトマーケティングオプトアウトのユーザーが考えられます。

```json
"xdm:marketingPreferences": {
     "xdm:default": {
        "xdm:choice": "unknown"
     },
     "xdm:details": [
        {
           "xdm:type": "email",
           "xdm:choice": "in",
           "xdm:subscriptions": {
              "weekly_mailer": {
                 "xdm:choice": "out",
                 "xdm:timestamp": "2019-02-03T15:52:25+00:00"
              },
              "daily_newsletter": {
                 "xdm:choice": "pending"
              }
           }
        },
        {
           "xdm:type": "iot",
           "xdm:choice": "out",
           "xdm:timestamp": "2019-01-01T15:52:25+00:00",
           "xdm:basisOfProcessing": "legitimate_interest",
           "xdm:subscriptions": {
              "out_of_milk": {
                 "xdm:choice": "in"
              }
           }
        }
     ]
  }
```

| プロパティ | 説明 |
| --- | --- |
| `xdm:default` | このオブジェクトに指定するデータは、ダイレクトマーケティング全体に対する顧客の好みを表します。 |
| `xdm:details` | オブジェクトの配列。顧客がプリファレンスを提供した特定のマーケティング使用事例ごとに1つずつ。 |
| `xdm:choice` | 一般的なマーケティングに対する顧客提供の同意の好み、または、それぞれの下に提供されるか、または下に提供されるかに応じて特定のマーケティングの使用例 `xdm:default``xdm:details`です。 使用できる値と定義については、 [付録](#choice-optOutValue-values) を参照してください。 |
| `xdm:subscriptions` | メールリストやニュースレターなど、会社固有の購読をキーで表すオブジェクトです。 各購読オブジェクトには、対象の購読の `xdm:choice` 値が順に含まれている必要があります。 |
| `xdm:type` | アレイ内のオブジェクトは、顧客が同意データを提供する特定のマーケティング使用事例を示すために、このフィールドを指定する必要があります。 `xdm:details` 使用できる値と定義については、 [付録](#type-values) を参照してください。 |
| `xdm:timestamp` | オプトアウト設定が変更された場合のISO 8601タイムスタンプ（該当する場合）。 |
| `xdm:basisOfProcessing` | データを収集して処理するプライバシー関連の基準を示します。 デフォルトでは、このフィールドはに設定され `consent`ており、ユーザーが同意した場合（反映された場合）にのみデータが処理されることを示し `xdm:choice`ます。<br><br>状況によっては、ダイレクトマーケティングの同意を求めるメッセージが表示されないことがあります。 `xdm:basisOfProcessing` の値は、同意プロンプトが提供されなかった理由を示して、この場合はオプトアウトオブジェクトに含める必要があります。 使用できる値と定義については、 [付録](#basisOfProcessing-values) を参照してください。 |

## Mixinを使用した同意データの取り込み {#ingest}

お客様の同意データを取り込むために [!DNL Privacy Consent] Mixinを使用するには、新規または既存のスキーマにMixinを追加し、そのスキーマに基づいてデータセットを作成する必要があります。

スキーマにミックスインを追加する手順については、UIでのスキーマの [作成に関するチュートリアルを参照してください](http://www.adobe.com/go/xdm-schema-editor-tutorial-en) 。 このミックスインは[!DNL  Privacy Consent] 、クラス [!DNL XDM Individual Profile] またはに基づくスキーマと互換性があり [!DNL XDM ExperienceEvent]ます。

Mixinを含むスキーマーを作成したら、既存のスキーマーでデータセットを作成する手順に従って、『データセットユーザーガイド [!DNL Privacy Consent] 』のデータセットの [](../../catalog/datasets/user-guide.md#create) 作成に関する節を参照してください。

>[!IMPORTANT]
>
>に同意データを送信する場合 [!DNL Real-time Customer Profile]は、mixinを含む [!DNL Profile]クラスに基づいて [!DNL XDM Individual Profile] 有効なスキーマを作成する必要があり [!DNL Privacy Consent] ます。 そのスキーマに基づいて作成したデータセットも有効にする必要があり [!DNL Profile]ます。 要件に関する具体的な手順については、上記のチュートリアルを参照して [!DNL Real-time Customer Profile] ください。

## 付録 {#appendix}

以下の節では、 [!DNL Privacy Consent] Mixinに関する追加の参照情報を示します。

### xdm:optOutTypeに指定できる値 {#optOutType-values}

次の表に、使用できる値の概要を示し `xdm:optOutType`ます。

| 値 | 説明 |
| --- | --- |
| `general_opt_out` | データはどの目的にも使用できません。 これは通常、処理の基本が「同意」でない場合を除き、データ収集をブロックします。<br><br>このオプトアウトタイプを使用する場合、受け入れられる値 `in` を使用し、次のコンテキスト的な意味 `out` を取得します。<ul><li>`in`:ユーザ **** は、一般処理に使用するデータに対して同意を与えた。</li><li>`out`:ユーザ **は、一般処理に使用されるデータに同意しない** 。</li></ul>詳しくは、xdm:optOutValueに [指定できる値の表を参照してください](#choice-optOutValue-values) 。 |
| `anonymous_analysis` | 特定のページが表示された回数など、ユーザーIDの種類を必要としない汎用Web指標には、このデータを使用できません。 |
| `device_linking` | 訪問者が使用する1つのデバイスのデータを、同じ訪問者が使用する別のデバイスのデータと組み合わせることはできません。 デバイスは、一般的なユーザー名や電子メールアドレスなどのテクニックを使用してリンクされ、多くの場合、AdobeデバイスのCo-opやプライベートデバイスグラフを介してリンクされます。 |
| `pseudonymous_analysis` | このデータは、Adobe Analyticsが提供するWeb指標には使用できません。偽名のIDを必要とする場合は、Webサイトを経由するパス（フォールアウトレポートなど）を識別し、セッションを確立したり、アトリビューションを目的として使用します。 |
| `sales_sharing_opt_out` | このデータは、販売目的や第三者との共有には使用できません。<br><br>このオプトアウトタイプを使用する場合、受け入れられる値 `in` を使用し、次のコンテキスト的な意味 `out` を取得します。<ul><li>`in`:ユーザー **は、販売および共有の目的で使用するデータに関して同意を示しました** 。</li><li>`out`:ユーザー **は、販売および共有の目的で使用されるデータに同意しません** 。</li></ul>詳しくは、xdm:optOutValueに [指定できる値の表を参照してください](#choice-optOutValue-values) 。 |

### xdm:basisOfProcessingに指定できる値 {#basisOfProcessing-values}

次の表に、使用できる値の概要を示し `xdm:basisOfProcessing`ます。

| 値 | 説明 |
| --- | --- |
| `consent` **（デフォルト）** | 個人が明示的な権限を与えている場合、指定した目的のデータ収集が許可されます。 他の値を指定しない `xdm:basisOfProcessing` 場合は、これがデフォルト値のです。 <br><br>**重要**:との値は、に設定した場合 `xdm:choice` にのみ使用され `xdm:optOutValue``xdm:basisOfProcessing``consent`ます。 代わりに、この表に示す他の値のいずれかが使用される場合 `xdm:basisOfProcessing` は、そのユーザーの同意の選択は無視されます。 |
| `compliance` | 特定の目的に対するデータの収集は、ビジネスの法的義務を満たすために必要です。 |
| `contract` | 特定の目的に応じたデータの収集は、個人との契約上の義務を満たすために必要です。 |
| `legitimate_interest` | 特定の目的でこのデータを収集して処理する正当なビジネス上の関心事は、個人に与える潜在的な損害を上回ります。 |
| `public_interest` | 特定の目的に係るデータの収集は、公益上のタスク又は職権の行使上の行為に必要とされる。 |
| `vital_interest` | 特定の目的に合わせたデータの収集は、個人の重要な利益を保護するために必要です。 |

### xdm:choiceおよびxdm:optOutValueに指定できる値 {#choice-optOutValue-values}

次の表に、 `xdm:choice` およびで使用できる値の概要を示し `xdm:optOutValue`ます。

| 値 | 説明 |
| --- | --- |
| `pending` | システムは、まだお客様から同意優先情報を受け取っていません。 同意を得ながら、Webサイトの最初のページに使用できます。 明示的に提供されるのではなく、同意が「想定」されることを示すためにも使用できます。 |
| `in` | ユーザーが同意の優先順位にオプトインしました。 言い換えると、本 **人は** 、本件の同意優先事項で示されるデータの使用に同意する。 |
| `out` | ユーザーが同意の優先順位をオプトアウトしました。 つまり、本人 **は** 、本件の同意優先事項で示されるデータの使用に同意しない。 |
| `not_applicable` | 本件の同意の好みは、お客様には当てはまらない。 |
| `not_provided` | お客様は同意優先情報を提供していません。 |
| `unknown` | お客様の同意好み情報が不明です。 |

### xdm:typeに指定できる値 {#type-values}

次の表に、使用できる値の概要を示し `xdm:type`ます。

| 値 | 説明 |
| --- | --- |
| `ads` | 無関係なWebサイトから表示できる広告。 |
| `content` | Webサイトに表示されるコンテンツ。 |
| `customer_support` | カスタマーサポートに関連するデータ。 |
| `email` | 電子メールメッセージ。 |
| `iot` | 「物のインターネット」に関連するデータ(IoT)。 |
| `in_app_messages` | アプリ内メッセージ. |
| `in_home` | ホームメッセージ。 |
| `in_store` | ストア内メッセージ。 |
| `in_vehicle` | 車内メッセージ。 |
| `offers` | 特殊オファー。 |
| `phone_calls` | 電話のインタラクションに関連するデータ。 |
| `push_notifications` | プッシュ通知. |
| `sms` | SMSメッセージ。 |
| `social_media` | ソーシャルメディアコンテンツ。 |
| `snail_mail` | 従来の郵便配信で送られたメッセージ。 |
| `third_party_content` | Webサイトに表示され、無関係なエンティティによって提供されるコンテンツまたは記事。 |
| `third_party_offers` | Webサイトの広告サービスに表示されるオファーまたは広告（関連のないエンティティからのもの）。 |

### フル [!DNL Privacy Consent] スキーマ {#full-schema}

次のJSONは、スキーマレジストリに表示される完全な [!DNL Privacy Consent] スキーマを表します。

```json
{
  "meta:license": [
    "Copyright 2019 Adobe Systems Incorporated. All rights reserved.",
    "This work is licensed under a Creative Commons Attribution 4.0 International (CC BY 4.0) license",
    "you may not use this file except in compliance with the License. You may obtain a copy",
    "of the License at https://creativecommons.org/licenses/by/4.0/"
  ],
  "$id": "https://ns.adobe.com/xdm/context/consent-preferences",
  "$schema": "http://json-schema.org/draft-06/schema#",
  "title": "Privacy/Marketing Preferences (Consent)",
  "description": "This schema captures privacy, personalization and marketing preferences (consents).",
  "type": "object",
  "meta:extensible": true,
  "meta:abstract": true,
  "definitions": {
    "consentValue": {
      "type": "string",
      "enum": [
        "not_provided",
        "pending",
        "in",
        "out",
        "unknown",
        "not_applicable"
      ],
      "meta:enum": {
        "not_provided": "Not provided",
        "pending": "Pending verification",
        "in": "Opt-in",
        "out": "Opt-out",
        "unknown": "Unknown",
        "not_applicable": "Not Applicable"
      }
    },
    "basisOfProcessing": {
      "title": "Basis Of Processing",
      "type": "string",
      "description": "Basis of Processing",
      "enum": [
        "consent",
        "legitimate_interest",
        "contract",
        "vital_interest",
        "compliance",
        "public_interest"
      ],
      "meta:enum": {
        "consent": "User Consent",
        "legitimate_interest": "Legitimate Interest",
        "contract": "Contract",
        "vital_interest": "Vital Interest of the Individual",
        "compliance": "Compliance with a Legal Obligation",
        "public_interest": "Public Interest"
      }
    },
    "timestamp": {
      "title": "Preference timestamp",
      "description": "Timestamp of this specific opt out or preference.",
      "type": "string",
      "format": "date-time"
    },
    "consent-preferences": {
      "properties": {
        "xdm:privacyOptOuts": {
          "title": "Privacy Preferences",
          "description": "Encapsulates data privacy preferences.",
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "xdm:optOutType": {
                "title": "Opt-out type",
                "type": "string",
                "description": "The type of user permission.",
                "enum": [
                  "general_opt_out",
                  "sales_sharing_opt_out",
                  "anonymous_analysis",
                  "pseudonymous_analysis",
                  "device_linking"
                ],
                "meta:enum": {
                  "general_opt_out": "General opt-out",
                  "sales_sharing_opt_out": "Sales/sharing opt-out",
                  "anonymous_analysis": "Anonymous Analysis",
                  "pseudonymous_analysis": "Pseudonymous Analysis",
                  "device_linking": "Device Linking"
                }
              },
              "xdm:optOutValue": {
                "title": "Opt Out Value",
                "description": "The value of the specific opt out.",
                "$ref": "#/definitions/consentValue"
              },
              "xdm:basisOfProcessing": {
                "$ref": "#/definitions/basisOfProcessing"
              },
              "xdm:timestamp": {
                "$ref": "#/definitions/timestamp"
              }
            }
          }
        },
        "xdm:personalizationPreferences": {
          "title": "Personalization Preferences",
          "description": "User's Personalization Preferences",
          "type": "object",
          "properties": {
            "xdm:default": {
              "title": "Global Personalization Preferences",
              "description": "User's Default Personalization Preference",
              "type": "object",
              "properties": {
                "xdm:choice": {
                  "title": "Default Personalization Preferences Value",
                  "description": "The default value for personalization",
                  "$ref": "#/definitions/consentValue"
                },
                "xdm:basisOfProcessing": {
                  "$ref": "#/definitions/basisOfProcessing"
                },
                "xdm:timestamp": {
                  "$ref": "#/definitions/timestamp"
                }
              }
            },
            "xdm:details": {
              "title": "Itemized Personalization Preferences",
              "description": "Preferences for specific types of personalization",
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "meta:enum": {
                    "content": "Personalize Content",
                    "in_app": "Personalize In App Messages",
                    "offers": "Personalize Offers",
                    "email": "Personalize eMail",
                    "snail_mail": "Personalize Regular Mail",
                    "phone_calls": "Personalize Phone Calls",
                    "push_notifications": "Personalize Push Notifications",
                    "sms": "Personalize SMS",
                    "customer_support": "Personalize Customer Support",
                    "in_store": "Personalize In Store",
                    "in_vehicle": "Personalize In Vehicle",
                    "in_home": "Personalize In Home",
                    "iot": "Personalize IoT",
                    "social_media": "Personalize Social Media",
                    "third_party_offers": "Personalize Third-party Offers",
                    "third_party_content": "Personalize Third-party Content",
                    "ads": "Personalize My Business's Ads on Other Sites"
                  }
                },
                "xdm:choice": {
                  "title": "Personalization Preference Value",
                  "description": "The value for this specific personalization preference",
                  "$ref": "#/definitions/consentValue"
                },
                "xdm:basisOfProcessing": {
                  "$ref": "#/definitions/basisOfProcessing"
                },
                "xdm:timestamp": {
                  "$ref": "#/definitions/timestamp"
                }
              }
            }
          }
        }
      },
      "xdm:marketingPreferences": {
        "title": "Marketing Preferences",
        "description": "User's Direct Marketing Preferences",
        "type": "object",
        "properties": {
          "xdm:default": {
            "title": "Default Direct Marketing Preference",
            "description": "User's Default Marketing Preference",
            "type": "object",
            "properties": {
              "xdm:choice": {
                "title": "Default Marketing Preferences Value",
                "description": "The default value for direct marketing preferences",
                "$ref": "#/definitions/consentValue"
              },
              "xdm:basisOfProcessing": {
                "$ref": "#/definitions/basisOfProcessing"
              },
              "xdm:timestamp": {
                "$ref": "#/definitions/timestamp"
              }
            }
          },
          "xdm:details": {
            "title": "Itemized Direct Marketing Preferences",
            "description": "Preferences for specific types of direct marketing",
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "xdm:type": {
                  "title": "Marketing Preference",
                  "type": "string",
                  "description": "The specific marketing preference.",
                  "enum": [
                    "email",
                    "push_notifications",
                    "in_app_messages",
                    "sms",
                    "phone_calls",
                    "snail_mail",
                    "in_vehicle_messages",
                    "in_home_messages",
                    "iot",
                    "social_media"
                  ],
                  "meta:enum": {
                    "email": "Receive eMail",
                    "push_notifications": "Receive Push Notifications",
                    "in_app_messages": "Receive In App Messages",
                    "sms": "Receive SMS",
                    "phone_calls": "Receive Phone Calls",
                    "snail_mail": "Receive Regular Mail",
                    "in_vehicle_messages": "Receive In Vehicle Messages",
                    "in_home_messages": "Receive In Home Messages",
                    "iot": "Receive IoT",
                    "social_media": "Receive Social Media Messages"
                  }
                },
                "xdm:choice": {
                  "title": "Marketing Preference Value",
                  "description": "The value for this specific marketing preference",
                  "$ref": "#/definitions/consentValue"
                },
                "xdm:basisOfProcessing": {
                  "$ref": "#/definitions/basisOfProcessing"
                },
                "xdm:timestamp": {
                  "$ref": "#/definitions/timestamp"
                },
                "xdm:subscriptions": {
                  "title": "Company-specific subscriptions",
                  "description": "Company-specific subscriptions, such as mailing lists or newsletters",
                  "type": "object",
                  "meta:xdmType": "map",
                  "additionalProperties": {
                    "xdm:choice": {
                      "title": "Marketing Preference Value",
                      "description": "The value for this specific marketing preference",
                      "$ref": "#/definitions/consentValue"
                    },
                    "xdm:timestamp": {
                      "$ref": "#/definitions/timestamp"
                    }
                  }
                }
              }
            }
          }
        }
      },
      "xdm:timestamp": {
        "title": "Consent Preferences timestamp",
        "description": "Timestamp of the complete set of user consent preferences.",
        "type": "string",
        "format": "date-time"
      },
      "xdm:version": {
        "title": "Preferences Version",
        "description": "Version of the Privacy Preferences Standard",
        "type": "string"
      },
      "xdm:userLocale": {
        "title": "User Locale",
        "description": "User location or jurisdiction that applies to the consents for this user",
        "type": "string"
      },
      "xdm:localeSource": {
        "title": "Locale Source",
        "description": "Method used to determine the user's locale",
        "enum": [
          "ip",
          "gps",
          "user_provided",
          "website_location",
          "inferred",
          "other"
        ],
        "meta:enum": {
          "ip": "IP Address",
          "gps": "Device GPS",
          "user_provided": "User Provided",
          "website_location": "Website eTDL",
          "inferred": "Inferred",
          "other": "Other"
        }
      }
    }
  },
  "allOf": [
    {
      "$ref": "https://ns.adobe.com/xdm/common/extensible#/definitions/@context"
    },
    {
      "$ref": "#/definitions/consent-preferences"
    }
  ],
  "meta:status": "experimental"
}
```

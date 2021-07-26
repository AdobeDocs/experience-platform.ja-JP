---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；通信；購読；通信；通信；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 通信購読スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「通信購読スキーマ」フィールドグループの概要を説明します。
source-git-commit: 19675e4042c28061a4b2ed4e68374d5e09216ba1
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 8%

---

# [!UICONTROL Telecom Subscriptionschemaフ] ィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Telecom Subscriptions] は、クラスの標準スキーマフィールドグループで、価格、パッケー [[!DNL XDM Individual Profile] ](../../classes/individual-profile.md) ジ、個々の製品サブスクリプションなど、顧客の通信サブスクリプションプランを記述します。

フィールドグループには、以下で説明するプロパティを持つ1つのオブジェクトタイプフィールド`telecomSubscription`が用意されています。

![通信購読構造](../../images/field-groups/telecom-subscription/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `internetSubscription` | オブジェクトの配列 | データ上限、接続タイプ、速度の詳細など、インターネット購読プランの詳細を説明します。 詳しくは、](#internetSubscription)の下の[の節を参照してください。 |
| `landlineSubscription` | オブジェクトの配列 | 選択した機能、分、ダイヤルプランなど、固定電話のサブスクリプションプランの詳細を説明します。 詳しくは、](#landlineSubscription)の下の[の節を参照してください。 |
| `mediaSubscription` | オブジェクトの配列 | チャネル数や含まれるストリーミングサービスなど、メディア購読プランの詳細について説明します。 詳しくは、](#mediaSubscription)の下の[の節を参照してください。 |
| `mobileSubscription` | オブジェクトの配列 | ライン数、データレート、コストなど、モバイル購読プランの詳細について説明します。 詳しくは、](#mobileSubscription)の下の[の節を参照してください。 |
| `primarySubscriber` | [[!UICONTROL ユーザー]](../../data-types/person.md) | 購読の所有者を示します。 |
| `bundleName` | 文字列 | `Internet + Media`など、顧客が登録されている購読バンドルの任意のタイプの名前を取り込みます。 |
| `primaryPartyID` | 文字列 | 購読を担当する主要人物の識別子。通常は、デバイスの電話番号を指定します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)

## `internetSubscription` {#internetSubscription}

`internetSubscription` はオブジェクトの配列を提供します。各オブジェクトの構造を以下に示します。

![internetSubscription](../../images/field-groups/telecom-subscription/internetSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `subscriptionDetails` | [[!UICONTROL 通信購読]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細について説明します。 サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細について説明します。 |
| `connectionType` | 文字列 | 購読の接続タイプ。 |
| `dataCap` | 整数 | アカウントのデータ上限(MB)。 |
| `downloadSpeed` | 整数 | サブスクリプションで使用可能な最大ダウンロード速度(MB)。 |
| `selfSetup` | Boolean | 顧客が技術者の訪問なしでインターネットセットアップの資格を持っているかどうかを示します。 |
| `uploadSpeed` | 整数 | サブスクリプションで使用できる最大アップロード速度(MB)。 |

{style=&quot;table-layout:auto&quot;}

## `landlineSubscription` {#landlineSubscription}

`landlineSubscription` はオブジェクトの配列を提供します。各オブジェクトの構造を以下に示します。

![landlineSubscription](../../images/field-groups/telecom-subscription/landlineSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `phoneNumber` | [[!UICONTROL 電話番号]](../../data-types/telecom-subscription.md) | このサブスクリプションに割り当てられた電話番号。 |
| `subscriptionDetails` | [[!UICONTROL 通信購読]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細について説明します。 |
| `callBlocking` | Boolean | 固定回線のサブスクリプション機能に呼び出しブロックが含まれるかどうかを示します。 |
| `callForwarding` | Boolean | 固定回線のサブスクリプション機能に通話転送が含まれているかどうかを示します。 |
| `callWaiting` | Boolean | 固定回線のサブスクリプション機能に通話待ちが含まれているかどうかを示します。 |
| `callerID` | Boolean | 固定回線のサブスクリプション機能に呼び出し元IDが含まれているかどうかを示します。 |
| `internationalCalling` | Boolean | 固定電話のサブスクリプション機能に国際通話が含まれるかどうかを示します。 |
| `minutes` | 整数 | サブスクリプション内で使用可能な月単位の分数。 |
| `threeWayCalling` | Boolean | 固定線のサブスクリプション機能に3方向呼び出しが含まれるかどうかを示します。 |
| `unlimitedDomesticLongDistance` | Boolean | 固定電話の定期購読機能に、無制限の国内長距離通話が含まれているかどうかを示します。 |
| `unlimitedLocalCalling` | Boolean | 固定回線のサブスクリプション機能に無制限のローカル呼び出しが含まれるかどうかを示します。 |
| `voicemail` | Boolean | 固定電話のサブスクリプション機能にボイスメールが含まれているかどうかを示します。 |

{style=&quot;table-layout:auto&quot;}

## `mediaSubscription` {#mediaSubscription}

`mediaSubscription` はオブジェクトの配列を提供します。各オブジェクトの構造を以下に示します。

![mediaSubscription](../../images/field-groups/telecom-subscription/mediaSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `streamingServices` | オブジェクトの配列 | サブスクリプションに含まれるすべてのストリーミングサービスのリスト。 各配列項目には、次のプロパティが含まれます。 <ul><li>`promotionLength`:ストリーミングサービスがプロモーションの一部として追加された場合のプロモーションの長さ（月単位）。</li><li>`promotionalAddition`:ストリーミングサービスがプロモーションの一部として追加されたかどうかを示します。</li><li>`serviceName`:ストリーミングサービスの名前。</li></ul> |
| `subscriptionDetails` | [[!UICONTROL 通信購読]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細について説明します。 |
| `channels` | 整数 | メディアサブスクリプションに含まれるチャネルの数。 |

{style=&quot;table-layout:auto&quot;}

## `mobileSubscription` {#mobileSubscription}

`mobileSubscription` はオブジェクトの配列を提供します。各オブジェクトの構造を以下に示します。

![mobileSubscription](../../images/field-groups/telecom-subscription/mobileSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `phoneNumber` | [[!UICONTROL 電話番号]](../../data-types/telecom-subscription.md) | このサブスクリプションに割り当てられた電話番号。 |
| `subscriptionDetails` | [[!UICONTROL 通信購読]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細について説明します。 |
| `earlyUpgradeEnrollment` | Boolean | お客様が早期のアップグレードをオプトインするかどうかを示します。 |
| `planLevel` | 文字列 | このサブスクリプションに割り当てられたモバイルプランの名前。 |
| `portedNumber` | Boolean | 顧客が別の通信事業者から番号をポートするかどうかを示します。 |

{style=&quot;table-layout:auto&quot;}


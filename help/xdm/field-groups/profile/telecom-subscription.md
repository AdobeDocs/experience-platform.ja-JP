---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；通信；購読；通信；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 通信購読スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「通信購読スキーマ」フィールドグループの概要を説明します。
exl-id: 00c20081-09d0-425c-9894-0f957558bd43
source-git-commit: 64e76c456ac5f59a2a1996e58eda405f1b27efa8
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 11%

---

# [!UICONTROL 通信配信登録] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 通信配信登録] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md) これは、価格、パッケージ、個々の製品サブスクリプションを含む、顧客の通信サブスクリプションプランを表します。

フィールドグループには、単一のオブジェクトタイプのフィールドが用意されています。 `telecomSubscription`以下で説明するプロパティを持つ。

![通信購読構造](../../images/field-groups/telecom-subscription/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `internetSubscription` | オブジェクトの配列 | データの上限、接続タイプ、速度の詳細など、インターネットサブスクリプションプランの詳細を表します。 詳しくは、 [以下の節](#internetSubscription) を参照してください。 |
| `landlineSubscription` | オブジェクトの配列 | 固定電話の契約プランの詳細（選択した機能、分、ダイヤルプランを含む）を表します。 詳しくは、 [以下の節](#landlineSubscription) を参照してください。 |
| `mediaSubscription` | オブジェクトの配列 | チャネル数やストリーミングサービス数など、メディアサブスクリプションプランの詳細について説明します。 詳しくは、 [以下の節](#mediaSubscription) を参照してください。 |
| `mobileSubscription` | オブジェクトの配列 | モバイル購読プランの詳細（行数、データレート、コストなど）を表します。 詳しくは、 [以下の節](#mobileSubscription) を参照してください。 |
| `primarySubscriber` | [[!UICONTROL 人物]](../../data-types/person.md) | 購読の所有者を表します。 |
| `bundleName` | 文字列 | 顧客が登録されている任意のタイプのサブスクリプションバンドルの名前をキャプチャします（例： ）。 `Internet + Media`. |
| `primaryPartyID` | 文字列 | サブスクリプションを担当する主要人物の識別子。通常は、デバイスの電話番号になります。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)

## `internetSubscription` {#internetSubscription}

`internetSubscription` はオブジェクトの配列として提供されます。 各オブジェクトの構造を以下に示します。

![internetSubscription](../../images/field-groups/telecom-subscription/internetSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `subscriptionDetails` | [[!UICONTROL 通信サブスクリプション]](../../data-types/telecom-subscription.md) | サブスクリプションに関する一般的な詳細（サブスクリプションの長さ、料金、ステータスなど）を説明します。 サブスクリプションに関する一般的な詳細（サブスクリプションの長さ、料金、ステータスなど）を説明します。 |
| `connectionType` | 文字列 | サブスクリプションの接続タイプ。 |
| `dataCap` | 整数 | アカウントのデータ上限 (MB)。 |
| `downloadSpeed` | 整数 | サブスクリプションで使用可能な最大ダウンロード速度 ( メガバイト (MB) 単位 )。 |
| `selfSetup` | ブール値 | 顧客が技術者の訪問なしでインターネット設定に適格かどうかを示します。 |
| `uploadSpeed` | 整数 | サブスクリプションで使用できる最大アップロード速度 ( メガバイト (MB) 単位 )。 |

{style=&quot;table-layout:auto&quot;}

## `landlineSubscription` {#landlineSubscription}

`landlineSubscription` はオブジェクトの配列として提供されます。 各オブジェクトの構造を以下に示します。

![landlineSubscription](../../images/field-groups/telecom-subscription/landlineSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `phoneNumber` | [[!UICONTROL 電話番号]](../../data-types/telecom-subscription.md) | この配信登録に割り当てられた電話番号。 |
| `subscriptionDetails` | [[!UICONTROL 通信サブスクリプション]](../../data-types/telecom-subscription.md) | サブスクリプションに関する一般的な詳細（サブスクリプションの長さ、料金、ステータスなど）を説明します。 |
| `callBlocking` | ブール値 | 固定電話の契約に通話ブロック機能が含まれているかどうかを示します。 |
| `callForwarding` | ブール値 | 固定電話の契約に着信転送機能が含まれているかどうかを示します。 |
| `callWaiting` | ブール値 | 固定電話の契約に通話待ち機能が含まれているかどうかを示します。 |
| `callerID` | ブール値 | 固定電話の契約に発信者番号が含まれているかどうかを示します。 |
| `internationalCalling` | ブール値 | 固定電話の契約機能に国際通話機能が含まれているかどうかを示します。 |
| `minutes` | 整数 | サブスクリプション内で使用可能な月単位の分数。 |
| `threeWayCalling` | ブール値 | 固定電話の契約に 3 方向通話機能が含まれているかどうかを示します。 |
| `unlimitedDomesticLongDistance` | ブール値 | 固定電話の契約機能に、国内の長距離通話が無制限に含まれているかどうかを示します。 |
| `unlimitedLocalCalling` | ブール値 | 固定電話の契約に、市内通話無制限機能が含まれているかどうかを示します。 |
| `voicemail` | ブール値 | 固定電話の契約にボイスメール機能が含まれているかどうかを示します。 |

{style=&quot;table-layout:auto&quot;}

## `mediaSubscription` {#mediaSubscription}

`mediaSubscription` はオブジェクトの配列として提供されます。 各オブジェクトの構造を以下に示します。

![mediaSubscription](../../images/field-groups/telecom-subscription/mediaSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `streamingServices` | オブジェクトの配列 | サブスクリプションに含まれるすべてのストリーミングサービスのリスト。 各配列項目には、次のプロパティが含まれます。 <ul><li>`promotionLength`:ストリーミングサービスがプロモーションの一部として追加された場合のプロモーションの長さ（月単位）。</li><li>`promotionalAddition`:ストリーミングサービスがプロモーションの一環として追加されたかどうかを示します。</li><li>`serviceName`:ストリーミングサービスの名前。</li></ul> |
| `subscriptionDetails` | [[!UICONTROL 通信サブスクリプション]](../../data-types/telecom-subscription.md) | サブスクリプションに関する一般的な詳細（サブスクリプションの長さ、料金、ステータスなど）を説明します。 |
| `channels` | 整数 | メディアサブスクリプションに含まれるチャネルの数。 |

{style=&quot;table-layout:auto&quot;}

## `mobileSubscription` {#mobileSubscription}

`mobileSubscription` はオブジェクトの配列として提供されます。 各オブジェクトの構造を以下に示します。

![mobileSubscription](../../images/field-groups/telecom-subscription/mobileSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `phoneNumber` | [[!UICONTROL 電話番号]](../../data-types/telecom-subscription.md) | この配信登録に割り当てられた電話番号。 |
| `subscriptionDetails` | [[!UICONTROL 通信サブスクリプション]](../../data-types/telecom-subscription.md) | サブスクリプションに関する一般的な詳細（サブスクリプションの長さ、料金、ステータスなど）を説明します。 |
| `earlyUpgradeEnrollment` | ブール値 | お客様が早期アップグレードをオプトインするかどうかを示します。 |
| `planLevel` | 文字列 | このサブスクリプションに割り当てられたモバイルプランの名前。 |
| `portedNumber` | ブール値 | お客様が番号を別の通信事業者から移植するかどうかを示します。 |

{style=&quot;table-layout:auto&quot;}

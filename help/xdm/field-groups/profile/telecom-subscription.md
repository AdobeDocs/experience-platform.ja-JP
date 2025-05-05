---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；個人プロファイル；フィールド；スキーマ；スキーマ；通信；購読；通信；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 通信購読スキーマフィールドグループ
description: 通信購読スキーマフィールドグループについて説明します。
exl-id: 00c20081-09d0-425c-9894-0f957558bd43
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '692'
ht-degree: 8%

---

# [!UICONTROL &#x200B; 通信配信登録 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Telecom Subscription] は、[[!DNL XDM Individual Profile]  クラスの標準スキーマフィールドグループで ](../../classes/individual-profile.md) 価格、パッケージ、個々の製品のサブスクリプションなど、顧客の通信サブスクリプションプランを記述します。

フィールドグループには、以下にプロパティを説明する単一のオブジェクトタイプフィールド `telecomSubscription` が用意されています。

![Telecom Subscription structure](../../images/field-groups/telecom-subscription/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `internetSubscription` | オブジェクトの配列 | インターネットサブスクリプションプランの詳細（データの上限、接続タイプ、速度の詳細など）を記述します。 詳しくは、[ 以下の節 ](#internetSubscription) を参照してください。 |
| `landlineSubscription` | オブジェクトの配列 | 固定電話の契約の詳細（選択した機能、分、ダイヤルプランを含む）を記述します。 詳しくは、[ 以下の節 ](#landlineSubscription) を参照してください。 |
| `mediaSubscription` | オブジェクトの配列 | メディアサブスクリプションプランの詳細（チャネル数やストリーミングサービスを含む）を記述します。 詳しくは、[ 以下の節 ](#mediaSubscription) を参照してください。 |
| `mobileSubscription` | オブジェクトの配列 | モバイル購読プランの詳細（回線数、データ料金、コストなど）を記述します。 詳しくは、[ 以下の節 ](#mobileSubscription) を参照してください。 |
| `primarySubscriber` | [[!UICONTROL &#x200B; 人物 &#x200B;]](../../data-types/person.md) | サブスクリプションの所有者を表します。 |
| `bundleName` | 文字列 | お客様が登録されている任意のタイプのサブスクリプションバンドルの名前をキャプチャします（`Internet + Media` など）。 |
| `primaryPartyID` | 文字列 | サブスクリプションに対して責任がある主要人物の識別子。通常は、デバイスの電話番号になります。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)

## `internetSubscription` {#internetSubscription}

`internetSubscription` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![internetSubscription](../../images/field-groups/telecom-subscription/internetSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `subscriptionDetails` | [[!UICONTROL &#x200B; 電気通信の登録 &#x200B;]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細を説明します。 サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細を説明します。 |
| `connectionType` | 文字列 | サブスクリプションの接続タイプ。 |
| `dataCap` | 整数 | アカウントのデータ上限（メガバイト （MB）単位）。 |
| `downloadSpeed` | 整数 | サブスクリプションで利用できる最大ダウンロード速度（メガバイト （MB）単位）。 |
| `selfSetup` | ブール値 | お客様が技術者の訪問なしでインターネット設定の対象かどうかを示します。 |
| `uploadSpeed` | 整数 | サブスクリプションで利用可能な最大アップロード速度（メガバイト （MB）単位）。 |

{style="table-layout:auto"}

## `landlineSubscription` {#landlineSubscription}

`landlineSubscription` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![landlineSubscription](../../images/field-groups/telecom-subscription/landlineSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `phoneNumber` | [[!UICONTROL &#x200B; 電話番号 &#x200B;]](../../data-types/telecom-subscription.md) | このサブスクリプションに割り当てられた電話番号。 |
| `subscriptionDetails` | [[!UICONTROL &#x200B; 電気通信の登録 &#x200B;]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細を説明します。 |
| `callBlocking` | ブール値 | 固定電話の契約に着信拒否機能が含まれているかどうかを示します。 |
| `callForwarding` | ブール値 | 固定電話の契約に着信転送機能が含まれているかどうかを示します。 |
| `callWaiting` | ブール値 | 固定電話の契約に割り込み通話機能が含まれるかどうかを示します。 |
| `callerID` | ブール値 | 固定電話の契約に発信者番号機能が含まれているかどうかを示します。 |
| `internationalCalling` | ブール値 | 固定電話の契約に国際通話機能が含まれているかどうかを示します。 |
| `minutes` | 整数 | サブスクリプション内で 1 月に利用できる分数。 |
| `threeWayCalling` | ブール値 | 固定電話の契約に 3 者通話機能が含まれているかどうかを示します。 |
| `unlimitedDomesticLongDistance` | ブール値 | 固定電話の契約に国内長距離通話無制限機能が含まれているかどうかを示します。 |
| `unlimitedLocalCalling` | ブール値 | 固定電話の契約に市内通話無制限機能が含まれているかどうかを示します。 |
| `voicemail` | ブール値 | 固定電話の契約にボイスメール機能が含まれているかどうかを示します。 |

{style="table-layout:auto"}

## `mediaSubscription` {#mediaSubscription}

`mediaSubscription` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![mediaSubscription](../../images/field-groups/telecom-subscription/mediaSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `streamingServices` | オブジェクトの配列 | サブスクリプションに含まれるすべてのストリーミングサービスのリスト。 各配列項目には、次のプロパティが含まれます。 <ul><li>`promotionLength`：ストリーミングサービスがプロモーションの一環として追加された場合の、プロモーションの長さ（月単位）。</li><li>`promotionalAddition`：ストリーミングサービスがプロモーションの一環として追加されたかどうかを示します。</li><li>`serviceName`: ストリーミングサービスの名前。</li></ul> |
| `subscriptionDetails` | [[!UICONTROL &#x200B; 電気通信の登録 &#x200B;]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細を説明します。 |
| `channels` | 整数 | メディアサブスクリプションに含まれるチャネル数。 |

{style="table-layout:auto"}

## `mobileSubscription` {#mobileSubscription}

`mobileSubscription` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![mobileSubscription](../../images/field-groups/telecom-subscription/mobileSubscription.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `phoneNumber` | [[!UICONTROL &#x200B; 電話番号 &#x200B;]](../../data-types/telecom-subscription.md) | このサブスクリプションに割り当てられた電話番号。 |
| `subscriptionDetails` | [[!UICONTROL &#x200B; 電気通信の登録 &#x200B;]](../../data-types/telecom-subscription.md) | サブスクリプションの長さ、料金、ステータスなど、サブスクリプションに関する一般的な詳細を説明します。 |
| `earlyUpgradeEnrollment` | ブール値 | 顧客が早期アップグレードをオプトインするかどうかを示します。 |
| `planLevel` | 文字列 | このサブスクリプションに割り当てられたモバイルプランの名前。 |
| `portedNumber` | ブール値 | 顧客が番号を別の通信事業者から移行するかどうかを示します。 |

{style="table-layout:auto"}

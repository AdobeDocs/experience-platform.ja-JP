---
title: 設定例
description: グラフシミュレーションツールを使用した設定例について説明します。
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: ba72abd9febc6d9e6491748519199b54a26ae1c5
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 21%

---

# グラフ設定の例

>[!NOTE]
>
>「CRM ID」はカスタム名前空間です。 したがって、以下の例では、「CRM ID」の表示名と ID 記号を持つカスタム名前空間を作成する必要があります。

次のセクションでは、グラフシミュレーションで発生する可能性のあるグラフシナリオの例を示します。

## CRM ID のみ

イベント：

* CRM ID:Tom、ECID:111

アルゴリズム設定：

| 優先度 | 表示名 | ID シンボル | ID タイプ | グラフごとに一意 |
| ---| --- | --- | --- | --- |
| 1 | CRM ID | CRM ID | CROSS_DEVICE | ○ |
| 2 | ECID | ECID | COOKIE | 不可 |

+++選択してシミュレーショングラフを表示

+++

## ハッシュ化されたメールを含む CRM ID

このシナリオでは、CRM ID が取り込まれ、オンライン（エクスペリエンスイベント）とオフライン（プロファイルレコード）の両方のデータを表します。 このシナリオでは、CRM レコードデータセット内で CRM ID と共に送信された別の名前空間を表す、ハッシュ化されたメールの取り込みも含まれます。

イベント：

* CRM ID:Tom、Email_LC_SHA256:tom<span>@acme.com
* CRM ID:Tom、ECID:111
* CRM ID：夏、Email_LC_SHA256：夏<span>@acme.com
* CRM ID：夏物、ECID:222

アルゴリズム設定：

| 優先度 | 表示名 | ID シンボル | ID タイプ | グラフごとに一意 |
| ---| --- | --- | --- | --- |
| 1 | CRM ID | CRM ID | CROSS_DEVICE | ○ |
| 2 | メール（SHA256、小文字） | Email_LC_SHA256 | メール | 不可 |
| 3 | ECID | ECID | COOKIE | 不可 |

+++選択してシミュレーショングラフを表示

+++

## ハッシュ化されたメール、ハッシュ化された電話、GAID および IDFA を含んだ CRM ID

イベント：

* CRM ID:Tom、Email_LC_SHA256:aabbcc、Phone_SHA256:123-4567
* CRM ID:Tom、ECID:111
* CRM ID:Tom、ECID:222、IDFA:A-A-A
* CRM ID：夏時間、Email_LC_SHA256:ddeeff、Phone_SHA256:765-4321
* CRM ID：夏物、ECID:333
* CRM ID：夏物、ECID:444、GAID:B-B-B

アルゴリズム設定：

| 優先度 | 表示名 | ID シンボル | ID タイプ | グラフごとに一意 |
| ---| --- | --- | --- | --- |
| 1 | CRM ID | CRM ID | CROSS_DEVICE | ○ |
| 2 | メール（SHA256、小文字） | Email_LC_SHA256 | メール | 不可 |
| 3 | 電話（SHA256） | Phone_SHA256 | Phone | 不可 |
| 4 | Google広告 ID （GAID） | GAID | デバイス | 不可 |
| 5 | Apple IDFA （Appleの ID） | IDFA | デバイス | 不可 |
| 6 | ECID | ECID | COOKIE | 不可 |

+++選択してシミュレーショングラフを表示

+++
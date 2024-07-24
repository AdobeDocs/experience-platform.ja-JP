---
title: 設定例
description: グラフシミュレーションツールを使用した設定例について説明します。
badge: ベータ版
source-git-commit: 536770d0c3e7e93921fe40887dafa5c76e851f5e
workflow-type: tm+mt
source-wordcount: '814'
ht-degree: 12%

---

# グラフ設定の例

>[!AVAILABILITY]
>
>ID グラフリンクルールは現在ベータ版です。 パーティシペーションの条件については、Adobeアカウントチームにお問い合わせください。 機能とドキュメントは変更される場合があります。

>[!NOTE]
>
>* 「CRMID」と「loginID」は、カスタム名前空間です。 このドキュメントでは、「CRMID」はユーザー識別子で、「loginID」は特定のユーザーに関連付けられたログイン識別子です。
>* このドキュメントで概要を説明するグラフシナリオの例をシミュレートするには、まず 2 つのカスタム名前空間を作成する必要があります。1 つは ID シンボル「CRMID」、もう 1 つは ID シンボル「loginID」です。 ID 記号では大文字と小文字が区別されます。

このドキュメントでは、ID データを操作する際に発生する可能性のある一般的なシナリオのグラフ設定の例について説明します。

## CRMID のみ

これは、オンラインイベント（CRMID および ECID）が取り込まれ、オフラインイベント（プロファイルレコード）が CRMID に対してのみ保存される、シンプルな実装シナリオの例です。

**実装：**

| 使用されている名前空間 | Web 行動収集方法 |
| --- | --- |
| CRMID、ECID | Web SDK |

**イベント：**

次のイベントをテキストモードにコピーすると、グラフシミュレーションでこのシナリオを作成できます。

* CRMID:Tom、ECID:111

**アルゴリズム設定：**

アルゴリズム設定に次の設定を行うことで、グラフシミュレーションでこのシナリオを作成できます。

| 優先度 | 表示名 | ID タイプ | グラフごとに一意 |
| ---| --- | --- | --- |
| 1 | CRMID | CROSS_DEVICE | ○ |
| 2 | ECID | COOKIE | × |

**リアルタイムプライマリプロファイル用の顧客 ID の選択：**

この設定のコンテキスト内で、プライマリ ID は次のように定義されます。

| 認証ステータス | イベントの名前空間 | プライマリ ID |
| --- | --- | --- |
| Authenticated | CRMID、ECID | CRMID |
| 未認証 | ECID | ECID |

>[!BEGINTABS]

>[!TAB  理想的な単一人物グラフシナリオ ]

以下は、CRMID が一意で最も優先度が高い、理想的な単一人物グラフの例です。

![CRMID が一意で優先順位が最も高い、理想的な一人称グラフのシミュレート例。](../images/graph-examples/crmid_only_single.png)

>[!TAB  複数人グラフシナリオ ]

次に、複数人グラフの例を示します。 この例では、「共有デバイス」シナリオが表示されます。このシナリオでは、2 つの CRMID があり、古い確立済みリンクを持つ CRMID が削除されます。

![ 複数人グラフのシミュレート例。 この例では、2 つの CRMID があり、古い確立されたリンクが削除される共有デバイスのシナリオを示しています。](../images/graph-examples/crmid_only_multi.png)

>[!ENDTABS]

## ハッシュ化されたメールを使用した CRMID

このシナリオでは、CRMID が取り込まれ、オンライン（エクスペリエンスイベント）とオフライン（プロファイルレコード）の両方のデータを表します。 このシナリオでは、ハッシュ化されたメールの取り込みも必要となります。これは、CRM レコードデータセット内で CRMID と共に送信される別の名前空間を表しています。

**実装：**

| 使用されている名前空間 | Web 行動収集方法 |
| --- | --- |
| CRMID、Email_LC_SHA256、ECID | Web SDK |

**イベント：**

次のイベントをテキストモードにコピーすると、グラフシミュレーションでこのシナリオを作成できます。

* CRMID: Tom、Email_LC_SHA256: tom<span>@acme.com
* CRMID:Tom、ECID:111
* CRMID：夏時間、Email_LC_SHA256:summer<span>@acme.com
* CRMID：夏、ECID:222

**アルゴリズム設定：**

アルゴリズム設定に次の設定を行うことで、グラフシミュレーションでこのシナリオを作成できます。

| 優先度 | 表示名 | ID タイプ | グラフごとに一意 |
| ---| --- | --- | --- |
| 1 | CRMID | CROSS_DEVICE | ○ |
| 2 | メール（SHA256、小文字） | メール | × |
| 3 | ECID | COOKIE | × |

**プロファイル** のプライマリ ID の選択

この設定のコンテキスト内で、プライマリ ID は次のように定義されます。

| 認証ステータス | イベントの名前空間 | プライマリ ID |
| --- | --- | --- |
| Authenticated | CRMID、ECID | CRMID |
| 未認証 | ECID | ECID |

>[!BEGINTABS]

>[!TAB  理想的な単一人物グラフシナリオ ]

![ この例では、2 つの別個のグラフが生成され、それぞれが 1 人の人物エンティティを表します。](../images/graph-examples/crmid_hashed_single.png)

>[!TAB  複数人グラフ：共有デバイス ]

![ この例では、Tom と Summer の両方が同じ ECID に関連付けられているので、シミュレーショングラフに「共有デバイス」のシナリオが表示されます。](../images/graph-examples/crmid_hashed_shared_device.png)

>[!TAB  複数人グラフ：一意でないメール ]

![ このシナリオは、「共有デバイス」シナリオに似ています。 ただし、人物エンティティで ECID を共有する代わりに、同じメールアカウントに関連付けられます。](../images/graph-examples/crmid_hashed_nonunique_email.png)

>[!ENDTABS]

## ハッシュ化されたメール、ハッシュ化された電話、GAID および IDFA を使用した CRMID

このシナリオは前のシナリオと似ています。 ただし、このシナリオでは、ハッシュ化されたメールと電話は、segment match で使用する ID としてマークされます。

**実装：**

| 使用されている名前空間 | Web 行動収集方法 |
| --- | --- |
| CRMID, Email_LC_SHA256, Phone_SHA256, GAID, IDFA, ECID | Web SDK |

**イベント：**

次のイベントをテキストモードにコピーすると、グラフシミュレーションでこのシナリオを作成できます。

* CRMID:Tom、Email_LC_SHA256:aabbcc、Phone_SHA256:123-4567
* CRMID:Tom、ECID:111
* CRMID:Tom、ECID:222、IDFA:A-A-A
* CRMID：夏、Email_LC_SHA256:ddeeff、Phone_SHA256:765-4321
* CRMID：夏、ECID:333
* CRMID：夏、ECID:444、GAID:B-B-B

**アルゴリズム設定：**

アルゴリズム設定に次の設定を行うことで、グラフシミュレーションでこのシナリオを作成できます。

| 優先度 | 表示名 | ID タイプ | グラフごとに一意 |
| ---| --- | --- | --- |
| 1 | CRMID | CROSS_DEVICE | ○ |
| 2 | メール（SHA256、小文字） | メール | × |
| 3 | 電話（SHA256） | Phone | × |
| 4 | Google広告 ID （GAID） | デバイス | × |
| 5 | Apple IDFA （Appleの ID） | デバイス | × |
| 6 | ECID | COOKIE | × |

**プロファイル** のプライマリ ID の選択

この設定のコンテキスト内で、プライマリ ID は次のように定義されます。

| 認証ステータス | イベントの名前空間 | プライマリ ID |
| --- | --- | --- |
| Authenticated | CRMID、IDFA、ECID | CRMID |
| Authenticated | CRMID、GAID、ECID | CRMID |
| Authenticated | CRMID、ECID | CRMID |
| 未認証 | GAID、ECID | GAID |
| 未認証 | IDFA、ECID | IDFA |
| 未認証 | ECID | ECID |

>[!BEGINTABS]

>[!TAB  理想的な単一人物グラフシナリオ ]

![](../images/graph-examples/crmid_hashed_single_seg_match.png)

>[!ENDTABS]

<!-- 
## Single CRMID with multiple login IDs (simple)

In this scenario, there is a single CRMID that represents a person entity. However, a person entity may have multiple login identifiers:

* A given person entity can have different account account types (personal vs. business, account by state, account by brand, etc.)
* A given person entity may use different email addresses for any number of accounts.

Therefore, **it is crucial that the CRMID is always sent for every user**. Failure to do so may result in a "dangling" login ID scenario, where a single person entity is assumed to be sharing a device with another person.

**Implementation:**

| Namespaces used | Web behavior collection method |
| --- | --- |
| CRMID, loginID, ECID | Web SDK |

**Events:**

You can create this scenario in graph simulation by copying the following events to text mode:

* CRMID: John, loginID: ID_A
* CRMID: John, loginID: ID_B
* loginID: ID_A, ECID: 111
* CRMID: Jane, loginID: ID_C
* CRMID: Jane, loginID: ID_D
* loginID: ID_C, ECID: 222

**Algorithm configuration:**

You can create this scenario in graph simulation by configuring the following setup for your algorithm configuration:

| Priority | Display name | Identity symbol | Identity type | Unique per graph |
| ---| --- | --- | --- | --- |
| 1 | CRMID | CRMID | CROSS_DEVICE | Yes |
| 2 | loginID | loginID | CROSS_DEVICE | No |
| 3 | ECID | ECID | COOKIE | No |

## Single CRMID with multiple login IDs (complex)

In this scenario, there is a single CRMID that represents a person entity. However, a person entity may have multiple login identifiers:

* A given person entity can have different account account types (personal vs. business, account by state, account by brand, etc.)
* A given person entity may use different email addresses for any number of accounts.

The case of "dangling" loginID also applies for this scenario.

**Implementation:**

| Namespaces used | Web behavior collection method |
| --- | --- |
| CRMID, Email_LC_SHA256, Phone_SHA256, loginID, ECID, AAID | Adobe Analytics source connector |


**Events:**

You can create this scenario in graph simulation by copying the following events to text mode:

* CRMID: John, Email_LC_SHA256: aabbcc, Phone_SHA256: 123-4567
* CRMID: John, loginID: ID_A
* CRMID: John, loginID: ID_B
* loginID:ID_A, ECID: 111, AAID: AAA
* CRMID: Jane, Email_LC_SHA256: ddeeff, Phone_SHA256: 765-4321
* CRMID: Jane, loginID: ID_C
* CRMID: Jane, loginID: ID_D
* loginID: ID_C, ECID: 222, AAID: BBB

**Algorithm configuration:**

You can create this scenario in graph simulation by configuring the following setup for your algorithm configuration:

| Priority | Display name | Identity symbol | Identity type | Unique per graph |
| ---| --- | --- | --- | --- |
| 1 | CRMID | CRMID | CROSS_DEVICE | Yes |
| 2 | Email_LC_SHA256 | Email_LC_SHA256 | EMAIL | No |
| 3 | Phone_SHA256 | Phone_SHA256 | PHONE | No |
| 4 | loginID | loginID | CROSS_DEVICE | No |
| 5 | ECID | ECID | COOKIE | No |
| 6 | AAID | AAID | COOKIE | No |
 -->

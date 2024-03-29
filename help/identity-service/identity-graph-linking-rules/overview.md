---
title: ID グラフリンクルールの概要
description: ID サービスの ID グラフリンクルールについて説明します。
hide: true
hidefromtoc: true
badge: アルファ版
exl-id: 317df52a-d3ae-4c21-bcac-802dceed4e53
source-git-commit: f21b5519440f7ffd272361954c9e32ccca2ec2bc
workflow-type: tm+mt
source-wordcount: '1022'
ht-degree: 1%

---

# ID グラフリンクルールの概要

>[!IMPORTANT]
>
>ID グラフのリンクルールは、現在Alpha中です。 機能とドキュメントは変更される場合があります。

## 目次 

* [概要](./overview.md)
* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [シナリオの例](./example-scenarios.md)

Adobe Experience Platform ID サービスとリアルタイム顧客プロファイルを使用すると、データが完全に取り込まれ、すべての結合プロファイルが、CRM ID などの個人 ID を通じて 1 人の個人を表していると簡単に想定できます。 ただし、特定のデータが複数の異なるプロファイルを 1 つのプロファイルに結合しようとするシナリオが考えられます（「プロファイルの折りたたみ」）。 これらの不要な結合を防ぐには、ID グラフのリンクルールを通じて提供される設定を使用し、ユーザーに対する正確なパーソナライゼーションを可能にします。

## プロファイルの折りたたみが発生する可能性のあるシナリオの例

* **共有デバイス**：共有デバイスは、複数の個人が使用するデバイスを指します。 共有デバイスの例としては、タブレット、ライブラリコンピューター、キオスクなどがあります。
* **無効なメールと電話番号**：無効な電子メールおよび電話番号は、エンドユーザーが「test」などの無効な連絡先情報を登録していることを意味します<span>電子メールの場合は@test.com、電話番号の場合は&quot;+1-111-111-1111&quot;です。
* **ID 値に誤りがあるか無効です**：誤った ID 値または正しくない ID 値は、CRM ID を結合する可能性のある一意でない ID 値を参照します。 例えば、IDFA の文字には 36 文字（32 文字と 4 つのハイフン）が必要ですが、ID 値が「user_null」の IDFA を取り込むことができるシナリオがあります。 同様に、電話番号は数字のみをサポートしますが、ID 値が「未指定」の電話名前空間が取り込まれる場合があります。

ID グラフのリンクルールの使用例のシナリオの詳細については、 [シナリオの例](./example-scenarios.md).

## ID グラフリンクルールの目的

ID グラフのリンクルールを使用すると、次のことができます。

* 一意の名前空間（制限）を設定して、各ユーザーに対して単一の ID グラフ/結合プロファイルを作成します。これにより、異なる 2 つのユーザー ID が 1 つの ID グラフに結合されるのを防ぎます。
* 優先度を設定して、オンラインで認証済みのイベントを個人に関連付ける

### 制限

一意の名前空間は、CRM ID、ログイン ID、ハッシュ化された電子メールなど、個人を表す識別子です。 名前空間が一意として指定されている場合、グラフはその名前空間 (`limit=1`) をクリックします。 これにより、同じグラフ内で 2 つの異なる人物識別子が結合されなくなります。

* 制限を設定しない場合、グラフ内の CRM ID 名前空間を持つ 2 つの ID など、望ましくないグラフ結合が発生する可能性があります。
* 制限を設定しない場合、グラフがガードレール内にある限り（50 個の ID/グラフ）、グラフは必要な数の名前空間を追加できます。
* 制限が設定されている場合、ID 最適化アルゴリズムは、制限が確実に適用されるようにします。

### ID 最適化アルゴリズム

ID 最適化アルゴリズムは、制限が適用されることを確認するルールです。 アルゴリズムは、最新のリンクに従い、最も古いリンクを削除して、特定のグラフが定義した制限内に確実に収まるようにします。

匿名イベントと既知の識別子の関連付けに関するアルゴリズムの影響を以下に示します。

* 次の条件を満たす場合、ECID は最後に認証されたユーザーに関連付けられます。
   * CRM ID が ECID（共有デバイス）で結合される場合。
   * 制限が 1 つの CRM ID のみに設定されている場合。

詳しくは、 [ID 最適化アルゴリズム](./identity-optimization-algorithm.md).

### 優先度

>[!IMPORTANT]
>
>名前空間の優先度は、現在、アルファには使用できません。

名前空間の優先度を使用して、他の名前空間よりも重要な名前空間を定義できます。 名前空間に設定した優先度は、プライマリ ID の定義に使用されます。プライマリ ID とは、プロファイルフラグメント（属性とイベントデータ）をリアルタイム顧客プロファイルに保存する ID です。 優先度設定が設定されている場合、Web SDK のプライマリ ID 設定を使用して、保存するプロファイルフラグメントを決定できなくなります。

* 制限と優先度は独立した設定で、実行します。 **not** お互いに影響し合う：
   * 制限は、ID サービスの ID グラフ設定です。
   * 優先度は、リアルタイム顧客プロファイルのプロファイルフラグメント設定です。
   * 優先度が **not** id グラフシステムのガードレールに影響を与えます。
* **名前空間の優先度は数値です** 相対的な重要性を示す名前空間に割り当てられます。 これは名前空間のプロパティです。
* **プライマリID とは、プロファイルフラグメントが保存される ID です**. プロファイルフラグメントは、特定のユーザーに関する情報 ( 属性（通常 CRM レコードを介して取り込まれる）やイベント（通常、エクスペリエンスイベント、オンラインデータから取り込まれる）を保存するデータのレコードです。
* 名前空間の優先度によって、エクスペリエンスイベントのプライマリ ID が決まります。
   * プロファイルレコードの場合、Experience PlatformUI のスキーマワークスペースを使用して、プライマリ ID を含む ID フィールドを定義できます。 次のガイドを読む： [UI での id フィールドの定義](../../xdm/ui/fields/identity.md) を参照してください。

>[!BEGINSHADEBOX]

**名前空間の優先度の例**

名前空間に次の優先度を設定したとします。

1. CRM ID：ユーザーを表します。
2. IDFA: iPhoneやiPadなどのAppleハードウェアデバイスを表します。
3. GAID:Google Pixel などのGoogleハードウェアデバイスを表します。
4. ECID:Firefox、Safari、Chrome などの Web ブラウザーを表します。
5. AAID:Web ブラウザーを表します。
ECID と AAID が同時に送信される場合、両方の ID は同じ Web ブラウザー（重複）を表します。

次のエクスペリエンスイベントがExperience Platformに取り込まれると、プロファイルフラグメントはより優先度の高い名前空間に対して保存されます。

**認証済みイベント：**

* ID マップに ECID、GAID、CRM ID が含まれる場合、イベント情報は CRM ID（プライマリ ID）に対して保存されます。
   * GAID はGoogleハードウェアデバイス ( 例：Google Pixel)、ECID は Web ブラウザー ( 例：Google Chrome)、CRM ID は認証済みユーザーを表します。
   * ID マップに CRM ID、ECID および AAID が含まれる場合、イベント情報は CRM ID（プライマリ ID）に対して保存されます。

**未認証イベント：**

* ID マップに ECID、IDFA および AAID が含まれている場合、イベント情報は IDFA（プライマリ ID）に対して保存されます。
   * IDFA はAppleハードウェアデバイス (iPhoneなど ) を表し、ECID と AAID は共に Web ブラウザー (Safari) を表します。

>[!ENDSHADEBOX]

## 次の手順

ID グラフのリンクルールの詳細については、次のドキュメントを参照してください。

* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [ID グラフのリンクルールの設定例](./example-scenarios.md)

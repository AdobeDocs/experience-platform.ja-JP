---
title: ID サービスのリンクロジック
description: ID サービスが異なる ID をリンクして顧客の包括的なビューを作成する方法について説明します。
exl-id: 1c958c0e-0777-48db-862c-eb12b2e7a03c
source-git-commit: 2b6700b2c19b591cf4e60006e64ebd63b87bdb2a
workflow-type: tm+mt
source-wordcount: '980'
ht-degree: 1%

---

# ID サービスのリンクロジック

2 つの ID 間のリンクは、ID 名前空間と ID 値が一致すると確立されます。

リンクされる ID には次の 2 つのタイプがあります。

* **プロファイルレコード**：これらの ID は通常、CRM システムから取得されます。
* **エクスペリエンスイベント**:ID は通常、WebSDK の実装またはAdobe Analyticsのソースから取得されます。

## リンクを確立する意味

ID は、実世界のエンティティを表します。 2 つの ID 間にリンクが確立されている場合、2 つの ID は互いに関連付けられています。 次に、この概念の例を示します。

| アクション | リンクが確立されました | 意味 |
| --- | --- | --- |
| エンドユーザーがコンピューターを使用してログインします。 | CRM ID と ECID は相互にリンクされています。 | 人物 (CRM ID) が、ブラウザー (ECID) を持つデバイスを所有しています。 |
| エンドユーザーがiPhoneを使用して匿名で閲覧します。 | IDFA は ECID とリンクされています。 | iPhoneなどのAppleハードウェアデバイス (IDFA) は、ブラウザー (ECID) に関連付けられています。 |
| エンドユーザーがGoogle Chrome、Firefox の順に使用してログインします。 | CRM ID は、2 つの異なる ECID にリンクされています。 | ユーザー (CRM ID) は 2 つの Web ブラウザー (**注意**：各ブラウザーには独自の ECID があります )。 |
| データエンジニアが、ID としてマークされた 2 つのフィールド（CRM ID と電子メール）を含む CRM レコードを取り込みます。 | CRM ID と電子メールがリンクされています。 | 人物 (CRM ID) は電子メールアドレスに関連付けられます。 |

## ID サービスのリンクロジックについて

ID は、ID 名前空間と ID 値で構成されます。

* ID 名前空間は、に対する指定された ID 値のコンテキストです。 ID 名前空間の一般的な例には、CRM ID、電子メール、電話があります。
* ID 値は、実際のエンティティを表す文字列です。 例： &quot;julien&quot;<span>@acme.com&quot;は Email 名前空間の id 値、555-555-1234は Phone 名前空間の対応する id 値です。

>[!TIP]
>
>ID 名前空間がないと、ID 値のコンテキストが失われ、ID を正しく一致させるのに十分な情報が得られないので、ID 名前空間は重要です。

ID サービスのリンクロジックの仕組みを視覚的に表現するには、次の図を参照してください。

>[!BEGINTABS]

>[!TAB 既存のグラフ]

次の 3 つの ID がリンクされた既存の ID グラフがあるとします。

* 電話：(555)-555-1234
* EMAIL:julien<span>@acme.com
* CRM ID:60013ABC

![既存のグラフ](../images/identity-settings/existing-graph.png)

>[!TAB 受信データ]

ID のペアがグラフに取り込まれ、このペアには次が含まれます。

* CRM ID:60013ABC
* ECID:100066526

![受信データ](../images/identity-settings/incoming-data.png)

>[!TAB 更新されたグラフ]

ID サービスは、CRM ID:60013ABC が既にグラフ内に存在することを認識するので、新しい ECID のみをリンクします

![更新されたグラフ](../images/identity-settings/updated-graph.png)

>[!ENDTABS]

## 顧客シナリオ

データエンジニアが、次の CRM データセット（プロファイルレコード）をExperience Platformに取り込む場合。

| CRM ID** | 電話* | メール* | 名 | 姓 |
| --- | --- | --- | --- | --- |
| 60013ABC | 555-555-1234 | julien<span>@acme.com | ジュリアン | Smith |
| 31260XYZ | 777-777-6890 | evan<span>@acme.com | エバン | Smith |

>[!NOTE]
>
>* `**`  — プライマリ ID としてマークされるフィールドを示します。
>* `*`  — セカンダリ ID としてマークされるフィールドを示します。
>
>ID サービスは、プライマリ ID とセカンダリ ID を区別しません。 フィールドが ID としてマークされている限り、そのフィールドは ID サービスに取り込まれます。

また、WebSDK を実装し、次のデータテーブルを使用して WebSDK データセット（エクスペリエンスイベント）を取り込みました。

| タイムスタンプ | イベント内の ID* | イベント |
| --- | --- | --- |
| `t=1` | ECID:38652 | ホームページを表示 |
| `t=2` | ECID:38652、CRM ID:31260XYZ | 靴を検索 |
| `t=3` | ECID:44675 | ホームページを表示 |
| `t=4` | ECID:44675、CRM ID:31260XYZ | 購入履歴を表示 |

各イベントのプライマリ ID は、 [データ要素タイプの設定方法](../../tags/extensions/client/web-sdk/data-element-types.md).

>[!NOTE]
>
>* CRM ID をプライマリとして選択した場合、認証済みイベント（CRM ID と ECID を含む ID マップを持つイベント）は、CRM ID のプライマリ ID を持ちます。 未認証イベント（ECID を含む ID マップのみを持つイベント）の場合、ECID のプライマリ ID が割り当てられます。 Adobeでは、このオプションを推奨しています。
>
>* 認証状態に関係なく、ECID をプライマリとして選択した場合、ECID がプライマリ ID になります。

この例では、次のようになります。

* `t=1`は、デスクトップコンピューター (ECID:38652) を使用し、ホームページを匿名で閲覧した。
* `t=2`は同じデスクトップコンピューターを使用し、ログイン (CRM ID:31260XYZ) してから靴を検索しました。
   * ユーザーがログインすると、イベントは ECID と CRM ID の両方を ID サービスに送信します。
* `t=3`は、ノートパソコン (ECID:44675) を使用し、匿名で閲覧しました。
* `t=4`は、同じノートブック PC を使用し、ログインし (CRM ID:31260XYZ)、購入履歴を表示しました。


>[!BEGINTABS]

>[!TAB timestamp=0]

時刻 `timestamp=0`の場合、2 つの異なる顧客の 2 つの ID グラフがあります。 どちらも、3 つのリンクされた ID で表されます。

| | CRM ID | メール | Phone |
| --- | --- | --- | --- |
| Customer one | 60013ABC | julien<span>@acme.com | 555-555-1234 |
| 顧客 2 | 31260XYZ | evan<span>@acme.com | 777-777-6890 |

![timestamp-zero](../images/identity-settings/timestamp-zero.png)

>[!TAB timestamp=1]

時刻 `timestamp=1`を使用している場合、顧客はノート PC を使用して e コマース Web サイトにアクセスし、ホームページを表示し、匿名で閲覧します。 この匿名の閲覧イベントは、ECID:38652として識別されます。 ID サービスは少なくとも 2 つの ID を持つイベントのみを保存するので、この情報は保存されません。

![timestamp-one](../images/identity-settings/timestamp-one.png)

>[!TAB timestamp=2]

時刻 `timestamp=2`と同じノート PC を使用して e コマース Web サイトにアクセスしている。 ユーザー名とパスワードの組み合わせでログインし、靴を探します。 ID サービスは、ログイン時に顧客のアカウントを識別します。これは、顧客の CRM ID(31260XYZ) に対応しているからです。 また、ECID:38562は、同じデバイスで同じブラウザーを使用しているので、CRM ID:31260XYZ に関連付けられます。

![timestamp-two](../images/identity-settings/timestamp-two.png)

>[!TAB timestamp=3]

時刻 `timestamp=3` 顧客がタブレットを使用して e コマース web サイトにアクセスし、匿名で閲覧します。 この匿名の閲覧イベントは、ECID:44675として識別されます。 ID サービスは少なくとも 2 つの ID を持つイベントのみを保存するので、この情報は保存されません。

![timestamp-three](../images/identity-settings/timestamp-three.png)

>[!TAB timestamp=4]

時刻 `timestamp=4`の場合は、顧客が同じタブレットを使用し、自分のアカウント (CRM ID:31260XYZ) にログインして、購入履歴を表示します。 このイベントは、CRM ID:31260XYZ を、匿名の閲覧アクティビティに割り当てられた Cookie 識別子 (ECID:44675) にリンクし、ECID:44675を顧客 2 の ID グラフにリンクします。

![timestamp-four](../images/identity-settings/timestamp-four.png)

>[!ENDTABS]

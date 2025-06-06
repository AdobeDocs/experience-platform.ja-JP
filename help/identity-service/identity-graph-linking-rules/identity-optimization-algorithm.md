---
title: ID 最適化アルゴリズム
description: ID サービスでの ID 最適化アルゴリズムについて説明します。
exl-id: 5545bf35-3f23-4206-9658-e1c33e668c98
source-git-commit: 0587ddf1012adb13e6d399953839735f73fe151e
workflow-type: tm+mt
source-wordcount: '1580'
ht-degree: 4%

---

# ID 最適化アルゴリズム {#identity-optimization-algorithm}

>[!CONTEXTUALHELP]
>id="platform_identities_uniquenamespace"
>title="一意の名前空間"
>abstract="1 つのグラフに、一意の名前空間を使用した 2 つの ID を含めることはできません。グラフがこの制限を超えようとした場合、最新のリンクが保持され、最も古いリンクが削除されます。"

ID 最適化アルゴリズムは、ID グラフが個人を表していることを確認し、リアルタイム顧客プロファイルでの不要な ID の結合を防ぐのに役立つ、ID サービス上のグラフアルゴリズムです。

## 入力パラメーター {#input-parameters}

一意の名前空間と名前空間の優先度について詳しくは、この節を参照してください。 これらの 2 つの概念は、ID 最適化アルゴリズムで必要な入力パラメーターとして機能します。

### 一意の名前空間 {#unique-namespace}

一意の名前空間は、グラフの折りたたみが発生した場合に削除されるリンクを決定します。

単一の結合プロファイルとそれに対応する ID グラフは、単一の個人（ユーザーエンティティ）を表す必要があります。 通常、1 人の個人は CRMID やログイン ID で表されます。 2 人の個人（CRMID）が 1 つのプロファイルまたはグラフに結合されないことを想定しています。

ID 最適化アルゴリズムを使用して、ID サービスの人物エンティティを表す名前空間を指定する必要があります。 例えば、1 つの CRMID および 1 つのメールアドレスに関連付けられるユーザーアカウントが CRM データベースで定義されている場合、このサンドボックスの ID 設定は次のようになります。

* CRMID 名前空間=一意
* メール名前空間=一意

一意と宣言した名前空間は、特定の ID グラフ内で最大限度が 1 になるように自動的に設定されます。 例えば、CRMID 名前空間を一意として宣言する場合、ID グラフには、CRMID 名前空間を含む ID を 1 つだけ含めることができます。 一意の名前空間を宣言しない場合、その名前空間を持つ複数の ID をグラフに含めることができます。

>[!NOTE]
>
>* 世帯エンティティ表現（「世帯グラフ」）は、現時点ではサポートされていません。
>
>* ユーザー識別子であり、ID グラフを生成するためにサンドボックスで使用されるすべての名前空間は、一意の名前空間としてマークする必要があります。 そうしないと、望ましくないリンク結果が生じる可能性があります。

### 名前空間の優先度 {#namespace-priority}

名前空間の優先度は、ID 最適化アルゴリズムがリンクを削除する方法を決定します。

ID サービスの名前空間には、重要な暗黙の相対順序があります。 ピラミッドのような構造のグラフを考えてみましょう。 上のレイヤに 1 つのノード、中央のレイヤに 2 つのノード、下のレイヤに 4 つのノードがあります。 名前空間の優先度は、人物エンティティが正確に表されるようにするために、この相対順序を反映する必要があります。

名前空間の優先度とその完全な機能および使用方法について詳しくは、[ 名前空間優先度ガイド ](./namespace-priority.md) を参照してください。

![ グラフレイヤーと名前空間の優先度。](../images/namespace-priority/graph-layers.png " グラフレイヤーと名前空間の優先度。"){zoomable="yes"}

## プロセス {#process}

新しい ID を取り込むと、ID サービスは新しい ID とそれに対応する名前空間が一意の名前空間設定に従っているかどうかを確認します。 設定に従うと、取り込みは続行され、新しい ID がグラフにリンクされます。 ただし、設定に従わない場合、ID 最適化アルゴリズムは次のようになります。

* 名前空間の優先度を考慮しながら、最新のイベントを取り込みます。
* 適切なグラフレイヤーから 2 つの人物エンティティを結合するリンクを削除します。

## ID 最適化アルゴリズムの詳細

一意の名前空間制約に違反すると、ID 最適化アルゴリズムはリンクを「再生」し、グラフを最初から再構築します。

* リンクは次の順序で並べ替えられます。
   * 最新のイベント。
   * 名前空間の優先度の合計によるタイムスタンプ（小計=上位）。
* グラフは、上記の順序に基づいて再確立されます。 リンクを追加することが制限制約に違反している場合（例えば、グラフに一意の名前空間を持つ複数の ID が含まれている場合）、リンクは削除されます。
* 結果のグラフは、設定した一意の名前空間制約に準拠します。

![ID 最適化アルゴリズムを視覚化した図。](../images/ido_algorithm.png "ID 最適化アルゴリズムを視覚化した図。"){zoomable="yes"}

## ID 最適化アルゴリズムのシナリオの例

次の節では、共有デバイスや同じタイムスタンプのデータの取り込みなどのシナリオ下での ID 最適化アルゴリズムの動作について説明します。

### 共有デバイス

共有デバイスとは、複数の個人が使用するデバイスを指します。 例えば、共有デバイスには、パートナーや家族と共有するラップトップやタブレット、ライブラリコンピューター、公開キオスクなどがあります。

>[!BEGINTABS]

>[!TAB  例 1]

| 名前空間 | 一意の名前空間 |
| --- | --- |
| CRMID | ○ |
| メール | ○ |
| ECID | × |

この例では、CRMID とメールの両方を一意の名前空間として指定します。 一意の名前空間設定により、`timestamp=0` 時に CRM レコードデータセットが取り込まれ、2 つの異なるグラフが作成されます。 各グラフには、CRMID とメール名前空間が含まれます。

* `timestamp=1`:Jane はノートパソコンを使用して e コマースの web サイトにログインします。 Jane は彼女の CRMID とメールで表され、彼女が使用するラップトップ上の web ブラウザーは ECID で表されます。
* `timestamp=2`: John は同じノートパソコンを使用して e コマースの web サイトにログインします。 John は CRMID とメールで表されていますが、使用した web ブラウザーは既に ECID で表されています。 同じ ECID が 2 つの異なるグラフにリンクされているので、ID サービスは、このデバイス（ラップトップ）が共有デバイスであることを知ることができます。
* ただし、一意の名前空間設定により、グラフごとに最大 1 つの CRMID 名前空間と 1 つのメール名前空間が設定されるので、ID 最適化アルゴリズムはグラフを 2 つに分割します。
   * 最後に、John は最後に認証されたユーザーなので、ラップトップを表す ECID は、Jane の代わりに彼のグラフにリンクされたままになります。

![ 共有デバイスの場合。](../images/identity-settings/shared-device-case-one.png " 共有デバイスの場合 "){zoomable="yes"}

>[!TAB  例 2]

| 名前空間 | 一意の名前空間 |
| --- | --- |
| CRMID | ○ |
| ECID | × |

この例では、CRMID 名前空間が一意の名前空間として指定されています。

* `timestamp=1`:Jane はノートパソコンを使用して e コマースの web サイトにログインします。 彼女は彼女の CRMID によって表され、ラップトップ上の Web ブラウザは ECID によって表されます。
* `timestamp=2`: John は同じノートパソコンを使用して e コマースの web サイトにログインします。 ユーザーは CRMID で表され、使用する web ブラウザーは同じ ECID で表されます。
   * このイベントは、2 つの独立した CRMID を同じ ECID にリンクします。これは、設定された上限である 1 つの CRMID を超えています。
   * その結果、ID 最適化アルゴリズムによって古いリンク（この場合は、`timestamp=1` にリンクされていた Jane の CRMID）が削除されます。
   * ただし、Jane の CRMID は ID サービスのグラフとしては存在しなくなりますが、リアルタイム顧客プロファイルのプロファイルとしては引き続き存在します。 これは、ID グラフに 2 つ以上のリンクされた ID を含める必要があり、リンクを削除した結果、Jane の CRMID にリンクする別の ID がなくなったためです。

![ ケース 2：共有デバイス。](../images/identity-settings/shared-device-case-two.png " 共有デバイスの場合 2。"){zoomable="yes"}

>[!ENDTABS]

### 無効な E メール

ユーザーが自分のメールや電話番号に間違った値を入力する場合があります。

| 名前空間 | 一意の名前空間 |
| --- | --- |
| CRMID | ○ |
| メール | ○ |
| ECID | × |

この例では、CRMID およびメール名前空間は一意として指定されます。 ジェーンとジョンが不正なメール値（test<span>@test.comなど）を使用して e コマース web サイトにサインアップしたシナリオを考えてみましょう。

* `timestamp=1`:Jane はiPhoneの Safari を使用して e コマース web サイトにログインし、CRMID （ログイン情報）と ECID （ブラウザー）を設定します。
* `timestamp=2`:John は、iPhoneでGoogle Chromeを使用して e コマース web サイトにログインし、CRMID （ログイン情報）と ECID （ブラウザー）を設定します。
* `timestamp=3`：データエンジニアが Jane の CRM レコードを取り込みました。その結果、彼女の CRMID が不正なメールにリンクされることになります。
* `timestamp=4`：データエンジニアが John の CRM レコードを取り込みました。その結果、CRMID が不正なメールにリンクされることになります。
   * これは、2 つの CRMID 名前空間を持つ単一のグラフを作成するので、一意の名前空間設定の違反になります。
   * その結果、ID 最適化アルゴリズムは古いリンクを削除します。この場合、これは CRMID 名前空間を持つ Jane の ID と test<span>@test を持つ ID の間のリンクです。

ID 最適化アルゴリズムを使用すると、偽の電子メールや電話番号などの不正な ID 値が、複数の異なる ID グラフに伝わることはありません。

![ 悪いメール取り込みの図。](../images/identity-settings/bad-email.png " 悪いメール取り込みの図。"){zoomable="yes"}

## 匿名イベントの関連付け

ECID には未認証（匿名）イベントが格納されるのに対して、CRMID には認証済みイベントが格納されます。 共有デバイスの場合、ECID （未認証イベントのベアラー）が **最後に認証されたユーザー** に関連付けられます。

匿名イベントの関連付けの仕組みをより深く理解するには、次の図を参照してください。

* ケビンとノラはタブレットを共有している。
   * `timestamp=1`:Kevin は自分のアカウントを使用して e コマースの web サイトにログインすることで、CRMID （ログイン情報）と ECID （ブラウザー）を確立します。 ログイン時に、Kevin は最後の認証済みユーザーと見なされます。
   * `timestamp=2`:Nora は自分のアカウントを使用して e コマース web サイトにログインし、それにより自分の CRMID （ログイン情報）と同じ ECID を確立します。 ログイン時に、Nora が最後の認証済みユーザーと見なされるようになりました。
   * `timestamp=3`:Kevin はタブレットを使用して e コマースの web サイトを閲覧しますが、自分のアカウントでログインすることはありません。 Kevin のブラウジングアクティビティは ECID に保存され、最後に認証されたユーザーであることから、Nora と関連付けられます。 この時点で、Nora が匿名イベントを所有しています。
      * Kevin が再度ログインするまで、Nora の結合プロファイルは、ECID に対して保存されたすべての未認証イベント（ECID がプライマリ ID であるイベントを含む）に関連付けられます。
   * `timestamp=4`: Kevin は 2 回目ログインします。 この時点で、このユーザーは再び最後の認証済みユーザーになり、未認証のイベントも所有するようになります。
      * 最初のログインの前に `timestamp=1`、および
      * Kevin の 1 回目のログインと 2 回目のログインの間に匿名でブラウジングを行う際に、Nora が行ったアクティビティ。

![ 匿名イベント関連付けを示す図。](../images/identity-settings/anon-event-association.png " 匿名イベントの関連付けを示す図。"){zoomable="yes"}


## 次の手順

[!DNL Identity Graph Linking Rules] について詳しくは、次のドキュメントを参照してください。

* [[!DNL Identity Graph Linking Rules] の概要](./overview.md)
* [実装ガイド](./implementation-guide.md)
* [グラフ設定の例](./example-configurations.md)
* [トラブルシューティングと FAQ](./troubleshooting.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション UI](./graph-simulation.md)
* [ID 設定 UI](./identity-settings-ui.md)
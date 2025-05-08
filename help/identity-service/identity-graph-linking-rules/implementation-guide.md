---
title: ID グラフリンクルールの実装ガイド
description: ID グラフリンクルール設定を使用してデータを実装する際に従うべき推奨手順を説明します。
exl-id: 368f4d4e-9757-4739-aaea-3f200973ef5a
source-git-commit: 1a6ca508f0f5e95ddad9014d6507a7a829592673
workflow-type: tm+mt
source-wordcount: '1958'
ht-degree: 6%

---

# ID グラフリンクルールの実装ガイド

>[!AVAILABILITY]
>
>ID グラフリンクルールは現在限定提供になっており、開発用サンドボックスのすべての顧客がアクセスできます。
>
>* **アクティベーション要件**：この機能は、[!DNL Identity Settings] ールを設定して保存するまで、非アクティブのままになります。 この設定がない場合、システムは引き続き正常に動作し、動作は変更されません。
>* **重要なメモ**：この限定提供フェーズでは、Edgeのセグメント化によって、予期しないセグメントメンバーシップの結果が生じる可能性があります。 ただし、ストリーミングおよびバッチセグメント化は期待どおりに機能します。
>* **次の手順**：実稼動サンドボックスでこの機能を有効にする方法については、Adobe アカウントチームにお問い合わせください。

>[!IMPORTANT]
>
>このドキュメントでは、データのない新しいサンドボックスで実装を開始することを前提としています。

Adobe Experience Platform ID サービスを使用してデータを実装する際に従うことができる、ステップバイステップのガイドについては、このドキュメントを参照してください。

操作手順の概要：

1. [実装の前提条件を満たす](#prerequisites-for-implementation)
2. [必要な ID 名前空間の作成](#namespace)
3. [グラフシミュレーションツールを使用して、ID 最適化アルゴリズムを理解します](#graph-simulation)
4. [ID 設定 UI を使用して、一意の名前空間を指定し、名前空間の優先度ランキングを設定します](#identity-settings)
5. [エクスペリエンスデータモデル（XDM）スキーマの作成](#schema)
6. [データセットの作成](#dataset)
7. [Experience Platformへのデータの取り込み](#ingest)

## 実装の前提条件 {#prerequisites-for-implementation}

この節では、データに [!DNL Identity Graph Linking Rules] を実装する前に完了する必要がある前提条件の手順の概要を説明します。

### 一意の名前空間

#### 1 人のユーザーの名前空間要件 {#single-person-namespace-requirement}

最高の優先度を持つ一意の名前空間が、常にすべてのプロファイルに存在することを確認する必要があります。 これにより、ID サービスは特定のグラフ内で適切な人物識別子を検出できます。

+++単一の人物識別子の名前空間を持たないグラフの例を表示する場合に選択します

ユーザー識別子を表す一意の名前空間がない場合、異なるユーザー識別子を同じ ECID にリンクするグラフが生成される場合があります。 この例では、B2BCRM と B2CCRM の両方が同じ ECID に同時にリンクされています。 このグラフは、トムが B2C ログインアカウントを使用して、Summer とデバイスを共有し、彼女の B2B ログインアカウントを使用したことを示しています。 ただし、システムはこれが 1 つのプロファイル（グラフの折りたたみ）であることを認識します。

![2 人の人物 ID が同じ ECID にリンクされているグラフシナリオ。](../images/graph-examples/multi_namespaces.png)

+++

+++選択すると、単一のユーザー識別子名前空間を持つグラフの例が表示されます

一意の名前空間（この場合、2 つの異なる名前空間ではなく CRMID）を指定すると、ID サービスは、ECID に最後に関連付けられたユーザー識別子を識別できます。 この例では、一意の CRMID が存在するので、ID サービスは、「共有デバイス」シナリオを認識できます。ここでは、2 つのエンティティが同じデバイスを共有しています。

![2 人の人物 ID が同じ ECID にリンクされているが、古いリンクは削除されている共有デバイスグラフのシナリオ。](../images/graph-examples/crmid_only_multi.png)

+++

### 名前空間の優先度設定

[Adobe Analytics ソースコネクタ ](../../sources/tutorials/ui/create/adobe-applications/analytics.md) を使用してデータを取り込む場合は、ID サービスが AAID をブロックするので、Adobe Analytics ID （AAID）よりも ECID の優先度を高める必要があります。 ECID に優先順位を付けることで、AAID ではなく ECID に未認証のイベントを保存するようリアルタイム顧客プロファイルに指示できます。

### XDM エクスペリエンスイベント {#xdm-experience-events}

>[!CONTEXTUALHELP]
>id="platform_identities_linkingrules_xdm"
>title="単一のユーザー識別子が含まれていることを確認"
>abstract="実装前のプロセス中に、Experience Platform に送信される認証済みイベントに、CRMID などの&#x200B;**単一**&#x200B;のユーザー識別子が常に含まれていることを確認する必要があります。"

実装前のプロセス中に、Experience Platform に送信される認証済みイベントに、CRMID などの&#x200B;**単一**&#x200B;のユーザー識別子が常に含まれていることを確認する必要があります。

* （推奨） 1 つの一意のユーザー識別子を持つ認証済みイベント。
* （非推奨） 2 つの一意のユーザー識別子を持つ認証済みイベント。 一意のユーザー ID が複数ある場合、不要なグラフの折りたたみが発生する可能性があります。
* （非推奨）一意のユーザー識別子を持たない認証済みイベント。 一意のユーザー ID がない場合は、未認証のイベントと認証済みのイベントの両方が ECID に対して保存されます。

>[!BEGINTABS]

>[!TAB 1 つのユーザー識別子を持つ認証済みイベント ]

```json
{
  "_id": "test_id",
  "identityMap": {
      "ECID": [
          {
              "id": "62486695051193343923965772747993477018",
              "primary": false
          }
      ],
      "CRMID": [
          {
              "id": "John",
              "primary": true
          }
      ]
  },
  "timestamp": "2024-09-24T15:02:32+00:00",
  "web": {
      "webPageDetails": {
          "URL": "https://business.adobe.com/",
          "name": "Adobe Business"
      }
  }
}
```

>[!TAB 2 人のユーザー識別子を持つ認証済みイベント ]

システムが 2 人の人物の識別子を送信した場合、実装が 1 人の人物による名前空間要件に失敗する可能性があります。 例えば、webSDK 実装の identityMap に CRMID、customerID および ECID 名前空間が含まれている場合、すべての単一のイベントに CRMID と customerID の両方が含まれるという保証はありません。

次のよ **にペイロードを送信する** しないでください。

```json
{
  "_id": "test_id",
  "identityMap": {
      "ECID": [
          {
              "id": "62486695051193343923965772747993477018",
              "primary": false
          }
      ],
      "CRMID": [
          {
              "id": "John",
              "primary": true
          }
      ],
      "customerID": [
          {
            "id": "Jane",
            "primary": false
          }
      ],
  },
  "timestamp": "2024-09-24T15:02:32+00:00",
  "web": {
      "webPageDetails": {
          "URL": "https://business.adobe.com/",
          "name": "Adobe Business"
      }
  }
}
```

ただし、2 人の人物 ID を送信できるとは言え、実装やデータエラーが原因で不要なグラフの折りたたみが防がれる保証はないことに注意してください。 次のシナリオについて考えてみます。

* `timestamp1` = John がログイン -> システムが `CRMID: John, ECID: 111` をキャプチャします。 ただし、`customerID: John` はこのイベントペイロードには存在しません。
* `timestamp2` = Jane がログイン – > システム キャプチャ `customerID: Jane, ECID: 111` ます。 ただし、`CRMID: Jane` はこのイベントペイロードには存在しません。

したがって、認証済みイベントで送信するユーザー識別子は 1 つのみにすることをお勧めします。

グラフのシミュレーションでは、この取り込みは次のようになります。

![ レンダリングされたグラフの例を示すグラフシミュレーション UI](../images/implementation/example-graph.png)

>[!TAB  ユーザー識別子のない認証済みイベント ]

この例では、エンドユーザーの John が認証中に web サイトを閲覧していた際に、次のイベントがExperience Platformに送信されたとします。 ただし、認証済みであるにもかかわらず、イベントでユーザーが識別できないため、Experience Platformは John を識別できません。 そのため、このイベントは、John に特別に関連付けられたオンラインアクティビティとして認識されるのではなく、Adobe Business の web サイトを閲覧する匿名ユーザーとして解釈されます。

```json
{
    "_id": "test_id",
    "identityMap": {
        "ECID": [
            {
                "id": "62486695051193343923965772747993477018",
                "primary": false
            }
        ]
    },
    "timestamp": "2024-09-24T15:02:32+00:00",
    "web": {
        "webPageDetails": {
            "URL": "https://business.adobe.com/",
            "name": "Adobe Business"
        }
    }
}
```

>[!ENDTABS]

## 権限の設定 {#set-permissions}

ID サービスの実装プロセスの最初の手順は、必要な権限がプロビジョニングされたロールにExperience Platform アカウントが確実に追加されるようにすることです。 管理者は、Adobe Experience Cloudの権限 UI に移動して、アカウントの権限を設定できます。 ここから、アカウントを次の権限を持つ役割に追加する必要があります。

* [!UICONTROL ID 設定の表示 &#x200B;]：この権限を適用して、ID 名前空間の参照ページで一意の名前空間と名前空間の優先度を表示できるようにします。
* [!UICONTROL ID 設定の編集 &#x200B;]:ID 設定を編集および保存できるようにするために、この権限を適用します。

権限について詳しくは、[ 権限ガイド ](../../access-control/abac/ui/permissions.md) を参照してください。

## ID 名前空間の作成 {#namespace}

データで名前空間が必要な場合、まず組織に適した名前空間を作成する必要があります。 カスタム名前空間の作成手順については、[UI でのカスタム名前空間の作成 ](../features/namespaces.md#create-custom-namespaces) に関するガイドを参照してください。

## グラフシミュレーションツールの使用 {#graph-simulation}

次に、ID サービス UI ワークスペースの [ グラフシミュレーションツール ](./graph-simulation.md) に移動します。 グラフシミュレーションツールを使用して、様々な一意の名前空間や名前空間の優先度設定で作成された ID グラフをシミュレートできます。

様々な設定を作成することで、グラフシミュレーションツールを使用して、ID 最適化アルゴリズムや特定の設定が、グラフの動作に与える影響をより深く理解できます。

## ID 設定の指定 {#identity-settings}

グラフがどのように動作するかを把握したら、ID サービス UI ワークスペースの [ID 設定 UI](./identity-settings-ui.md) に移動します。 ID 設定 UI にアクセスするには、左側のナビゲーションから **[!UICONTROL ID]** を選択してから、**[!UICONTROL 設定]** を選択します。

![ 設定ボタンがハイライト表示された ID 参照ページ。](../images/implementation/settings.png)

ID 設定 UI を使用して、一意の名前空間を指定し、優先順に名前空間を設定します。 設定の適用が完了したら、新しい設定が ID サービスに反映されるまで少なくとも 6 時間かかるので、データの取り込みに進むまでに少なくとも 6 時間待つ必要があります。

詳しくは、[ID 設定 UI ガイド ](./identity-settings-ui.md) を参照してください。

## XDM スキーマの作成 {#schema}

独自の名前空間と名前空間の優先順位が確立されたので、データを取り込むために必要な設定に進むことができます。 まず、XDM スキーマを作成する必要があります。 データに応じて、XDM 個人プロファイルと XDM ExperienceEvent の両方のスキーマを作成する必要が生じる場合があります。

リアルタイム顧客プロファイルにデータを取り込むには、スキーマに、プライマリ ID として指定されたフィールドが少なくとも 1 つ含まれていることを確認する必要があります。 プライマリ ID を設定することで、特定のスキーマをプロファイル取り込みに対して有効にできます。

スキーマの作成方法について詳しくは、[UI での XDM スキーマの作成 ](../../xdm/tutorials/create-schema-ui.md) に関するガイドを参照してください。

## データセットの作成 {#dataset}

次に、取り込むデータの構造を提供するデータセットを作成します。 データセットは、スキーマ（列）とフィールド（行）で構成されるデータコレクション（通常はテーブル）を格納し管理するための構造です。データセットはスキーマと連携し、リアルタイム顧客プロファイルにデータを取り込むには、データセットがプロファイル取り込みに対して有効になっている必要があります。 データセットをプロファイルで有効にするには、プロファイルの取り込みが有効になっているスキーマを参照する必要があります。

データセットの作成方法については、[ データセット UI ガイド ](../../catalog/datasets/user-guide.md) を参照してください。

## データの取り込み {#ingest}

この時点で、次の情報が得られます。

* ID サービス機能にアクセスするために必要な権限。
* データの名前空間。
* 名前空間に一意の名前空間を指定し、優先順位を設定します。
* 1 つ以上の XDM スキーマ。 （データと特定のユースケースに応じて、プロファイルスキーマとエクスペリエンスイベントスキーマの両方を作成する必要がある場合があります）。
* スキーマに基づくデータセット。

上記のすべての項目を完了したら、データをExperience Platformに取り込み始めることができます。 様々な方法でデータ取り込みを実行できます。 次のサービスを使用してデータをExperience Platformに取り込むことができます。

* [ バッチおよびストリーミング取得 ](../../ingestion/home.md)
* [Experience Platformでのデータ収集](../../collection/home.md)
* [Experience Platform ソース](../../sources/home.md)

>[!TIP]
>
>データが取り込まれても、XDM 生データペイロードは変更されません。 プライマリ ID 設定は、UI に表示される場合があります。 ただし、これらの設定は ID 設定によって上書きされます。

フィードバックについては、ID サービス UI ワークスペースの **[!UICONTROL Beta feedback]** オプションを使用してください。

## グラフの検証 {#validate}

ID ダッシュボードを使用すると、全体的な ID 数とグラフ数のトレンド、名前空間別の ID 数、グラフサイズ別のグラフ数など、ID グラフの状態に関するインサイトを得ることができます。 ID ダッシュボードを使用して、2 つ以上の ID を持つグラフのトレンドを名前空間別に整理して表示することもできます。

省略記号（`...`）を選択し、**[!UICONTROL さらに表示]** を選択して、詳細を確認したり、折りたたまれたグラフがないことを検証します。

![ID サービス UI ワークスペースの ID ダッシュボード。](../images/implementation/identity_dashboard.png)

表示されるウィンドウを使用して、折りたたまれたグラフの情報を表示します。 この例では、メールと電話の両方が一意の名前空間としてマークされているので、サンドボックスには折りたたまれたグラフはありません。

![ 複数の ID を持つグラフのポップアップウィンドウ。](../images/implementation/graphs.png)

## 付録 {#appendix}

ID 設定と一意の名前空間を実装する際に参照できる追加情報については、この節を参照してください。

### Dangling loginID シナリオ {#dangling-loginid-scenario}

次のグラフは、「ぶら下がっている」 loginID シナリオをシミュレートします。 この例では、2 つの異なる loginID が同じ ECID にバインドされています。 ただし、`{loginID: ID_C}` は CRMID にリンクされていません。 したがって、ID サービスが、これら 2 つの loginID が 2 つの異なるエンティティを表していることを検出する方法はありません。

>[!BEGINTABS]

>[!TAB  あいまいな loginID]

この例では、CRMID に対するリンクが解除されたまま、`{loginID: ID_C}` が残っています。 したがって、この loginID を関連付ける必要がある人物エンティティはあいまいなままになります。

![ 「ぶら下がっている」 loginID シナリオを含むグラフの例。](../images/graph-examples/dangling_example.png)

>[!TAB loginID は CRMID にリンクされています ]

この例では、`{loginID: ID_C}` は `{CRMID: Tom}` にリンクされています。 そのため、システムは、この loginID が Tom に関連付けられていることを識別できます。

![LoginID は CRMID にリンクされています。](../images/graph-examples/id_c_tom.png)

>[!TAB loginID は別の CRMID にリンクされています ]

この例では、`{loginID: ID_C}` は `{CRMID: Summer}` にリンクされています。 したがって、システムはこの loginID が別の人物エンティティ（この場合は Summer）に関連付けられていることを識別できます。

この例では、Tom と Summer が、`{ECID: 111}` で表されるデバイスを共有する人物エンティティを分離することも示しています。

![ ログイン ID は別の CRMID にリンクされています。](../images/graph-examples/id_c_summer.png)

>[!ENDTABS]

## 次の手順

[!DNL Identity Graph Linking Rules] について詳しくは、次のドキュメントを参照してください。

* [[!DNL Identity Graph Linking Rules] の概要](./overview.md)
* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [グラフ設定の例](./example-configurations.md)
* [トラブルシューティングと FAQ](./troubleshooting.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション UI](./graph-simulation.md)
* [ID 設定 UI](./identity-settings-ui.md)

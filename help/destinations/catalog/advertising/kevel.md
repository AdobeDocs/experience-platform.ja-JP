---
title: Kevel Connection
description: Kevel ストリーミング宛先を使用して、オーディエンスをKevelのUserDBおよびセグメント管理APIに直接活性化し、決定時にリアルタイムのターゲティングをサポートします。
last-substantial-update: 2026-01-27T00:00:00Z
exl-id: 53ce2864-6a3b-4859-b14d-a03c2ce18884
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 8%

---

# [!DNL Kevel] 接続 {#kevel}

## 概要 {#overview}

[[!DNL Kevel]](https://www.kevel.com/)は、革新的なコマース リーダーが小売メディアを立ち上げ、拡大し、成功するのに役立つAI対応テクノロジーとエキスパート ガイダンスを提供します。 [!DNL Kevel]のRetail Media Cloudは、オンサイトおよびオフサイトの広告にターゲットを絞り、アトリビューション可能でカスタマイズ可能な広告フォーマットを提供します。

[!DNL Kevel]の[!DNL Adobe Experience Platform] ストリーミング宛先を使用すると、お客様は[!DNL Kevel]のUserDBおよびセグメント管理APIにAdobe オーディエンスを直接アクティベートして、広告決定時にリアルタイムのターゲティングをサポートできます。

>[!IMPORTANT]
>
>[!DNL Kevel]の宛先またはそのドキュメントに関する質問がある場合、または更新をリクエストする場合は、[!DNL Kevel] チーム（[support@kevel.com](mailto:support@kevel.com)）にメールでお問い合わせください。

## ユースケース {#use-cases}

小売企業のメディア体験をまたいで、豊富な1st パーティの行動オーディエンスを活用することで、より関連性の高い広告とより強力なパフォーマンスを提供できます。 Experience Platformでは、頻繁に買い物をする利用者や、最近商品に興味を持った利用者など、価値の高いインテントベースのオーディエンスを構築し、それらのメンバーシップをリアルタイムで[!DNL Kevel]に同期します。 [!DNL Kevel]では、これらのセグメントを直ちに広告決定に利用できるようにし、検索、閲覧、アプリ体験をまたいで、スポンサー付き商品やその他の形式を正確にターゲティングできるようにします。 利用者の適格性を把握し、そうしたシグナルに基づいて行動することで、より関連性の高いインプレッション、より優れたターゲティング、測定およびROASの向上を実現できます。

## 前提条件 {#prerequisites}

[!DNL Kevel]宛先の使用に備えるには、次の前提条件が満たされていることを確認してください。

- アクティブな&#x200B;**[!DNL Kevel]ネットワーク**&#x200B;とAPI アクセスが必要です。
- セグメントの作成とUserDB レコードの更新を行う権限を持つ&#x200B;**[!DNL Kevel]API キー**&#x200B;が必要です。
- [!DNL Kevel]広告リクエスト中にサイトまたはアプリが送信するIDにマッピングするID名前空間をExperience Platformで設定する必要があります（ECID、GAID、IDFA、ロイヤルティ IDなど）。
- Adobeのお客様は、リアルタイム広告リクエスト中に使用されるIDのみをマッピングする必要があります。各IDはUserDB レコードになります。

## サポートされている ID {#supported-identities}

[!DNL Kevel]宛先は、広告リクエストを[!DNL Kevel]に送信する際にアプリケーションが使用できる任意のIDのアクティベーションをサポートしています。 対応するUserDB レコードを生成するために、最大3つのID名前空間をマッピングできます。

[!DNL Kevel]は、次のExperience Platform ID名前空間をサポートしています。

| ID 名前空間 | 説明 | 一般的な使用状況 |
|--------------------|---------------------------------|----------------------------------------------------------------|
| **ECID** | Experience Cloud ID | オンサイトのパーソナライゼーションとAdobe間のIDに使用されます。 |
| **GAID** | GOOGLE ADVERTISING ID | Android アプリ/デバイストラフィックに使用されます。 |
| **IDFA** | APPLE ADVERTISING ID | IOSのアプリ/デバイストラフィックに使用されます（ATTの同意が必要です）。 |
| **EXTERNAL_ID** | 外部ID （カスタム識別子） | 独自またはバックエンドで生成されたIDを渡します。 |

{style="table-layout:auto"}

### カスタム ID名前空間のサポート {#custom-identity-namespaces}

[!DNL Kevel]の宛先&#x200B;**は、Experience Platformの実装で定義されているカスタム名前空間**&#x200B;も受け入れます。

インタラクションがあります。

- **顧客固有のID名前空間**&#x200B;をマッピングできます（例：`loyalty_id`、`gigya_id`、またはIdentity Serviceで定義した任意のカスタム ID）。
- これらの名前空間は、グローバル名前空間と同じように`kevel_user_key1`、`kevel_user_key2`、または`kevel_user_key3`に割り当てることができます。
- [!DNL Kevel]は、マッピングされたID **ごとに** 1つのUserDB レコードを生成し、システムが送信する各識別子の広告決定時にリアルタイム一致を可能にします。

### ID マッピング動作 {#identity-mapping-behavior}

- **最大3つの**&#x200B;個のExperience Platform ID名前空間を[!DNL Kevel]の3つのID スロットにマッピングできます。
- アクティブ化されたプロファイルごとに、[!DNL Kevel]は、マッピングされたID **ごとに、インスタンスごとに** 1つのUserDB レコードを受け取ります。
- ユーザーは、不要なUserDB ストレージを避けるために、実際に広告リクエストで送信するIDのみを[!DNL Kevel]にマッピングする必要があります。

![Kevel Destination](/help/destinations/assets/catalog/advertising/kevel-destination-mappings.png)のマッピング例

## サポートされるオーディエンス {#supported-audiences}

| オーディエンスの由来 | サポートあり | 説明 |
|-----------------------|-----------|---------------------------------------------------------- |
| セグメント化サービス | ○ | セグメンテーションエンジンによって評価されたAdobe プロファイルのオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
|------|------|-------|
| 書き出しタイプ | **セグメントの書き出し** | [!DNL Kevel]は、プロファイルがオーディエンスに適格であるか、またはオーディエンスを終了するたびに更新を受け取ります。 |
| 書き出し頻度 | **ストリーミング** | アップデートは、Destination SDKストリーミングフレームワークを使用してリアルタイムで送信されます。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

標準のExperience Platform [connect a destination](../../ui/connect-destination.md) ワークフローに従います。

>[!IMPORTANT]
>
>**宛先の表示**&#x200B;および&#x200B;**宛先の管理**&#x200B;権限が必要です。

### 宛先に対する認証 {#authenticate}

[!DNL Kevel]に接続する場合は、次のフィールドを指定します。

- **ベアラートークン** — [!DNL Kevel] API キー。

![Kevel Destination](/help/destinations/assets/catalog/advertising/kevel-destination-authentication.png)の認証オプション

### 宛先の詳細の入力 {#destination-details}

認証後、次の項目を設定します。

- **名前** – この宛先インスタンスを識別するラベル。
- **説明** – この宛先インスタンスを説明するオプションのテキスト。
- **[!DNL Kevel]ネットワーク ID** — [!DNL Kevel] ネットワーク ID。

![Kevel Destination](/help/destinations/assets/catalog/advertising/kevel-destination-details.png)の宛先の詳細

## この宛先に対してオーディエンスをアクティブ化 {#activate}

オーディエンスを[!DNL Kevel]に送信するには、[ ストリーミング宛先に対するオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md)のワークフローに従います。

### オーディエンスの非アクティブ化 {#deactivate}

オーディエンスがExperience Platformの[!DNL Kevel]宛先から非アクティブ化または削除されると、Experience Platformはそのオーディエンスに対するプロファイルの適格性更新の送信を停止します。 [!DNL Kevel]で作成された既存のセグメントは引き続き使用でき、自動的には削除されません。

アクティブなキャンペーンで[!DNL Kevel] セグメントが現在使用されている場合、[!DNL Kevel]は削除を防ぎ、ライブ配信を中断しないようにします。 この場合、Experience Platformで非アクティブ化すると、次の結果になります。

- Experience Platform データフローが停止します
- [!DNL Kevel] セグメントは引き続き存在し、手動で削除するかキャンペーンが更新されるまで、キャンペーンにアタッチされたままになる可能性があります

[!DNL Kevel]のターゲティングを完全に停止するには、[!DNL Kevel]のキャンペーン管理システムでアクティブなキャンペーンからセグメントが削除されていることを確認します。

### 属性と ID のマッピング {#map}

[!DNL Kevel]には次が必要です：

- **ID名前空間** — [!DNL Kevel]個のID スロットにマッピングされた最大3つのID名前空間。
- **セグメントメンバーシップ** – 手動マッピングは必要ありません。Experience Platformは、セグメントメンバーシップ IDとエイリアスを自動的に渡します。

アクティブ化中に、[!DNL Kevel]用に設定したID名前空間を選択します。 各IDは、独自のUserDB更新呼び出しを生成します。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

プロファイルがオーディエンスの対象となるか、オーディエンスを終了すると、Experience Platformはストリーミング更新を[!DNL Kevel]に送信します。

### [!DNL Kevel] UserDBが受信したペイロードのサンプル {#sample-payload}

```json
PUT /udb/{networkId}/segments?userKey=ECID-12345
{
  "segments": [1723, 3344, 9988]
}
```

| パラメーター | 説明 |
|-----------|-------------|
| **userKey** | マッピングされたAdobe IDから派生します。 |
| **セグメント** | プロファイルが現在実現されているAdobe オーディエンスに対応する[!DNL Kevel] セグメント IDのセット。 |

{style="table-layout:auto"}

### 書き出し時に使用されるExperience Platform プロファイルの例 {#sample-profile}

[!DNL Kevel]宛先に対してオーディエンスをアクティブ化すると、Experience Platformは、**セグメントの選定**&#x200B;と、顧客&#x200B;**によって**&#x200B;のID スロットにマッピングされた[!DNL Kevel]IDの両方を含むプロファイルフラグメントを送信します。

次に、書き出されたプロファイルの例を示します。

- `kevel_user_key1`、`kevel_user_key2`および`kevel_user_key3`にマッピングされた複数のID名前空間
- `ups`名前空間で1つのアクティブ化されたセグメント

```json
{
  "segmentMembership": {
    "ups": {
      "9d161bbb-c785-474a-965b-7d7bc2adf879": {
        "status": "realized",
        "lastQualificationTime": "2025-12-10T21:43:38.541076Z"
      }
    }
  },
  "identityMap": {
    "kevel_user_key1": [
      {
        "id": "ECID-fN1zo"
      },
      {
        "id": "ECID-9Xr2p"
      }
    ],
    "kevel_user_key2": [
      {
        "id": "GAID-4oic4"
      }
    ],
    "kevel_user_key3": [
      {
        "id": "IDFA-nB5fU"
      }
    ]
  }
}
```

#### [!DNL Kevel]がこのプロファイルをどのように解釈しているか {#kevel-profile-interpretation}

[!DNL Kevel]宛先設定では、マッピングされた各IDが個別のUserDB レコードを生成します。つまり、[!DNL Kevel]は次を受信します。

- `ECID-fN1zo`の更新プログラム 1件
- `ECID-9Xr2p`の更新プログラム 1件
- `GAID-4oic4`の更新プログラム 1件
- `IDFA-nB5fU`の更新プログラム 1件

これにより、同じ人物が利用可能なIDのいずれかを使用して決定時に認識され、各IDには同一のセグメントメンバーシップのセットが含まれます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

- [[!DNL Kevel] UserDB リファレンス ](https://dev.kevel.com/reference/userdb)
- [[!DNL Kevel]  ユーザーセグメントのターゲット設定](https://dev.kevel.com/docs/segment-targeting)

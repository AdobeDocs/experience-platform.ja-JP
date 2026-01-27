---
title: ケベル接続
description: ケベルのストリーミング宛先を使用して、オーディエンスをケベルの UserDB API およびセグメント管理 API に直接アクティブ化し、意思決定時のリアルタイムターゲティングをサポートします。
source-git-commit: d820485fd81efd08d8626f8476338558c4585c20
workflow-type: tm+mt
source-wordcount: '1037'
ht-degree: 9%

---


# [!DNL Kevel] 接続 {#kevel}

## 概要 {#overview}

[[!DNL Kevel]](https://www.kevel.com/) は、革新的なコマースリーダーがリテールメディアでローンチ、拡大、成功するのを支援する、AI 対応のテクノロジーとエキスパートガイダンスを提供します。 [!DNL Kevel] の Retail Media Cloud は、オンサイト広告とオフサイト広告のために、ターゲットを絞った、帰属可能なカスタマイズ可能な広告フォーマットを強化します。

Adobe Experience Platformの [!DNL Kevel] ストリーミング先を使用すると、お客様は、Adobe オーディエンスを [!DNL Kevel] の UserDB API および Segment Management API に直接アクティブ化して、広告の意思決定時のリアルタイムターゲティングをサポートできます。

>[!IMPORTANT]
> 
>[!DNL Kevel] の宛先またはそのドキュメントに関するご質問や更新をリクエストする場合は、[!DNL Kevel] チーム （[support@kevel.com](mailto:support@kevel.com)）にお問い合わせください。

## ユースケース {#use-cases}

小売メディアエクスペリエンス全体でリッチなファーストパーティ行動オーディエンスをアクティブ化して、より関連性の高い広告とより強力なパフォーマンスを提供できます。 Experience Platformでは、頻繁に買い物をするユーザーや商品に最近興味を持つユーザーなど、価値の高いインテントベースのオーディエンスを作成し、それらのメンバーシップをリアルタイムで [!DNL Kevel] に同期します。 [!DNL Kevel] は、これらのセグメントをすぐに広告決定に使用できるようになり、検索、参照、アプリエクスペリエンスをまたいで、スポンサー付き製品やその他の形式の正確なターゲティングを可能にします。 ユーザーが選定されるとすぐに、これらのシグナルに基づいて、より関連性の高いインプレッション数、ターゲット設定の改善、測定と ROAS の向上を推進できます。

## 前提条件 {#prerequisites}

[!DNL Kevel] の宛先の使用を準備するには、次の前提条件が満たされていることを確認します。

- **[!DNL Kevel]ネットワークがアクティブで** API アクセスが必要です。
- セグメントを作成し **[!DNL Kevel]UserDB レコードを更新する権限を持つ** API キーが必要です。
- [!DNL Kevel] 広告リクエスト中にサイトまたはアプリから送信される ID （例えば、ECID、GAID、IDFA、ロイヤルティ ID など）にマッピングされる ID 名前空間を、Experience Platformで設定する必要があります。
- Adobeのお客様は、リアルタイムの広告リクエスト中に使用される ID のみをマッピングする必要があります。各 ID は UserDB レコードになるからです。

## サポートされている ID {#supported-identities}

[!DNL Kevel] の宛先では、広告リクエストを [!DNL Kevel] に送信する際にアプリケーションが使用できる任意の ID のアクティベーションをサポートしています。 対応する UserDB レコードを生成するために、最大 3 つの ID 名前空間をマッピングできます。

[!DNL Kevel] は、次のExperience Platform ID 名前空間をサポートしています。

| ID 名前空間 | 説明 | 典型的な使用方法 |
|--------------------|---------------------------------|----------------------------------------------------------------|
| **ECID** | Experience Cloud ID | オンサイトのパーソナライゼーションとAdobe間の識別に使用します。 |
| **GAID** | GOOGLE ADVERTISING ID | Android アプリ/デバイスのトラフィックに使用されます。 |
| **IDFA** | APPLE ADVERTISING ID | iOS アプリ/デバイストラフィックに使用されます（ATT の同意に基づく）。 |
| **EXTERNAL_ID** | 外部 ID （カスタム識別子） | 独自の ID またはバックエンドで生成された ID を渡します。 |

{style="table-layout:auto"}

### カスタム ID 名前空間のサポート

Experience Platformの実装で定義されているとおり、[!DNL Kevel] の宛先 **カスタム名前空間も受け入れます**。

インタラクションがあります。

- **顧客固有の ID 名前空間** をマッピングできます（例：`loyalty_id`、`gigya_id`、または ID サービスで定義したカスタム ID）。
- これらの名前空間は、グローバル名前空間と同じ方法で `kevel_user_key1`、`kevel_user_key2` または `kevel_user_key3` に割り当てることができます。
- [!DNL Kevel] は、マッピングされた各 ID のインスタンスごとに 1 つの **UserDB レコード** を生成し、システムが送信する各識別子に対して、広告決定時にリアルタイムのマッチングを可能にします。

### ID マッピング動作

- Experience Platformの ID 名前空間は **最大 3 つ**&#x200B;[!DNL Kevel]ID スロットにマッピングできます。
- アクティブ化され [!DNL Kevel] プロファイルごとに、**マッピングされた ID ごとのインスタンスごとに 1 つの UserDB レコード** を受け取ります。
- 不要な UserDB ストレージを避けるために、実際に広告リクエストで送信した ID のみを [!DNL Kevel] にマッピングする必要があります。

![&#x200B; ケベル宛先のマッピング例 &#x200B;](/help/destinations/assets/catalog/advertising/kevel-destination-mappings.png)

## サポートされるオーディエンス {#supported-audiences}

| オーディエンスオリジン | サポートあり | 説明 |
|-----------------------|-----------|---------------------------------------------------------- |
| セグメント化サービス | ○ | セグメント化エンジンによって評価されたAdobe プロファイルオーディエンス。 |
| カスタムアップロード | × | 現時点ではサポートされていません。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

| 項目 | タイプ | メモ |
|------|------|-------|
| 書き出しタイプ | **セグメントの書き出し** | プロファイル [!DNL Kevel] オーディエンスに適合するか、オーディエンスから離脱するたびに更新を受け取ります。 |
| 書き出し頻度 | **ストリーミング** | 更新は、Destination SDK ストリーミングフレームワークを使用してリアルタイムで送信されます。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

標準のExperience Platform[&#x200B; 宛先を接続 &#x200B;](../../ui/connect-destination.md) ワークフローに従います。

>[!IMPORTANT]
> 
>**宛先の表示** および **宛先の管理** 権限が必要です。

### 宛先に対する認証 {#authenticate}

[!DNL Kevel] に接続する際には、次のフィールドを指定します。

- **ベアラートークン** - [!DNL Kevel] API キー。

![&#x200B; ケベル宛先の認証オプション &#x200B;](/help/destinations/assets/catalog/advertising/kevel-destination-authentication.png)

### 宛先の詳細の入力 {#destination-details}

認証後、以下を設定します。

- **名前** – この宛先インスタンスを識別するラベル。
- **説明** – この宛先インスタンスを説明するオプションのテキスト。
- **[!DNL Kevel]Network ID** — [!DNL Kevel] のネットワーク識別子。

![&#x200B; ケベル宛先の宛先詳細 &#x200B;](/help/destinations/assets/catalog/advertising/kevel-destination-details.png)

## この宛先に対してセグメントをアクティブ化 {#activate}

オーディエンスを [!DNL Kevel] に送信するには、のワークフローに従います\
[&#x200B; ストリーミングセグメント書き出し宛先に対するプロファイルとセグメントのアクティブ化 &#x200B;](/help/destinations/ui/activate-segment-streaming-destinations.md)。

### オーディエンスの無効化 {#deactivate}

Experience Platformでオーディエンスが [!DNL Kevel] の宛先からディアクティベートまたは削除されると、Experience Platformはそのオーディエンスのプロファイルの選定に関する更新をさらに送信しなくなります。 [!DNL Kevel] で作成された既存のセグメントは引き続き使用でき、自動的には削除されません。

[!DNL Kevel] セグメントが現在アクティブなキャンペーンで使用されている場合は、削除を防 [!DNL Kevel] てライブ配信が中断されないようにします。 この場合、Experience Platformでアクティベートを解除すると、次のような結果になります。

- Experience Platform データフローが停止します
- [!DNL Kevel] セグメントは引き続き存在し、手動で削除するか、キャンペーンを更新するまで、キャンペーンに関連付けられたままの場合があります

[!DNL Kevel] でのターゲティングを完全に停止するには、[!DNL Kevel] のキャンペーン管理システムでアクティブなキャンペーンからセグメントが削除されていることを確認します。

### 属性と ID のマッピング {#map}

[!DNL Kevel] には次が必要です。

- **ID 名前空間** - [!DNL Kevel] 個の ID スロットにマッピングされる最大 3 つの ID 名前空間。
- **セグメントメンバーシップ** – 手動のマッピングは必要ありません。Experience Platformは、セグメントメンバーシップの識別情報とエイリアスを自動的に渡します。

アクティベーション時に、[!DNL Kevel] 用に設定した ID 名前空間を選択します。 各 ID は、独自の UserDB 更新呼び出しを生成します。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

プロファイルがオーディエンスに適合するか、オーディエンスから離脱すると、Experience Platformはストリーミング更新を [!DNL Kevel] に送信します。

### [!DNL Kevel] UserDB が受信したサンプルペイロード

```json
PUT /udb/{networkId}/segments?userKey=ECID-12345
{
  "segments": [1723, 3344, 9988]
}
```

| パラメーター | 説明 |
|-----------|-------------|
| **userKey** | マッピングされたAdobe ID から派生。 |
| **セグメント** | 現在プロファイルが実現 [!DNL Kevel] れているAdobe オーディエンスに対応するセグメント ID のセット。 |

{style="table-layout:auto"}

### 書き出し時に使用されるサンプル Experience Platform プロファイル {#sample-profile}

[!DNL Kevel] の宛先に対してオーディエンスをアクティブ化する場合、Experience Platformは **セグメントの選定** および **顧客によってマッピングされた ID** の両方を含むプロファイルフラグメントを [!DNL Kevel] の ID スロットに送信します。

次に、書き出されたプロファイルの例を示します。

- `kevel_user_key1`、`kevel_user_key2` および `kevel_user_key3` にマッピングされた複数の ID 名前空間
- `ups` 名前空間の単一のアクティブ化されたセグメント

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

#### このプロファイル [!DNL Kevel] 解釈方法

[!DNL Kevel] の宛先設定を使用すると、マッピングされた各 ID が個別の UserDB レコードを生成し、[!DNL Kevel] が次の情報を受け取ります。

- `ECID-fN1zo` 用の 1 つの更新
- `ECID-9Xr2p` 用の 1 つの更新
- `GAID-4oic4` 用の 1 つの更新
- `IDFA-nB5fU` 用の 1 つの更新

これにより、使用可能な任意の ID を使用して、同じ人物を意思決定時に認識でき、各 ID には同一のセグメントメンバーシップセットが割り当てられます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

- [[!DNL Kevel] UserDB リファレンス &#x200B;](https://dev.kevel.com/reference/userdb)
- [[!DNL Kevel]  ユーザーセグメントのターゲティング &#x200B;](https://dev.kevel.com/docs/segment-targeting)

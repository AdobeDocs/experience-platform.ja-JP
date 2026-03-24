---
keywords: ストリーミング、Qualtricsの宛先
title: Qualtrics Automations
description: エクスペリエンスと運用上の顧客データを同期し、大規模なパーソナライゼーションを実現。 Adobe Adobe Experience Platformにおいて、複数のデータソースから収集したデータをQualtrics Experience iDのインプットとして利用することで、顧客をより深く理解し、顧客の意図、感情、エクスペリエンスの要因を把握する際に、ターゲットを絞ったアウトリーチを通じてギャップを埋めることができます。
last-substantial-update: 2023-10-25T00:00:00Z
exl-id: 3289ed4c-8542-4e22-a574-e49cc6527a24
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1259'
ht-degree: 23%

---

# Qualtrics Automations

## 概要 {#overview}

エクスペリエンスと運用上の顧客データを同期し、大規模なパーソナライゼーションを実現。

[!DNL Adobe Experience Platform]の複数の運用データ ソースの集約をQualtrics Experience iDの入力として使用して、顧客をより深く理解し、意図、感情、エクスペリエンスの要因を理解する際に、ターゲットを絞ったアウトリーチを行ってギャップを埋めることができます。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、Qualtrics チームによって作成および管理されます。 お問い合わせやアップデートのリクエストについては、[&#x200B; カスタマーサクセスハブ &#x200B;](https://support-portal.qualtrics.com/)にログインして直接お問い合わせください。

## ユースケース {#use-cases}

*Qualtrics Automations*&#x200B;の宛先を使用する方法とタイミングをより深く理解できるように、この宛先を使用して[!DNL Adobe Experience Platform]のお客様が解決できるユースケースの例を次に示します。

### ユースケース #1 {#use-case-1}

**シナリオ**：企業は、web サイトやモバイルアプリなど、さまざまなデジタル接点で顧客満足度を測定したいと考えています。 [!DNL Adobe Experience Platform]を使用して、購入の完了や特定のweb ページへの訪問など、利用者とのやり取りに基づいてQualtrics調査をトリガーします。

**成果**: リアルタイムのフィードバックを収集することで、データ主導の改善を顧客体験にもたらし、顧客満足度とロイヤルティを向上させることができます。

### ユースケース #2 {#use-case-2}

**シナリオ**：組織は、従業員オンボーディングプロセスの強化を目指しています。 彼らは[!DNL Adobe Experience Platform]を利用して、Qualtrics アンケートを通じて新入社員からフィードバックを収集します。 アンケートは、事前に定義されたオンボーディング期間が経過すると自動的にトリガーされます。

**成果**：継続的なフィードバックにより、組織はオンボーディングプロセスを適応および改善することができ、その結果、新入社員のエンゲージメントと生産性が向上します。

## 前提条件 {#prerequisites}

[!DNL Adobe Experience Platform]でQualtrics宛先を設定する前に、次の前提条件が満たされていることを確認してください。

* お客様はQualtrics アカウントを保有しています。
* Qualtricsから必要なAPI トークンを取得しました。

### API トークンの取得 {#obtaining-api-token}

以下は、QualtricsからAPI トークンを取得するために必要な手順です。

1. Qualtrics アカウントにログインします。
2. **アカウント設定**&#x200B;に移動します。
3. **Qualtrics ID**&#x200B;を選択します。
4. このページで、**API** セクションを探します。このセクションには、**トークン** フィールドが含まれています。 これはAPI トークンで、宛先の設定中に必要になります。

## サポートされている ID {#supported-identities}

*Qualtrics Automations*&#x200B;は、次の表に示すIDのアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | プレーンテキストのメールアドレス | Qualtricsでは、プレーンテキストのメールアドレスのみがサポートされています。 |
| external_id | カスタムユーザーID | ソース IDがカスタム名前空間である場合は、このターゲット IDを選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Segment export]** | *Qualtrics Automations*&#x200B;の宛先で使用されている識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

認証の一環として、**ユーザー名**&#x200B;と&#x200B;**パスワード**&#x200B;を入力する必要があります。 ユーザー名はQualtricsのユーザー名で、パスワードはQualtrics アカウントのAPI トークンです。 API トークンを取得するには、上記の「**前提条件**」セクションの指示に従います。

![認証](/help/destinations/assets/catalog/survey/qualtrics/authentication.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL URL]**: [&#x200B; ワークフローをQualtrics](https://www.qualtrics.com/support/survey-platform/actions-module/json-events/#About)でトリガーする[JSON イベント &#x200B;](https://www.qualtrics.com/support/survey-platform/actions-module/setting-up-actions/#About)に見つかったURL。 例については、以下のスクリーンショットを参照してください。

![URL](/help/destinations/assets/catalog/survey/qualtrics/json-event-url.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に対してオーディエンスをアクティブ化する手順については、[&#x200B; ストリーミング宛先に対するオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

この宛先にはオープンなスキーマがあるため、任意のプロパティをQualtricsに送信できます。

#### 属性のマップ {#map-attributes}

マッピングに属性を追加するには、新しいマッピングの追加時に&#x200B;**カスタム属性**&#x200B;を選択します。 属性には任意の名前を入力できます。 Qualtricsでは、属性名の&#x200B;*camelCase*&#x200B;命名規則を推奨しています（例については、以下のスクリーンショットを参照）。

![&#x200B; カスタム属性](/help/destinations/assets/catalog/survey/qualtrics/custom-attribute.png)

可能な属性マッピングの例については、以下のスクリーンショットを参照してください。

![&#x200B; マッピングの例](/help/destinations/assets/catalog/survey/qualtrics/example-mappings.png)

#### ID のマッピング {#map-identities}

この宛先のID名前空間を選択する必要があります。 ターゲットフィールドマッピングに使用できるソースフィールドは次の2つです。

| ソースフィールド | ターゲットフィールド |
|--------------------|-----------------------|
| IdentityMap：電子メール | ID：電子メール |
| IdentityMap: ECID | ID: external_id |

例については、以下のスクリーンショットを参照してください。

![ID名前空間](/help/destinations/assets/catalog/survey/qualtrics/identity-namespace.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

前述したように、この宛先はオープンスキーマを使用しているため、すべてのプロパティをQualtricsに送信できます。 ただし、Qualtricsに送信されるデータは、次の構造に従います。

```json
{
  "person": {
    "name": {
      "firstName": "Dave"
    }
  },
  "mobilePhone": {
    "number": "0123456789"
  },
  "identityMap": {
    "Email": [
      {
        "id": "Email-2Sf6C"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "046456e3b-18e1-48a6-9bda-d68547861283": {
        "lastQualificationTime": "2023-09-05T10:43:55.602687Z",
        "status": "realized"
      },
      "007844dd1-9e5d-4531-a4ee-05470doe759dd": {
        "lastQualificationTime": "2023-09-05T10:43:55.602689Z",
        "status": "realized"
      }
    }
  }
}
```

Qualtricsにデータが取り込まれていることを確認するには、**JSON イベント**&#x200B;を含むワークフローに移動し、そこから&#x200B;**実行履歴**&#x200B;に移動して、ワークフローの実行を確認します。 各ワークフローのステータスは、**成功**&#x200B;または&#x200B;**失敗**&#x200B;です。 特定の実行を選択すると、その実行に関する詳細な情報が表示され、問題が発生した場合にトラブルシューティングを行うことができます。

**実行履歴**&#x200B;に実行が表示されない場合は、ワークフローがまだトリガーされていないため、問題が発生している可能性があります。 ワークフローが有効になっており、**の宛先の** URL[!DNL Adobe Experience Platform]が正しいことを確認してください。 ワークフローは瞬時に実行されないため、完了するまでしばらく待たなければならない場合があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

---
keywords: ストリーミング、Qualtrics 宛先
title: Qualtrics の自動化
description: エクスペリエンスと運用顧客データを同期して、大規模にパーソナライゼーションを解き放ちます。 Qualtrics Experience iD では、Adobe Experience Platformの複数のオペレーショナルデータソースの集計を入力として使用することで、顧客をより深く理解し、目的、感情、エクスペリエンスの要因を理解する際に、ターゲットを絞ったアウトリーチによってギャップを埋めることができます。
last-substantial-update: 2023-10-25T00:00:00Z
exl-id: 3289ed4c-8542-4e22-a574-e49cc6527a24
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1120'
ht-degree: 26%

---

# Qualtrics の自動化

## 概要 {#overview}

エクスペリエンスと運用顧客データを同期して、大規模にパーソナライゼーションを解き放ちます。

Qualtrics Experience iD では、Adobe Experience Platformの複数のオペレーショナルデータソースの集計を入力として使用することで、顧客をより深く理解し、目的、感情、エクスペリエンスの要因を理解する際に、ターゲットを絞ったアウトリーチによってギャップを埋めることができます。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、Qualtrics チームが作成および管理します。 お問い合わせや更新のリクエストについては、[ カスタマーサクセスハブ ](https://support-portal.qualtrics.com/) にログインして直接ご連絡ください。

## ユースケース {#use-cases}

*Qualtrics Automations* 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### のユースケース#1 {#use-case-1}

**シナリオ**：企業は、web サイトやモバイルアプリなど、様々なデジタルタッチポイントをまたいで顧客満足度を測定したいと考えています。 Adobe Experience Platformを使用して、購入の完了や特定の web ページへのアクセスなど、ユーザーとのインタラクションに基づく Qualtrics 調査をトリガーします。

**成果**：リアルタイムのフィードバックを収集することで、企業は顧客体験をデータに基づいて改善し、満足度とロイヤルティの向上につなげることができます。

### のユースケース#2 {#use-case-2}

**シナリオ**：組織は、従業員のオンボーディングプロセスの強化を目指します。 Adobe Experience Platformを活用し、Qualtrics 調査を通じて新規採用者の声を収集しています。 調査は、事前に定義されたオンボーディング期間の後に自動的にトリガーされます。

**結果**：継続的なフィードバックにより、組織はオンボーディングプロセスを適応させ、改善できるので、新入社員間のエンゲージメントと生産性が向上します。

## 前提条件

Adobe Experience Platformで Qualtrics の宛先を設定する前に、次の前提条件が満たされていることを確認してください。

* Qualtrics アカウントを持っています。
* Qualtrics から必要な API トークンを取得しています。

### API トークンの取得

Qualtrics から API トークンを取得するために必要な手順は次のとおりです。

1. Qualtrics アカウントにログインします。
2. **アカウント設定** に移動します。
3. 「**Qualtrics IDs**」を選択します。
4. このページでは、「**API**」セクションを探します。「**トークン**」フィールドが含まれています。 これは API トークンで、宛先の設定時に必要になります。

## サポートされている ID {#supported-identities}

*Qualtrics オートメーション* では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | プレーンテキストのメールアドレス | Qualtrics では、プレーンテキストのメールアドレスのみがサポートされています。 |
| external_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Segment export]** | *Qualtrics Automations* 宛先で使用される識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

認証の一環として、**ユーザー名** と **パスワード** を入力する必要があります。 ユーザー名は Qualtrics のユーザー名で、パスワードは Qualtrics アカウントの API トークンです。 API トークンを取得するには、上記の **前提条件** 節の手順に従います。

![認証](/help/destinations/assets/catalog/survey/qualtrics/authentication.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL URL]**: [ ワークフローをトリガーする ](https://www.qualtrics.com/support/survey-platform/actions-module/json-events/#About)JSON イベント [ 内で Qualtrics で見つかった URL](https://www.qualtrics.com/support/survey-platform/actions-module/setting-up-actions/#About). 例については、以下のスクリーンショットを参照してください。

![URL](/help/destinations/assets/catalog/survey/qualtrics/json-event-url.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

この宛先には開かれたスキーマがあるので、任意のプロパティを Qualtrics に送信できます。

#### 属性のマッピング

マッピングに属性を追加するには、新しいマッピングを追加する際に **カスタム属性** を選択するだけです。 属性には任意の名前を入力できます。 Qualtrics は、属性名に *camelCase* の命名規則を推奨しています（例については、以下のスクリーンショットを参照してください）。

![ カスタム属性 ](/help/destinations/assets/catalog/survey/qualtrics/custom-attribute.png)

可能な属性マッピングの例については、以下のスクリーンショットを参照してください。

![ マッピングの例 ](/help/destinations/assets/catalog/survey/qualtrics/example-mappings.png)

#### ID のマッピング

この宛先の ID 名前空間を選択する必要があります。 ターゲットフィールドにマッピングできるソースフィールドは次の 2 つです。

| ソースフィールド | ターゲットフィールド |
|--------------------|-----------------------|
| IdentityMap：メール | ID : メール |
| IdentityMap:ECID | ID : external_id |

例については、以下のスクリーンショットを参照してください。

![ID 名前空間 ](/help/destinations/assets/catalog/survey/qualtrics/identity-namespace.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

前述のように、この宛先ではオープンスキーマを使用しているので、プロパティは Qualtrics に送信できます。 ただし、Qualtrics に送信されるデータは、次の構造に従います。

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

データが Qualtrics に取り込まれたことを確認するには、**JSON イベント** を含むワークフローに移動し、そこから **実行履歴** に移動して、ワークフローの実行を確認します。 各ワークフローのステータスは **成功** または **失敗** です。 特定の実行を選択すると、その実行に関する詳細情報が表示され、問題が発生した場合にトラブルシューティングをおこなうことができます。

**実行履歴** に実行が表示されない場合は、ワークフローがまだトリガーされておらず、問題がある可能性があることを示しています。 ワークフローが有効であること、およびAdobe Experience Platformのコピー先の **URL** が正しいことを確認します。 ワークフローの実行は即座には行われないので、完了するまで少し待つ必要がある場合があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

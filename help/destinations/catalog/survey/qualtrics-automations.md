---
keywords: ストリーミング、Qualtrics の宛先
title: Qualtrics 自動化
description: エクスペリエンスと運用上の顧客データを同期して、パーソナライゼーションの規模を拡大/縮小できます。 Adobe Experience Platformの複数の運用データソースの集計を Qualtrics Experience ID の入力として使用すると、顧客をより深く理解し、目的、感情、エクスペリエンスの推進要因を把握する際に、ターゲットを絞ったアウトリーチを可能にしてギャップを埋めることができます。
last-substantial-update: 2023-10-25T00:00:00Z
source-git-commit: 57d3e136902201f9ba9bd2f427ebe0f876900671
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 32%

---


# Qualtrics 自動化

## 概要 {#overview}

エクスペリエンスと運用上の顧客データを同期して、パーソナライゼーションの規模を拡大/縮小できます。

Adobe Experience Platformの複数の運用データソースの集計を Qualtrics Experience ID の入力として使用すると、顧客をより深く理解し、目的、感情、エクスペリエンスの推進要因を把握する際に、ターゲットを絞ったアウトリーチを可能にしてギャップを埋めることができます。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、Qualtrics チームが作成および管理します。 お問い合わせや更新のご依頼については、 [カスタマーサクセスハブ](https://support-portal.qualtrics.com/).

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 *Qualtrics 自動化* の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1 {#use-case-1}

**シナリオ**：会社は、Web サイトやモバイルアプリなど、様々なデジタルタッチポイントをまたいで顧客満足度を測定したいと考えています。 ユーザーのインタラクション（購入の完了、特定の Web ページへの訪問など）に基づいてAdobe Experience Platformを使用して Qualtrics 調査をトリガー化します。

**結果**：リアルタイムでフィードバックを収集することで、顧客体験に対するデータ駆動型の改善を実現し、満足度とロイヤルティを向上させます。

### 使用例#2 {#use-case-2}

**シナリオ**：組織は、従業員のオンボーディングプロセスの強化を目指しています。 Adobe Experience Platformを利用して、Qualtrics 調査を通じて新しい採用者からフィードバックを収集します。 事前定義されたオンボーディング期間の後、調査が自動的にトリガーされます。

**結果**：継続的なフィードバックにより、組織はオンボーディングプロセスを適応および改善でき、新規従業員間のエンゲージメントと生産性が向上します。

## 前提条件

Adobe Experience Platformで Qualtrics の宛先を設定する前に、次の前提条件が満たされていることを確認してください。

* Qualtrics アカウントがある。
* 必要な API トークンを Qualtrics から取得している。

### API トークンの取得

Qualtrics から API トークンを取得するために必要な手順を以下に示します。

1. Qualtrics アカウントにログインします。
2. に移動します。 **アカウント設定**.
3. 選択 **Qualtrics ID**.
4. このページで、 **API** セクションの **トークン** フィールドに入力します。 これは API トークンで、宛先の設定時に必要になります。

## サポートされている ID {#supported-identities}

*Qualtrics 自動化* では、以下の表で説明する id のアクティブ化をサポートしています。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | プレーンテキストの E メールアドレス | Qualtrics では、プレーンテキストの E メールアドレスのみがサポートされます。 |
| external_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | セグメント（オーディエンス）のすべてのメンバーを、 *Qualtrics 自動化* 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

認証の一環として、 **ユーザー名** および **パスワード**. ユーザー名は Qualtrics のユーザー名、パスワードは Qualtrics アカウントの API トークンです。 API トークンを取得するには、 **前提条件** 」の節を参照してください。

![認証](/help/destinations/assets/catalog/survey/qualtrics/authentication.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL URL]**: [JSON イベント](https://www.qualtrics.com/support/survey-platform/actions-module/json-events/#About) トリガーする [Qualtrics でのワークフロー](https://www.qualtrics.com/support/survey-platform/actions-module/setting-up-actions/#About). 例については、以下のスクリーンショットを参照してください。

![URL](/help/destinations/assets/catalog/survey/qualtrics/json-event-url.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

この宛先にはオープンスキーマがあるので、プロパティを Qualtrics に送信できます。

#### マップ属性

マッピングに属性を追加するには、 **カスタム属性** 新しいマッピングを追加する際に使用します。 属性には任意の名前を入力できます。 Qualtrics は、 *camelCase* 属性名の命名規則（例は下のスクリーンショットを参照）。

![カスタム属性](/help/destinations/assets/catalog/survey/qualtrics/custom-attribute.png)

以下のスクリーンショットに、使用可能な属性マッピングの例を示します。

![マッピングの例](/help/destinations/assets/catalog/survey/qualtrics/example-mappings.png)

#### ID のマッピング

この宛先の ID 名前空間の選択は必須です。 ターゲットフィールドマッピングに使用できるソースフィールドは次の 2 つです。

| ソースフィールド | ターゲットフィールド |
|--------------------|-----------------------|
| ID マップ：電子メール | ID : E メール |
| ID マップ： ECID | ID: external_id |

例については、以下のスクリーンショットを参照してください。

![ID 名前空間](/help/destinations/assets/catalog/survey/qualtrics/identity-namespace.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

前述のように、この宛先では開いたスキーマが使用されるので、プロパティは Qualtrics に送信される場合があります。 それにもかかわらず、Qualtrics に送信されるデータは次の構造に従います。

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

データが Qualtrics に取り込まれたことを確認するには、 **JSON イベント**、そこから、に移動します。 **実行履歴** ここで、ワークフローの実行を確認します。 各ワークフローのステータスは次のいずれかです： **成功** または **失敗**. 特定の実行を選択すると、その実行に関する詳細情報が表示され、問題が発生した場合のトラブルシューティングをおこなうことができます。

で実行が表示されない場合 **実行履歴**&#x200B;とは、まだワークフローがトリガーされておらず、問題が発生している可能性があることを示しています。 ワークフローが有効になっていること、および **URL** の宛先がAdobe Experience Platformで正しい場合。 ワークフローの実行はすぐには完了しないので、完了するまでしばらく待つ必要が生じる場合があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

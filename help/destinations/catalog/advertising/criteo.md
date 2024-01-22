---
keywords: 広告；条件；
title: 条件の接続
description: Criteo は、信頼できる効果的な広告を提供し、オープンインターネットを介してすべての消費者に豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI を備えた Criteo は、ショッピングジャーニー全体の各タッチポイントをパーソナライズし、適切な広告を適切なタイミングで顧客に届けます。
exl-id: e6f394b2-ab82-47bb-8521-1cf9d01a203b
source-git-commit: c3ef732ee82f6c0d56e89e421da0efc4fbea2c17
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 25%

---

# （ベータ版）Criteo 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Criteo によって作成および管理されます。 現在はベータ版の製品であり、機能は変更される可能性があります。お問い合わせや更新のご依頼については、Criteo に直接お問い合わせください [ここ](mailto:criteoTechnicalPartnerships@criteo.com).

Criteo は、信頼できる効果的な広告を提供し、オープンインターネットを介してすべての消費者に豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI を備えた Criteo は、ショッピングジャーニー全体の各タッチポイントをパーソナライズし、適切な広告を適切なタイミングで顧客に届けます。

## 前提条件 {#prerequisites}

* の管理者ユーザーアカウントが必要です [Criteo 管理センター](https://marketing.criteo.com).
* Criteo 広告主 ID が必要です（この ID を持っていない場合は、Criteo の連絡先にお問い合わせください）。
* 次を提供する必要があります： [!DNL GUM caller ID]を使用する場合は、 [!DNL GUM ID] を識別子として使用します。

## 制限事項 {#limitations}

* 条件は次のみを受け入れます： [!DNL SHA-256] — ハッシュ化されたテキスト形式の E メール ( 変換後： [!DNL SHA-256] （送信前）。 PII（個人の名前や電話番号など、個人を特定できる情報）は送信しないでください。
* Criteo は、クライアントから提供される識別子を少なくとも 1 つ必要とします。 優先順位付け [!DNL GUM ID] を、ハッシュ化された電子メールを介した識別子として使用することで、一致率が向上します。

![前提条件](../../assets/catalog/advertising/criteo/prerequisites.png)

## サポートされている ID {#supported-identities}

Criteo では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --- | --- | --- |
| `email_sha256` | SHA-256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA-256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 [!UICONTROL 変換を適用] オプションを使用し、アクティベーション時に Platform が自動的にデータをハッシュ化する必要があります。 |
| `gum_id` | Criteo [!DNL GUM] cookie 識別子 | [!DNL GUM IDs] 顧客がユーザー識別システムと Criteo のユーザー ID([!DNL UID]) をクリックします。 識別子のタイプが `gum_id`、追加のパラメーター、 [!DNL GUM Caller ID]、も含める必要があります。 該当するについては、Criteo アカウントチームにお問い合わせください [!DNL GUM Caller ID] またはこの詳細を取得する [!DNL GUM ID] 同期（必要に応じて） |

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
| --- | --- | --- |
| 書き出しタイプ | オーディエンスの書き出し | [!DNL Criteo] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | ストリーミング | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](../../destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

## ユースケース {#use-cases}

を使用する方法をより深く理解できるように、 [!DNL Criteo] の宛先として、Adobe Experience Platformのお客様が達成できる目標の一部を次に示します。 [!DNL Criteo]:

### 使用例 1 ：トラフィックの取得

関連する製品オファーと柔軟なクリエイティブでビジネスを紹介します。 インテリジェントな製品レコメンデーションを使用すると、広告に、訪問やエンゲージメントをトリガーにする可能性が最も高い製品が自動的に表示されます。 柔軟なターゲティングにより、Criteo のコマースデータセット、または独自の見込み客リストやAdobeCDP セグメントからオーディエンスを構築できます。

### 使用例 2 :Web サイトのコンバージョンを増やす

訪問者が Web サイトを離れたときに、特別サービスや関連性の高いオファーを次にどこに行くかで示すことでコンバージョンを増やすリターゲティング広告で何が欠けているかを通知します。 AdobeCDP オーディエンスを接続して、既存の顧客に再び関わらせたり、最も常連の買い物客と同様にターゲット消費者にしたりします。

## Criteo に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### Criteo への認証

接続の手順は次のとおりです。

1. Adobe Experience Platformにログインし、Criteo の宛先に接続します。

   ![ログイン](../../assets/catalog/advertising/criteo/connect-destination.png)

1. 接続を認証するために、Criteo にリダイレクトされます。 最初に Criteo の資格情報を使用してログインする必要が生じる場合があります。

   ![条件ログイン](../../assets/catalog/advertising/criteo/log-in-1.png)

   ![条件ログイン](../../assets/catalog/advertising/criteo/log-in-2.png)

   ![条件ログイン](../../assets/catalog/advertising/criteo/log-in-3.png)


### 接続パラメーター {#connection-parameters}

宛先の認証後、次の接続パラメーターを入力してください。

![接続パラメータ](../../assets/catalog/advertising/criteo/connection-parameters.png)

| フィールド | 説明 | 必須 |
| --- | --- | --- |
| 名前 | 将来この宛先を認識するのに役立つ名前です。 ここで選択する名前は、 [!DNL Audience] Criteo Management Center での名前。後の段階では変更できません。 | ○ |
| 説明 | 今後この宛先を識別するのに役立つ説明。 | × |
| 広告主 ID | 組織の Criteo 広告主 ID。 この情報を入手するには、Criteo のアカウントマネージャーにお問い合わせください。 | ○ |
| Criteo [!DNL GUM caller ID] | [!DNL GUM Caller ID] 組織内で使用できます。 該当するについては、Criteo アカウントチームにお問い合わせください [!DNL GUM Caller ID] またはこの詳細を取得する [!DNL GUM] 同期（必要に応じて） | はい、いつでも [!DNL GUM ID] は識別子として指定されます。 |

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate-segments}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

エクスポートされたオーディエンスは、 [条件管理センター](https://marketing.criteo.com/audience-manager/dashboard).

ユーザープロファイルを追加する要求本文で、 [!DNL Criteo] 接続は次のようになります。

```json
{
  "data": {
    "type": "ContactlistWithUserAttributesAmendment",
    "attributes": {
      "operation": "add",
      "identifierType": "gum",
      "gumCallerId": "123",
      "identifiers": [
        {
          "identifier": "456",
          "attributes": [
            { "key": "ctoid_GumCaller", "value": "123" },
            { "key": "ctoid_Gum", "value": "456" },
            {
              "key": "ctoid_HashedEmail",
              "value": "98833030dc03751f2b2c1a0017078975fdae951aa6908668b3ec422040f2d4be"
            }
          ]
        }
      ]
    }
  }
}
```

で受け取ったユーザープロファイルを削除するリクエスト本文 [!DNL Criteo] 接続は次のようになります。

```json
{
  "data": {
    "type": "ContactlistWithUserAttributesAmendment",
    "attributes": {
      "operation": "remove",
      "identifierType": "gum",
      "gumCallerId": "123",
      "identifiers": [
        {
          "identifier": "456",
          "attributes": [
            { "key": "ctoid_GumCaller", "value": "123" },
            { "key": "ctoid_Gum", "value": "456" },
            {
              "key": "ctoid_HashedEmail",
              "value": "98833030dc03751f2b2c1a0017078975fdae951aa6908668b3ec422040f2d4be"
            }
          ]
        }
      ]
    }
  }
}
```

## データの使用とガバナンス {#data-usage}

すべてのAdobe Experience Platformの宛先は、データの処理時にデータ使用ポリシーに準拠しています。 Adobe Experience Platformによるデータガバナンスの強制方法について詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

## その他のリソース

* [Criteo ヘルプセンター](https://help.criteo.com/kb/en)
* [Criteo 開発者ポータル](https://developers.criteo.com)

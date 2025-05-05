---
keywords: 広告；criteo;
title: Criteo 接続
description: Criteo は、信頼性の高い効果的な広告を強化し、オープンインターネットを通じてすべての消費者により豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI により、Criteo はショッピングジャーニー全体の各タッチポイントが適切な広告で適切なタイミングで顧客に届くようにパーソナライズされるようにします。
exl-id: e6f394b2-ab82-47bb-8521-1cf9d01a203b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 24%

---

# Criteo 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Criteo によって作成および管理されます。 お問い合わせや更新のリクエストについては、Criteo に直接お問い合わせください [ こちら ](mailto:criteoTechnicalPartnerships@criteo.com)。

Criteo は、信頼性の高い効果的な広告を強化し、オープンインターネットを通じてすべての消費者により豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI により、Criteo はショッピングジャーニー全体の各タッチポイントが適切な広告で適切なタイミングで顧客に届くようにパーソナライズされるようにします。

## 前提条件 {#prerequisites}

* [Criteo 管理センター ](https://marketing.criteo.com) の管理者ユーザーアカウントが必要です。
* Criteo 広告主 ID が必要です（この ID をお持ちでない場合は、Criteo 担当者にお問い合わせください）。
* 識別子として使用する場合は、[!DNL GUM caller ID] を指定する必要 [!DNL GUM ID] あります。

## 制限事項 {#limitations}

* Criteo は、[!DNL SHA-256] ハッシュ化されたプレーンテキストのメール（送信前に [!DNL SHA-256] に変換される）のみを受け付けます。 PII （個人を特定できる情報、例えば個人の名前や電話番号）を送信しないでください。
* Criteo は、クライアントが提供する識別子を少なくとも 1 つ必要とします。 マッチング率の向上に貢献するため、ハッシュ化されたメールよりも識別子としての [!DNL GUM ID] が優先されます。

![前提条件](../../assets/catalog/advertising/criteo/prerequisites.png)

## サポートされている ID {#supported-identities}

Criteo では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --- | --- | --- |
| `email_sha256` | SHA-256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platformでは、プレーンテキストと SHA-256 でハッシュ化されたメールアドレスの両方がサポートされています。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「[!UICONTROL &#x200B; 変換を適用 &#x200B;]」オプションをオンにして、アクティブ化時にExperience Platformがデータを自動的にハッシュ化するように設定します。 |
| `gum_id` | Criteo [!DNL GUM] cookie 識別子 | クライアント [!DNL GUM IDs] ユーザー識別システムと Criteo のユーザー識別（[!DNL UID]）の間の通信を維持することを可能にします。 識別子タイプが `gum_id` の場合、追加のパラメーター [!DNL GUM Caller ID] も含める必要があります。 適切な [!DNL GUM Caller ID] については、Criteo アカウントチームにお問い合わせいただくか、必要に応じてこの [!DNL GUM ID] 同期に関する詳細情報をご確認ください。 |

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
| --- | --- | --- |
| 書き出しタイプ | オーディエンスのエクスポート | [!DNL Criteo] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | ストリーミング | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](../../destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

## ユースケース {#use-cases}

[!DNL Criteo] の宛先の使用方法をより深く理解するために、Adobe Experience Platformのお客様が [!DNL Criteo] で達成できる目標を次に示します。

### ユースケース 1：トラフィックの取得

関連する製品オファーと柔軟なクリエイティブでビジネスを紹介します。 インテリジェントな商品レコメンデーションを使用すると、トリガーの訪問やエンゲージメントの可能性が最も高い商品が自動的に広告に表示されます。 柔軟なターゲティングにより、Criteo のコマースデータセット、または独自の見込み客リストやAdobe CDP セグメントからオーディエンスを構築できます。

### ユースケース 2 :Web サイトのコンバージョンを増やす

訪問者が web サイトを離れると、リターゲティング広告で欠落しているものを思い出させ、次にどこへ行っても、特別なお得な情報や非常に関連性の高いオファーを表示してコンバージョンを高めます。 Adobe CDP オーディエンスを結び付けて、既存の顧客を再エンゲージしたり、最も常連客に近い顧客をターゲットに設定したりします。

## Criteo への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### Criteo への認証

接続の手順は次のとおりです。

1. Adobe Experience Platformにログインし、Criteo の宛先に接続します。

   ![ ログイン ](../../assets/catalog/advertising/criteo/connect-destination.png)

1. 接続を認証するために、Criteo にリダイレクトされます。 最初に Criteo 資格情報を使用してログインする必要がある場合があります。

   ![Criteo ログイン ](../../assets/catalog/advertising/criteo/log-in-1.png)

   ![Criteo ログイン ](../../assets/catalog/advertising/criteo/log-in-2.png)

   ![Criteo ログイン ](../../assets/catalog/advertising/criteo/log-in-3.png)


### 接続パラメーター {#connection-parameters}

宛先への認証が完了したら、以下の接続パラメーターを入力してください。

![接続パラメータ](../../assets/catalog/advertising/criteo/connection-parameters.png)

| フィールド | 説明 | 必須 |
| --- | --- | --- |
| 名前 | 今後この宛先を認識するのに役立つ名前。 ここで選択した名前は、Criteo 管理センターの [!DNL Audience] 名となり、後で変更することはできません。 | ○ |
| 説明 | 今後この宛先を識別するのに役立つ説明。 | × |
| 広告主 ID | 組織の Criteo 広告主 ID。 この情報を入手するには、Criteo 担当営業にお問い合わせください。 | ○ |
| Criteo [!DNL GUM caller ID] | 組織の [!DNL GUM Caller ID]。 適切な [!DNL GUM Caller ID] については、Criteo アカウントチームにお問い合わせいただくか、必要に応じてこの [!DNL GUM] 同期に関する詳細情報をご確認ください。 | はい、識別子と [!DNL GUM ID] て指定された場合は常に実行されます |

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate-segments}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## 書き出したデータ {#exported-data}

書き出されたオーディエンスは、[Criteo 管理センター ](https://marketing.criteo.com/audience-manager/dashboard) で確認できます。

[!DNL Criteo] 接続で受け取ったユーザープロファイルを追加するリクエスト本文は、次のようになります。

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

[!DNL Criteo] 接続が受信したユーザープロファイルを削除するリクエスト本文は、次のようになります。

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

Adobe Experience Platformのすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 Adobe Experience Platformによるデータガバナンスの実施方法について詳しくは、[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja) を参照してください。

## その他のリソース

* [Criteo ヘルプセンター ](https://help.criteo.com/kb/en)
* [Criteo デベロッパーポータル ](https://developers.criteo.com)

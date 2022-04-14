---
keywords: '広告；criteo '
title: 条件の接続
description: Criteo は、信頼できる効果的な広告を提供し、オープンインターネットを介してすべての消費者に豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI を備えた Criteo は、ショッピングジャーニー全体の各タッチポイントをパーソナライズし、適切な広告を適切なタイミングで顧客に届けます。
source-git-commit: a3263a322012a48f37cee6137054c7fcf3cdb8a2
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 9%

---


# （ベータ版）Criteo 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>このドキュメントページは Criteo が作成しました。 これは現在ベータ版製品です。 お問い合わせや更新のご依頼については、Criteo に直接お問い合わせください [ここ](mailto:criteoTechnicalPartnerships@criteo.com).

Criteo は、信頼できる効果的な広告を提供し、オープンインターネットを介してすべての消費者に豊かなエクスペリエンスを提供します。 世界最大のコマースデータセットとクラス最高の AI を備えた Criteo は、ショッピングジャーニー全体の各タッチポイントをパーソナライズし、適切な広告を適切なタイミングで顧客に届けます。

## 前提条件 {#prerequisites}

* の管理者ユーザーアカウントが必要です [Criteo 管理センター](https://marketing.criteo.com).
* Criteo 広告主 ID が必要です（この ID を持っていない場合は、Criteo の連絡先にお問い合わせください）。
* Criteo は SHA-256 とプレーンテキストの E メールのみを受け取ります（送信前に SHA-256 に変換できます）。 PII（個人の名前や電話番号など、個人を特定できる情報）は送信しないでください。

![前提条件](../../assets/catalog/advertising/criteo/prerequisites.png)

## サポートされる ID {#supported-identities}

Criteo では、以下の表で説明する ID のアクティブ化をサポートしています。 詳細情報： [id](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started).

| ターゲット ID | 説明 | 注意点 |
| --- | --- | --- |
| `email_sha256` | SHA-256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA-256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 [!UICONTROL 変換を適用] オプションを使用し、アクティベーション時に Platform が自動的にデータをハッシュ化する必要があります。 |

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
| --- | --- | --- |
| 書き出しタイプ | セグメントエクスポート | セグメント（オーディエンス）のすべてのメンバーを、 [!DNL Criteo] 宛先。 |
| 書き出し頻度 | ストリーミング | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](../../destination-types.md#streaming-destinations). |

## ユースケース {#use-cases}

を使用する方法をより深く理解できるように、 [!DNL Criteo] の宛先として、Adobe Experience Platformのお客様が達成できる目標を以下に示します。 [!DNL Criteo]:

### 使用例 1 :トラフィックの取得

関連する製品オファーと柔軟なクリエイティブでビジネスを紹介します。 インテリジェントな製品レコメンデーションを使用すると、広告に、訪問やエンゲージメントをトリガーにする可能性が最も高い製品が自動的に表示されます。 柔軟なターゲティングにより、Criteo のコマースデータセット、または独自の見込み客リストやAdobeCDP セグメントからオーディエンスを構築できます。

### 使用例 2 :Web サイトのコンバージョンの増加

訪問者が Web サイトを離れたときに、特別サービスや関連性の高いオファーを次にどこに行くかで示すことでコンバージョンを増やすリターゲティング広告で何が欠けているかを通知します。 AdobeCDP セグメントを接続して、既存の顧客を再び関与させたり、最も常連の買い物客と同様にターゲット消費者を設定したりします。

## Criteo に接続 {#connect}

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### Criteo への認証

接続の手順は次のとおりです。

1. Adobe Experience Platformにログインし、Criteo の宛先に接続します。

   ![にログインする](../../assets/catalog/advertising/criteo/connect-destination.png)

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
| 説明 | 将来この宛先を識別するのに役立つ説明。 | × |
| API バージョン | Criteo API バージョン。 [ プレビュー ] を選択してください。 | ○ |
| 広告主 ID | 組織の Criteo 広告主 ID。 この情報を入手するには、Criteo のアカウントマネージャーにお問い合わせください。 | ○ |

## この宛先に対してセグメントをアクティブ化 {#activate-segments}

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## 書き出したデータ {#exported-data}

書き出されたセグメントは、 [条件管理センター](https://marketing.criteo.com/audience-manager/dashboard).

が受け取ったリクエスト本文 [!DNL Criteo] 接続は次のようになります。

```json
{ 
  "data": { 
    "type": "ContactlistWithUserAttributesAmendment", 
    "attributes": { 
      "operation": "add", 
      "identifierType": "sha256email", 
      "identifiers": [ 
        { 
          "identifier": "1c8494bbc4968277345133cca6ba257b9b3431b8a84833a99613cf075a62a16d", 
          "attributes": [{ "key": "customValue", "value": "1" }] 
        } 
      ] 
    } 
  } 
} 
```

## データの使用とガバナンス {#data-usage}

すべてのAdobe Experience Platformの宛先は、データの処理時にデータ使用ポリシーに準拠しています。 Adobe Experience Platformによるデータガバナンスの強制方法について詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=en).

## その他のリソース

* [Criteo ヘルプセンター](https://help.criteo.com/kb/en)
* [Criteo 開発者ポータル](https://developers.criteo.com/marketing-solutions/v2022.04/reference/modifyaudienceuserswithattributes)

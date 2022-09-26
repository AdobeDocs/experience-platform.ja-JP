---
title: Verizon MediaYahoo DataX 接続
description: DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。
exl-id: 7d02671d-8650-407d-9c9f-fad7da3156bc
source-git-commit: f61771ec11b8bd2d19e303b39e57e82da8f11ead
workflow-type: tm+mt
source-wordcount: '789'
ht-degree: 19%

---

# [!DNL Verizon Media/Yahoo DataX] 接続

## 概要 {#overview}

[!DNL DataX] は集計です [!DNL Verizon Media/Yahoo] を可能にする様々なコンポーネントをホストするインフラストラクチャ [!DNL Verizon Media/Yahoo] 外部パートナーと安全で自動化された拡張性の高い方法でデータを交換する。

>[!IMPORTANT]
>
>このドキュメントページは次のユーザーが作成しました： [!DNL Verizon Media/Yahoo]&#39;s [!DNL DataX] チーム。 お問い合わせや更新のご依頼は、直接 [dataops@verizonmedia.com](mailto:dataops@verizonmedia.com)

## 前提条件 {#prerequisites}

**MDM ID**

これは、 [!DNL Yahoo DataX] この宛先へのデータエクスポートを設定する際には、必須のフィールドです。 この ID がわからない場合は、 [!DNL Yahoo DataX] アカウントマネージャー。

**分類メタデータ**

分類リソースは、ベースを介して拡張を定義します [!DNL DataX] メタデータの構造

```
{

  >>(Base DataX Metadata)<<

        "extensions": { "action":
        {string}, "incrementalData":
        {
                "taxonomyId": {string}
                },
                "links": [{
                "rel": "https://datax.yahooapis.com/rels/fullTaxonomy", "title": "Full
                Taxonomy post processing",
                "href": {string}
                ]
        }
}
```

詳細を表示 [分類メタデータ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/) 内 [!DNL DataX] 開発者向けドキュメント。

## レート制限とガードレール {#rate-limits-guardrails}

>[!IMPORTANT]
>
>100 を超えるセグメントをアクティブ化する場合 [!DNL Verizon Media/Yahoo DataX]の場合は、宛先からレート制限エラーが発生する可能性があります。 次に対してセグメントをアクティブ化する場合 [!DNL Yahoo/DataX] 宛先の場合は、1 つの activation データフローで 100 個未満のセグメントをアクティブ化することをお勧めします。 さらにセグメントをアクティブ化する必要がある場合は、同じアカウントで新しい宛先を作成します。

[!DNL DataX] は、 [DataX ドキュメント](https://developer.verizonmedia.com/datax/guide/rate-limits/).


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429 リクエストが多すぎます | レート制限を 1 時間に超えました **( 制限：100)** | プロバイダーあたり 1 時間で許可されるリクエストの数。 |

{style=&quot;table-layout:auto&quot;}

## サポートされる ID {#supported-identities}

[!DNL Verizon Media] では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。 |
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | Verizon Media の宛先で使用される識別子（E メール、GAID、IDFA）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## ユースケース {#use-cases}

[!DNL DataX] API は、 [!DNL Verizon Media] (VMG) は、VMG のほぼリアルタイム API を使用して、新しいセグメントをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

![Platform UI での Yahoo DataX 宛先カード](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL MDM ID]**:これは、 [!DNL Yahoo DataX] この宛先へのデータエクスポートを設定する際には、必須のフィールドです。 この ID がわからない場合は、 [!DNL Yahoo DataX] アカウントマネージャー。  MDM ID を使用すると、データは特定の排他的ユーザー（広告主のファーストパーティデータなど）とのみ使用するように制限できます。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-segment-streaming-destinations.md) 宛先に対してオーディエンスセグメントをアクティブ化する手順については、を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

## その他のリソース {#additional-resources}

詳しくは、 [!DNL Yahoo/Verizon Media] [ドキュメント [!DNL DataX]](https://developer.verizonmedia.com/datax/guide/).

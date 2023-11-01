---
title: Verizon MediaYahoo DataX 接続
description: DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。
exl-id: 7d02671d-8650-407d-9c9f-fad7da3156bc
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 48%

---

# [!DNL Verizon Media/Yahoo DataX] 接続

## 概要 {#overview}

[!DNL DataX] は集計です [!DNL Verizon Media/Yahoo] を可能にする様々なコンポーネントをホストするインフラストラクチャ [!DNL Verizon Media/Yahoo] 外部パートナーと安全で自動化された拡張性の高い方法でデータを交換する。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、によって作成および管理されます [!DNL Verizon Media/Yahoo]&#39;s [!DNL DataX] チーム。 お問い合わせや更新のご依頼は、直接お問い合わせください。 [dataops@verizonmedia.com](mailto:dataops@verizonmedia.com)

## 前提条件 {#prerequisites}

**MDM ID**

これは、 [!DNL Yahoo DataX] この宛先へのデータエクスポートを設定する際には、必須のフィールドです。 この ID がわからない場合は、 [!DNL Yahoo DataX] アカウントマネージャー。

**分類メタデータ**

分類リソースは、ベースを介して拡張を定義します。 [!DNL DataX] メタデータの構造

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

詳細を表示： [分類メタデータ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/) （内） [!DNL DataX] 開発者向けドキュメント。

## レート制限とガードレール {#rate-limits-guardrails}

>[!IMPORTANT]
>
>100 を超えるオーディエンスをアクティブ化する場合 [!DNL Verizon Media/Yahoo DataX]の場合は、宛先からレート制限エラーが発生する可能性があります。 この宛先に対してオーディエンスをアクティブ化する場合は、1 つのアクティベーションデータフローで 100 未満のオーディエンスをアクティブ化するようにします。 さらにセグメントをアクティブ化する必要がある場合は、同じアカウントで新しい宛先を作成します。

[!DNL DataX] は、分類およびオーディエンスの投稿に対するクォータ制限に従って、 [DataX ドキュメント](https://developer.verizonmedia.com/datax/guide/rate-limits/).


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429 リクエストが多すぎます | レート制限を 1 時間に超えました **（上限：100）** | プロバイダーあたり 1 時間で許可されるリクエストの数。 |

{style="table-layout:auto"}

## サポートされている ID {#supported-identities}

[!DNL Verizon Media] では、以下の表で説明する ID のアクティベーションをサポートしています。[ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Verizon Media の宛先で使用される識別子（E メール、GAID、IDFA）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL DataX] API は、 [!DNL Verizon Media] (VMG) は、VMG のほぼリアルタイム API を使用して、新しいオーディエンスをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

![Platform UI での Yahoo DataX 宛先カード](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL MDM ID]**：これは、 [!DNL Yahoo DataX] この宛先へのデータエクスポートを設定する際には、必須のフィールドです。 この ID がわからない場合は、 [!DNL Yahoo DataX] アカウントマネージャー。  MDM ID を使用すると、データは特定の排他的ユーザー（広告主のファーストパーティデータなど）とのみ使用するように制限できます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

読み取り [宛先へのプロファイルとオーディエンスのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、 [!DNL Yahoo/Verizon Media] [に関するドキュメント [!DNL DataX]](https://developer.verizonmedia.com/datax/guide/).

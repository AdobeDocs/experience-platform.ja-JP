---
title: Verizon MediaYahoo DataX 接続
description: DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。
exl-id: 7d02671d-8650-407d-9c9f-fad7da3156bc
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 43%

---

# [!DNL Verizon Media/Yahoo DataX] 接続

## 概要 {#overview}

[!DNL DataX] は、安全で自動化されたスケーラブルな方法で外部パートナーとデータを交換で [!DNL Verizon Media/Yahoo] る様々なコンポーネントをホストする集約 [!DNL Verizon Media/Yahoo] インフラストラクチャです。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、[!DNL Verizon Media/Yahoo] の [!DNL DataX] チームが作成および管理します。 お問い合わせや更新のリクエストについては、[dataops@verizonmedia.comまで直接ご連絡ください ](mailto:dataops@verizonmedia.com)

## 前提条件 {#prerequisites}

**MDM ID**

これは [!DNL Yahoo DataX] の一意の ID で、この宛先へのデータ書き出しを設定するための必須フィールドです。 この ID が不明な場合は、[!DNL Yahoo DataX] アカウントマネージャーにお問い合わせください。

**分類メタデータ**

分類リソースは、ベース [!DNL DataX] メタデータ構造に対する拡張を定義します

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

[ 分類メタデータ ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/) の詳細については、[!DNL DataX] 開発者向けドキュメントを参照してください。

## レート制限とガードレール {#rate-limits-guardrails}

>[!IMPORTANT]
>
>[!DNL Verizon Media/Yahoo DataX] に 100 を超えるオーディエンスをアクティブ化すると、宛先からレート制限エラーを受け取る場合があります。 この宛先に対してオーディエンスをアクティブ化する場合、1 つのアクティベーションデータフローでアクティブ化するオーディエンスが 100 個未満になるようにしてください。 さらにセグメントをアクティブ化する必要がある場合は、同じアカウントに新しい宛先を作成します。

[DataX ドキュメント ](https://developer.verizonmedia.com/datax/guide/rate-limits/) に概説されている分類やオーディエンスの投稿の割り当て量の制限に対して、[!DNL DataX] れはレート制限されています。


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429 リクエストが多すぎます | 1 時間あたりのレート制限を超えています **（制限：100）** | プロバイダーごとに 1 時間に許可されるリクエストの数。 |

{style="table-layout:auto"}

## サポートされている ID {#supported-identities}

[!DNL Verizon Media] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | Verizon Media の宛先で使用される識別子（メール、GAID、IDFA）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL Verizon Media] （VMG）のメールアドレスをキーに特定のオーディエンスグループをターゲットにしたい広告主は、[!DNL DataX] API を使用して、新しいオーディエンスをすばやく作成し、VMG のほぼリアルタイムの API を使用して目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

![Experience Platform UI の Yahoo DataX 宛先カード ](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL MDM ID]**：これは [!DNL Yahoo DataX] の一意の ID で、この宛先へのデータの書き出しを設定するための必須フィールドです。 この ID が不明な場合は、[!DNL Yahoo DataX] アカウントマネージャーにお問い合わせください。  MDM ID を使用すると、特定の排他的なユーザーのセット（広告主向けのファーストパーティデータなど）でのみデータを使用するように制限できます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

宛先にオーディエンスをアクティブ化する手順については、[ 宛先へのプロファイルとオーディエンスのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[!DNL Yahoo/Verizon Media][ のドキュメント  [!DNL DataX]](https://developer.verizonmedia.com/datax/guide/) を参照してください。

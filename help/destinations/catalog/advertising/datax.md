---
title: Verizon MediaYahoo DataX 接続
description: DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。
exl-id: 7d02671d-8650-407d-9c9f-fad7da3156bc
source-git-commit: b1945d42b82b549985d848071762fa6ee2451368
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 17%

---

# Verizon Media/Yahoo DataX 接続

## 概要 {#overview}

DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。

>[!IMPORTANT]
>
>このドキュメントページは、Verizon Media/Yahoo の DataX チームが作成しました。 お問い合わせや更新のご依頼は、直接 [dataops@verizonmedia.com](mailto:dataops@verizonmedia.com)

## 前提条件 {#prerequisites}

**MDM ID**

これは Yahoo DataX での一意の識別子で、この宛先へのデータエクスポートを設定するための必須フィールドです。 この ID が不明な場合は、Yahoo Data X アカウントマネージャーにお問い合わせください。

**レート制限**

DataX は、 [DataX ドキュメント](https://developer.verizonmedia.com/datax/guide/rate-limits/).


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429 リクエストが多すぎます | レート制限を 1 時間に超えました **( 制限：100)** | プロバイダーあたり 1 時間で許可されるリクエストの数。 |

{style=&quot;table-layout:auto&quot;}

**分類メタデータ**

分類リソースは、基本 DataX メタデータ構造に対する拡張を定義します

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

詳細を表示 [分類メタデータ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/) （ DataX 開発者向けドキュメント）。

## サポートされる ID {#supported-identities}

Verizon Media では、以下の表で説明する ID のアクティブ化をサポートしています。 詳細情報： [id](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started).

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

Verizon Media(VMG) の電子メールアドレスをキーにした特定のオーディエンスグループをターゲットにしたい広告主は、VMG のほぼリアルタイム API を使用して、新しいセグメントをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

![Platform UI での Yahoo DataX 宛先カード](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL MDM ID]**:これは Yahoo DataX での一意の識別子で、この宛先へのデータエクスポートを設定するための必須フィールドです。 この ID が不明な場合は、Yahoo Data X アカウントマネージャーにお問い合わせください。  MDM ID を使用すると、データは特定の排他的ユーザー（広告主のファーストパーティデータなど）とのみ使用するように制限できます。

## この宛先へのセグメントのアクティブ化 {#activate}

読み取り [宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-segment-streaming-destinations.md) 宛先に対してオーディエンスセグメントをアクティブ化する手順については、を参照してください。

## データの使用とガバナンス {#data-usage-governance}

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

## その他のリソース {#additional-resources}

詳しくは、 Yahoo/Verizon のメディアを参照してください。 [DataX に関するドキュメント](https://developer.verizonmedia.com/datax/guide/).

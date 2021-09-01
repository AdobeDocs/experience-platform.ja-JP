---
title: Verizon MediaYahoo DataXとの統合
description: DataXは、Verizon Media/Yahooが外部パートナーと安全で自動化された拡張性の高い方法でデータを交換できる様々なコンポーネントをホストする、Verizon Media/Yahooの集約インフラストラクチャです。
source-git-commit: 07c8a7afaf6cd2af249ad52eabfbc5f8cd6689ae
workflow-type: tm+mt
source-wordcount: '585'
ht-degree: 6%

---


# Verizon Media/Yahoo DataX

## 概要 {#overview}

DataXは、Verizon Media/Yahooが外部パートナーと安全で自動化された拡張性の高い方法でデータを交換できる様々なコンポーネントをホストする、Verizon Media/Yahooの集約インフラストラクチャです。

>[!IMPORTANT]
>
>このドキュメントページは、Verizon Media/YahooのDataXチームによって作成されました。 お問い合わせや更新のご依頼は、[dataops@verizonmedia.com](mailto:dataops@verizonmedia.com)まで直接お問い合わせください。

## 前提条件 {#prerequisites}

**MDM ID**

これはYahoo DataXでの一意の識別子で、この宛先へのデータエクスポートを設定するための必須フィールドです。 このIDが不明な場合は、Yahoo Data Xアカウントマネージャーにお問い合わせください。

**レート制限**

DataXは、[DataXドキュメント](https://developer.verizonmedia.com/datax/guide/rate-limits/)で説明されている分類およびオーディエンスの投稿に対するクォータ制限に従って、レート制限されます。


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429リクエストが多すぎます | 1時間あたりのレート制限を超えました&#x200B;**(制限：100)** | プロバイダーあたり1時間で許可される要求の数。 |

{style=&quot;table-layout:auto&quot;}

**分類メタデータ**

分類リソースは、ベースDataXメタデータ構造を介して拡張を定義します

```
{

  >>(Base DataX Metadata)<<

        "extensions" : { "action" :
        {string}, "incrementalData" :
        {
                "taxonomyId": {string}
                },
                "links" : [{
                "rel"   : "https://datax.yahooapis.com/rels/fullTaxonomy", "title" : "Full
                Taxonomy post processing",
                "href": {string}
                ]
        }
}
```

詳しくは、DataX開発者向けドキュメントの[分類メタデータ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/)を参照してください。

## サポートされるID {#supported-identities}

Verizon Mediaでは、以下の表で説明するIDのアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started)の詳細を説明します。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストとSHA256ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に[!DNL Platform]でデータを自動的にハッシュ化します。 |
| GAID | Google広告ID | ソースIDがGAID名前空間の場合は、GAIDターゲットIDを選択します。 |
| IDFA | Apple の広告主 ID | ソースIDがIDFA名前空間の場合は、IDFAターゲットIDを選択します。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  - Verizon Mediaの宛先で使用される識別子（Eメール）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。

## ユースケース {#use-cases}

Verizon Media(VMG)の電子メールアドレスをキーにした特定のオーディエンスグループをターゲットにしたい広告主は、VMGのほぼリアルタイムAPIを使用して、新しいセグメントをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

![Platform UIでのYahoo DataX宛先カード](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL MDM ID]**:これはYahoo DataXでの一意の識別子で、この宛先へのデータエクスポートを設定するための必須フィールドです。このIDが不明な場合は、Yahoo Data Xアカウントマネージャーにお問い合わせください。  MDM IDを使用すると、特定の排他的ユーザー（広告主のファーストパーティデータなど）とのみ使用するようにデータを制限できます。

## この宛先へのセグメントのアクティブ化 {#activate}

宛先に対するオーディエンスセグメントをアクティブ化する方法については、[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、「[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html)」を参照してください。

## その他のリソース {#additional-resources}

詳しくは、Yahoo/Verizon Media [のDataX](https://developer.verizonmedia.com/datax/guide/)に関するドキュメントを参照してください。
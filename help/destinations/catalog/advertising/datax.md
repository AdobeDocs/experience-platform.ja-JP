---
title: Verizon MediaYahoo DataX 接続
description: DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。
exl-id: 7d02671d-8650-407d-9c9f-fad7da3156bc
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 17%

---

# Verizon Media/Yahoo DataX 接続

## 概要 {#overview}

DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。

>[!IMPORTANT]
>
>このドキュメントページは、Verizon Media/Yahoo の DataX チームが作成しました。 お問い合わせや更新のご依頼は、[dataops@verizonmedia.com](mailto:dataops@verizonmedia.com) まで直接お問い合わせください。

## 前提条件 {#prerequisites}

**MDM ID**

これは Yahoo DataX での一意の識別子で、この宛先へのデータエクスポートを設定するための必須フィールドです。 この ID が不明な場合は、Yahoo Data X アカウントマネージャーにお問い合わせください。

**レート制限**

DataX は、[DataX ドキュメント ](https://developer.verizonmedia.com/datax/guide/rate-limits/) で概要を説明している分類およびオーディエンスの投稿に対するクォータ制限に従って、レート制限されます。


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429 要求が多すぎます | 1 時間あたりのレート制限を超えました **( 制限：100)** | プロバイダーあたり 1 時間で許可される要求の数。 |

{style=&quot;table-layout:auto&quot;}

**分類メタデータ**

分類リソースは、ベース DataX メタデータ構造を介して拡張を定義します

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

[ 分類メタデータ ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/) について詳しくは、DataX 開発者向けドキュメントを参照してください。

## サポートされる ID {#supported-identities}

Verizon Media では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) の詳細をご覧ください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化された電子メールアドレス | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。[!DNL Platform] |
| GAID | Google 広告 ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメントのエクスポート**  - Verizon Media の宛先で使用される識別子（E メール）を使用して、セグメント（オーディエンス）のすべてのメンバーをエクスポートします。

## ユースケース {#use-cases}

Verizon Media(VMG) の電子メールアドレスをキーにした特定のオーディエンスグループをターゲットにしたい広告主は、VMG のほぼリアルタイム API を使用して、新しいセグメントをすばやく作成し、目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

![Platform UI での Yahoo DataX 宛先カード](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL MDM ID]**:これは Yahoo DataX での一意の識別子で、この宛先へのデータエクスポートを設定するための必須フィールドです。この ID が不明な場合は、Yahoo Data X アカウントマネージャーにお問い合わせください。  MDM ID を使用すると、データは特定の排他的ユーザー（広告主のファーストパーティデータなど）とのみ使用するように制限できます。

## この宛先へのセグメントのアクティブ化 {#activate}

宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ 宛先へのプロファイルとセグメントのアクティブ化 ](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、「[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)」を参照してください。

## その他のリソース {#additional-resources}

詳しくは、Yahoo/Verizon Media [ の DataX](https://developer.verizonmedia.com/datax/guide/) に関するドキュメントを参照してください。

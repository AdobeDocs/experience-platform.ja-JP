---
title: Pinterest Customer List の接続
description: 顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成します。
exl-id: e601f75f-0d40-4cd0-93ca-54d7439f1db7
source-git-commit: b1945d42b82b549985d848071762fa6ee2451368
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 7%

---

# [!DNL Pinterest Customer List] 接続

## 概要 {#overview}

顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成します。

>[!IMPORTANT]
>
>この宛先はPinterestチームが作成しました。 お問い合わせや更新のご依頼は、https://help.pinterest.com/en/contactから直接お問い合わせください。

## 前提条件 {#prerequisites}

* ユーザーは、オーディエンスの追加先の広告主アカウントにアクセスできるPinterestアカウントを使用して認証する必要があります。 広告主アカウントの共有に関する詳細は、 [ここ](https://help.pinterest.com/en/business/article/share-and-manage-access-to-your-ad-accounts). 特に、ユーザーには「オーディエンス」アクセスレベルが必要です。
* 顧客リストの ID 形式の詳細が見つかります [ここ](https://help.pinterest.com/en/business/article/audience-targeting).


## サポートされる ID {#supported-identities}

この [!DNL Pinterest Customer List] の宛先では、以下の表で説明する id のアクティブ化がサポートされます。 詳細情報： [id](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started).

内 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 宛先のアクティベーションワークフローで、目的の id を「ターゲット」フィールドにマッピングします。 *pinterest_audience*. ID は、Pinterestにデータを取り込む際に識別され、解決されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | を *GAID* ターゲット id フィールドへのソース id 名前空間 *pinterest_audience*. ID は、Pinterestにデータを取り込む際に識別され、解決されます。 |
| IDFA | [!DNL Apple ID for Advertisers] | を *IDFA* ターゲット id フィールドへのソース id 名前空間 *pinterest_audience*. ID は、Pinterestにデータを取り込む際に識別され、解決されます。 |
| EMAIL | 電子メールアドレス（クリアテキストまたは SHA256 アルゴリズムでハッシュ化） | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 <br> を *電子メール* または *Email_LC_SHA256* ターゲット id フィールドへのソース id 名前空間 *pinterest_audience*. |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | pinterestの顧客リストの宛先で使用されている識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Pinterest Customer List] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。


### ユースケース 1

顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成します。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).


### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL 広告主 ID]**:pinterest広告主 ID。

## この宛先へのセグメントのアクティブ化 {#activate}

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

## その他のリソース {#additional-resources}

詳しくは、 [Pinterest Help Center ページ](https://help.pinterest.com/en/business/article/audience-targeting) を参照してください。

---
title: Pinterest Customer Listの接続
description: 顧客リスト、サイトを訪問した人、またはPinterest上でコンテンツに対して既にインタラクションを起こしている人からオーディエンスを作成します。
source-git-commit: dc7e43a16923cb17a39a8ddb4ba114c0e9c0cc39
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 6%

---


# Pinterest Customer Listの接続

## 概要 {#overview}

顧客リスト、サイトを訪問した人、またはPinterest上でコンテンツに対して既にインタラクションを起こしている人からオーディエンスを作成します。

>[!IMPORTANT]
>
>この宛先はPinterestチームが作成しました。 お問い合わせや更新のご要望は、https://help.pinterest.com/en/contactまで直接お問い合わせください。

## 前提条件 {#prerequisites}

* ユーザーは、オーディエンスの追加先の広告主アカウントにアクセスできるPinterestアカウントを認証する必要があります。 広告主アカウントの共有に関する詳細は、[こちら](https://help.pinterest.com/en/business/article/share-and-manage-access-to-your-ad-accounts)を参照してください。 特に、ユーザーには「オーディエンス」アクセスレベルが必要です。
* 顧客リストのID形式の詳細は、[こちら](https://help.pinterest.com/en/business/article/audience-targeting)を参照してください。


## サポートされるID {#supported-identities}

pinterestの顧客リストの宛先では、以下の表で説明するIDのアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started)の詳細を説明します。

宛先のアクティベーションワークフローの[マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)で、目的のIDをターゲットフィールド&#x200B;*pinterest_audience*&#x200B;にマッピングします。 IDは、Pinterestにデータを取り込む際に識別され、解決されます。

| ターゲットID | 説明 | 注意点 |
|---|---|---|
| GAID | Google広告ID | *GAID*&#x200B;ソースID名前空間をターゲットIDフィールド&#x200B;*pinterest_audience*&#x200B;にマッピングします。 IDは、Pinterestにデータを取り込む際に識別され、解決されます。 |
| IDFA | Apple の広告主 ID | *IDFA*&#x200B;ソースID名前空間をターゲットIDフィールド&#x200B;*pinterest_audience*&#x200B;にマッピングします。 IDは、Pinterestにデータを取り込む際に識別され、解決されます。 |
| EMAIL | 電子メールアドレス（クリアテキストまたはSHA256アルゴリズムでハッシュ化） | プレーンテキストとSHA256ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 <br> Eメールア ** ドレス *Email_LC_SHA256* ソースID名前空間をターゲットIDフィールド *pinterest_audience*&#x200B;にマッピングします。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメントの書き出し**  - Pinterestの顧客リストの宛先で使用されている識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。

## ユースケース {#use-cases}

pinterest Customer Listの宛先をいつどのように使用すべきかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。


### ユースケース 1

顧客リスト、サイトを訪問した人、またはPinterest上でコンテンツに対して既にインタラクションを起こしている人からオーディエンスを作成します。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。



### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウントID]**:pinterest広告アカウントID。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

すべての[!DNL Adobe Experience Platform]宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform]によるデータガバナンスの強制方法について詳しくは、「[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html)」を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[Pinterestヘルプセンターのページ](https://help.pinterest.com/en/business/article/audience-targeting)を参照してください。
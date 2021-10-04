---
title: Pinterest Customer List の接続
description: 顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成する。
exl-id: e601f75f-0d40-4cd0-93ca-54d7439f1db7
source-git-commit: b8924fcced518757ae1f784728d0f5395719e8a0
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 6%

---

# [!DNL Pinterest Customer List] 接続

## 概要 {#overview}

顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成する。

>[!IMPORTANT]
>
>この宛先はPinterestチームが作成しました。 お問い合わせや更新のご依頼は、直接https://help.pinterest.com/en/contactまでお問い合わせください。

## 前提条件 {#prerequisites}

* ユーザーは、オーディエンスの追加先の広告主アカウントにアクセスできるPinterestアカウントを認証する必要があります。 広告主アカウントの共有について詳しくは、[](https://help.pinterest.com/en/business/article/share-and-manage-access-to-your-ad-accounts) を参照してください。 特に、ユーザーには「オーディエンス」アクセスレベルが必要です。
* 顧客リストの ID 形式の詳細は、[ こちら ](https://help.pinterest.com/en/business/article/audience-targeting) を参照してください。


## サポートされる ID {#supported-identities}

[!DNL Pinterest Customer List] の宛先は、次の表で説明する ID のアクティブ化をサポートします。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) の詳細をご覧ください。

宛先のアクティベーションワークフローの [ マッピング手順 ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) で、目的の ID をターゲットフィールド *pinterest_audience* にマッピングします。 ID は、Pinterestへのデータ取り込み時に識別され、解決されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | [!DNL Google Advertising ID] | *GAID* ソース ID 名前空間をターゲット ID フィールド *pinterest_audience* にマッピングします。 ID は、Pinterestへのデータ取り込み時に識別され、解決されます。 |
| IDFA | [!DNL Apple ID for Advertisers] | *IDFA* ソース ID 名前空間をターゲット ID フィールド *pinterest_audience* にマッピングします。 ID は、Pinterestへのデータ取り込み時に識別され、解決されます。 |
| EMAIL | 電子メールアドレス（クリアテキストまたは SHA256 アルゴリズムでハッシュ化） | プレーンテキストと SHA256 ハッシュ化された電子メールアドレスの両方が、Adobe Experience Platformでサポートされています。 <br> 電子メー ** ル *Email_LC_SHA256* ソース ID 名前空間をターゲット ID フィールド *pinterest_audience*&#x200B;にマッピングします。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメントのエクスポート**  - Pinterestの顧客リストの宛先で使用されている識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーをエクスポートします。

## ユースケース {#use-cases}

[!DNL Pinterest Customer List] の宛先をいつどのように使用するかをより深く理解できるように、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。


### ユースケース 1

顧客リスト、サイトを訪問した人、またはPinterestで既にコンテンツに対してインタラクションを起こした人からオーディエンスを作成する。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。



### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:pinterest広告アカウント ID。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメント書き出し先へのプロファイルとセグメントのアクティブ化 ](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、「[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)」を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[Pinterestヘルプセンターのページ ](https://help.pinterest.com/en/business/article/audience-targeting) を参照してください。

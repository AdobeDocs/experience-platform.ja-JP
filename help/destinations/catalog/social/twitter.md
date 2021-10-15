---
title: Twitter Custom Audiences 接続
description: twitterで既存のフォロワーと顧客をターゲット設定し、Adobe Experience Platform内で構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します
exl-id: fd244e58-cd94-4de7-81e4-c321eb673b65
source-git-commit: 74c5924406797c30d53f91782e37ac5e953aab94
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 18%

---

# [!DNL Twitter Custom Audiences] 接続

## 概要 {#overview}

Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform 内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。

## 前提条件 {#prerequisites}

[!DNL Twitter Custom Audiences] の宛先を設定する前に満たす必要がある、以下の Twitter の前提条件を確認してください。

1. [!DNL Twitter Ads] アカウントは広告を利用する資格を持っている必要があります。新しい [!DNL Twitter Ads] アカウントは、最初に作成してから 2 週間は広告を利用する資格がありません。
2. [!DNL Twitter Audience Manager] でアクセスを承認したTwitterユーザーアカウントで、*[!DNL Partner Audience Manager]* 権限が有効になっている必要があります。


## サポートされる ID {#supported-identities}

[!DNL Twitter Custom Audiences] では、以下の表で説明する ID のアクティブ化をサポートしています。[ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started) の詳細をご覧ください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| device_id | IDFA/AdID/Android ID | Google広告 ID(GAID) とApple ID for Advertisers(IDFA) は、Adobe Experience Platformでサポートされています。 宛先のアクティベーションワークフローの [ マッピング手順 ](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) で、ソーススキーマからこれらの名前空間や属性をマッピングしてください。 |
| 電子メール | ユーザーの電子メールアドレス | プレーンテキストの電子メールアドレスと SHA256 ハッシュの電子メールアドレスを、このフィールドにマッピングしてください。 ソースフィールドにハッシュ化されていない属性が含まれている場合、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時にデータを自動的にハッシュ化します。 [!DNL Platform]顧客の電子メールアドレスをAdobe Experience Platformにアップロードする前にハッシュ化する場合、これらの ID は、ソルトなしで SHA256 を使用してハッシュ化する必要があります。 |

{style=&quot;table-layout:auto&quot;}

## 書き出しタイプ {#export-type}

**セグメントのエクスポート**  - Twitter Custom Audiences の宛先で使用される識別子を使用して、セグメント（オーディエンス）のすべてのメンバーをエクスポートします。

## ユースケース {#use-cases}

[!DNL Twitter Custom Audiences] の宛先をいつどのように使用するかをより深く理解できるように、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### ユースケース 1

twitterで既存のフォロワーと顧客をターゲット設定し、Twitterで [!DNL List Custom Audiences] としてAdobe Experience Platform内に作成されたオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:この宛先が将来認識される名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:アカウ [!DNL Twitter Ads] ント ID。これは [!DNL Twitter Ads] 設定にあります。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対するオーディエンスセグメントをアクティブ化する手順については、[ ストリーミングセグメント書き出し先へのプロファイルとセグメントのアクティブ化 ](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] の宛先はすべて、データを処理する際のデータ使用ポリシーに準拠しています。 [!DNL Adobe Experience Platform] によるデータガバナンスの強制方法について詳しくは、「[ データガバナンスの概要 ](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)」を参照してください。

## その他のリソース {#additional-resources}

オーディエンスセグメントを Twitter にマッピングする場合は、以下のセグメント命名要件を満たしていることを確認してください。

1. 人間が読み取り可能なセグメントマッピング名を指定する。Experience Platformセグメントに使用したのと同じ名前を使用することをお勧めします。
2. 特殊文字 (+ &amp; , %) は使用しないでください。;@ / = ?$) がセグメントおよびセグメントマッピング名に含まれています。 Experience Platformセグメント名にこれらの文字が含まれている場合は、セグメントをTwitterの宛先にマッピングする前に、これらの文字を削除してください。

twitterの [!DNL List Custom Audiences] について詳しくは、[Twitterのドキュメント ](https://business.twitter.com/en/help/campaign-setup/campaign-targeting/custom-audiences/lists.html) を参照してください。

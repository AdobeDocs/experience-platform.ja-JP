---
title: Twitter Custom Audiences 接続
description: twitterで既存のフォロワーや顧客をターゲットに設定し、Adobe Experience Platform内で作成したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します
exl-id: fd244e58-cd94-4de7-81e4-c321eb673b65
source-git-commit: c5d2427635d90f3a9551e2a395d01d664005e8bc
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 17%

---

# [!DNL Twitter Custom Audiences] 接続

## 概要 {#overview}

Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform 内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。

## 前提条件 {#prerequisites}

[!DNL Twitter Custom Audiences] の宛先を設定する前に満たす必要がある、以下の Twitter の前提条件を確認してください。

1. [!DNL Twitter Ads] アカウントは広告を利用する資格を持っている必要があります。新規 [!DNL Twitter Ads] アカウントは、作成後最初の 2 週間は、広告を利用する資格がありません。
2. でアクセスを承認したTwitterユーザーアカウント [!DNL Twitter Audience Manager] は、 *[!DNL Partner Audience Manager]* 権限が有効です。


## サポートされる ID {#supported-identities}

[!DNL Twitter Custom Audiences] では、以下の表で説明する id のアクティブ化をサポートしています。 詳細情報： [id](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja#getting-started).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| device_id | IDFA/AdID/Android ID | Google Advertising ID(GAID) とApple Advertisers(IDFA) の両方がAdobe Experience Platformでサポートされています。 これらの名前空間や属性は、ソーススキーマから適切にマッピングしてください。 [マッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping) 宛先のアクティベーションワークフローの |
| 電子メール | ユーザーの電子メールアドレス | プレーンテキストの電子メールアドレスと SHA256 ハッシュ化された電子メールアドレスを、このフィールドにマッピングしてください。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、 **[!UICONTROL 変換を適用]** オプション [!DNL Platform] 有効化時に、データを自動的にハッシュ化します。 顧客の電子メールアドレスをAdobe Experience Platformにアップロードする前にハッシュ化する場合、これらの ID は、SHA256 を使用して、ソルトなしでハッシュ化する必要があります。 |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントエクスポート]** | twitter Custom Audiences の宛先で使用される識別子を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Twitter Custom Audiences] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### ユースケース 1

twitterで既存のフォロワーや顧客をターゲットに設定し、Adobe Experience Platform内で構築されたオーディエンスを [!DNL List Custom Audiences] twitter

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 名前]**:将来この宛先を認識するための名前。
* **[!UICONTROL 説明]**:今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL アカウント ID]**:お使いの [!DNL Twitter Ads] アカウント ID。 これは、 [!DNL Twitter Ads] 設定。

## この宛先へのセグメントのアクティブ化 {#activate}

読み取り [ストリーミングセグメントの書き出し先に対するプロファイルとセグメントのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

## データの使用とガバナンス {#data-usage-governance}

すべて [!DNL Adobe Experience Platform] の宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja).

## その他のリソース {#additional-resources}

オーディエンスセグメントを Twitter にマッピングする場合は、以下のセグメント命名要件を満たしていることを確認してください。

1. 人間が読み取り可能なセグメントマッピング名を指定する。Experience Platformセグメントに使用したのと同じ名前を使用することをお勧めします。
2. 特殊文字 (+ &amp; 、 % ) は使用しないでください。;@ / = ?$) がセグメントおよびセグメントマッピング名に含まれています。 Experience Platformセグメント名にこれらの文字が含まれている場合は、セグメントをTwitterの宛先にマッピングする前に、これらの文字を削除してください。

詳細情報： [!DNL List Custom Audiences] twitterの [Twitterドキュメント](https://business.twitter.com/en/help/campaign-setup/campaign-targeting/custom-audiences/lists.html).

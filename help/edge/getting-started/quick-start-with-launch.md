---
title: 起動のクイックスタート
seo-title: Adobe Experience Platform Web SDKの概要（起動）
description: Experience Platform Web SDK Extensionを使用してデータを収集するためのクイックスタートガイド
seo-description: Experience Platform Web SDK Extensionを使用してデータを収集するためのクイックスタートガイド
translation-type: tm+mt
source-git-commit: 0cc6e233646134be073d20e2acd1702d345ff35f

---


# （ベータ）前提条件

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDKは現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

現在、Adobe Experience Platform Web SDKは、XDMを使用したAdobe Experience Platformへのデータの送信のみをサポートしています。 次の前提条件を満たす必要があります。

- ファーストパ [ーティドメイン(CNAME)を有効にします](https://docs.adobe.com/content/help/en/core-services/interface/ec-cookies/cookies-first-party.html) 。 既にAnalyticsのCNAMEをお持ちの場合は、そのCNAMEを使用する必要があります。
- Adobe Experience Platformの権利を付与
- 訪問者IDサービスの最新バージョンを使用している

## プラットフォームの準備

Adobe Experience Platformにデータを送信するには、XDMスキーマと、そのスキーマを使用するデータセットを作成する必要があります。

- [次のミックスインを使用して](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/schema_editor_tutorial/schema_editor_tutorial.md) 、スキーマを作成します。
   - ExperienceEventの導入の詳細
   - ExperienceEvent環境の詳細
   - ExperienceEvent Webの詳細
- 作成したスキーマにAdobe Experience Platform Web SDKミックスインを追加します。
- [データの取得先](https://platform.adobe.com/dataset/overview) 、スキーマを持つデータセットを作成します。

## 設定IDのリクエスト

SDKを使用するには、設定IDが必要です。 設定IDを使用すると、データを適切な場所にルーティングできます。 設定IDは、コンサルタントから、またはClientCareから入手できます。 次の情報が必要です。

- **組織ID:** これは、次の手順で確認できま [す。](https://docs.adobe.com/content/help/en/core-services/interface/manage-users-and-products/organizations.html)
- **データセットID:** データセットをクリックすると、データセットのUIに表示されます。
- **スキーマID:** これは、スキーマ作成画面のURLで使用できます。
- **わかりやすい名前：** これは、この設定の今後のUIで使用されるわかりやすい名前です。

## 起動時のSDKのインストール

起動にログインし、拡張機能をインストール `AEP Web SDK` します。 SDKのインストールの一環として、拡張機能の設定を求めるプロンプトが表示されます。 上記で要求した設定IDを入力します。 拡張機能によって、組織IDが自動的に入力されます。

様々な設定オプションについて詳しくは、SDKの設定を [参照してください](../fundamentals/configuring-the-sdk.md)。

## イベントの送信

拡張機能のインストール後、AEP Web SDK拡張機能から「Send Beacon」アクションを追加して、イベントの送信を開始します。 「ビューの開始時に発生する」オプションをオンにしてページが読み込まれるたびに、少なくとも1つのイベントを送信することをお勧めします。

イベントの追跡方法について詳しくは、イベントの追跡を参照 [してください](../fundamentals/tracking-events.md)。

## データの送信

イベントと共に、前に作成したスキーマと一致するデータを送信できます。 例えば、コマースサイトを所有し、コマースミックスインをスキーマに追加した場合、誰かが製品を表示したときに次の構造が送信されます。

```javascript
{
  "commerce": {
    "productListAdds": {
        "value":1
    }
  },
  "productListItems":{
      "name":"Floppy Green Hat",
      "SKU":"HATFLP123",
      "product":"1234567",
      "quantity":2
  }
}
```

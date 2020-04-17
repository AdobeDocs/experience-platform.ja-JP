---
title: Launch のクイックスタート
seo-title: Adobe Experience Platform Web SDK：Launch のクイックスタート
description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
seo-description: Experience Platform Web SDK 拡張機能を使用してデータを収集するためのクイックスタートガイド
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46

---


# （ベータ版）前提条件

>[!IMPORTANT]
>
>Adobe Experience Platform Web SDK は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

現在、Adobe Experience Platform Web SDK は、XDM を使用した Adobe Experience Platform へのデータの送信のみをサポートしています。次の前提条件を満たす必要があります。

- [ファーストパーティドメイン（CNAME）](https://docs.adobe.com/content/help/ja-JP/core-services/interface/ec-cookies/cookies-first-party.html)が有効になっている。既に Analytics 用 CNAME をお持ちの場合は、その CNAME を使用する必要があります。
- Adobe Experience Platform　を使用する資格がある
- 訪問者 ID サービスの最新バージョンを使用している

## プラットフォームの準備

Adobe Experience Platform にデータを送信するには、XDM スキーマと、そのスキーマを使用するデータセットを作成する必要があります。

- 次の mixin を使用して[スキーマを作成](../../xdm/tutorials/create-schema-ui.md)します。
   - ExperienceEvent の実装の詳細
   - ExperienceEvent の環境の詳細
   - ExperienceEvent Web の詳細
- 作成したスキーマに Adobe Experience Platform Web SDK mixin を追加します。
- スキーマを使用してデータの宛先となる[データセットを作成](https://platform.adobe.com/dataset/overview)します。

## 設定 ID のリクエスト

SDK を使用するには、設定 ID が必要です。設定 ID を使用すると、データを適切な場所にルーティングできます。設定 ID は、コンサルタントまたは Client Care から入手できます。次の情報が必要になります。

- **組織 ID：**[こちら](https://docs.adobe.com/content/help/ja-JP/core-services/interface/manage-users-and-products/organizations.html)の手順を使用して確認できます。
- **データセット ID：**&#x200B;データセットをクリックすると、データセットの UI に表示されます。
- **スキーマ ID：**&#x200B;スキーマ作成画面の URL で使用できます。
- **わかりやすい名前：**&#x200B;今後の UI でこの設定に使用されるわかりやすい名前です。

## Launch での SDK のインストール

Launch にログインし、`AEP Web SDK` 拡張機能をインストールします。SDK のインストールの一環として、拡張機能を設定するよう求めるプロンプトが表示されます。上記で要求した設定 ID を入力します。拡張機能によって、組織の ID が自動的に入力されます。

様々な設定オプションについて詳しくは、[SDK の設定](../fundamentals/configuring-the-sdk.md)を参照してください。

## イベントの送信

拡張機能をインストールした後、AEP Web SDK 拡張機能から「ビーコンを送信」アクションを追加して、イベントの送信を開始します。「ビューの開始時に発生する」オプションをオンにして、ページが読み込まれるたびに 1 つ以上のイベントを送信することをお勧めします。

イベントの追跡方法について詳しくは、[イベントのトラッキング](../fundamentals/tracking-events.md)を参照してください。

## データの送信

イベントとともに、以前作成したスキーマと一致するデータを送信できます。例えば、コマースサイトを所有しており、コマース mixin をスキーマに追加した場合、誰かが製品を表示すると次の構造を送信することになります。

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

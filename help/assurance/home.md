---
title: Adobe Experience Platform Assurance の概要
description: Adobe Experience Platform Assurance を使用すると、モバイルアプリケーションでデータを収集したりエクスペリエンスを提供したりする方法を検査、配達確認、シミュレートおよび検証できます。
exl-id: e887f5f6-3db0-4521-be2d-20ef3d08e7d0
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 5%

---

# Adobe Experience Platform Assurance

Adobe Experience Platform Assurance は ](https://www.adobe.com/experience-cloud.html)0}Adobe Experience Cloud} の製品で、モバイルアプリでデータを収集したりエクスペリエンスを提供したりする方法を検査、配達確認、シミュレートおよび検証するのに役立ちます。[

>[!IMPORTANT]
>
> プロジェクト Griffon は現在 **Assurance** として知られています。
>
> Project Griffon は、Adobe Experience Cloudのお客様 **すべて** が Assurance として一般公開されるようになりました。 この移行について詳しくは、[ ユーザーアクセスガイド ](./user-access.md) を参照してください。

>[!INFO]
>
>Assurance パブリック API を利用できます。
>
>[Assurance API](https://developer.adobe.com/adobe-assurance-public-apis/) は、AdobeAssurance Mobile SDK を実装すると、ユーザーが web およびモバイルアプリをテストしデバッグできるようになる API のコレクションです。

## 一般公開（GA）

2022 年 10 月 15 日（PT）以降、Assurance はすべてのAdobe Experience Cloudで一般公開されます。

### 何が変わってるの？

10 月 15 日（PT） – Assurance へのアクセスは、Admin Consoleを通じて管理されます。 引き続きアクセスが中断されないようにするには、[ ユーザーアクセスガイド ](./user-access.md) を参照してください。

既存の Assurance の統合、セッションおよびイベントでは、その他の変更や中断は行われません。 Assurance には、[https://griffon.adobe.com](https://griffon.adobe.com) 経由で引き続きアクセスできます **または**[https://experience.adobe.com/assurance](https://experience.adobe.com/assurance) を使用する（およびブックマークする）ことができます。

## Assurance で何が実現しますか？

### クイックセットアップ

数行のコードから急いで開始してください。 モバイルアプリの場合、Assurance はAdobe Experience Platform Mobile SDK と連携して、アプリイベント、ロケーションシグナル、設定パラメーター、SDK ログ、デバイス情報などを検査、シミュレーション、検証するのに役立ちます。

### 手間のかからない接続

Assurance を使用すると、アプリを Platform に簡単かつ信頼性よく接続できます。 ネットワークプロキシ、[MiTM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)）などのネットワーク体操を使用する必要はありません。アプリを Assurance に接続するのは、QR コードをスキャンしたり、ボタンをタップしたりするのと同じくらい簡単です。

![](./images/index/no-hassle-connection.png)

### リアルタイムの検査、シミュレーション、検証

Assurance に接続すると、ライブストリーミングアプリのイベントとアクティビティを調べ、フィルタリングと検索でノイズを排除できます。 イベントには、モバイルアプリ実装の検証、デバッグ、トラブルシューティングに関する詳細が含まれています。 また、Assurance ではスクリーンショットの表示や、場所の信号のシミュレーションなどをリアルタイムで行えます。

![](./images/index/real-time-insepction.png)

### Adobe Experience Cloud との統合

クライアントサイドのデータとエクスペリエンスには、マーケターに焦点を当てたユーザーインターフェイスでユーザーがレポートルール、アクティビティおよびキャンペーンをどのように設定するかによってコンテキストが与えられます。 この 2 つの点を結び付けるのに役立つように、アドビは、Adobe Experience Platform、Adobe Analytics、Adobe Target、Places Service などのAdobe Experience Cloud ソリューションと統合しています。

![](./images/index/integration.png)

## 機能

### Adobe Experience Platform Mobile SDK イベント、ログなど

Assurance は、Adobe Experience Platform Mobile SDK で生成された生の SDK イベントを調べるのに役立ちます。 SDK で収集されたすべてのイベントは、検査に使用できます。 SDK イベントは、時間順に並べ替えられたリスト表示で読み込まれます。 各イベントには、詳細を提供する詳細ビューがあります。 SDK 設定、データ要素、共有状態、SDK 拡張機能のバージョンを参照するための追加のビューも提供されます。

### Adobe Analytics

Adobe Analytics/Analytics イベント ビューは、Adobe Analytics モバイル実装に関連するイベントを表示するビューに焦点を当てています。 リストビューには、ライフサイクルイベントまたはアクション/ステートイベント、Postで処理された「ステータス」および必要なイベントの詳細が、特別にフォーマットされたビューで表示されます。 Post処理済みステータスは、イベントに処理ルールが適用された後に、Adobe Analyticsがイベントをどのように処理したかを示します。

### ストリーミングメディア用 Adobe Analytics

Adobe Analytics/Media Analytics イベントビューには、オーディオおよびビデオ分析実装用のイベントが表示されます。 イベントの詳細ビューには、再生セッションごとに追跡される標準メタデータとカスタムメタデータが表示されます。 さらに、処理後のステータスと処理後のメディア分析データ（メディア視聴時間や合計バッファー時間など）を表示できます。

### 場所（位置情報サービス）

場所サービス ビューは、検証を容易にするためにユーザーの場所のエントリと離脱イベントを表示するオンデバイス表示です。 この便利なビューは、コンテキスト内デバッグ用にクライアント上で検査するための、場所固有のデータポイントを表示する便利なインターフェイスを提供します。

## Assurance は安全ですか？

Assurance では、次のセキュリティ対策を実施しています。

* Assurance と Assurance Web UI の両方に、接続用の安全な PIN ベースのハンドシェイクがあります。 ユーザーは明示的にハンドシェイクを作成する必要があり、これにより、「誤って」 Assurance 接続がエンドユーザーによって作成されるのを防ぐことができます。
* Assurance と、同じAdobe Experience Cloud組織 ID に属する Assurance web UI 間の接続のみがサポートされます。
* Adobe Experience Platform Mobile SDK のイベントは、HTTPS で転送されます。
* Assurance およびAdobe Experience Platform Mobile SDK は TLS 1.2 を使用します
* アシュランスセッションは、30 日後に削除されます。
* アシュランスセッションデータは、ストレージのベストプラクティスに従って、保存時に暗号化されます。

## はじめに

Assurance をセットアップするには、まずアプリケーションに Assurance 拡張機能をインストールする必要があります。 この方法について詳しくは、[Assurance 拡張機能の実装 ](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#add-the-aep-assurance-extension-to-your-app) に関するチュートリアルを参照してください。

アプリに Assurance を追加したら、デバイスに接続できる Assurance セッションを作成できます。 Assurance の使用方法については、[Assurance の使用に関するガイド ](./tutorials/using-assurance.md) を参照してください。

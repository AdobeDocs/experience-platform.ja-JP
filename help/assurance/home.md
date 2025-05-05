---
title: Adobe Experience Platform Assurance の概要
description: Adobe Experience Platform Assurance を使用すると、モバイルアプリケーションでデータを収集したりエクスペリエンスを提供したりする方法を検査、配達確認、シミュレートおよび検証できます。
exl-id: e887f5f6-3db0-4521-be2d-20ef3d08e7d0
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 6%

---

# Adobe Experience Platform Assurance

Adobe Experience Platform Assuranceは、[Adobe Experience Cloud](https://www.adobe.com/experience-cloud.html) の製品で、モバイルアプリでデータを収集したりエクスペリエンスを提供したりする方法を検査、配達確認、シミュレートおよび検証するのに役立ちます。

>[!IMPORTANT]
>
> プロジェクト Griffon は **Assurance** になりました。
>
> Project Griffon が、Adobe Experience Cloudの **すべて** のお客様がAssuranceとして一般公開されました。 この移行について詳しくは、[ ユーザーアクセスガイド ](./user-access.md) を参照してください。

>[!INFO]
>
>Assuranceのパブリック API を利用できます。
>
>[Assurance API](https://developer.adobe.com/adobe-assurance-public-apis/) は、Adobe Assurance Mobile SDKを実装すると、web およびモバイルアプリのテストとデバッグをおこなえるようになる API のコレクションです。

## 一般公開（GA）

2022 年 10 月 15 日（PT）以降、AssuranceはすべてのAdobe Experience Cloudで一般公開されます。

### 何が変わってるの？

10 月 15 日（PT） - Assuranceへのアクセスは、Admin Consoleを通じて管理されます。 引き続きアクセスが中断されないようにするには、[ ユーザーアクセスガイド ](./user-access.md) を参照してください。

既存のAssuranceの統合、セッションおよびイベントでは、それ以外の変更や中断は行われません。 Assuranceには、[https://griffon.adobe.com](https://griffon.adobe.com) から引き続きアクセスできます **または**&#x200B;[https://experience.adobe.com/assurance](https://experience.adobe.com/assurance) を使用する（およびブックマークする）ことができます。

## Assuranceは何に役立つか

### クイックセットアップ

数行のコードから急いで開始してください。 モバイルアプリの場合、AssuranceはAdobe Experience Platform Mobile SDKと連携して、アプリイベント、場所シグナル、設定パラメーター、SDK ログ、デバイス情報などを検査、シミュレーションおよび検証するのを支援します。

### 手間のかからない接続

Assuranceを使用すると、アプリをExperience Platformと簡単かつ信頼性の高い方法で接続できます。 ネットワークプロキシ、[MiTM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)）などのネットワーク体操を使用する必要はありません。アプリをAssuranceに接続するのは、QR コードをスキャンしたり、ボタンをタップしたりするのと同じくらい簡単です。

![](./images/index/no-hassle-connection.png)

### リアルタイムの検査、シミュレーション、検証

Assuranceに接続すると、ライブストリーミングアプリのイベントとアクティビティを調べ、フィルタリングと検索でノイズを排除できます。 イベントには、モバイルアプリ実装の検証、デバッグ、トラブルシューティングに関する詳細が含まれています。 Assuranceでは、スクリーンショットの作成や、場所の信号のシミュレーションなどをリアルタイムで行うこともできます。

![](./images/index/real-time-insepction.png)

### Adobe Experience Cloud との統合

クライアントサイドのデータとエクスペリエンスには、マーケターに焦点を当てたユーザーインターフェイスでユーザーがレポートルール、アクティビティおよびキャンペーンをどのように設定するかによってコンテキストが与えられます。 この 2 つの点を結び付けるのに役立つように、アドビは、Adobe Experience Platform、Adobe Analytics、Adobe Target、Places Service などのAdobe Experience Cloud ソリューションと統合しています。

![](./images/index/integration.png)

## 機能

### Adobe Experience Platform Mobile SDKのイベント、ログなど

Assuranceは、Adobe Experience Platform Mobile SDKで生成された生のSDK イベントを調べるのに役立ちます。 SDKで収集されたすべてのイベントは、閲覧できます。 SDK イベントは、時間順に並べ替えられたリストビューに読み込まれます。 各イベントには、詳細を提供する詳細ビューがあります。 SDK設定、データ要素、共有状態、SDK拡張機能の各バージョンを参照するための追加のビューも提供されます。

### Adobe Analytics

Adobe Analytics/Analytics イベント ビューは、Adobe Analytics モバイル実装に関連するイベントを表示するビューに焦点を当てています。 リスト表示では、ライフサイクルまたはアクション/状態イベント、後処理の「ステータス」、および必要なイベントの詳細が特別にフォーマットされたビューで表示されます。 後処理ステータスは、イベントに処理ルールが適用された後に、Adobe Analyticsによってイベントがどのように処理されたかを示します。

### ストリーミングメディア用 Adobe Analytics

Adobe Analytics/Media Analytics イベントビューには、オーディオおよびビデオ分析実装用のイベントが表示されます。 イベントの詳細ビューには、再生セッションごとに追跡される標準メタデータとカスタムメタデータが表示されます。 さらに、処理後のステータスと処理後のメディア分析データ（メディア視聴時間や合計バッファー時間など）を表示できます。

### 場所（位置情報サービス）

場所サービス ビューは、検証を容易にするためにユーザーの場所のエントリと離脱イベントを表示するオンデバイス表示です。 この便利なビューは、コンテキスト内デバッグ用にクライアント上で検査するための、場所固有のデータポイントを表示する便利なインターフェイスを提供します。

## Assuranceは安全ですか？

Assuranceには、次のセキュリティ対策が講じられています。

* AssuranceとAssurance Web UI には、どちらも接続用の安全な PIN ベースのハンドシェイクがあります。 ユーザーは明示的にハンドシェイクを作成する必要があり、これにより、「誤って」Assurance接続がエンドユーザーによって作成されるのを防ぐことができます。
* Assuranceと、同じAdobe Experience Cloud組織 ID に属するAssurance web UI 間の接続のみがサポートされます。
* Adobe Experience Platform Mobile SDK のイベントは、HTTPS で転送されます。
* AssuranceおよびAdobe Experience Platform Mobile SDK は TLS 1.2 を使用します
* Assurance セッションは、30 日後に削除されます。
* Assurance セッションのデータは、ストレージのベストプラクティスに従って、保存時に暗号化されます。

## はじめに

Assuranceを設定するには、まずAssurance拡張機能をアプリケーションにインストールする必要があります。 その方法については、[Assurance拡張機能の実装 ](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#add-the-aep-assurance-extension-to-your-app) に関するチュートリアルを参照してください。

Assuranceをアプリに追加した後、デバイスに接続できるAssurance セッションを作成できます。 Assuranceの使用方法については、[Assuranceの使用に関するガイド ](./tutorials/using-assurance.md) を参照してください。

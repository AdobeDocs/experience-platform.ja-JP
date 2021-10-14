---
keywords: Experience Platform；はじめに；Attribution Ai；人気の高いトピック
feature: Attribution AI
title: はじめに (Attribution AI)
topic-legacy: Getting started
description: 以下のガイドでは、Attribution AI の使用に関連する様々な Adobe Experience Platform サービスについて理解している必要があります。このチュートリアルを始める前に、次のドキュメントを確認してください。
exl-id: ab269c24-97ac-4da9-9b6c-7d2dde61f0dc
source-git-commit: c3320f040383980448135371ad9fae583cfca344
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 70%

---

# Attribution AI の概要

以下のガイドでは、Attribution AIの使用に関わる様々な [!DNL Adobe Experience Platform] サービスについて理解しておく必要があります。 このチュートリアルを始める前に、次のドキュメントを確認してください。

- [エクスペリエンスデータモデル (XDM) システムの概要](../../xdm/home.md):XDM は、Experience Platformを活用し [!DNL Adobe Experience Cloud]て、適切なチャネルで、適切なタイミングで適切な人に適切なメッセージを届けることを可能にする、基本的なフレームワークです。Experience Platform を構築する方法論である XDM システムによって、Platform サービスでエクスペリエンスデータモデルスキーマを操作できるようになります。
- [スキーマ構成の基本](../../xdm/schema/composition.md):このドキュメントでは、Experience Data Model(XDM) スキーマの概要と、で使用するスキーマを構成するための構成要素、原則、ベストプラクティスを示し [!DNL Adobe Experience Platform]ます。
- [スキーマの構築](../../xdm/tutorials/create-schema-ui.md)：このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。

Attribution AIには、データセットが [ エクスペリエンスデータモデル (XDM)](../../xdm/home.md) スキーマフィールドグループであるコンシューマーエクスペリエンスイベント (CEE) スキーマに準拠している必要があります。 このデータを実装または変更するには、アドビサポート（attributionai-support@adobe.com）にお問い合わせください。メディア支出データが存在する場合は、売上高や ROI の増分など、さらに分析することができます。顧客プロファイルデータが使用可能な場合、クレジットを顧客プロファイルレベルに属性付けできます。

## 用語

- **コンバージョンイベント：**&#x200B;目標に向けたマイルストーンを示すために顧客が実行するデジタルイベントまたはデジタルインタラクション（会議の登録など）。その他の例としては、有料コンバージョン、無料アカウントの新規登録、特性の認定などがあります。

- **タッチポイント：**&#x200B;目標に向けたパスにおいて顧客が実行するデジタルイベントまたはデジタルインタラクション。例としては、購入前のマーケティング活動、ディスプレイ広告インプレッションの表示、有料検索のクリックなどがあります。

## Attribution AIスコアのダウンロード

>[!NOTE]
>
>未加工のスコアをダウンロードする必要がない場合は、この手順をスキップして、次の手順 ](#next-steps) に進みます。[

Attribution AIスコアのダウンロードは、API 呼び出しの組み合わせを通じておこなわれます。 Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Platform API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。API 呼び出し例のドキュメントで使用される表記について詳しくは、『Experience Platform トラブルシューティングガイド』の [API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## 次の手順 {#next-steps}

準備が整い、すべての資格情報とスキーマが整ったら、[Attribution AI ユーザーインターフェイスガイド](./user-guide.md)に従って開始します。このガイドでは、インスタンスを作成し、トレーニングおよびスコア付け用に送信する手順を説明します。

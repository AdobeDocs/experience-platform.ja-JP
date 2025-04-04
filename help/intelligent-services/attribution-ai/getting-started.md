---
keywords: Experience Platform；はじめに；アトリビューション ai；人気のトピック
feature: Attribution AI
title: アトリビューション AI の基本を学ぶ
description: 以下のガイドでは、Attribution AI の使用に関連する様々な Adobe Experience Platform サービスについて理解している必要があります。チュートリアルを開始する前に、次のドキュメントを確認してください。
exl-id: ab269c24-97ac-4da9-9b6c-7d2dde61f0dc
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 57%

---

# Attribution AI の概要

以下のガイドを参照するには、アトリビューション AI の使用に関連する様々な [!DNL Adobe Experience Platform] サービスについて理解している必要があります。 このチュートリアルを始める前に、次のドキュメントを確認してください。

- [ エクスペリエンスデータモデル（XDM）システムの概要 ](../../xdm/home.md):XDM は、Experience Platformを活用し、適切なユーザーに、適切なチャネルで、適切なタイミングで適切なメッセージを配信で [!DNL Adobe Experience Cloud] る基本的なフレームワークです。 Experience Platformの基礎となる XDM システムは、Experience Data Model スキーマをExperience Platform サービスで操作できるようにします。
- [ スキーマ構成の基本 ](../../xdm/schema/composition.md)：このドキュメントでは、エクスペリエンスデータモデル（XDM）スキーマの概要と、[!DNL Adobe Experience Platform] で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介します。
- [スキーマの構築](../../xdm/tutorials/create-schema-ui.md)：このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。

アトリビューション AI では、データセットが消費者エクスペリエンスイベント（CEE）スキーマに準拠している必要があります。これは、[ エクスペリエンスデータモデル（XDM） ](../../xdm/home.md) スキーマフィールドグループです。 このデータを実装または変更するには、アドビサポート（attributionai-support@adobe.com）にお問い合わせください。メディア支出データが存在する場合は、売上高や ROI の増分など、さらに分析することができます。顧客プロファイルデータが使用可能な場合、クレジットを顧客プロファイルレベルに属性付けできます。

## 用語

- **コンバージョンイベント：**&#x200B;目標に向けたマイルストーンを示すために顧客が実行するデジタルイベントまたはデジタルインタラクション（会議の登録など）。その他の例としては、有料コンバージョン、無料アカウントの新規登録、特性の認定などがあります。

- **タッチポイント：**&#x200B;目標に向けたパスにおいて顧客が実行するデジタルイベントまたはデジタルインタラクション。例としては、購入前のマーケティング活動、ディスプレイ広告インプレッションの表示、有料検索のクリックなどがあります。

## アトリビューション AI スコアのダウンロード

>[!NOTE]
>
>生のスコアをダウンロードする必要がない場合は、この手順をスキップして [ 次の手順 ](#next-steps) に進んでください。

アトリビューション AI スコアのダウンロードは、API 呼び出しを組み合わせて行われます。 Experience Platform API を呼び出すには、まず[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id: `{ORG_ID}`

Experience Platform のすべてのリソースは、特定の仮想サンドボックスに分離されています。Experience Platform API へのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platform のサンドボックスについて詳しくは、[サンドボックスの概要ドキュメ ント](../../sandboxes/home.md)を参照してください。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md)に関する節を参照してください。

## アクセス制御 {#access-control}

役割ベースのアクセス制御を使用する場合、**アトリビューション AI を表示** および **アトリビューション AI を管理** 権限により、アトリビューション AI の様々な機能へのアクセス権が付与されます。 **アトリビューション AI を管理** を使用すると、インスタンスの **作成**、**クローン**、**編集**、**削除**、**有効化**、**無効化** を実行でき、**アトリビューション AI を表示** では、インスタンスの **読み取り** または **表示** を実行できます。 **作成**、**編集** および **削除** アクションは監査ログで記録されます。

[アクセス制御の権限の割り当て](../../../help/access-control/home.md)または[監査ログを使用してアクセスとアクティビティを監視](../../../help/landing/governance-privacy-security/audit-logs/overview.md)する方法については、ドキュメントを参照してください。

## 次の手順 {#next-steps}

準備が整い、すべての資格情報とスキーマが整ったら、[Attribution AI ユーザーインターフェイスガイド](./user-guide.md)に従って開始します。このガイドでは、インスタンスを作成し、トレーニングおよびスコア付け用に送信する手順を説明します。

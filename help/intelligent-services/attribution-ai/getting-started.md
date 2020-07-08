---
keywords: Experience Platform;getting started;attribution ai;popular topics
solution: Experience Platform
title: Attribution AIの概要
topic: Getting started
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 0%

---


# Attribution AIの概要

以下のガイドでは、Attribution AIの使用に関連する様々な [!DNL Adobe Experience Platform] サービスについて理解している必要があります。 チュートリアルを開始する前に、次のドキュメントを確認してください。

- [Experience Data Model(XDM)システム概要](../../xdm/home.md): XDMは、Experience Platformを利用し [!DNL Adobe Experience Cloud]て適切なメッセージを適切な人に、適切なチャネルに、ちょうど適切なタイミングで配信できる基本的なフレームワークです。 Experience Platformを構築する方法論、XDM Systemは、Platformサービスで使用するExperience Data Modelスキーマを運用します。
- [スキーマ構成の基本](../../xdm/schema/composition.md): このドキュメントでは、Experience Data Model(XDM)スキーマと、で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介 [!DNL Adobe Experience Platform]します。
- [スキーマの作成](../../xdm/tutorials/create-schema-ui.md): このチュートリアルでは、Experience Platform内でスキーマエディタを使用してスキーマを作成する手順を説明します。

アトリビューションAIの場合、データセットは、 [Experience Data Model](../../xdm/home.md) (XDM)のミックスインであるConsumer Experience Data Model(CEE)スキーマに準拠する必要があります。 このデータを実装または変更するには、アドビサポート(attributionai-support@adobe.com)にお問い合わせください。 メディア支出データが存在する場合は、売上の増加やROI（投資収益率）など、さらに分析を行うことができます。 顧客プロファイルデータが使用可能な場合は、クレジットを顧客プロファイルレベルに関連付けることができます。

## 用語

- **コンバージョンイベント:** 会議の登録など、目標に向けたマイルストーンを示すために顧客が行うデジタルイベントまたはデジタルインタラクション。 その他の例としては、有料コンバージョン、無料アカウントのサインアップ、特性の見極めなどがあります。

- **タッチポイント：** 顧客が目標に向かって行うデジタルイベントまたはデジタルインタラクション。 購入前関連のマーケティング活動、表示された広告インプレッションの表示、有料検索クリックなどが例です。

## アトリビューションAIスコアのダウンロード

>[!NOTE]
>
>生のスコアをダウンロードする必要がない場合は、この手順をスキップして [次の手順に進みます](#next-steps)。

アトリビューションAIスコアのダウンロードは、API呼び出しの組み合わせを通じて行われます。 PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、Experience PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md) を参照してください。

## 次の手順 {#next-steps}

準備が整い、すべての資格情報とスキーマが整ったら、『 [アトリビューションAIユーザーインターフェイスガイド](./user-guide.md)』に従って開始します。 このガイドでは、インスタンスの作成、およびトレーニングとスコアリングのための送信に関する手順を説明します。
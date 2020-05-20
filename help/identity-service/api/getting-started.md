---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: はじめに
topic: API guide
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 0%

---


# IDサービスAPI開発者ガイド

Adobe Experience Platform Identity Serviceは、Adobe Experience Platform内でIDグラフと呼ばれる、デバイス間、チャネル間、およびほぼリアルタイムでの顧客の識別を管理します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

- [IDサービス](../home.md): お客様のプロファイルデータの断片化に伴う基本的な課題を解決します。 これは、顧客がブランド化を行うデバイスやシステム間でIDを橋渡しすることで実現します。
- [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [Experience Data Model(XDM)](../../xdm/home.md): プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。

以下の節では、Identity Service APIの呼び出しを正常に行うために知る必要がある、または手元にある情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

### 地域ベースのルーティング

Identity Service APIでは、リクエストパスの一部としてのを含める必要がある地域固有のエンドポイント `{REGION}` を採用しています。 IMS組織のプロビジョニング中に、領域が決定され、IMS組織プロファイル内に保存されます。 各エンドポイントで正しい領域を使用すると、Identity Service APIを使用して行われたすべての要求が、必ず適切な領域にルーティングされます。

IDサービスAPIで現在サポートされている領域は2つあります。 VA7とNLD2。

次の表に、領域を使用したパスの例を示します。

| サービス | 地域： VA7 | 地域： NLD2 |
| ------ | -------- |--------- |
| IDサービスAPI | https://</span>platform-va7.adobe.</span>io/data/core/identity/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/identity/{ENDPOINT} |
| ID名前空間API | https://</span>platform-va7.adobe.</span>io/data/core/idnamespace/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/idnamespace{ENDPOINT} |

>[!NOTE] 領域を指定せずに要求を行うと、誤った領域に対する呼び出しルーティングが発生したり、呼び出しが予期せず失敗したりする場合があります。

IMS組織プロファイル内で地域が見つからない場合は、システム管理者に問い合わせてください。

## IDサービスAPIの使用

これらのサービスで使用されるIDパラメーターは、次の2つの方法のいずれかで表現できます。 compositeまたはXID。

複合IDは、ID値と名前空間の両方を含む構成です。 複合IDを使用する場合、名前空間は名前(`namespace.code`)またはID(`namespace.id`)で指定できます。

IDが持続すると、IDサービスはIDを生成し、ネイティブID(XID)と呼ばれるIDを割り当てます。 クラスターAPIとマッピングAPIのすべてのバリエーションは、リクエストと応答で複合IDとXIDの両方をサポートしています。 これらのAPIの使用には、いずれかのパラメーター(または `xid` と [`ns` の組み合わせ)が必要 `nsid`]`id` です。

応答でペイロードを制限するために、APIは、使用するID構成体のタイプに応答を適応させます。 つまり、XIDを渡すと、応答にXIDが割り当てられ、複合IDを渡すと、応答は要求で使用される構造に従います。

このドキュメントの例では、Identity Service APIの全機能については説明しません。 完全なAPIについては、『 [Swagger API Reference](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)』を参照してください。

>[!NOTE] 要求でネイティブXIDが使用されている場合、返されるIDはすべてネイティブXID形式になります。 ID/名前空間フォームの使用をお勧めします。 詳しくは、IDのXIDの [取得に関する節を参照してください](./create-custom-namespace.md)。

## 次の手順

必要な資格情報を収集したら、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切にフォーマットされたペイロードを示すサンプル **リクエスト** 、および正常な呼び出しのためのサンプル **応答** が含まれます。

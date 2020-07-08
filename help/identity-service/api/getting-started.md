---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: はじめに
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 1%

---


# [!DNL Identity Service] API開発者ガイド

Adobe Experience Platform [!DNL Identity Service] は、デバイス間、チャネル間、Adobe Experience Platform内のIDグラフと呼ばれる顧客のほぼリアルタイムでの識別を管理します。

## はじめに

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

- [!DNL Identity Service](../home.md): お客様のプロファイルデータの断片化に伴う基本的な課題を解決します。 これは、顧客がブランド化を行うデバイスやシステム間でIDを橋渡しすることで実現します。
- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

以下の節では、 [!DNL Identity Service] APIを正しく呼び出すために知る必要がある、または手元にある情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

### 地域ベースのルーティング

この [!DNL Identity Service] APIでは、リクエストパスの一部としてのを含める必要がある、地域固有のエンドポイント `{REGION}` を採用しています。 IMS組織のプロビジョニング中に、領域が決定され、IMS組織プロファイル内に保存されます。 正しい領域を各エンドポイントと共に使用すると、 [!DNL Identity Service] APIを使用したすべてのリクエストが確実に適切な領域にルーティングされます。

APIで現在サポートされている領域は2つあり [!DNL Identity Service] ます。 VA7とNLD2。

次の表に、領域を使用したパスの例を示します。

| サービス | 地域： VA7 | 地域： NLD2 |
| ------ | -------- |--------- |
| [!DNL Identity Service] API | https://</span>platform-va7.adobe.</span>io/data/core/identity/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/identity/{ENDPOINT} |
| [!DNL Identity Namespace] API | https://</span>platform-va7.adobe.</span>io/data/core/idnamespace/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/idnamespace{ENDPOINT} |

>[!NOTE]
>
>領域を指定せずに要求を行うと、誤った領域に対する呼び出しルーティングが発生したり、呼び出しが予期せず失敗したりする場合があります。

IMS組織プロファイル内で地域が見つからない場合は、システム管理者に問い合わせてください。

## Using the [!DNL Identity Service] API

これらのサービスで使用されるIDパラメーターは、次の2つの方法のいずれかで表現できます。 compositeまたはXID。

複合IDは、ID値と名前空間の両方を含む構成です。 複合IDを使用する場合、名前空間は名前(`namespace.code`)またはID(`namespace.id`)で指定できます。

IDが持続する場合、はIDを [!DNL Identity Service] 生成し、ネイティブID(XID)と呼ばれるIDを割り当てます。 クラスターAPIとマッピングAPIのすべてのバリエーションは、リクエストと応答で複合IDとXIDの両方をサポートしています。 これらのAPIの使用には、いずれかのパラメーター(または `xid` と [`ns` の組み合わせ)が必要 `nsid`]`id` です。

応答でペイロードを制限するために、APIは、使用するID構成体のタイプに応答を適応させます。 つまり、XIDを渡すと、応答にXIDが割り当てられ、複合IDを渡すと、応答は要求で使用される構造に従います。

このドキュメントの例では、 [!DNL Identity Service] APIの全機能について説明していません。 完全なAPIについては、『 [Swagger API Reference](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)』を参照してください。

>[!NOTE]
>
>要求でネイティブXIDが使用されている場合、返されるIDはすべてネイティブXID形式になります。 ID/名前空間フォームの使用をお勧めします。 詳しくは、IDのXIDの [取得に関する節を参照してください](./create-custom-namespace.md)。

## 次の手順

必要な資格情報を収集したら、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切にフォーマットされたペイロードを示すサンプル **リクエスト** 、および正常な呼び出しのためのサンプル **応答** が含まれます。

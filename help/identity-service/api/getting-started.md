---
keywords: Experience Platform；ホーム；人気のあるトピック；IDサービスapi;IDサービス開発者ガイド；地域
solution: Experience Platform
title: IDサービスAPIガイド
topic-legacy: API guide
description: IDサービスAPIを使用すると、開発者は、Adobe Experience PlatformのIDグラフを使用して、デバイス間、チャネル間、ほぼリアルタイムでの顧客の識別を管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: d612af38-4648-4c3e-8cfd-3f306c9370e1
source-git-commit: f269a7b1584a6e4a0e1820a0c587a647c0c8f7b5
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 72%

---

# [!DNL Identity Service] API ガイド

Adobe Experience Platform [!DNL Identity Service]は、Adobe Experience Platform内のIDグラフと呼ばれる、デバイス間、チャネル間、およびほぼリアルタイムでの顧客の識別を管理します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [[!DNL Identity Service]](../home.md):顧客プロファイルデータの断片化によって生じる基本的な課題を解決します。これは、顧客がブランドと対話するデバイスやシステム間で ID を橋渡しするすることで実現します。
- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合された消費者プロファイルをリアルタイムで提供します。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。

以下の節では、[!DNL Identity Service] APIを正しく呼び出すために知っておく必要がある、または手元に置く必要がある追加情報を示します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

- Content-Type: application/json

### 地域ベースのルーティング

[!DNL Identity Service] APIは、リクエストパスの一部として`{REGION}`を含める必要がある地域固有のエンドポイントを採用しています。 IMS 組織のプロビジョニング中に、地域が決定され、IMS Org プロファイル内に保存されます。各エンドポイントで正しい領域を使用すると、[!DNL Identity Service] APIを使用しておこなわれたすべてのリクエストが適切な領域にルーティングされます。

[!DNL Identity Service] APIで現在サポートされている領域は2つあります。VA7とNLD2。

次の表に、地域を使用したパスの例を示します。

| サービス | 地域：VA7 | 地域：NLD2 |
| ------ | -------- |--------- |
| [!DNL Identity Service] API | https://</span>platform-va7.adobe.</span>io/data/core/identity/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/identity/{ENDPOINT} |
| [!DNL Identity Namespace] API | https://</span>platform-va7.adobe.</span>io/data/core/idnamespace/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/idnamespace{ENDPOINT} |

>[!NOTE]
>
>地域を指定しないでリクエストをおこなうと、誤った地域に対する呼び出しルーティングが発生したり、呼び出しが予期せず失敗したりする場合があります。

IMS Org プロファイル内で地域が見つからない場合は、システム管理者に問い合わせてください。

## [!DNL Identity Service] APIの使用

これらのサービスで使用される ID パラメーターは、次の 2 つの方法、複合または XID のいずれかで表現できます。

複合 ID は、ID 値と名前空間の両方を含む構成です。複合 ID を使用する場合、名前空間は名前（`namespace.code`）または ID（`namespace.id`）で指定できます。

IDが保持されると、[!DNL Identity Service]はネイティブID(XID)と呼ばれるIDを生成し、そのIDに割り当てます。 クラスター API とマッピング API のすべてのバリエーションは、リクエストと応答で複合 ID と XID の両方をサポートしています。これらのAPIを使用するには、次のパラメーターのいずれかが必要です。`xid`、[`ns`または`nsid`]および`id`の組み合わせ。

応答のペイロードを制限するため、API は、使用される ID 構文のタイプに応じて、それらの応答を調整します。つまり、XID を渡すと応答に XID が含まれ、複合 ID を渡すと、応答はリクエストで使用される構造に従います。

このドキュメントの例では、[!DNL Identity Service] APIの全機能については説明しません。 完全な API については、「[Swagger API リファレンス](https://www.adobe.io/experience-platform-apis/references/identity-service)」を参照してください。

>[!NOTE]
>
>リクエストでネイティブ XID が使用される場合、返される ID はすべてネイティブ XID 形式になります。ID／名前空間フォームの使用詳しくは、「[ID の XID の取得](./create-custom-namespace.md)」に関する節を参照してください。

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。

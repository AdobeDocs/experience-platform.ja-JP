---
keywords: Experience Platform；ホーム；人気のトピック；identity service api;identity service デベロッパーガイド；地域
solution: Experience Platform
title: ID サービス API ガイド
description: ID サービス API を使用すると、デベロッパーはAdobe Experience Platformの ID グラフを使用して、クロスデバイス、クロスチャネル、ほぼリアルタイムでの顧客の ID を管理できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
role: Developer
exl-id: d612af38-4648-4c3e-8cfd-3f306c9370e1
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 61%

---

# [!DNL Identity Service] API ガイド

Adobe Experience Platform [!DNL Identity Service] は、クロスデバイス、クロスチャネル、ほぼリアルタイムでの顧客の ID （Adobe Experience Platform内の ID グラフ）を管理します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [[!DNL Identity Service]](../home.md)：顧客プロファイルデータの断片化によって発生する基本的な課題を解決します。 これを実現するには、顧客がブランドとやり取りするデバイスやシステム間で ID を橋渡しします。
- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。

次の節では、[!DNL Identity Service] API の呼び出しを正しくおこなうために知っておく必要がある、または手元に用意しておく必要がある追加情報を提供します。

### API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

- Content-Type: application/json

### 地域ベースのルーティング

[!DNL Identity Service] API では、リクエストパスの一部として `{REGION}` を含める必要がある、地域固有のエンドポイントを使用します。 組織のプロビジョニング中に、地域が決定され、組織プロファイル内に保存されます。 各エンドポイントで正しい領域を使用すると、[!DNL Identity Service] API を使用して行われたすべてのリクエストが適切な領域にルーティングされます。

現在、[!DNL Identity Service] API でサポートされているリージョンは VA7 および NLD2 です。

次の表に、地域を使用したパスの例を示します。

| サービス | 地域：VA7 | 地域：NLD2 |
| ------ | -------- |--------- |
| [!DNL Identity Service] API | https://</span>platform-va7.adobe.</span>io/data/core/identity/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/identity/{ENDPOINT} |
| [!DNL Identity Namespace] API | https://</span>platform-va7.adobe.</span>io/data/core/idnamespace/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/idnamespace{ENDPOINT} |

>[!NOTE]
>
>地域を指定せずに要求を行うと、誤った地域への呼び出しがルーティングされたり、呼び出しが予期せず失敗したりする可能性があります。

組織プロファイル内で地域が見つからない場合は、システム管理者にお問い合わせください。

## [!DNL Identity Service] API の使用

これらのサービスで使用される ID パラメーターは、次の 2 つの方法、複合または XID のいずれかで表現できます。

複合 ID は、ID 値と名前空間の両方を含む構成です。複合 ID を使用する場合、名前空間は名前（`namespace.code`）または ID（`namespace.id`）で指定できます。

ID が永続化されると、[!DNL Identity Service] はネイティブ ID （XID）と呼ばれる ID を生成してその ID に割り当てます。 クラスター API とマッピング API のすべてのバリエーションは、リクエストと応答で複合 ID と XID の両方をサポートしています。これらのAPIを使用するには、次のパラメーターのいずれかが必要です。`xid`、[`ns`または`nsid`]および`id`の組み合わせ。

応答のペイロードを制限するため、API は、使用される ID 構文のタイプに応じて、それらの応答を調整します。つまり、XID を渡すと応答に XID が含まれ、複合 ID を渡すと、応答はリクエストで使用される構造に従います。

このドキュメントの例では、[!DNL Identity Service] API の完全な機能については説明していません。 完全な API については、「[Swagger API リファレンス](https://www.adobe.io/experience-platform-apis/references/identity-service)」を参照してください。

>[!NOTE]
>
>リクエストでネイティブ XID が使用されている場合、返されるすべての ID はネイティブ XID 形式です。 ID／名前空間フォームの使用詳しくは、「[ID の XID の取得](./create-custom-namespace.md)」に関する節を参照してください。

## 次の手順

必要な資格情報を収集したので、開発者ガイドの残りをお読みください。各節では、エンドポイントに関する重要な情報を提供し、CRUD 操作を実行するための API 呼び出しの例を示します。各呼び出しには、一般的な API 形式、必要なヘッダーと適切な形式のペイロードを示すサンプルリクエスト、および正常な呼び 出しのサンプル応答が含まれます。

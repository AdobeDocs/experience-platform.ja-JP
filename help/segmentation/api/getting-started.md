---
keywords: 経験のあるプラットホーム、home、ポピュラーな話題、セグメンテーション、セグメンテーション、分類セグメンテーションサービス。
solution: Experience Platform
title: セグメンテーションサービス API を使用した作業の開始
topic-legacy: developer guide
description: 次のマニュアルには、セグメンテーション API を使用して正常に動作するために必要な追加情報が記載されています。
exl-id: 41c0e50b-afed-45b8-85d7-a0c84ae090f5
source-git-commit: 8325ae6fd7d0013979e80d56eccd05b6ed6f5108
workflow-type: tm+mt
source-wordcount: '353'
ht-degree: 47%

---

# セグメンテーションサービス API を使用した作業の開始 {#getting-started}

Adobe エクスペリエンスプラットフォーム [!DNL Segmentation Service] を使用して、データからセグメントを作成し、対象ユーザーを adobe の操作プラットフォームで生成でき [!DNL Real-time Customer Profile] ます。

デベロッパーガイドでは、を使用するための様々なサービスについて、理解しておく必要があり [!DNL Experience Platform] [!DNL Segmentation Service] ます。

- [[!DNL Segmentation]](../home.md)「」: データから対象ユーザーセグメントを作成できます [!DNL Real-time Customer Profile] 。
- [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Experience Platform] に使用される標準化されたフレームワーク。セグメンテーションを最大限に活用するには、データモデリングのベストプラクティスに従い、データをプロファイルとイベントとして ingested にしておく必要が [ ](../../xdm/schema/best-practices.md) あります。
- [[!DNL Real-time Customer Profile]](../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の各セクションでは、API を正常に使用するために必要な追加情報について説明し [!DNL Segmentation] ます。

## API 呼び出し例の読み取り

[!DNL Segmentation Service] API ドキュメントには、API 呼び出しの例とリクエストの形式を指定する方法が示されています。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

## 必須ヘッダー

また、API ドキュメントでは、 エンドポイントを正しく呼び出すために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了している必要があります。[!DNL Platform]次に示すように、[!DNL Experience Platform] API 呼び出しにおける各必須ヘッダーの値は認証チュートリアルで説明されています。

- Authorization: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。Api に対するすべての要求には [!DNL Platform] 、操作を行うサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>でのサンドボックスの操作について詳しくは、 [!DNL Experience Platform] 「サンドボックスの概要」を参照してください [ ](../../sandboxes/home.md) 。

## 次の手順

API を使用して呼び出しを行うに [!DNL Segmentation Service] は、左側のナビゲーションまたはデベロッパーズガイドのいずれかを使用して、使用可能なエンドポイントガイドのいずれかを選択します。 [](./overview.md)

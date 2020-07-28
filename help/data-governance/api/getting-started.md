---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データポリシーサービス API 開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 0534fe8dcc11741ddc74749d231e732163adf5b0
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 56%

---


# モジュール [!DNL Policy Service] API開発者ガイド

Data Usage Labeling and Enforcement (DULE) is the core mechanism of Adobe Experience Platform [!DNL Data Governance]. The DULE [!DNL Policy Service] provides a RESTful API that allows you to create and manage data usage policies to determine what marketing actions can be taken against data that has been labeled with certain data usage labels.

This document provides instructions for performing the key operations available in the [!DNL Policy Service] API. まだデータフレームワークに慣れていない場合は、まず「[データガバナンスの概要](../home.md)」を確認し、データフレームワークに慣れてください。データポリシーの作成と適用の手順については、「[データポリシーのチュートリアル](../policies/create.md)」を参照してください。

This document provides an introduction to the core concepts you need to know before attempting to make calls to the [!DNL Policy Service] API.

## Getting started with DULE [!DNL Policy Service]

Before beginning to work with the [!DNL Policy Service], data on [!DNL Experience Platform] must have appropriate DULE labels applied. データ使用ラベルをデータセットおよびフィールドに適用する手順については、『[データラベルユーザーガイド](../labels/user-guide.md)』を参照してください。

## 前提条件

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [!DNL Data Governance](../home.md): データ使用のコンプライアンスを [!DNL Experience Platform] 適用するフレームワーク。
   * [ラベルの集計](../labels/overview.md): データ使用ラベルは、 [!DNL Experience Data Model] (XDM)データフィールドに適用され、そのデータへのアクセス方法に関する制限を指定します。
* [!DNL Experience Data Model (XDM) System](../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
* [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [!DNL Sandboxes](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## API 呼び出し例の読み取り

ここでは、リクエストの形式を説明するために API 呼び出しの例を示します。これには、パス、必須ヘッダー、適切に書式設定されたリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

## 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to [!DNL Data Governance], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* Content-Type: application/json

## コアリソースとカスタムリソース

Within the [!DNL Policy Service] API, all policies and marketing actions are referred to as either `core` or `custom` resources.

`core` リソースはアドビが定義および管理するものですが、`custom` リソースは個々の顧客が作成および管理するものなので、作成した IMS 組織のみに表示され、一意のものです。このため、`core` リソースに対して許可される操作は、リストと参照の操作（`GET`）のみです。一方、`custom` リソースに対しては、リスト、参照、および更新の操作（`POST`、`PUT`、`PATCH`、`DELETE`）を使用できます。

## ポリシーステータス

データ使用ポリシーには、`DRAFT`、`ENABLED`、`DISABLED`の 3 つのステータスのいずれかを設定できます。 

デフォルトでは、「有効」ポリシーのみがポリシー評価に関与します。

「ドラフト」ポリシーは、`?includeDraft=true` クエリパラメーターを設定してのみ、ポリシー評価でも考慮できます。ポリシー評価の詳細については、本文書末尾にある「[ポリシーの適用](../enforcement/overview.md) 」の節を参照してください。

## マーケティングアクション名 {#marketing-actions}

マーケティングアクション名は、マーケティングアクションの一意の識別子です。各 `core` マーケティングアクションには、すべての IMS 組織に適用される一意の名前が付けられます。これらの名前はアドビによって定義および管理されます。一方、顧客が定義したマーケティングアクション（`custom` リソース）はすべて、個々の組織内で一意であり、他の IMS 組織で表示されたり他の IMS 組織と共有されることはありません。

Steps for working with marketing actions in the [!DNL Policy Service] API are outlined in the [Marketing Actions](#marketing-actions) section later in this document.

## 次の手順

前提条件となる知識と認証情報を取得したので、この開発者ガイドで提供されている API 呼び出し例をお読みください。

* [マーケティングアクション](marketing-actions.md)
* [ポリシー](policies.md)
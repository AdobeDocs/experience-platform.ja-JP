---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: DULE Policy Service API開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '678'
ht-degree: 0%

---


# モジュール [!DNL Policy Service] API開発者ガイド

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platformデータガバナンスの中核的なメカニズムです。 DULE PolicyサービスはRESTful APIを提供します。このAPIを使用すると、データ使用ポリシーを作成および管理して、特定のデータ使用ラベルでラベル付けされたデータに対して実行できるマーケティングアクションを決定できます。

このドキュメントでは、Policy Service APIで使用可能な主要操作を実行する手順を説明します。 まだDULEフレームワークに慣れていない場合は、 [データ・ガバナンスの概要を見直して](../home.md) 、DULEフレームワークに慣れてください。 DULEポリシーを作成および適用する手順については、「 [SCHEDULE policy tutorial](../policies/create.md)」を参照してください。

このドキュメントでは、Policy Service APIを呼び出す前に知っておく必要があるコア概念の概要を説明します。

## Getting started with DULE [!DNL Policy Service]

で作業を始める前に、のデータに適切なDULEラベルが適用されてい [!DNL Policy Service][!DNL Experience Platform] る必要があります。 データ使用ラベルをデータセットとフィールドに適用する詳しい手順は、『 [DULE labels user guide](../labels/user-guide.md)』を参照してください。

## 前提条件

このガイドでは、次のAdobe Experience Platformのコンポーネントについて、十分に理解している必要があります。

* [Data Governance](../home.md): データ使用のコンプライアンスを [!DNL Experience Platform] 適用するフレームワーク。
   * [ラベルの集計](../labels/overview.md): データ使用ラベルは、エクスペリエンスデータモデル(XDM)データフィールドに適用され、そのデータへのアクセス方法に関する制限を指定します。
* [Experience Data Model(XDM)System](../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
* [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

## 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

に属するリソース [!DNL Experience Platform]を含む、のすべてのリソースは、特定の仮想サンドボックスに分離され [!DNL Data Governance]ます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## コアリソースとカスタムリソース

API内では、すべてのポリシーおよびマーケティングアクションが、「または」 [!DNL Policy Service] リソースと呼ばれ `core``custom` ます。

リソースはアドビが定義および管理するものですが、 `core``custom` リソースは個々のお客様が作成および管理するものなので、リソースを作成したIMS組織のみに表示され、一意のものです。 したがって、リソースに対して許可される操作は、リスト化操作と参照操作(`GET`)のみです。一方、リスト化操作、参照操作と更新操作( `core` 、`POST`、 `PUT`および `PATCH``DELETE``custom` )は、リソースに対して使用できます。

## ポリシーの状態

データ使用ポリシーには、次の3つのステータスのいずれかを設定できます。 `DRAFT`、 `ENABLED`または `DISABLED`。

デフォルトでは、「有効」なポリシーのみがポリシー評価に関与します。

「ドラフト」ポリシーもポリシー評価で考慮できますが、クエリパラメーターを設定する場合のみ考慮でき `?includeDraft=true`ます。 ポリシーの評価の詳細については、このドキュメントの最後にある「 [ポリシーの適用に関するドキュメント](../enforcement/overview.md) 」の節を参照してください。

## マーケティングアクション名 {#marketing-actions}

マーケティングアクション名は、マーケティングアクションに対して一意の識別子です。 各 `core` マーケティングアクションには、すべてのIMS組織に適用される一意の名前が付けられます。 これらの名前は、アドビによって定義および管理されます。 一方、顧客定義のマーケティングアクション(`custom` リソース)はすべて、貴社の組織内で一意であり、他のIMS組織と表示または共有されることはありません。

APIでマーケティングアクションを使用する手順は、この [!DNL Policy Service] ドキュメント後半の「 [マーケティングアクション](#marketing-actions) 」セクションで説明されています。

## 次の手順

必要な知識と資格情報が揃ったので、この開発者ガイドに記載されているサンプルAPI呼び出しを引き続き読むことができます。

* [マーケティングアクション](marketing-actions.md)
* [ポリシー](policies.md)
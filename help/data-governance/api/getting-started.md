---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: DULE Policy Service API開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: eec5b07427aa9daa44d23f09cfaf1b38f8e811f3
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 0%

---


# DULE Policy Service API開発者ガイド

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platform Data Governanceの中核的なメカニズムです。 DULE PolicyサービスはRESTful APIを提供します。このAPIを使用すると、データ使用ポリシーを作成および管理して、特定のデータ使用ラベルでラベル付けされたデータに対して実行できるマーケティングアクションを決定できます。

このドキュメントでは、Policy Service APIで使用可能な主要操作を実行する手順を説明します。 まだDULEフレームワークに慣れていない場合は、 [データ・ガバナンスの概要を見直して](../home.md) 、DULEフレームワークに慣れてください。 DULEポリシーを作成および適用する手順については、「 [SCHEDULE policy tutorial](../policies/create.md)」を参照してください。

このドキュメントでは、Policy Service APIを呼び出す前に知っておく必要があるコア概念の概要を説明します。

## DULE Policy Serviceの概要

Policy Serviceを使用する前に、Experience Platformのデータに適切なDULEラベルが適用されている必要があります。 データ使用ラベルをデータセットとフィールドに適用する詳しい手順は、『 [DULE labels user guide](../labels/user-guide.md)』を参照してください。

## 前提条件

このガイドでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [Data Governance](../home.md): Experience Platformがデータ使用のコンプライアンスを適用するフレームワークです。
   * [ラベルの集計](../labels/overview.md): データ使用ラベルは、エクスペリエンスデータモデル(XDM)データフィールドに適用され、そのデータへのアクセス方法に関する制限を指定します。
* [Experience Data Model(XDM)System](../../xdm/home.md): エクスペリエンスプラットフォームが顧客エクスペリエンスデータを編成する際に使用する標準化されたフレームワークです。
* [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのプラットフォームインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

## 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Data Governanceに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## コアリソースとカスタムリソース

Policy Service API内では、すべてのポリシーとマーケティングアクションを「または」 `core``custom` リソースと呼びます。

リソースはアドビが定義および管理するものですが、 `core``custom` リソースは個々のお客様が作成および管理するものなので、リソースを作成したIMS組織のみに表示され、一意のものです。 したがって、リソースに対して許可される操作は、リスト化操作と参照操作(`GET`)のみです。一方、リスト化操作、参照操作と更新操作( `core` 、`POST`、 `PUT`および `PATCH``DELETE``custom` )は、リソースに対して使用できます。

## ポリシーの状態

データ使用ポリシーには、次の3つのステータスのいずれかを設定できます。 `DRAFT`、 `ENABLED`または `DISABLED`。

デフォルトでは、「有効」なポリシーのみがポリシー評価に関与します。

「ドラフト」ポリシーもポリシー評価で考慮できますが、クエリパラメーターを設定する場合のみ考慮でき `?includeDraft=true`ます。 ポリシーの評価の詳細については、このドキュメントの最後にある「 [ポリシーの適用に関するドキュメント](../enforcement/overview.md) 」の節を参照してください。

## マーケティングアクション名 {#marketing-actions}

マーケティングアクション名は、マーケティングアクションに対して一意の識別子です。 各 `core` マーケティングアクションには、すべてのIMS組織に適用される一意の名前が付けられます。 これらの名前は、アドビによって定義および管理されます。 一方、顧客定義のマーケティングアクション(`custom` リソース)はすべて、貴社の組織内で一意であり、他のIMS組織と表示または共有されることはありません。

Policy Service APIでマーケティングアクションを使用する手順は、このドキュメントの後半の「 [Marketing Actions](#marketing-actions) 」セクションで説明されています。

## 次の手順

必要な知識と資格情報が揃ったので、この開発者ガイドに記載されているサンプルAPI呼び出しを引き続き読むことができます。

* [マーケティングアクション](marketing-actions.md)
* [ポリシー](policies.md)
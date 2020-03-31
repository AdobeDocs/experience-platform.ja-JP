---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: MODULE Policy Service API開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: eec5b07427aa9daa44d23f09cfaf1b38f8e811f3

---


# MODULE Policy Service API開発者ガイド

Data Usage Labeling and Enforcement(DULE)は、Adobe Experience Platform Data Governanceの中核的なメカニズムです。 DULE Policy Serviceは、データ使用ポリシーを作成および管理し、特定のデータ使用ラベルでラベル付けされたデータに対してどのマーケティングアクションを実行できるかを決定するRESTful APIを提供します。

このドキュメントでは、Policy Service APIで使用できる主要な操作を実行する手順を説明します。 まだDULEフレームワークに慣れていない場合は、まず [Data Governanceの概要を確認し](../home.md) 、DULEフレームワークに慣れてください。 DULEポリシーの作成と適用の手順については、「 [DULE policy tutorial」を参照してください](../policies/create.md)。

このドキュメントでは、Policy Service APIを呼び出す前に知っておく必要があるコア概念の概要を説明します。

## DULE Policy Serviceの概要

Policy Serviceを使用する前に、Experience Platformのデータに適切なDULEラベルが適用されている必要があります。 データ使用ラベルをデータセットおよびフィールドに適用する手順は、『 [DULE labels user guide』を参照してください](../labels/user-guide.md)。

## 前提条件

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

* [データガバナンス](../home.md):Experience Platformがデータの使用に対するコンプライアンスを強制するフレームワークです。
   * [ラベルのスケジュール](../labels/overview.md):データ使用ラベルは、エクスペリエンスデータモデル(XDM)データフィールドに適用され、そのデータへのアクセス方法に関する制限を指定します。
* [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
* [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
* [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

## サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

## 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Data Governanceに属するリソースを含む、エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

* コンテンツタイプ：application/json

## コアリソースとカスタムリソース

Policy Service API内では、すべてのポリシーとマーケティングアクションは、「または」リソースと `core` 呼ばれ `custom` ます。

リソー `core` スはアドビが定義および管理するものですが、リソースは `custom` 個々の顧客が作成および管理するものなので、リソースを作成したIMS組織のみに表示され、一意のものです。 このため、リソースに対して許可される操作は、リストと参照操作(`GET`)のみです。一方、リソースに対しては、リスト、参照操作、および更新操作( `core` 、`POST`、、 `PUT`および `PATCH`)を使用で `DELETE``custom` きます。

## ポリシーの状態

データ使用ポリシーには、次の3つのステータスのいずれかを設定できます。 `DRAFT`、ま `ENABLED`たは `DISABLED`。

デフォルトでは、「有効」ポリシーのみがポリシー評価に関与します。

「ドラフト」ポリシーは、ポリシー評価でも考慮できますが、ポリシーパラメーターを設定することでのみクエリできま `?includeDraft=true`す。 ポリシーの評価の詳細については、このドキュメントの最後にある [「ポリシーの適用に関する](../enforcement/overview.md) ドキュメント」の節を参照してください。

## マーケティングアクション名 {#marketing-actions}

マーケティングアクション名は、マーケティングアクションの一意の識別子です。 各マーケテ `core` ィングアクションには、すべてのIMS組織に適用される一意の名前が付けられます。 これらの名前はアドビによって定義および管理されます。 一方、顧客が定義したマーケティングアクション(`custom` リソース)はすべて、個々の組織内で一意であり、他のIMS組織との表示や共有は行われません。

Policy Service APIでマーケティングアクションを使用する手順は、このドキュメントの後半の「 [Marketing Actions](#marketing-actions) 」セクションで説明されています。

## 次の手順

これで、必要な知識と資格情報が得られたので、この開発者ガイドに記載されているサンプルAPI呼び出しを読み続けることができます。

* [マーケティングアクション](marketing-actions.md)
* [ポリシー](policies.md)
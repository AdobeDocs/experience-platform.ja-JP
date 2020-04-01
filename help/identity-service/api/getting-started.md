---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: はじめに
topic: API guide
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53

---


# IDサービスAPI開発者ガイド

Adobe Experience Platform Identity Serviceは、Adobe Experience Platform内のIDグラフと呼ばれる、デバイス間、チャネル間、およびほぼリアルタイムでの顧客の識別を管理します。

## はじめに

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する作業を理解している必要があります。

- [IDサービス](../home.md):お客様のデータの断片化によって生じる基本的なプロファイルを解決します。 これは、顧客がブランド化するデバイスやシステム間でIDをブリッジすることで実現します。
- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたプロファイルをリアルタイムで提供します。
- [エクスペリエンスデータモデル(XDM)](../../xdm/home.md):プラットフォームが顧客エクスペリエンスデータを整理する際に使用する標準化されたフレームワーク。

以下の節では、Identity Service APIを正しく呼び出すために知っておく必要がある、または手元に置く必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- 認証：無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

- コンテンツタイプ：application/json

### 地域ベースのルーティング

IDサービスAPIは、リクエストパスの一部としてを含める必要がある地域固有のエ `{REGION}` ンドポイントを採用しています。 IMS組織のプロビジョニング中に、領域が決定され、IMS組織の領域に保存されます。プロファイル 各エンドポイントで正しい領域を使用すると、Identity Service APIを使用して行われたすべての要求が適切な領域にルーティングされます。

IDサービスAPIで現在サポートされている領域は2つあります。VA7とNLD2。

次の表に、領域を使用したパスの例を示します。

| サービス | 地域：VA7 | 地域：NLD2 |
| ------ | -------- |--------- |
| IDサービスAPI | https://</span>platform-va7.adobe.</span>io/data/core/identity/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/identity/{ENDPOINT} |
| ID名前空間API | https://</span>platform-va7.adobe.</span>io/data/core/idnamespace/{ENDPOINT} | https://</span>platform-nld2.adobe.</span>io/data/core/idnamespace{ENDPOINT} |

>[!NOTE] 領域を指定しないでリクエストを行うと、誤った領域に対する呼び出しルーティングが発生したり、呼び出しが予期せず失敗したりする場合があります。

IMS組織の地域が見つからない場合は、プロファイル管理者に問い合わせてください。

## IDサービスAPIの使用

これらのサービスで使用されるIDパラメーターは、次の2つの方法のいずれかで表現できます。複合またはXID。

複合IDは、ID値と名前空間の両方を含む構成です。 複合IDを使用する場合、名前空間は名前(`namespace.code`)またはID(`namespace.id`)で指定できます。

IDが永続化されると、IDサービスはネイティブID(XID)と呼ばれるIDを生成し、そのIDに割り当てます。 クラスターAPIとマッピングAPIのすべてのバリエーションは、リクエストと応答で複合IDとXIDの両方をサポートしています。 これらのAPIのいずれか、または `xid` の組み合わせ、お [`ns` よびを使 `nsid`] 用す `id` る必要があります。

応答のペイロードを制限するため、APIは、使用されるID構文のタイプに応じて、それらの応答を調整します。 つまり、XIDを渡すと応答にXIDが含まれ、複合IDを渡すと、応答は要求で使用される構造に従います。

このドキュメントの例では、Identity Service APIの全機能について説明していません。 完全なAPIについては、Swagger APIリファレンスを参 [照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)。

>[!NOTE] 要求でネイティブXIDが使用される場合、返されるIDはすべてネイティブXID形式になります。 ID/名前空間フォームの使用 詳しくは、IDのXIDの取得に関する [節を参照してください](./create-custom-namespace.md)。

## 次の手順

これで、必要な資格情報を収集したので、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切な形式のペイロードを示す **サンプルリクエスト、および正常な呼び** 出しのサンプル応答が含まれます **** 。

---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの概要
topic: overview
translation-type: tm+mt
source-git-commit: 564940f37b66159c84ca7402bd3648010232182b

---


# サンドボックスの概要

Adobe Experience Platformは、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。 企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

In order to address this need, Experience Platform provides **sandboxes** which partition a single Platform instance into separate virtual environments to help develop and evolve digital experience applications.

このドキュメントでは、エクスペリエンスプラットフォームのサンドボックスの概要を説明します。

## サンドボックスについて

サンドボックスは、Experience Platformの単一のインスタンス内の仮想パーティションで、デジタルエクスペリエンスアプリケーションの開発プロセスとシームレスに統合できます。 Experience Platformインスタンスは、1つの実稼働用サンドボックスと複数の非実稼働用サンドボックスをサポートし、各サンドボックスは、プラットフォームリソース(スキーマ、データセット、プロファイルなど)の独立したライブラリを維持します。  サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスにのみ限定され、他のサンドボックスには影響しません。

実稼動用以外のサンドボックスを使用すると、実稼働用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定を行うことができます。 さらに、実稼働用以外のサンドボックスには、顧客が作成したすべてのリソースをサンドボックスから削除するリセット機能があります。 実稼働用以外のサンドボックスは、実稼働用サンドボックスに変換できません。

>[!NOTE] サンドボックスを最初に作成したときは、データは含まれません。 各サンドボックスは独自の独立したデータストアを維持するので、データを個別に取り込む必要もあります。

要約すると、サンドボックスには次の利点があります。

* **アプリケーションのライフサイクル管理**:別々の仮想環境を作成し、デジタルエクスペリエンスアプリケーションを開発し発展させます。
* **プロジェクトとブランドの管理**:分離とアクセス制御を提供しながら、同じIMS組織内で複数のプロジェクトを並行して実行できるようにします。 今後のリリースでは、複数の地域でのデプロイのサポートが提供される予定です。
* **柔軟な開発エコシステム**:調査、有効化、デモの目的で、シームレスで拡張性が高く、コスト効率の高い方法でサンドボックスを提供します。

## アクセス制御

デフォルトでは、組織のすべてのユーザーが実稼働用サンドボックスにアクセスできます。 実稼働以外のサンドボックスへのアクセスは、システム管理者、製品管理者、または製品プロファイル管理者が [Adobe Admin Consoleを通じて許可する必要があります](https://adminconsole.adobe.com)。

非実稼働サンドボックスの表示、作成、更新または削除を行うには、ユーザーにSandbox管理権限も付与されている必要があります。

サンドボックスのロールと権限の管理について詳しくは、 [アクセス制御の概要](../access-control/home.md)。

## エクスペリエンスプラットフォームUIのサンドボックス

ユーザーは [Experience Platformユーザーインターフェイスで](https://platform.adobe.com)**** 、画面の左上にあるサンドボックス切り替えコントロールを使用して、アクセス権を持つサンドボックスを切り替えることができます。  Sandbox管理権限を持つユーザーは、左側のナビゲーションの「 **Sandbox** 」タブにもアクセスでき、組織のサンドボックスの表示と管理を行うことができます。 UIでサンドボックスを使用する方法について詳しくは、Sandboxユーザーガイドを参 [照してください](ui/overview.md)。

## エクスペリエンスプラットフォームAPIのサンドボックス

エクスペリエンスプラットフォームAPIを呼び出す場合は、ヘッダーの下にサンドボックス名を指定する必要がありま `x-sandbox-name`す。 例えば、 [Catalog Service APIを呼び出して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) Production Sandbox内のすべてのデータセットを表示する場合、サンドボックスの名前(「prod」)はAPIリクエストのヘッダーとして提供されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: prod'
```

がAPI呼 `x-sandbox-name` び出しに含まれていない場合、システムは代わりにデフォルトのサンドボックスを使用します。 ただし、デフォルトのサンドボックスを使用している場合でも、常にこのヘッダーをすべてのAPI呼び出しに含めることをお勧めします。 このため、Experience PlatformのAPIドキュメントは必須のヘッダーと `x-sandbox-name` して扱います。

### Sandbox API

Sandbox APIを使用すると、RESTful API操作を使用してサンドボックスを管理できます。 適切にフォーマッ [トされたリクエストや応答例など](api/getting-started.md) 、APIの使用方法について詳しくは、サンドボックス開発者ガイドを参照してください。

## 次の手順

このドキュメントを読むことで、Experience Platformのサンドボックスに関する基本的な概念を紹介します。 サンドボックスの管理方法について詳しくは、UIのユーザーガ [イド](ui/overview.md) 、またはAPIの開発者ガ [イドを参照してください](./api/getting-started.md) 。

サンドボックスは、開発チーム向けのプラットフォーム環境を分離する貴重なツールですが、Adobe Admin Consoleを使用して、より詳細なアクセス制御を管理することもできます。 See the [access control overview](../access-control/home.md) for more information.
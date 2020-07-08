---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの概要
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '680'
ht-degree: 4%

---


# サンドボックスの概要

Adobe Experience Platformは、デジタルエクスペリエンスアプリケーションをグローバルに拡張するために構築されています。 企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

In order to address this need, Experience Platform provides **sandboxes** which partition a single Platform instance into separate virtual environments to help develop and evolve digital experience applications.

このドキュメントでは、Experience Platformのサンドボックスの概要を説明します。

## サンドボックスについて

サンドボックスは、Experience Platformの単一のインスタンス内の仮想パーティションです。これにより、デジタルエクスペリエンスアプリケーションの開発プロセスとのシームレスな統合が可能になります。 Experience Platformインスタンスは、1つの実稼働用サンドボックスと複数の非実稼働用サンドボックスをサポートし、各サンドボックスは独自のPlatformリソースライブラリ(スキーマ、データセット、プロファイルなど)を維持します。  サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスにのみ制限され、他のサンドボックスには影響しません。

実稼動用以外のサンドボックスを使用すると、実稼働用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定を行うことができます。 また、実稼働以外のサンドボックスにはリセット機能があり、お客様が作成したすべてのリソースがサンドボックスから削除されます。 実稼動用以外のサンドボックスは、実稼動用のサンドボックスに変換できません。

>[!NOTE]
>
>サンドボックスが最初に作成されたときは、そのサンドボックスにはデータが含まれません。 各サンドボックスは独自の独立したデータストアを維持するので、データを個別に取り込む必要もあります。

要約すると、サンドボックスには次のような利点があります。

* **アプリケーションライフサイクル管理**: デジタルエクスペリエンスアプリケーションを開発し発展させるために、別々の仮想環境を作成します。
* **プロジェクトとブランドの管理**: 分離とアクセス制御を提供しながら、同じIMS組織内で複数のプロジェクトを並行して実行できるようにします。 今後のリリースでは、複数の地域でのデプロイのサポートが提供される予定です。
* **柔軟な開発エコシステム**: 調査、有効化、デモを行うための、シームレスで拡張性が高く、コスト効率に優れた方法でサンドボックスを提供します。

## サンドボックスのアクセス制御

デフォルトでは、組織のすべてのユーザーが実稼働用サンドボックスにアクセスできます。 実稼動用以外のサンドボックスへのアクセスは、 [AdobeAdmin Consoleを通じて、システム管理者、製品プロファイル管理者または製品管理者によって許可される必要があります](https://adminconsole.adobe.com)。

実稼動用以外のサンドボックスの表示、作成、更新または削除を行うには、ユーザーにもSandboxの管理権限が与えられる必要があります。

サンドボックスのロールおよび権限の管理について詳しくは、 [アクセス制御の概要を参照してください](../access-control/home.md)。

## Experience PlatformUIのサンドボックス

[Experience Platformユーザーインターフェイスで](https://platform.adobe.com)、ユーザーは画面の左上にある **** サンドボックス切り替えコントロールを使用して、アクセス権を持つサンドボックスを切り替えることができます。  Sandbox管理権限を持つユーザーは、左側のナビゲーションにある「 **Sandbox** 」タブにもアクセスできます。このタブでは、組織のサンドボックスの表示と管理を行うことができます。 UIでサンドボックスを使用する方法について詳しくは、 [Sandboxユーザーガイドを参照してください](ui/overview.md)。

## Experience PlatformAPIのサンドボックス

Experience PlatformAPIを呼び出す場合は、ヘッダーの下にサンドボックス名を指定する必要があり `x-sandbox-name`ます。 例えば、実稼働用サンドボックス内のすべてのデータセットを表示するために [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) Catalog Service APIを呼び出す場合、サンドボックス名(「prod」)はAPIリクエストのヘッダーとして提供されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: prod'
```

API呼び出しに含ま `x-sandbox-name` れない場合、システムは代わりにデフォルトのサンドボックスを使用します。 ただし、デフォルトのサンドボックスを使用している場合でも、常にこのヘッダーをすべてのAPI呼び出しに含めることをお勧めします。 このため、Experience Platform用のAPIドキュメントは必須のヘッダー `x-sandbox-name` として扱います。

### Sandbox API

Sandbox APIを使用すると、RESTful API操作を使用してサンドボックスを管理できます。 適切にフォーマットされたリクエストやサン [プル応答など、APIの使用方法に関する詳細については、](api/getting-started.md) サンドボックス開発者ガイドを参照してください。

## 次の手順

このドキュメントを読むことで、Experience Platformのサンドボックスに関する基本的な概念を紹介しました。 サンドボックスの管理方法に関する詳細な手順については、UIの [ユーザーガイド](ui/overview.md) （英語）またはAPIの [開発者ガイド](./api/getting-started.md) （英語）を参照してください。

サンドボックスは開発チームのPlatform環境を分離するための貴重なツールですが、AdobeAdmin Consoleを使用して、より詳細なアクセス制御を管理することもできます。 See the [access control overview](../access-control/home.md) for more information.
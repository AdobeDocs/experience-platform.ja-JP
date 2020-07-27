---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: サンドボックスの概要
topic: overview
translation-type: tm+mt
source-git-commit: c52d8cdbc5a4ee6fab8c2b1b284efea5f735d424
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 95%

---


# サンドボックスの概要

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

このニーズに対応するために、Experience Platform は、**サンドボックス**&#x200B;を提供します。サンドボックスでは、単一の Platform インスタンスを別々の仮想環境に分割することができ、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。

このドキュメントでは、Experience Platform のサンドボックスの概要を説明します。

## サンドボックスについて

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションで、デジタルエクスペリエンスアプリケーションの開発プロセスとシームレスに統合できます。Experience Platform インスタンスは、1 つの実稼働用サンドボックスと複数の非実稼働用サンドボックスをサポートし、各サンドボックスは、Platform リソース（スキーマ、データセット、プロファイルなど）の独立したライブラリを維持します。  サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスにのみ限定され、他のサンドボックスには影響しません。

非実稼働用サンドボックスを使用すると、実稼働用サンドボックスに影響を与えることなく、機能のテスト、実験の実行、カスタム設定をおこなうことができます。さらに、非実稼働用サンドボックスには、顧客が作成したすべてのリソースをサンドボックスから削除するリセット機能があります。非実稼働用サンドボックスを実稼働用サンドボックスに変換することはできません。

>[!NOTE]
>
> サンドボックスを最初に作成したときは、データは含まれません。各サンドボックスは独自の独立したデータストアを維持するので、データを個別に取得する必要もあります。

要約すると、サンドボックスには次の利点があります。

* **アプリケーションのライフサイクル管理**：別々の仮想環境を作成し、デジタルエクスペリエンスアプリケーションを開発し発展させます。
* **プロジェクトとブランドの管理**：分離とアクセス制御を提供しながら、同じ IMS 組織内で複数のプロジェクトを並行して実行できるようにします。今後のリリースでは、複数の地域での展開のサポートが提供される予定です。
* **柔軟な開発エコシステム**：調査、有効化、デモの目的で、シームレスで拡張性が高く、コスト効率の高い方法でサンドボックスを提供します。

## サンドボックスのアクセス制御

デフォルトでは、組織のすべてのユーザーが実稼働用サンドボックスにアクセスできます。非実稼働用サンドボックスへのアクセスは、システム管理者、製品管理者、または製品プロファイル管理者が [Adobe Admin Console](https://adminconsole.adobe.com) を通じて許可する必要があります。

非実稼働サンドボックスの表示、作成、更新、削除をおこなうには、ユーザーにサンドボックス管理権限が付与されている必要があります。

サンドボックスの役割と権限の管理について詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

## Experience Platform UI でのサンドボックス

[Experience Platform ユーザーインターフェイス](https://platform.adobe.com)では、画面の左上にある&#x200B;**サンドボックス切り替え**&#x200B;コントロールを使用して、アクセス権を持つサンドボックス間を切り替えることができます。  サンドボックス管理権限を持つユーザーは、左側のナビゲーションの「**[!UICONTROL サンドボックス]**」タブにアクセスして、組織のサンドボックスの表示と管理をおこなうこともできます。UI でサンドボックスを使用する方法について詳しくは、『[サンドボックスユーザガイド](ui/overview.md)』を参照してください。

## Experience Platform API でのサンドボックス

Experience Platform API を呼び出す場合は、ヘッダーの `x-sandbox-name` でサンドボックス名を指定する必要があります。For example, when making a call to the [!DNL Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) to view all datasets within the Production sandbox, the sandbox&#39;s name (&quot;prod&quot;) is provided as a header in the API request:

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: prod'
```

`x-sandbox-name`が API 呼び出しに含まれていない場合、システムは代わりにデフォルトのサンドボックスを使用します。ただし、デフォルトのサンドボックスを使用している場合でも、ベストプラクティスは常にこのヘッダーをすべての API 呼び出しに含めることです。このため、Experience Platform の API ドキュメントは `x-sandbox-name` を必須のヘッダーとして扱っています。

### サンドボックス API

サンドボックス API を使用すると、RESTful API 操作を使用してサンドボックスを管理できます。適切に書式設定されたリクエストや応答例など、API の使用方法について詳しくは、『[サンドボックス開発者ガイド](api/getting-started.md)』を参照してください。

## 次の手順

このドキュメントでは、Experience Platform のサンドボックスに関する基本的な概念を紹介しました。サンドボックスの管理方法について詳しくは、UI の[ユーザガイド](ui/overview.md) 、または API の[開発者ガイド](./api/getting-started.md)を参照してください。

サンドボックスは、開発チーム向けの Platform 環境を分離する貴重なツールですが、Adobe Admin Console を使用して、より詳細なアクセス制御を管理することもできます。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。
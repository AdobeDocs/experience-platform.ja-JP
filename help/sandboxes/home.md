---
keywords: Experience Platform；ホーム；人気の高いトピック；サンドボックス；サンドボックス；テスト；テスト
solution: Experience Platform
title: サンドボックスの概要
topic-legacy: overview
description: サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションで、デジタルエクスペリエンスアプリケーションの開発プロセスとシームレスに統合できます。
exl-id: b760a979-8134-4a44-8433-ec6fb49bc508
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '1005'
ht-degree: 60%

---

# サンドボックスの概要

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

このニーズに対応するために、Experience Platform は、サンドボックスを提供します。サンドボックスでは、単一の Platform インスタンスを別々の仮想環境に分割することができ、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。

このドキュメントでは、Experience Platform のサンドボックスの概要を説明します。

## サンドボックスについて

サンドボックスは、Experience Platform の単一のインスタンス内の仮想パーティションで、デジタルエクスペリエンスアプリケーションの開発プロセスとシームレスに統合できます。サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスのみに限定され、他のサンドボックスには影響しません。Experience Platformでは、2 種類のサンドボックスがサポートされています。

* **実稼動サンドボックス**:実稼働用サンドボックスは、実稼働環境のプロファイルと共に使用するためのものです。 Platform では、運用の分離を維持しながら、データに適した機能を提供するために、複数の実稼動サンドボックスを作成できます。 この機能を使用すると、特定の実稼動用サンドボックスを、ビジネス、ブランド、プロジェクトまたは地域の異なる行に専用にできます。 実稼働用サンドボックスは、ライセンスを受けた最大量の実稼働用プロファイルをサポートします [!DNL Profile] コミットメント（承認されたすべての実稼動サンドボックス全体で累積的に測定されます）。 権限を持つユーザーごとに、ライセンス付き平均プロファイルを使用する権利があります [!DNL Profile] （承認されたすべての実稼動サンドボックス全体で累積的に測定されます）。
* **開発サンドボックス**:開発サンドボックスは、実稼動以外のプロファイルでの開発およびテストにのみ使用できるサンドボックスです。 開発サンドボックスは、ライセンスを受けたユーザーの最大 10%の非実稼動プロファイルをサポートします [!DNL Profile] コミットメント（承認済みのすべての開発サンドボックス全体で累積的に測定されます）。 次の権限を持っています。
   * 認証済みの非実稼動プロファイルあたりの平均非実稼動プロファイルの充実度は 75 KB です（承認されたすべての開発サンドボックスに累積的に測定されます）。
   * 開発サンドボックスごとに 1 日に 1 つのバッチセグメント化ジョブ
   * 平均 120 [!DNL Profile] API 呼び出し（個別） [!DNL Profile]年ごとに測定されます（認証済みのすべての開発サンドボックスに累積的に測定されます）。

1 つのExperience Platformインスタンスは、複数の実稼動および開発サンドボックスをサポートし、各サンドボックスは、Platform リソース（スキーマ、データセット、プロファイルなど）の独立した独自のライブラリを保持します。 また、実稼働用サンドボックスと開発用サンドボックスの両方に、顧客が作成したすべてのリソースをサンドボックスから削除するリセット機能があります。 開発用サンドボックスを実稼働用サンドボックスに変換することはできません。

デフォルトのExperience Platformライセンスにより、合計 5 つのサンドボックスが許可されます。このサンドボックスは、実稼動または開発として分類できます。 追加のサンドボックス 10 個のサンドボックスのライセンスを、合計で最大 75 個まで取得できます。 これらの追加のサンドボックスは、実稼働用サンドボックスと開発用サンドボックスの両方の作成に使用できます。 詳しくは、IMS 組織管理者またはアドビのセールス担当者にお問い合わせください。

最後に、デフォルトの実稼動サンドボックスは、IMS 組織が最初に作成されたときに作成される最初の実稼動サンドボックスです。 デフォルトの実稼働用サンドボックスを使用すると、Platform からデータを取り込んだり、使用したりできます。また、サンドボックス名やサンドボックス ID の値を含まないリクエストを受け入れることもできます。

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

Experience Platform API を呼び出す場合は、ヘッダーの `x-sandbox-name` でサンドボックス名を指定する必要があります。例えば、 [[!DNL Catalog Service API]](https://www.adobe.io/experience-platform-apis/references/catalog/) 実稼働用サンドボックス内のすべてのデータセットを表示するには、サンドボックスの名前 (「prod」) が API リクエストのヘッダーとして提供されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: prod'
```

`x-sandbox-name`が API 呼び出しに含まれていない場合、システムは代わりにデフォルトのサンドボックスを使用します。ただし、デフォルトのサンドボックスを使用している場合でも、ベストプラクティスは常にこのヘッダーをすべての API 呼び出しに含めることです。このため、Experience Platform の API ドキュメントは `x-sandbox-name` を必須のヘッダーとして扱っています。

### サンドボックス API

サンドボックス API を使用すると、RESTful API 操作を使用してサンドボックスを管理できます。適切に書式設定されたリクエストや応答例など、API の使用方法について詳しくは、『[サンドボックス開発者ガイド](api/overview.md)』を参照してください。

## 次の手順

このドキュメントでは、Experience Platform のサンドボックスに関する基本的な概念を紹介しました。サンドボックスの管理方法について詳しくは、UI の[ユーザガイド](ui/overview.md) 、または API の[開発者ガイド](./api/getting-started.md)を参照してください。

サンドボックスは、開発チーム向けの Platform 環境を分離する貴重なツールですが、Adobe Admin Console を使用して、より詳細なアクセス制御を管理することもできます。詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

---
keywords: Experience Platform;ホーム;人気のトピック;サンドボックス;テスト;
solution: Experience Platform
title: サンドボックスの概要
description: サンドボックスは、Experience Platform の単一インスタンス内の仮想パーティションで、デジタルエクスペリエンスアプリケーションの開発プロセスとシームレスに統合できます。
exl-id: b760a979-8134-4a44-8433-ec6fb49bc508
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 81%

---

# サンドボックスの概要

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

このニーズに対応するために、Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つサンドボックスが用意されています。

このドキュメントでは、Experience Platform のサンドボックスの概要を説明します。

## サンドボックスについて

サンドボックスは、Experience Platform の単一インスタンス内の仮想パーティションで、デジタルエクスペリエンスアプリケーションの開発プロセスとシームレスに統合できます。サンドボックス内で実行されるすべてのコンテンツとアクションは、そのサンドボックスのみに限定され、他のサンドボックスには影響しません。Experience Platform でサポートされているサンドボックスには、次の 2 種類があります。

* **実稼動用サンドボックス**：実稼働用サンドボックスは、実稼働環境のプロファイルと共に使用するためのものです。 Experience Platformでは、運用の分離を維持しながらデータに適した機能を提供するために、複数の実稼動用サンドボックスを作成できます。 この機能を使用すると、特定の実稼動用サンドボックスを、異なる業種、ブランド、プロジェクトまたは地域専用にすることができます。実稼働用サンドボックスでは、ライセンスを取得した [!DNL Profile] ボリュームを上限とする実稼働プロファイル（承認されたすべての実稼働用サンドボックスについて累積的に測定されます）をサポートします。ライセンスを取得した合計データ量全体（承認されたすべての実稼動用サンドボックスについて累積的に測定されます）を使用する権利があります。

* **開発用サンドボックス**：開発用サンドボックスは、非実稼動プロファイルでの開発およびテストにのみ使用できるサンドボックスです。 開発用サンドボックスでは、ライセンスを取得した [!DNL Profile] ボリュームの 10％を上限とする非実稼働プロファイル（許可されたすべての開発用サンドボックスについて累積的に測定されます）をサポートします。 次を上限とする使用権限があります。
   * 開発用サンドボックスごとに 1 日に 1 つのバッチセグメント化ジョブ。
   * [!DNL Profile] ごとに年間平均 120 回の [!DNL Profile] API 呼び出し（許可されたすべての開発用サンドボックスについて累積的に測定されます）。

Experience Platform インスタンスでは、複数の実稼動用および開発用サンドボックスをサポートしており、サンドボックスごとに専用の独立したExperience Platform リソース（スキーマ、データセット、プロファイルなど）ライブラリを維持管理しています。 さらに、実稼動用および開発用サンドボックスには、顧客が作成したすべてのリソースをサンドボックスから削除するリセット機能があります。開発用サンドボックスを実稼動用サンドボックスに変換することはできません。

デフォルトの Experience Platform ライセンスでは合計 5 つのサンドボックスが付与され、これらを実稼動用または開発用として分類できます。 追加のサンドボックスのライセンスを 10 個 1 パック単位で（合計 75 個まで）追加で取得することができます。これらの追加のサンドボックスは、実稼動用サンドボックスと開発用サンドボックスの両方の作成に使用できます。詳しくは、組織の管理者またはアドビのセールス担当者にお問い合わせください。

最後に、デフォルトの実稼動用サンドボックスは、組織が最初に作成される際に作成される最初の実稼動用サンドボックスです。デフォルトの実稼動用サンドボックスを使用すると、Experience Platformからデータを取り込んだり使用したりできます。また、サンドボックス名やサンドボックス ID の値を含まないリクエストを受け入れることもできます。

>[!NOTE]
>
> サンドボックスを最初に作成したときは、データは含まれていません。各サンドボックスは独自の独立したデータストアを維持するので、データを個別に取得する必要もあります。

要約すると、サンドボックスには次の利点があります。

* **アプリケーションのライフサイクル管理**：別々の仮想環境を作成し、デジタルエクスペリエンスアプリケーションを開発し発展させます。
* **プロジェクトとブランドの管理**：分離とアクセス制御を実現しながら、同じ組織内で複数のプロジェクトを並行して実行できるようにします。
* **柔軟な開発エコシステム**：調査、有効化、デモの目的で、シームレスで拡張性が高く、コスト効率の高い方法でサンドボックスを提供します。

## サンドボックスのアクセス制御

デフォルトでは、組織のすべてのユーザーが実稼働用サンドボックスにアクセスできます。非実稼働用サンドボックスへのアクセスは、システム管理者、製品管理者、または製品プロファイル管理者が [Adobe Admin Console](https://adminconsole.adobe.com) を通じて許可する必要があります。

非実稼働サンドボックスの表示、作成、更新、削除をおこなうには、ユーザーにサンドボックス管理権限が付与されている必要があります。

サンドボックスの役割と権限の管理について詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

## Experience Platform UI でのサンドボックス

[Experience Platform ユーザーインターフェイス](https://platform.adobe.com)では、画面の左上にある&#x200B;**サンドボックス切り替え**&#x200B;コントロールを使用して、アクセス権を持つサンドボックス間を切り替えることができます。  サンドボックス管理権限を持つユーザーは、左側のナビゲーションの「**[!UICONTROL サンドボックス]**」タブにアクセスして、組織のサンドボックスの表示と管理をおこなうこともできます。UI でサンドボックスを使用する方法について詳しくは、『[サンドボックスユーザガイド](ui/overview.md)』を参照してください。

## Experience Platform API でのサンドボックス

Experience Platform API を呼び出す場合は、ヘッダーの `x-sandbox-name` でサンドボックス名を指定する必要があります。例えば、[[!DNL Catalog Service API]](https://www.adobe.io/experience-platform-apis/references/catalog/) を呼び出して実稼動用サンドボックス内のすべてのデータセットを表示する場合、サンドボックスの名前（「prod」）は API リクエストのヘッダーとして指定されます。

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

このドキュメントでは、Experience Platform のサンドボックスに関する基本的な概念を紹介しました。サンドボックスの管理方法について詳しくは、UI の[ユーザガイド](ui/overview.md)、または API の[開発者ガイド](./api/getting-started.md)を参照してください。

サンドボックスは、開発チームにとってExperience Platform環境を分離する貴重なツールですが、Adobe Admin Consoleを使用すると、より詳細なアクセス制御を管理することもできます。 詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

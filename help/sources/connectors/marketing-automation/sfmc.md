---
title: Salesforce Marketing Cloud（V2）Sourceの概要
description: API またはユーザーインターフェイスを使用してSalesforce Marketing Cloud（V2）をAdobe Experience Platformに接続する方法について説明します。
source-git-commit: 3c200ff1a29c3462a5d4fef554f6a410cfcbdde8
workflow-type: tm+mt
source-wordcount: '1014'
ht-degree: 1%

---

# [!DNL Salesforce Marketing Cloud] （V2）ソースの概要

>[!IMPORTANT]
>
>元の [[!DNL Salesforce Marketing Cloud]  （V1） &#x200B;](salesforce-marketing-cloud.md) ソースは、2026 年 1 月をもって非推奨（廃止予定）となりました。 この非推奨ソースに利用可能な移行はありません。新しい [!DNL Salesforce Marketing Cloud] （V2）ソースを使用してデータを再実装する必要があります。

Adobe [Real-Time CDP](../../../rtcdp/overview.md) と [!DNL Salesforce Marketing Cloud] の統合は、柔軟性と制御に優れたデータ拡張機能を活用するように設計されています。 事前定義済みのフィールドに限定され、主にシステムレベルのトラッキングを提供する標準のシステムテーブル（データビューと組み込みオブジェクト）とは異なり、データ拡張機能を使用してカスタムフィールドを定義し、ビジネス固有の様々なデータを整理し、独自の要件に合わせてデータ構造を調整できます。

このレベルのカスタマイズと拡張性により、Real-Time CDPと [!DNL Salesforce Marketing Cloud] の統合は、標準のシステムテーブルではなくデータ拡張機能に依存します。 このアプローチは、データを管理するための、より柔軟で拡張性が高く統合された基盤を提供し、ビジネス目標に確実に合致するようにします

[!DNL Salesforce Marketing Cloud] ソースを使用すると、[!DNL Salesforce Marketing Cloud] アカウントをReal-Time CDPとAdobe Experience Platformに接続できます。 開始する方法については、以下のドキュメントを参照してください。

## 使用例 {#use-case-examples}

### 連絡先データを使用したメールキャンペーンのパーソナライズ

小売ブランドが、顧客のライフサイクルステージ（新規顧客、リピート購入者、常連客など）に基づいてメールキャンペーンをパーソナライズしたいと考えています。 これを行うには、連絡先データ拡張機能を作成して、名前、メール、ライフサイクルステージ、購入行動などの主要な顧客情報を保存します。 その後、このデータはExperience Platformに取り込まれ、より深いセグメント化とターゲティングが行われます。

Contact Data Extension からExperience Platformにデータを取り込むことで、ブランドはライフサイクルステージと購入行動に基づいて顧客をセグメント化できます。 例えば、新規顧客に歓迎オファーを送信したり、リピート購入者にロイヤルティ報酬を送信したり、非アクティブな顧客をターゲットオファーで再エンゲージしたりできます。 このアプローチにより、パーソナライズされたコミュニケーションが保証され、より関連性が高く効果的な顧客エンゲージメントが可能になります。

### パーソナライズされたセグメント化のための Campaign データの取り込み

マーケティングチームが、以前のキャンペーンとのエンゲージメントに基づいて顧客をターゲティングすることで、メールキャンペーンを最適化したいと考えています。 これを行うために、[!DNL Salesforce Marketing Cloud] で Campaign データ拡張機能を作成して、メールの開封数、クリック数、キャンペーンの応答などの顧客エンゲージメントデータを保存します。 その後、このデータはExperience Platformに取り込まれ、さらなるセグメント化とパーソナライゼーションが行われます。

このデータを Campaign データ拡張機能からExperience Platformに取り込むことで、マーケティングチームは、製品オファーをクリックした顧客や積極的に応答した顧客など、過去のエンゲージメントに基づいて顧客をセグメント化できます。 エンゲージメントを行った顧客は今後のメールでターゲットを絞ったプロモーションを受け取ることができますが、否定的に回答した顧客はカスタマーサービスのフォローアップを送信できます。 このExperience Platformとの統合により、マーケティングチームが、お客様の行動に基づいて、よりパーソナライズされた適切なコンテンツを提供できるようになります。

### アクティビティデータに基づいた顧客のターゲティング

マーケティングチームは、web サイトの訪問、フォームの送信、以前のメールキャンペーンとのインタラクションなど、顧客のアクティビティに基づいて顧客をターゲットにしたいと考えています。 これを実現するために、アクティビティデータ拡張機能を作成して、各顧客のエンゲージメントアクティビティに関する情報を保存します。 その後、このデータはExperience Platformに取り込まれ、さらなるセグメント化とパーソナライズされたキャンペーンターゲティングが行われます。

マーケティングチームは、アクティビティデータ拡張機能からExperience Platformにデータを取り込むことで、エンゲージメント履歴に基づいて顧客をセグメント化できます。 例えば、最近 web サイトを訪問したものの購入を完了しなかった顧客には、特別オファーが含まれるリマインダーメールを送信できます。 同様に、フォームに入力した顧客は、パーソナライズされたフォローアップコミュニケーションを受け取ることができます。 このアプローチにより、各顧客が最新のアクティビティに基づいて関連性の高いコンテンツを確実に受け取り、エンゲージメントとコンバージョン率を向上させることができます。

## 前提条件 {#prerequisites}

ソースをExperience Platformに接続する前に完了する必要がある前提条件の設定については、以下の節を参照してください。

### 認証用のアプリケーションの設定 {#set-up-application-for-authentication}

[!DNL Salesforce Marketing Cloud] との統合を構築する場合、最初の手順の 1 つは、**内に** インストール済みパッケージ [!DNL Salesforce Marketing Cloud] を作成することです。 インストールされたパッケージは、API 呼び出しの認証に必要なクライアント資格情報を生成し、統合タイプと関連する権限範囲を定義します。 さらに、インストールされたパッケージには、使用しているテナントに適した API エンドポイントが含まれています。 また、アクセスの管理、モニタリング、取り消しを行う管理コンテナとしても機能し、すべての統合のセキュリティが確保され、監査可能となり、Salesforceの推奨認証モデルと連携するようにします。

インストール済みパッケージを作成するには、[!DNL Salesforce Marketing Cloud] UI を使用して **[!DNL Setup]**/**[!DNL Apps]**/**[!DNL Installed Packages]** に移動し、「**[!DNL New]**」を選択します。 [!DNL New Package Details] インターフェイスを使用して、パッケージの名前と情報を指定します。 終了したら「**[!DNL Save]**」を選択します。

![Salesforce Marketing Cloud UI の新しいパッケージインターフェイス &#x200B;](../../images/tutorials/create/sfmc/new-package.png)

新しいパッケージを作成したら、「**[!DNL Add Component]**」を選択します。

![Salesforce Marketing Cloud UI の Add コンポーネントインターフェイス &#x200B;](../../images/tutorials/create/sfmc/add-component.png)

コンポーネントタイプとして **[!DNL API Integration]** を選択します。

![&#x200B; 「API 統合」が選択されたコンポーネント選択ウィンドウ &#x200B;](../../images/tutorials/create/sfmc/api-integration.png)

統合タイプとして **[!DNL Server-to-Server]** を選択します。

![&#x200B; 統合タイプ選択ウィンドウ &#x200B;](../../images/tutorials/create/sfmc/server-to-server.png)

最後に、**[!DNL Scope]**/**[!DNL Data]** に移動します。 「**[!DNL Data Extensions]**」で、「**[!DNL Read]**」を選択します。

![Salesforce Marketing の「Scope」の「data extensions」セクション &#x200B;](../../images/tutorials/create/sfmc/data-extensions.png)

**[!DNL Save]** を選択し、**クライアントシークレット** をコピーして保存します。 完了したら、「**[!DNL Finish]**」を選択します。

![&#x200B; クライアントシークレットを生成するためのSalesforce Marketing Cloud ウィンドウ。](../../images/tutorials/create/sfmc/generate-secret.png)

[!DNL Salesforce Marketing Cloud] UI を離れる前に、**クライアント ID** と **一意のベース URI プレフィックス** をコピーします。両方の値を使用してExperience Platformへの接続を作成するからです。 認証ベース URI の場合は、`.auth.marketingcloudapis.com/` 以降のすべてを削除してください

![&#x200B; クライアント ID と一意のベース URI を取得できるSalesforce Marketing Cloud コンポーネントインターフェイス &#x200B;](../../images/tutorials/create/sfmc/client-id-and-uri.png)

インストールしたパッケージを作成する手順について詳しくは、[[!DNL Salesforce]  ドキュメント &#x200B;](https://trailhead.salesforce.com/content/learn/modules/marketing-cloud-developer-basics/set-up-your-developer-environment) を参照してください。

### 必要な資格情報の収集 {#gather-required-credentials}

Experience Platformに接続するには、次の資格情報の値を指定する必要 [!DNL Salesforce Marketing Cloud] あります。

| 資格情報 | 説明 |
| --- | --- |
| クライアント ID | [!DNL Salesforce Marketing Cloud] がExperience Platformの認証を行う際に、アカウントを識別するために使用する、公開されている識別子。 クライアント ID は、[!DNL Salesforce Marketing Cloud] UI のコンポーネントパネルから取得できます。 |
| クライアントシークレット | クライアントアプリケーションおよび認証サーバーにのみ認識される秘密鍵。 [&#x200B; 前述のアプリケーション設定手順 &#x200B;](#set-up-application-for-authentication) に従って、クライアントシークレットを生成できます。 |
| 基本エンドポイント | [!DNL Salesforce Marketing Cloud] の認証ベース URI のプレフィックス。 |

{style="table-layout:auto"}

## [!DNL Salesforce Marketing Cloud] をExperience Platformに接続

次に、Experience Platform内で [!DNL Salesforce Marketing Cloud] ソース接続を設定します。 UI を使用した接続の設定手順については、[&#x200B; こちらのチュートリアル &#x200B;](../../tutorials/ui/create/marketing-automation/sfmc.md) を参照してください。 このチュートリアルでは、[!DNL Salesforce Marketing Cloud] アカウントの接続、データの選択、フィールドのマッピング、取り込みのスケジュール設定およびデータフローの監視について説明します。

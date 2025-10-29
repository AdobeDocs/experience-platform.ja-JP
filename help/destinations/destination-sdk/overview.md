---
description: Adobe Experience Platform Destination SDKは、Experience Platformの宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントまたはストレージの場所に配信できるようにする設定 API のセットです。 設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。
title: Adobe Experience Platform Destination SDK
exl-id: 7aca9f40-98c8-47c2-ba88-4308fc2b1798
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1012'
ht-degree: 22%

---

# Adobe Experience Platform Destination SDK

Adobe Experience Platform Destination SDKは、Experience Platformの宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントまたはストレージの場所に配信できるようにする設定 API のスイートです。 設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。

Destination SDK ドキュメントでは、Adobe Experience Platform Destination SDKを使用してAdobe Experience Platformとの製品化された宛先統合を設定、テスト、リリースし、増え続ける宛先カタログに宛先を含める手順を説明しています。 Destination SDKを使用すると、独自のカスタムプライベート宛先を作成して、ニーズに合わせてデータを書き出すこともできます。

![&#x200B; 宛先カタログを示す、Experience Platform UI からのスクリーンショット。](assets/destinations-catalog-overview.png)

## クイックスタート – 重要な情報を調べる {#quick-start}

Destination SDKを介した宛先の設定と送信をすばやく開始するには、以下のリンクのドキュメントを参照してください。

>[!BEGINSHADEBOX]

<table style="border: 0;">
  <tbody>
    <tr>
        <td>
            <p><b>設定ページ</b></p>
            <ul>
                <li><a href="/help/destinations/destination-sdk/functionality/configuration-options.md">すべての設定オプション</a></li>
                <li> 宛先サーバー設定 – <a href="/help/destinations/destination-sdk/functionality/destination-server/server-specs.md"> サーバー仕様 </a> および <a href="/help/destinations/destination-sdk/functionality/destination-server/templating-specs.md"> テンプレート仕様 </a></li>
                <li><a href="/help/destinations/destination-sdk/functionality/destination-configuration/customer-data-fields.md">顧客データフィールドおよびその他の宛先設定コンポーネント</a></li>
                <li><a href="https://experienceleague.adobe.com/en/docs/experience-platform/destinations/destination-sdk/functionality/destination-server/message-format">テンプレート化とマクロ</a></li>
            </ul>
        </td>
        <td>
            <p><b>ガイド</b></p>
            <ul>
                <li><a href="/help/destinations/destination-sdk/overview.md#process">大まかな統合プロセス</a></li>
                <li><a href="/help/destinations/destination-sdk/guides/configure-destination-instructions.md">ストリーミング宛先の設定</a></li>
                <li><a href="/help/destinations/destination-sdk/guides/configure-file-based-destination-instructions.md">ファイルベースの宛先の設定</a></li>
                <li><a href="/help/destinations/destination-sdk/guides/batch/configure-prospect-audience-destination.md">見込み客プロファイルをエクスポートする宛先の設定</a></li>
                <li><a href="/help/destinations/destination-sdk/guides/submit-destination.md">公開用に宛先を送信</a></li>
            </ul>
        </td>
                <td>
            <p><b>API リファレンス</b></p>
            <ul>
                <li><a href="https://developer.adobe.com/experience-platform-apis/references/destination-authoring/#tag/Destination-servers-and-templates">宛先サーバーエンドポイント API リファレンス</a></li>
                <li><a href="https://developer.adobe.com/experience-platform-apis/references/destination-authoring/#tag/Destination-configurations">宛先エンドポイント API リファレンス</a></li>
                <li><a href="https://developer.adobe.com/experience-platform-apis/references/destination-authoring/#tag/Audience-metadata-templates">オーディエンスメタデータ API リファレンス</a></li>
                <li><a href="https://developer.adobe.com/experience-platform-apis/references/destination-authoring/#tag/Destination-testing">テスト API リファレンス</a></li>
                <li><a href="https://developer.adobe.com/experience-platform-apis/references/destination-authoring/#tag/Destination-publishing">宛先公開 API リファレンス</a></li>
            </ul>
        </td>
    </tr>
  </tbody>
</table>

<table style="border: 0;">
  <tbody>
    <tr>
        <td>
            <p><b>ストリーミング宛先の設定 – チートシート</b></p>
            <ul>
                <li><a href="/help/destinations/destination-sdk/guides/configure-destination-instructions.md">ストリーミング宛先のエンドツーエンドガイドの設定</a></li>
                <li><a href="/help/destinations/destination-sdk/functionality/destination-server/message-format.md">Pebble テンプレートを使用したデータ変換について </a> および <a href="/help/destinations/destination-sdk/functionality/destination-server/supported-functions.md"> ビューでサポートされるテンプレート関数 </a></li>
                <li><a href="/help/destinations/destination-sdk/functionality/destination-configuration/aggregation-policy.md">データ集計ポリシーについて</a></li>
                <li><a href="https://experienceleague.adobe.com/en/docs/experience-platform/destinations/destination-sdk/functionality/destination-server/message-format">ライブ設定の例</a></li>
                <li><a href="/help/destinations/destination-sdk/testing-api/streaming-destinations/streaming-destination-testing-overview.md">ストリーミングの宛先のテスト</a></li>
            </ul>
        </td>
        <td>
            <p><b>ファイルベースの宛先の設定 – チートシート</b></p>
            <ul>
                <li><a href="/help/destinations/destination-sdk/guides/configure-file-based-destination-instructions.md">ファイルベースの宛先のエンドツーエンドガイドの設定</a></li>
                <li><a href="/help/destinations/destination-sdk/guides/batch/configure-file-formatting-options.md">書き出すファイルのファイル形式の設定</a></li>
                <li><a href="/help/destinations/destination-sdk/guides/batch/configure-amazon-s3-destination-with-predefined-file-formatting.md">Amazon S3 の宛先のライブ設定の例</a></li>
                <li><a href="/help/destinations/destination-sdk/functionality/destination-configuration/batch-configuration.md"> バッチ設定 </a>：ファイルの書き出しスケジュールとファイルの命名</li>
                <li><a href="/help/destinations/destination-sdk/testing-api/batch-destinations/file-based-destination-testing-overview.md">ファイルベースの宛先のテスト</a></li>
            </ul>
        </td>
        <td>
            <p><b>その他の重要な情報</b></p>
            <ul>
                <li><a href="/help/destinations/destination-sdk/getting-started.md#obtain-authentication-credentials">API を使用するために必要な認証資格情報を取得する</a></li>
                <li><a href="/help/destinations/destination-sdk/integration-prerequisites.md">統合の前提条件</a></li>
                <li><a href="/help/destinations/destination-sdk/glossary.md">Destination SDKの用語</a></li>                
                <li><a href="/help/destinations/destination-sdk/functionality/rate-limiting-retry-policy.md">レート制限と再試行ポリシー</a></li>
                <li><a href="/help/destinations/destination-sdk/docs-framework/self-service-template.md">宛先をドキュメント化するセルフサービステンプレート</a></li>
            </ul>
        </td>
    </tr>
  </tbody>
</table>


>[!ENDSHADEBOX]

## 製品化されたカスタム統合 {#productized-custom-integrations}

>[!IMPORTANT]
>
> プライベートなカスタムの宛先を作成するこの機能は、[Adobe Real-Time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) のお客様のみが利用できます。

Destination SDK のパートナーは、製品化された宛先を [Experience Platform カタログ](../catalog/overview.md)に追加することで、次のメリットを得ることができます。

1. 事前設定済みのパラメーターを使用して顧客全体で統合設定を標準化し、顧客の設定エクスペリエンスを簡素化できます。
2. Experience Platform の宛先カタログでブランド化した宛先カードを紹介できるため、顧客の設定の簡素化や認知の向上につながります。
3. Adobe Experience PlatformとAdobe Real-Time Customer Data Platformとの製品化された宛先統合として取り上げられます。

Experience Platformをご利用のお客様は、アクティブ化のニーズに最適な、独自のプライベートなカスタム宛先を作成することもできます。

![&#x200B; 宛先開発者がDestination SDKとやり取りする方法と、Real-Time CDPのお客様が製品化された宛先とプライベートな宛先から得られるメリットを示す概要図。](assets/destination-sdk-visual.png)

## サポートされる統合タイプ {#supported-integration-types}

### リアルタイム（ストリーミング）統合 {#real-time-integrations}

Adobe Experience Platformは、Destination SDKを通じて、REST API エンドポイントを持つ宛先とのリアルタイム（ストリーミングとも呼ばれます）統合をサポートしています。 Experience Platform とのリアルタイム統合は、次のような機能をサポートします。

* メッセージの変換と集計
* プロファイルのバックフィル
* オーディエンス設定とデータ転送を初期化するための設定可能なメタデータ統合
* 設定可能な認証
* 宛先設定をテストして反復するためのテスト API および検証 API のスイート

### ファイルベースの統合 {#file-based-integrations}

Destination SDKを通じて、選択したストレージの場所にファイルを定期的に書き出す統合を設定することもできます。 Experience Platformとのファイルベースの統合は、次のような機能をサポートします。

* サポートされている複数の形式（CSV、Parquet、JSON）でのファイル書き出し
* 設定可能なファイル形式オプション。ダウンストリーム要件を満たすように、書き出すファイルの形式を構造化できます。

宛先側の技術要件については [&#x200B; 統合の前提条件 &#x200B;](integration-prerequisites.md) の記事を、サポートされるすべての設定については [&#x200B; 設定オプション &#x200B;](functionality/configuration-options.md) の記事を参照してください

## Destination SDK へのアクセスの取得 {#get-access}

Destination SDKへのアクセスは、Real-Time CDPのお客様におけるパートナーまたはExperience Platformのステータスによって異なります。 詳しくは、次の表を参照してください。

| パートナーまたは顧客のタイプ | Destination SDK へのアクセス方法 |
|---------|----------|
| 独立系ソフトウェアベンダー（ISV） | [Adobe テクノロジーパートナープログラムに参加し &#x200B;](https://partners.adobe.com/technologyprogram/experiencecloud.html)Destination SDKにアクセスできるようプロビジョニングされたExperience Platform サンドボックスの取得をリクエストします。 |
| システムインテグレーター（SI） | Experience Platform サンドボックスをプロビジョニングしてDestination SDKにアクセスするには、[Adobe ソリューションパートナープログラム &#x200B;](https://solutionpartners.adobe.com/home.html) で Gold レベルまたは Platinum レベルである必要があります。 |
| [Real-Time CDP Ultimate パッケージ &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) のExperience Platformのお客様 | デフォルトでは、Experience Platform サンドボックスとDestination SDKにアクセスでき、組織用のプライベートな宛先を作成できます。 |

{style="table-layout:auto"}

## プロセスの概要 {#process}

Experience Platform で宛先を設定するプロセスの概要を次に示します。

1. ISV または SI の場合は、前述の節に記載されている [&#x200B; アクセスの取得 &#x200B;](#get-access) 情報を参照してください。 [Real-Time CDP Ultimate パッケージ &#x200B;](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) この手順は省略できます。
2. [Experience Platform サンドボックスのプロビジョニングをリクエスト](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360037457812-Adobe-Experience-Platform-Sandbox-Accounts-Access-Adding-Users-and-Support)し、宛先オーサリング権限を有効にします。
3. 統合を構築します。 製品ドキュメントの手順に従って、[&#x200B; ストリーミング宛先 &#x200B;](guides/configure-destination-instructions.md) または [&#x200B; ファイルベースの宛先 &#x200B;](guides/configure-file-based-destination-instructions.md) を設定します。
4. 統合をテストします。 製品ドキュメントの手順に従って、[&#x200B; ストリーミング宛先 &#x200B;](testing-api/streaming-destinations/streaming-destination-testing-overview.md) または [&#x200B; ファイルベースの宛先 &#x200B;](testing-api/batch-destinations/file-based-destination-testing-overview.md) をテストします。
5. [&#x200B; 製品化された統合 &#x200B;](./overview.md#productized-custom-integrations) を作成する ISV または SI の場合は、Adobeのレビュー用に [&#x200B; 統合を送信 &#x200B;](guides/submit-destination.md) します（標準的な応答時間は 5 営業日です）。
6. 製品化された統合を作成する ISV または SI の場合は、[&#x200B; セルフサービスのドキュメントプロセス &#x200B;](docs-framework/documentation-instructions.md) を使用して、宛先の製品ドキュメントページをExperience Leagueに作成します。
7. 製品化された統合の場合、Adobeが承認すると、[Experience Platform カタログ &#x200B;](../catalog/overview.md) に統合が表示されます。
8. 統合を更新する場合は、同じ手順に従います。

## リファレンス {#reference}

アドビでは、次の Experience Platform ドキュメントを読んで理解を深めることをお勧めします。

* [Adobe Experience Platform 宛先の概要](https://experienceleague.adobe.com/docs/experience-platform/destinations/home.html?lang=ja)
* [XDM スキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja)
* [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)

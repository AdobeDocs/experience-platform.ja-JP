---
title: Real-Time CDP製品ドキュメント
description: Adobe Real-Time CDPの使用方法を学ぶ。
solution: Real-Time Customer Data Platform
product: Real-Time Customer Data Platform
source-git-commit: d5a8faa7b854f6d0b4ef36dc86bd78bf4e6ad6f4
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 4%

---

# Adobe [!DNL Real-Time CDP] のドキュメント {#rtcdp-documentation}

Adobe Real-time Customer Data Platform(Real-Time CDP) を使用して、複数のエンタープライズソースから既知の匿名データを統合し、すべてのチャネルとデバイスにわたってリアルタイムにパーソナライズされた顧客体験を提供するための顧客プロファイルを作成します。 詳しくは、 [Real-Time CDPの概要](/help/rtcdp/overview.md) を参照してください。

## 新着情報 {#whats-new}

Real-Time CDP製品およびドキュメントの最新の機能強化の概要を説明します。 機能、改善点、修正の包括的なリストについては、詳細な[リリースノート](/help/release-notes/latest/latest.md)を参照してください。ドキュメントの最新の変更については、 [毎月のリリースノートのドキュメントの更新に関する節](/help/release-notes/latest/latest.md#documentation-updates).

>[!BEGINTABS]

>[!TAB ユースケースプレイブック]

The [!UICONTROL ユースケースプレイブック] の機能は、すべてのReal-Time CDPおよびAdobe Journey Optimizerのユーザーに対して一般に利用できるようになりました。 [!UICONTROL ユースケースプレイブック] は、Real-time Customer Data PlatformまたはAdobe Journey Optimizerを使い始める際に、課題の克服に役立つように設計されています。 開始する場所や目的の使用例に適したアセットの作成方法が不明な場合は、ユースケースプレイブックにインスピレーションが得られ、準備が整ったら実稼動環境にテストして読み込むための様々なアセットを作成できます。

[![画像](assets/do-not-localize/learn-more-button.svg)](/help/use-case-playbooks/playbooks/overview.md)

>[!TAB サンドボックスツール]

サンドボックスツール機能を使用すると、サンドボックス間の設定の精度を向上し、サンドボックス間で設定をシームレスに書き出し、読み込むことができます。 サンドボックスツール機能を使用すると、異なるオブジェクトを選択してパッケージにエクスポートできます。

[![画像](assets/do-not-localize/learn-more-button.svg)](/help/sandboxes/ui/sandbox-tooling.md)

>[!TAB 計算済み属性]

直感的な UI を使用して、イベントデータを簡単にプロファイル属性に要約し、動作ベースのセグメント化、パーソナライゼーション、アクティベーションを強化します。 この機能を使用すると、計算済み属性をセルフサービス方式で作成し、管理し、セグメント化、Real-Time CDP宛先、Adobe Journey Optimizerで使用できます。 さらに、計算済み属性は、セグメント化とジャーニーワークフローを簡素化し、関連するエクスペリエンスをシームレスに配信できるようにします。

[![画像](assets/do-not-localize/learn-more-button.svg)](/help/profile/computed-attributes/overview.md)

>[!TAB データエクスポート]

データセットの書き出し機能が一般に利用できるようになりました。 詳しくは、 [Experience Platformアプリに基づいて書き出すことができるデータセット](../destinations/ui/export-datasets.md#datasets-to-export) 購入した後、 [データセットを書き出すためのガードレール](/help/destinations/guardrails.md#dataset-exports).

[![画像](assets/do-not-localize/learn-more-button.svg)](../destinations/ui/export-datasets.md)

>[!TAB パートナーデータのサポート]

新しい顧客にリーチし、ファーストパーティデータをエンリッチメントするために、パートナーソースの見込み客プロファイルとパートナー ID を使用して、Real-Time CDPで上位ファネルマーケティングを実行します。 任意のデータパートナーからの cookie にも対応した識別子とハッシュ化された PII を活用して、新しいお客様にリーチし、サードパーティ cookie の依存関係を減らします。

[![画像](assets/do-not-localize/learn-more-button.svg)](/help/rtcdp/partner-data/prospecting.md)

>[!ENDTABS]

## 基本について学ぶ {#start-with-basics}

まず、以下のリンクにある資料を読んで、Real-Time CDPの概念、ユーザーインターフェイス、ワークフロー、推奨される使用例に慣れてください。

<table style="table-layout:fixed">
  <tr style="border: 0;">
    <td>
    <a href="/help/rtcdp/get-started.md"><img src="assets/do-not-localize/start-quick.png"></a>
    <div><strong>基本を学ぶ</strong><br/>Real-Time CDPの概要を説明し、Real-Time CDPで大規模にデータを取得、管理、アクティブ化する方法を学びます。</div>
    </td>
    <td>
    <a href="/help/rtcdp/overview.md#rtcdp-editions"><img src="assets/do-not-localize/start-campaign.jpeg"></a>
    <div><strong>私の会社に適したReal-Time CDPエディション</strong><br/>Real-Time CDPの各エディションの違いを理解し、どのエディションが会社に最適かを決定します。</div>
    </td>
    <td>
    <a href="/help/landing/ui-guide.md"><img src="assets/do-not-localize/start-interface.jpeg"></a>
    <div><strong>ユーザーインターフェイス</strong><br/>Real-Time CDP UI ホームページの要素、製品内エクスペリエンスをナビゲートおよび最適化する方法について説明します。</div>
    </td>
    <td>
    <a href="/help/landing/end-to-end-tutorial.md"><img src="assets/do-not-localize/start-journey.jpeg"></a>
    <div><strong>エンドツーエンドのワークフローの調査</strong><br/>取り込みから、プロファイルのエンリッチメントやオーディエンスの作成を通じたデータのフローから、アクティブ化までのデータのフローを理解します。
    </div>
    </td>
    <td>
    <a href="/help/rtcdp/use-case-guides/overview.md"><img src="assets/do-not-localize/start-use-cases.jpeg"></a>
    <div><strong>サンプルReal-Time CDPの使用例</strong><br/>Real-Time CDPが、新しい顧客の獲得、既知のプロファイルの強化などを支援する様々な方法を探ります。
    </div>
    </td>
  </tr>
  <tr style="border: 0;">
    <td align="center"><a href="/help/rtcdp/overview.md"><img src="assets/do-not-localize/learn-more-button.svg"></a></td>
    <td align="center"><a href="/help/rtcdp/overview.md#rtcdp-editions"><img src="assets/do-not-localize/learn-more-button.svg"></a></td>
    <td align="center"><a href="/help/rtcdp/home-page-dashboards.md"><img src="assets/do-not-localize/learn-more-button.svg"></a></td>
    <td align="center"><a href="/help/landing/end-to-end-tutorial.md"><img src="assets/do-not-localize/learn-more-button.svg"></a></td>
    <td align="center"><a href="/help/rtcdp/use-case-guides/overview.md"><img src="assets/do-not-localize/learn-more-button.svg"></a></td>
    </tr>
</table>

## Real-Time CDPの中核となる柱とドキュメントを探る {#explore-core-pillars}

以下に役立つ、Adobe Real-Time CDPの 4 つの主要な柱を理解します。

* 無数のソースコネクタとデータ収集方法を使用して、データを収集または取り込む
* 最も価値の高いプロファイルを管理し、オーディエンスにセグメント化する。
* データの使用に適した様々な規制、制限、ポリシーに準拠しながら、データを管理していることを理解する。
* 顧客に市場を提供し、様々なチャネルでのエクスペリエンスをパーソナライズします。

![Adobe Real-Time CDPの 4 本柱を示すスライドの抜粋。](/help/rtcdp/assets/rtcdp-four-pillars.png)

以下の製品ドキュメントのリンクを参照して、Adobe Real-Time CDPの 4 つの主要な柱が企業の使用例の達成にどのように役立つかを理解してください。

<table style="table-layout:auto">
  <tr style="border: 0;">
    <td>
      <img src="assets/do-not-localize/icon-data.svg" width="35px"><br/>
      <strong>データの取り込みと管理</strong><br/>データをReal-Time CDPに取り込む様々な方法を確認します。 <br/><a href="/help/ingestion/batch-ingestion/overview.md">バッチ取得</a> - <a href="/help/ingestion/streaming-ingestion/overview.md">ストリーミング取得</a> - <a href="/help/sources/home.md">ソース</a> - <a href="/help/xdm/schema/composition.md">スキーマ</a> - <a href="/help/catalog/datasets/overview.md">データセット</a> - <a href="/help/query-service/home.md">クエリ</a>
    </td>
    <td>
      <img src="assets/do-not-localize/icon_profile-audience.svg" width="35px"><br/>
      <strong>プロファイルとオーディエンス</strong><br/>様々なタイプのプロファイルを管理し、共通の関心、行動、特性に基づいてオーディエンスにグループ化します。 <br/><a href="/help/segmentation/home.md">オーディエンス</a> - <a href="/help/profile/home.md">プロファイル</a> - <a href="/help/identity-service/home.md">ID</a> - <a href="/help/dashboards/guides/license-usage.md">ライセンスの使用状況</a> - <a href="/help/privacy-service/home.md">プライバシー管理</a>
    </td>
    <td>
      <img src="assets/do-not-localize/icon-lock.svg" width="35px"><br/>
      <strong>データガバナンスと信頼</strong><br/>データの使用に適した様々な規制、制限、ポリシーへのコンプライアンスを確保する方法を理解します。 <br/><a href="/help/data-governance/home.md">データガバナンス</a> - <a href="/help/data-governance/labels/overview.md">データ使用ラベル</a> - <a href="/help/data-governance/policies/overview.md">データ使用ポリシー</a>
    </td>
    <td>
      <img src="assets/do-not-localize/icon-content.svg" width="35px"><br/>
      <strong>データのアクティベーション</strong><br/>様々なチャネルで顧客に市場を提供し、Real-Time CDPから目的のストレージの場所にデータを書き出します。 <br/><a href="/help/destinations/catalog/overview.md">宛先カタログ</a> - <a href="/help/destinations/destination-types.md">宛先のタイプ</a> - <a href="/help/destinations/ui/activation-overview.md">オーディエンスをアクティブ化</a>  - <a href="/help/destinations/ui/export-datasets.md">データセットを書き出し</a>
    </td>
  </tr>
  <tr style="border: 0;">
    <td>
      <img src="assets/do-not-localize/icon-configure.svg" width="35px"><br/>
      <strong>設定と管理</strong><br/>管理者は、適切な権限、アクセス制御、サンドボックス設定でチームを支援する方法について学びます。 <br/><a href="/help/access-control/home.md">アクセス制御</a> - <a href="/help/access-control/abac/overview.md">属性ベースのアクセス制御</a> - <a href="/help/access-control/abac/end-to-end-guide.md">エンドツーエンドガイド</a> - <a href="/help/sandboxes/home.md">サンドボックス管理</a> 
    </td>
    <td>
      <img src="assets/do-not-localize/icon-cloud.svg" width="35px"><br/>
      <strong>クラウドおよび AI/ML 機能</strong><br/>AI および ML 機能は、複数のダッシュボードでのあらゆる手順で役立ちます。 主な特徴は以下の領域です。 <br/> <a href="/help/segmentation/ui/lookalike-audiences.md">類似オーディエンス</a> - <a href="/help/rtcdp/segmentation/customer-ai.md">顧客 AI</a> - <a href="/help/rtcdp/b2b-ai-ml-services/related-accounts.md">関連するアカウント</a> - <a href="/help/rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md">予測リードとアカウントスコアリング</a> - <a href="/help/rtcdp/b2b-ai-ml-services/lead-to-account-matching.md">リード — アカウント間照合</a>
    </td>
    <td>
      <img src="assets/do-not-localize/icon-learn.svg" width="35px"><br/>
      <strong>ガードレールとベストプラクティス</strong><br/>Real-Time CDPでデータを操作する際のベストプラクティスと現在の制限事項について説明します。<br/><a href="/help/rtcdp/guardrails/overview.md">Guardrails</a> - <a href="/help/landing/license-usage-and-guardrails/data-management-best-practices.md">データ管理ライセンス権限のベストプラクティス</a> - <a href="/help/xdm/schema/best-practices.md">データモデリングのベストプラクティス</a> - <a href="/help/privacy-service/best-practices.md">Privacy Serviceのベストプラクティス</a> 
    </td>
    <td>
      <img src="assets/do-not-localize/icon-code.svg" width="35px"><br/>
      <strong>開発者</strong><br/>Real-Time CDPが提供する様々な API および SDK を使用して、データ取得の設定、プロファイルの管理、オーディエンスの作成などをおこないます。 <br/><a href="/help/landing/api-authentication.md">API の認証と使用の手引き</a> - <a href="https://developer.adobe.com/experience-platform-apis/">完全な API リファレンス</a> - <a href="/help/destinations/destination-sdk/overview.md">Destination SDK</a> - <a href="/help/sources/sources-sdk/overview.md">ソース SDK</a> - <a href="https://developer.adobe.com/client-sdks/home/getting-started/get-the-sdk/">モバイル SDK</a>
    </td>
  </tr>
</table>

## 紹介ビデオ {#featured-videos}

Real-Time CDP、そのアーキテクチャ、インターフェイス、および大きなAdobe Experience Cloud内での適合方法について詳しく理解するには、次の 3 つの紹介ビデオを参照してください。

<table style="margin-top: 0 !important">
<tr>
  <td>
    <a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/native-applications.html">
      <img alt="Real-Time CDP as a Adobe Experience Cloudビデオの一部" src="/help/rtcdp/assets/platform-apps-overview.png" />
    </a>
    <div>
      <a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/native-applications.html">
    <strong>Adobe Experience Cloudの一部としてのReal-Time CDP</strong>
    </a>
    </div>
    <p>
    <em>Real-Time CDPが大きなAdobe Experience Cloud内でどこに適合するかの把握</em>
    <p>
  </td>
  <td>
    <a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/basic-architecture.html?lang=ja">
      <img alt="「Adobe Experience Platformの基本アーキテクチャ」ビデオのサムネール画像" src="/help/rtcdp/assets/platform-architecture-overview.png" />
    </a>
    <div>
      <a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/basic-architecture.html?lang=ja">
    <strong>Adobe Experience Platformの基本アーキテクチャ</strong>
    </a>
    </div>
    <p>
    <em>概要図のガイド付きガイド付きガイド付きのガイド付き説明から、Adobe Experience Platformのアーキテクチャの概要を学びます。</em>
    <p>
  </td>
  <td>
    <a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/interface-tour.html?lang=en">
      <img alt="「Adobe Experience Platformのインターフェイスツアー」ビデオのサムネール画像" src="/help/rtcdp/assets/rtcdp-ui-overview.png" />
    </a>
    <div>
      <a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/interface-tour.html?lang=en">
    <strong>Adobe Experience Platformのインターフェイスツアー</strong>
    </a>
    </div>
    <p>
    <em>Real-Time CDPユーザーインターフェイスの操作方法を説明します</em>
    <p>
  </td>
  </tr>
  </table>

## その他のリソース {#additional-resources}

<table style="table-layout:fixed"><tr style="border: 0;">
<td><strong>Real-Time CDP</strong><br/>
<a href="https://experienceleague.adobe.com/docs/platform-learn/tutorials/overview.html?lang=ja" target="_blank">Tutorials</a> - <a href="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html" target="_blank">製品説明Real-Time CDP B2C Edition</a> - <a href="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html" target="_blank">B2B エディション</a> - <a href="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html" target="_blank">B2B エディション</a> - <a href="https://www.adobe.com/content/dam/cc/en/trust-center/ungated/whitepapers/experience-cloud/ADB_Experience_Platform_Security_Overview.pdf" target="_blank">セキュリティの概要 (PDF)</a> - <a href="https://experienceleague.adobe.com/docs/blueprints-learn/architecture/overview.html?lang=ja" target="_blank">実装ブループリント</a>
</td>
<td><strong>Adobe Experience Platform</strong><br/>
<a href="https://experienceleague.adobe.com/docs/experience-platform/landing/home.html?lang=ja" target="_blank">ドキュメント</a> - <a href="https://developer.adobe.com/experience-platform-apis/" target="_blank">API リファレンス — <a href="https://experienceleague.adobe.com/docs/courses/using/experienceplatform-u-1-2020-1.html" target="_blank">コース：Experience Platformの概要</a></a>
</td>
</tr></table>

<table style="table-layout:auto"><tr style="border: 0;"><td><img src="assets/do-not-localize/newsletter.png"></td><td>
<b>常に情報を提供し、コミュニティに貢献し、Adobe Real-Time CDPのエクスペリエンスを向上させます。</b><br/>Real-time Customer Data Platformコミュニティにアクセスして、実務担当者と機能について話し合ってください。 <a href="https://experienceleaguecommunities.adobe.com/t5/real-time-customer-data-platform/ct-p/Real-time-CDP">今すぐコミュニティに参加</a></td></tr></table>

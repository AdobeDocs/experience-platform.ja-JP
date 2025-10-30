---
keywords: adform 統合；adform;
title: 未認証のリターゲティングの Adform 統合
description: このAdobe Experience Platform統合を使用すると、ECID に基づいてユーザーを再ターゲットできます。
last-substantial-update: 2025-03-26T00:00:00Z
exl-id: 37eb9453-fc3c-481e-94ea-54d9b1545631
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 3%

---

# [!DNL Adform] 拡張機能の概要

[[!DNL Adform]](https://www.adformhelp.com/hc/en-us/articles/29635608709137-Use-the-Adform-S2S-Site-Tracking-Extension-With-Adobe-Experience-Cloud) 拡張機能により、Adobe Experience Platformのサーバーサイドイベント転送が可能になり、広告主、報道機関、パブリッシャーは Adform の ID Fusion システムと直接データを同期できます。 この統合により、企業は複数のチャネルにわたってオーディエンスを引き付け、キャンペーンのパフォーマンスを向上させ、デジタル広告戦略を調整し広告費用対効果を最大化するためのカスタマイズされたソリューションを提供できます。

従来のクライアントサイドトラッキングとは異なり、この拡張機能では、ファーストパーティ ID （特に、Adform と同期される ECID （Experience Cloud ID））を活用することで、サードパーティ cookie が不要になります。 これにより、クライアントサイド JavaScriptのデプロイメントを必要とせずに、シームレスなオーディエンスのリターゲティングが可能になります。

このガイドでは、訪問者のシームレスなリターゲティングを可能にするために、ブランドのデジタルプロパティからAdobe Edge Networkを介して Adform にイベントを転送する拡張機能をインストール、設定、デプロイする方法について説明します。

## オフサイトリターゲティング

オフサイトリターゲティングを通じて、Web サイトを訪問したがコンバージョンしなかった潜在的な顧客を再び関与させることができます。 Adform は、様々なプラットフォームをまたいでこれらのオーディエンスにリーチし、ブランドのプレゼンスを強化し、コンバージョンの機会を増やすのに役立ちます。 この統合を使用すると、次のことができます。

* サードパーティ cookie を使用せずに、不明な訪問者を再エンゲージします。
* サードパーティ cookie 代替識別子やデジタルプロパティの追加タグを使用せずに、ECID で直接オーディエンスをアクティブ化します。

Adform を使用すると、次のことができます。

* ECID で示されるファーストパーティオーディエンスを、デジタル広告キャンペーンのアドレス可能な ID に簡単に変換できます。
* ECID を 40 以上の入札可能な ID ソリューションにリンクし、ターゲットオーディエンスに対するリーチ、頻度、パフォーマンスを最適化します。

### [!DNL Adform] を使用したサーバーサイドのオーディエンスアクティベーション {#server-side-activation}

従来のクライアントサイド ID デプロイメントとは異なり、この統合では、デジタルプロパティに ID ベンダーのソリューションをデプロイする必要はありません。 代わりに、Adform と既に同期されている ECID を活用して、サーバーサイドオーディエンスのアクティベーションを有効にします。 主なメリットは次のとおりです。

* **クライアントサイド JavaScriptのデプロイメントなし**：クライアントサイドの訪問者認識ロジックを管理したり、クライアントサイドの ID を長期間有効な安定したバージョンに復号化したりする必要はありません。
* **シームレスなオーディエンス同期**:ECID は Adform の内部 ID グラフにマッピングされ、プラットフォーム間で効率的にリターゲティングできます。
* **リーチと重複排除の強化**:ID Fusion グラフは、ECID を複数の識別子に接続して、高品質のオーディエンスマッチングを確保します。

## 前提条件 {#prerequisites}

Adform をAdobeと統合する前に、次の前提条件が満たされていることを確認してください。

1. **Adobe Web SDKの設定**: データ収集とイベント転送を容易にするために、Adobe Web SDKを実装する必要があります。

2. **CDP または Connection SKU**：クライアントサイドとサーバーサイドのシームレスな通信を可能にするには、Adobe Customer Data Platform （CDP）PrimeまたはUltimate SKU、または Connection SKU が必要です。

3. **Adobe Experience Platform Edge Networkの設定**:
   * オフサイトリターゲティング用のリアルタイムイベント転送をサポートするようにEdge Networkが設定されていることを確認します。 詳しくは、Adobe[ イベント転送の概要 ](https://experienceleague.adobe.com/en/docs/experience-platform/tags/event-forwarding/getting-started) を参照してください。
   * この手順は、Adform のサーバーサイドエンドポイントにデータを効率的に送信するために重要です。

これらの前提条件が満たされたら、引き続き [!DNL Adform] 拡張機能の設定とデプロイを行うことができます。

## [!DNL Adform] 拡張機能の設定 {#configure-adform-extension}

[!DNL Adform] 拡張機能を設定するには、以下の節で説明する手順に従います。

### 拡張機能のインストールと設定

イベント転送 UI の [!DNL Adform extension] に移動し、必要な値を入力します。

| 入力 | 説明 |
| --- | --- |
| トラッキング設定 ID | Adform がトラッキングイベント用に提供する一意の ID。 |
| グローバルドメイン | `a1.adform.net` を使用すると、パフォーマンスを最適化し、地域ごとの待ち時間の問題を回避できます。 |

これらの詳細を入力した後、設定を保存します。

<!-- ![Installing and configuring the Adform extension in Adobe Experience Platorm]() -->

### トラッキングパラメーターの定義

「追跡」アクションはプライマリイベントルールです。 事前定義済みのトリガーに基づいてアクションが決定されます。通常は、次のパラメーターが含ま `page load.` ます。

**必須パラメーター：**

| パラメーター | 説明 |
| --- | --- |
| `Page Name` | ページまたはユーザーのアクションを識別します。 |
| `User Agent` | オーディエンスマッチング用の情報をキャプチャします。 |
| `IP Address` | 正確なターゲティングとリターゲティングに重要です。 |

**推奨パラメーター：**

| パラメーター | 説明 |
| --- | --- |
| `Page URL` | ページまたはユーザーのアクションを識別します。 |
| `Referral URL` | オーディエンスマッチング用の情報をキャプチャします。 |
| `ECID` | 正確なターゲティングとリターゲティングに重要です。 |
| `Source Domain` | 正確なターゲティングとリターゲティングに重要です。 |

<!-- ![Tracking parameters for Adform]() -->

### ルールを添付

正しく機能させるには、拡張機能をルールに添付する必要があります。 条件が設定されていない場合は、常に実行されるようにグローバルルールを作成します。

>[!NOTE]
>
>拡張機能が起動しない場合は、Adobe Experience Platform Data Collection の有効なルールに拡張機能が添付されていることを確認します。

<!-- ![Attach a rule to the Adform extension]() -->

## 検証とデプロイ

拡張機能がインストールされ、正しく設定されていること、および次を含む必要なすべてのデータ要素がマッピングされていることを確認します。

* [ECID](/help/identity-service/features/ecid.md)
* ページ名
* リファラル URL
* ユーザーエージェント
* IP アドレス

すべての必須フィールドに入力してテストを完了したら、「**ビルド**」を選択して拡張機能をデプロイします。

## 次の手順

これで、Adform とAdobeのサーバーサイド機能との統合方法を理解し、既存のインフラストラクチャ内での統合の実現可能性を評価できるようになりました。 詳しくは、[Adform の公式ドキュメント ](https://www.adformhelp.com/hc/en-us/articles/29635608709137-Use-the-Adform-S2S-Site-Tracking-Extension-With-Adobe-Experience-Cloud) を参照してください。

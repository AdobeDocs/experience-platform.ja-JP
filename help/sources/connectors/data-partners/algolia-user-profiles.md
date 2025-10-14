---
title: Algolia ユーザープロファイルSourceの概要
description: Adobe Experience Platformの Algolia ユーザープロファイルソースについて説明します
last-substantial-update: 2025-04-29T00:00:00Z
exl-id: b35d4753-4c33-4074-9ed5-50f94dedd8a4
source-git-commit: 9bc7d372eba9ffcfe64f90d2d58a532411e5f1ce
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 1%

---

# [!DNL Algolia User Profiles]

[[!DNL Algolia]](https://www.algolia.com/) は、企業が迅速で関連性が高くカスタマイズ可能な検索エクスペリエンスを提供できるようにする、強力な検索および検出 API プラットフォームです。 誤字許容値、フィルタリング、ファセット、AI を利用した関連度の調整などの機能を備えたリアルタイム検索機能を提供します。 [!DNL Algolia] は、Web サイト、e コマースプラットフォーム、アプリケーションに高性能の検索ソリューションを提供することで、企業がユーザーエンゲージメント、コンバージョン率、全体的な顧客体験を向上させるのに役立ちます。

[!DNL Algolia] の主なメリットには、次のようなものがあります。

* 瞬時に結果を得られる高速な検索。
* AI を活用した関連性の高いレコメンデーション。
* ビジネスニーズに優先順位を付けるためのカスタマイズ可能なランキング。
* 高いトラフィック負荷に簡単に対応できる拡張性。

詳しくは、[[!DNL Algolia]  製品ドキュメント &#x200B;](https://resources.algolia.com/) を参照してください。

## アーキテクチャ

セルフサービスソース（バッチ SDK）は、認証、ページネーション、完全なデータ取り込みと部分的なデータ取り込みの両方など、必要なすべての機能を提供します。 [!DNL Algolia User Profiles] ソースは、これらの機能を使用して統合を完了します。

![&#x200B; アルゴリア・Experience Platform統合の在り方 &#x200B;](../../images/tutorials/create/algolia/user-profiles/algolia-aep-user-profiles-arch.png)

## 前提条件 {#prerequisites}

[!DNL Algolia] アカウントをExperience Platformに接続するには、次の前提条件の手順を完了する必要があります。

1. [[!DNL Algolia]  ダッシュボード &#x200B;](https://dashboard.algolia.com/users/sign_up) を使用して、[!DNL Algolia] アカウントにログインするか、新しいアカウントを作成します。
2. [&#x200B; インデックスを準備 &#x200B;](https://www.algolia.com/doc/guides/sending-and-managing-data/prepare-your-data/in-depth/prepare-data-in-depth/) します。
3. [&#x200B; ファセットを設定します &#x200B;](https://www.algolia.com/doc/guides/managing-results/refine-results/faceting/)。
4. [&#x200B; ユーザーイベントの送信 &#x200B;](https://www.algolia.com/doc/guides/sending-events/getting-started/).
5. [&#x200B; インデックスをパーソナライズ &#x200B;](https://www.algolia.com/doc/guides/personalization/advanced-personalization/configure/setup/indices/) します。

### Experience Platformに対する権限の設定

[!DNL Algolia] アカウントをExperience Platformに接続するには、アカウントで **[!UICONTROL ソースの表示]** および **[!UICONTROL ソースの管理]** 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[&#x200B; アクセス制御 UI ガイド &#x200B;](../../../access-control/abac/ui/permissions.md) を参照してください。

### IP アドレスの許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 許可リストに加える詳しくは、[IP アドレス &#x200B;](../../ip-address-allow-list.md) ページを参照してください。

## [!DNL Algolia] アカウントのExperience Platformへの接続

前提条件を満たしたら、次の手順に進み、[&#x200B; アカウントをExperience Platformに接続  [!DNL Algolia]  することができます &#x200B;](../../tutorials/ui/create/data-partners/algolia-user-profiles.md)。

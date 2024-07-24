---
audience: user
user-guide-title: Adobe Experience Platform ID サービス
breadcrumb-title: Platform ID サービスガイド
user-guide-description: デバイスやシステムをまたいで顧客 ID を結び付け、パーソナライズされたデジタルエクスペリエンスを提供します。
feature: Identities
role: Admin,Developer
source-git-commit: 536770d0c3e7e93921fe40887dafa5c76e851f5e
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 75%

---


# Adobe Experience Platform ID サービス {#identity}

- [ID サービスの概要](home.md)
- [ID サービスとリアルタイム顧客プロファイル](identity-and-profile.md)
- 機能 {#features}
   - [ID 名前空間](./features/namespaces.md)
   - [ID リンクロジック](./features/identity-linking-logic.md)
   - [ID グラフビューア](./features/identity-graph-viewer.md)
   - [ID サービスでの削除](./features/deletion.md)
   - のルールをリンクする ID グラフ {#identity-graph-linking-rules}
      - [機能の概要](./identity-graph-linking-rules/overview.md)
      - [設定ガイド](./identity-graph-linking-rules/configuration.md)
      - [ID 最適化アルゴリズム](./identity-graph-linking-rules/identity-optimization-algorithm.md)
      - [名前空間の優先度](./identity-graph-linking-rules/namespace-priority.md)
      - [グラフシミュレーション UI](./identity-graph-linking-rules/graph-simulation.md)
      - [ID 設定](./identity-graph-linking-rules/identity-settings-ui.md)
      - [顧客シナリオの例](./identity-graph-linking-rules/example-scenarios.md)
      - [グラフ設定の例](./identity-graph-linking-rules/example-configurations.md)
   - [ECID の概要](./features/ecid.md)
- [実装ガイド](implementation.md)
- [ID データ用のガードレール](guardrails.md)
- ID サービス API {#api}
   - [はじめに](api/getting-started.md)
   - [フィールドを ID としてラベル付け](api/label-identities.md)
   - [クラスター ID のリスト](api/list-cluster-identites.md)
   - [ID のクラスター履歴のリスト](api/list-cluster-history.md)
   - [ID マッピングのリストの表示](api/list-identity-mappings.md)
   - [名前空間のリスト](api/list-namespaces.md)
   - [カスタム名前空間の作成](api/create-custom-namespace.md)
   - [ID のネイティブ ID のリスト](api/list-native-id.md)
   - [API リファレンス](https://www.adobe.io/experience-platform-apis/references/identity-service)
- [共有デバイスの検出](shared-device-detection.md)
- [UI での ID フィールドの定義](label-identities.md)
- [プライバシーリクエストの処理](privacy.md)
- [トラブルシューティングガイド](troubleshooting-guide.md)
- [Platform リリースノート](https://experienceleague.adobe.com/ja/docs/experience-platform/release-notes/latest)
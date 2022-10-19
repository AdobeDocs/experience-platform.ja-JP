---
title: データストリームの概要
description: クライアントサイドの Experience Platform SDK 統合を、アドビ製品およびサードパーティの宛先と接続します。
keywords: 設定;データストリーム;datastreamId;エッジ;データストリーム id;環境設定;edgeConfigId;ID;id 同期有効;ID 同期コンテナ ID;サンドボックス;ストリーミングインレット;イベントデータセット;Target;クライアントコード;プロパティトークン;Target 環境 ID;Cookie 宛先;url 宛先;Analytics 設定ブロック;レポートスイート id;データ収集のためのデータ準備;データ準備;マッパー;XDM マッパー;エッジ上のマッパー;
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: 2cec87d3f45b1b774925a9b669b53a958e65e57a
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 21%

---

# データストリームの概要

データストリームは、Adobe Experience Platform Web および Mobile SDK を実装する際のサーバーサイド設定を表します。SDK の [configure コマンド](../fundamentals/configuring-the-sdk.md)がクライアント上で処理される必要があるもの（`edgeDomain` など）を制御するのに対して、データストリームは、SDK のその他すべての設定を処理します。リクエストが Adobe Experience Platform Edge Network に送付されると、データストリームを参照するために `edgeConfigId` が使用されます。これにより、web サイト上でコードを変更することなく、サーバーサイド設定を更新できます。

データストリームの作成と管理は、 **[!UICONTROL データストリーム]** ( Adobe Experience Platform UI またはデータ収集 UI 内の左側のナビゲーション )

![UI の「データストリーム」タブ](../assets/datastreams/overview/datastreams-tab.png)

UI でのデータストリームの設定方法について詳しくは、 [設定ガイド](./configure.md).

## データストリーム内の機密データの処理 {#sensitive}

>[!IMPORTANT]
>
>このドキュメントの内容は法的な助言ではなく、その代用になるものでもありません。 機密データの取り扱いに関するアドバイスについては、貴社の法務部門にお問い合わせください。

企業のデータ管理ポリシーおよび規制要件により、機密性の高い顧客データを収集、処理、使用する方法に関する制限が増えています。 これには、HIPAA(Health Insurance Portability and Accountability Act) などの規制の対象となる PHI(Protected Health Data) の収集、処理、使用が含まれます。

データストリームには、機密性の高いデータを安全に処理するための 3 つの方法が用意されています。

* [暗号化の強化](#encryption)
* [データガバナンス](#governance)
* [監査ログ](#audit-logs)

### 暗号化の強化 {#encryption}

Edge ネットワーク経由の送信中のすべてのデータは、 [HTTPS TLS 1.2](https://datatracker.ietf.org/doc/html/rfc5246). データストリームがデータをExperience Platformに取り込む場合、データは暗号化され、Experience Platformデータレイクでの保存時に暗号化されます。 次のドキュメントを参照してください： [Experience Platformのデータ暗号化](../../landing/governance-privacy-security/encryption.md) を参照してください。

### データガバナンス {#governance}

データストリームは、Experience Platformの組み込みのデータガバナンス機能を活用して、HIPAA 対応以外のサービスに機密データが送信されるのを防ぎます。 データストリームスキーマ内の機密データを含む特定のフィールドにラベルを付けることで、特定の目的で使用できるデータフィールドを詳細に制御できます。

次のビデオでは、UI のデータストリームに対するデータ使用制限の設定と適用の仕組みについて簡単に説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3409588/?quality=12&learn=on&speedcontrol=on)

Experience Platformで、 [機密データ使用ラベル](../../data-governance/labels/reference.md#sensitive) を、組織で機密性が高いと見なすデータを含むスキーマおよびフィールドに追加する。 例えば、 `RHD` ラベルは Protected Health Information(PHI) を表すために使用され、 `S1` label は位置情報データを表します。

>[!NOTE]
>
>の [!UICONTROL スキーマ] 」タブ (Experience PlatformUI またはデータ収集 UI) で、 [スキーマのラベル付けのチュートリアル](../../xdm/tutorials/labels.md).

新しいデータストリームを作成する際、選択したスキーマに機密データ使用ラベルが含まれている場合、データストリームは HIPAA 対応の宛先にデータを送信するようにのみ設定できます。 現在、データストリームでサポートされる HIPAA 対応の宛先はAdobe Experience Platformのみです。 Adobe Target、Adobe Analytics、Adobe Audience Manager、イベント転送、エッジ宛先などの他の宛先サービスは、機密データ使用ラベルを含むデータストリームでは無効になります。

HIPAA 対応以外のサービスを持つ既存のデータストリームでスキーマが使用されている場合、機密データ使用ラベルをスキーマに追加しようとすると、ポリシー違反メッセージが表示され、操作が妨げられます。 メッセージは、違反をトリガーしたデータストリームを指定し、問題を解決するために、HIPAA 対応でないサービスをデータストリームから削除することを提案します。

### 監査ログ

Experience Platformでは、データストリームアクティビティを監査ログの形式で監視できます。 監査ログは、に通知します。 **who** 実行済み **what** アクションおよび **when**&#x200B;と共に、データストリームに関する問題のトラブルシューティングに役立つその他のコンテキストデータを使用して、企業のデータ管理ポリシーおよび規制要件にビジネスが準拠するのに役立ちます。

ユーザーがデータストリームを作成、更新、削除するたびに、アクションを記録する監査ログが作成されます。 マッピングを作成、更新、または削除する際にも、同じことが発生します。 [データ収集用のデータ準備](./data-prep.md). データストリームか更新されたマッピングかに関係なく、結果の監査ログは [!UICONTROL データストリーム] リソースタイプ。

詳しくは、 [監査ログ](../../landing/governance-privacy-security/audit-logs/overview.md) データストリームやその他のサポート対象サービスからのログの解釈方法に関する詳細

## 次の手順

このガイドでは、データストリームと、データ収集でのデータストリームの使用および機密データの処理に関する概要を説明しました。 新しいデータストリームの設定手順については、 [datastream 設定ガイド](./configure.md).

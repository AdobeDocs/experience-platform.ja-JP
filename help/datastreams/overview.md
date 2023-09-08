---
title: データストリームの概要
description: クライアントサイドの Experience Platform SDK 統合を、アドビ製品およびサードパーティの宛先と接続します。
keywords: 設定;データストリーム;datastreamId;エッジ;データストリーム id;環境設定;edgeConfigId;ID;id 同期有効;ID 同期コンテナ ID;サンドボックス;ストリーミングインレット;イベントデータセット;Target;クライアントコード;プロパティトークン;Target 環境 ID;Cookie 宛先;url 宛先;Analytics 設定ブロック;レポートスイート id;データ収集のためのデータ準備;データ準備;マッパー;XDM マッパー;エッジ上のマッパー;
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: 5f2358c2e102c66a13746004ad73e2766e933705
workflow-type: ht
source-wordcount: '780'
ht-degree: 100%

---


# データストリームの概要

データストリームは、Adobe Experience Platform Web および Mobile SDK を実装する際のサーバーサイド設定を表します。SDK の [configure コマンド](../edge/fundamentals/configuring-the-sdk.md)がクライアント上で処理される必要があるもの（`edgeDomain` など）を制御するのに対して、データストリームは、SDK のその他すべての設定を処理します。リクエストが Adobe Experience Platform Edge Network に送付されると、データストリームを参照するために `edgeConfigId` が使用されます。これにより、web サイト上でコードを変更することなく、サーバーサイド設定を更新できます。

Adobe Experience Platform UI またはデータ収集 UI 内の左側のナビゲーションの&#x200B;**[!UICONTROL データストリーム]**&#x200B;を選択することで、データストリームを作成および管理できます。

![UI の「データストリーム」タブ](assets/overview/datastreams-tab.png)

UI でのデータストリームの設定方法について詳しくは、[設定ガイド](./configure.md)を参照してください。

## データストリーム内の機密データの取り扱い {#sensitive}

>[!IMPORTANT]
>
>このドキュメントの内容は法的な助言ではなく、法的な助言に代わるものでもありません。機密データの取り扱いに関するアドバイスについて詳しくは、貴社の法務部門にお問い合わせください。

企業のデータ管理ポリシーおよび規制要件では、機密性の高い顧客データを収集、処理、使用する方法に関する制限が増えています。これには、HIPAA（Health Insurance Portability and Accountability Act）などの規制の対象となる 保護対象保健情報（PHI）の収集、処理、使用が含まれます。

データストリームには、機密性の高いデータを安全に取り扱うための 3 つの方法が用意されています。

* [暗号化の強化](#encryption)
* [データガバナンス](#governance)
* [監査ログ](#audit-logs)

### 暗号化の強化 {#encryption}

Edge Network 経由の送信中のすべてのデータは、[HTTPS TLS 1.2](https://datatracker.ietf.org/doc/html/rfc5246) を使用した安全で暗号化された接続を介して行われます。データストリームがデータを Experience Platform に取り込む場合、データは、Experience Platform データレイクでの保管時に暗号化されます。詳しくは、[Experience Platform でのデータ暗号化](../landing/governance-privacy-security/encryption.md)のドキュメントを参照してください。

### データガバナンス {#governance}

データストリームは、Experience Platform の組み込みのデータガバナンス機能を活用して、HIPAA 非対応のサービスに機密データが送信されるのを防ぎます。データストリームスキーマ内の機密データを含む特定のフィールドにラベルを付けることで、どのデータフィールドを特定の目的に使用できるかなどきめ細かく制御できます。

次のビデオでは、UI のデータストリームに対するデータ使用制限の設定と適用の仕組みについて簡単に説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3409588/?quality=12&learn=on&speedcontrol=on)

Experience Platform で、組織で機密性が高いと見なすデータを含むスキーマおよびフィールドに[機密データ使用ラベル](../data-governance/labels/reference.md#sensitive)を適用できます。例えば、`RHD` ラベルは保護対象保健情報（PHI）を表すために使用され、`S1` ラベルは位置情報データを表します。

>[!NOTE]
>
>Experience Platform UI またはデータ収集 UI の「[!UICONTROL スキーマ]」タブ内でデータ使用ラベルを適用する方法について詳しくは、[スキーマのラベル付けのチュートリアル](../xdm/tutorials/labels.md)を参照してください。

新しいデータストリームを作成する際、選択したスキーマに機密データ使用ラベルが含まれている場合、データストリームは HIPAA 対応の宛先にデータを送信するようにのみ設定できます。現在、データストリームでサポートされる HIPAA 対応の宛先は Adobe Experience Platform のみです。Adobe Target、Adobe Analytics、Adobe Audience Manager、イベント転送、エッジ宛先などの他の宛先サービスは、機密データ使用ラベルを含むデータストリームでは無効になります。

HIPAA 非対応のサービスを持つ既存のデータストリームでスキーマが使用されている場合、機密データ使用ラベルをスキーマに追加しようとすると、ポリシー違反メッセージが表示され、操作は阻止されます。メッセージは、違反をトリガーしたデータストリームを指定し、問題を解決するために、HIPAA 非対応サービスをデータストリームから削除することを提案します。

### 監査ログ

Experience Platform では、データストリームアクティビティを監査ログの形式でモニタリングできます。監査ログは、**誰**&#x200B;が&#x200B;**どのような**&#x200B;アクションを&#x200B;**いつ**&#x200B;実行したかを記録し、データストリームに関する問題のトラブルシューティングに役立つその他のコンテキストデータを使用して、企業のデータ管理ポリシーおよび規制要件にビジネスが準拠するのに役立ちます。

ユーザーがデータストリームを作成、更新または削除するたびに、アクションを記録する監査ログが作成されます。[データ収集用のデータ準備](./data-prep.md)を介してユーザーがマッピングを作成、更新または削除するたびに、同じことが発生します。データストリームの更新またはマッピングの更新に関係なく、結果の監査ログは[!UICONTROL データストリーム]リソースタイプ下に分類されます。

データストリームやその他のサポート対象サービスからのログの解釈方法について詳しくは、[監査ログ](../landing/governance-privacy-security/audit-logs/overview.md)のドキュメントを参照してください。

## 次の手順

このガイドでは、データストリームとデータ収集でのデータストリームの使用および機密データの処理に関する概要を説明します。新しいデータストリームの設定手順について詳しく、[データストリーム設定ガイド](./configure.md)を参照してください。

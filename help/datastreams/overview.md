---
title: データストリームの概要
description: クライアントサイドのExperience Platform SDK統合をAdobe製品やサードパーティの宛先と結び付ける上でデータストリームがどのように役立つかを説明します。
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: bb90bbddf33bc4b0557026a0f34965ac37475c65
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 60%

---

# データストリームの概要

データストリームは、Adobe Experience Platform Web および Mobile SDK のサーバーサイド設定を表します。 SDKの [`configure`](/help/collection/js/commands/configure/overview.md) コマンドでクライアントサイドの設定（`edgeDomain` など）を処理するのに対して、データストリームでは、その他すべての設定を管理します。

Edge Networkにリクエストを送信すると、`datastreamId` はデータが送信されたデータストリームを参照します。 これにより、web サイトのコードを変更せずにサーバーサイド設定を更新できます。

Adobe Experience Platform UI またはデータ収集 UI 内の左側のナビゲーションで **[!UICONTROL Datastreams]** を選択することで、データストリームを作成および管理できます。

![UI の「データストリーム」タブ](assets/overview/datastreams-tab.png)

UI でのデータストリームの設定方法について詳しくは、[設定ガイド](./configure.md)を参照してください。

## データストリーム内の機密データの取り扱い {#sensitive}

>[!IMPORTANT]
>
>このドキュメントの内容は法的な助言ではなく、法的な助言に代わるものでもありません。機密データの取り扱いに関するアドバイスについては、会社の法務部門にお問い合わせください。

企業のデータ管理ポリシーおよび規制要件では、機密性の高い顧客データを収集、処理、使用する方法に関する制限が増えています。これには、HIPAA（Health Insurance Portability and Accountability Act）などの規制の対象となる 保護対象保健情報（PHI）の収集、処理、使用が含まれます。

データストリームには、機密データを安全に処理するのに役立つ、次の 3 つの方法が用意されています。

* [暗号化の強化](#encryption)
* [データガバナンス](#governance)
* [監査ログ](#audit-logs)

### 暗号化の強化 {#encryption}

Edge Network 経由の送信中のすべてのデータは、[HTTPS TLS 1.2](https://datatracker.ietf.org/doc/html/rfc5246) を使用した安全で暗号化された接続を介して行われます。データストリームがデータを Experience Platform に取り込む場合、データは、Experience Platform データレイクでの保管時に暗号化されます。詳しくは、[Experience Platform でのデータ暗号化](../landing/governance-privacy-security/encryption.md)のドキュメントを参照してください。

### データガバナンス {#governance}

データストリームは、Experience Platformの組み込みデータガバナンス機能を使用して、機密データが HIPAA に対応していないサービスに送信されるのを防ぎます。 データストリームスキーマ内の機密データを含む特定のフィールドにラベルを付けることで、どのデータフィールドを特定の目的に使用できるかなどきめ細かく制御できます。

次のビデオでは、UI のデータストリームに対するデータ使用制限の設定と適用の仕組みについて簡単に説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3409588/?quality=12&learn=on&speedcontrol=on)

Experience Platform で、組織で機密性が高いと見なすデータを含むスキーマおよびフィールドに[機密データ使用ラベル](../data-governance/labels/reference.md#sensitive)を適用できます。例えば、`RHD` ラベルは保護対象保健情報（PHI）を表すために使用され、`S1` ラベルは位置情報データを表します。

>[!NOTE]
>
>Experience Platform UI またはデータ収集 UI の「[!UICONTROL Schemas]」タブでデータ使用ラベルを適用する方法について詳しくは、[ スキーマのラベル付けに関するチュートリアル ](../xdm/tutorials/labels.md) を参照してください。

データストリームを作成する際に、選択したスキーマに機密のデータ使用ラベルが含まれている場合、データストリームを設定してそのデータを HIPAA 対応の宛先に送信することのみ可能です。 現在、データストリームでサポートされる HIPAA 対応の宛先は Adobe Experience Platform のみです。Adobe Target、Adobe Analytics、Adobe Audience Manager、イベント転送、エッジ宛先などの他の宛先サービスは、機密データ使用ラベルを含むデータストリームでは無効になります。

HIPAA 非対応のサービスを持つ既存のデータストリームでスキーマが使用されている場合、機密データ使用ラベルをスキーマに追加しようとすると、ポリシー違反メッセージが表示され、操作は阻止されます。メッセージは、違反をトリガーしたデータストリームを指定し、問題を解決するためにデータストリームから HIPAA に対応していないサービスを削除することを提案します。

### 監査ログ

Experience Platform では、データストリームアクティビティを監査ログの形式でモニタリングできます。監査ログは、**誰が** 実行 **何** アクション、**いつ** を、データストリームに関連する問題のトラブルシューティングに役立つその他のコンテキストデータと共に示し、企業のデータ管理ポリシーおよび規制要件に準拠するのに役立ちます。

ユーザーがデータストリームを作成、更新または削除するたびに、アクションを記録する監査ログが作成されます。[データ収集用のデータ準備](./data-prep.md)を介してユーザーがマッピングを作成、更新または削除するたびに、同じことが発生します。更新されたデータストリームかマッピングかに関係なく、結果の監査ログは [!UICONTROL Datastreams] リソースタイプに分類されます。

データストリームやその他のサポート対象サービスからのログの解釈方法について詳しくは、[監査ログ](../landing/governance-privacy-security/audit-logs/overview.md)のドキュメントを参照してください。

## 次の手順

このガイドでは、データストリームとデータ収集でのデータストリームの使用および機密データの処理に関する概要を説明します。新しいデータストリームの設定手順について詳しく、[データストリーム設定ガイド](./configure.md)を参照してください。

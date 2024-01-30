---
solution: Experience Platform
title: 既知の制限事項とプレイブックに関する問題のトラブルシューティング
description: プレイブックの既知の問題と一般的な問題と、それらのトラブルシューティング方法について詳しく説明します
exl-id: 2604ce26-bcf9-46e1-bc10-30252a113159
source-git-commit: d6be5d3e21ea924ff98c400b972709b1f60c25eb
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 2%

---


# トラブルシューティングと既知の制限事項 {#troubleshooting-known-limitations}

## トラブルシューティング {#troubleshooting}

### Adobe Journey Optimizerサーフェスが設定されていません

プレイブックのインスタンスを作成すると、次のメッセージが表示される場合があります。

![トラブルシューティング](/help/use-case-playbooks/assets/playbooks/troubleshooting/troubleshooting-ajo.png)

これは、Journey Optimizer Playbook が E メール、プッシュ、SMS チャネル用のメッセージを作成するからです。 詳しくは、 [使い始める](/help/use-case-playbooks/playbooks/get-started.md#configure-sandbox-and-channel-surfaces-in-journey-optimizer) 様々なサーフェスを設定するためのガイド。

### ステータス *失敗* 新しいインスタンスを作成する場合

インスタンスの作成時に失敗したメッセージが表示される場合は、管理者が必要なユーザー権限を付与していない可能性があります。 プレイブックには様々なアセットが多数含まれており、プレイブックのインスタンスを正しく作成するには、ユーザーがこれらのアセットを作成する権限が必要です。 詳しくは、 [権限](/help/use-case-playbooks/playbooks/get-started.md#grant-your-team-the-required-access-permissions) 権限の設定方法に関するこのガイドの節を参照してください。

## 既知の制限事項

プレイブックのインスタンスを作成してアセットを生成する場合、いくつかの既知の制限事項が表示されます。

* 生成されたスキーマについて、スキーマがプレイブックの 1 つのインスタンスで生成され、編集した場合は、別のスキーマ *次の条件を満たさない* プレイブックの別のインスタンスを有効にした場合に生成されます。 代わりに、インスタンス内で編集したスキーマも引き続き使用します。

* を使用する場合、 [データ認識機能](/help/use-case-playbooks/playbooks/data-awareness.md) スキーマをインスピレーション可能なサンドボックスから開発用サンドボックスに昇格させるには、次のようなエラーが発生する場合があります。

![schema-errors](/help/use-case-playbooks/assets/playbooks/troubleshooting/schema-errors.png)

これは、スキーマから生成された一部のフィールドが、コピー先の開発サンドボックスのスキーマに存在しないためです。 そのフィールドを調べなさい。 次に、次の操作をおこなえる開発用サンドボックスに戻ります。

* これらのフィールドを含む新しいフィールドグループを作成するか、
* スキーマに、見つからないフィールドを含む、事前定義済みの標準フィールドグループを含めます。

これらのフィールドを開発サンドボックスのスキーマに含めたら、ワークフローに戻り、スキーマフィールドをインスピレーションを得たサンドボックスから開発サンドボックスにコピーします。 エラーは消えました。

詳しくは、以下のビデオを見て、スキーマフィールドグループを作成してください。

>[!VIDEO](https://video.tv.adobe.com/v/27013/?learn=on)

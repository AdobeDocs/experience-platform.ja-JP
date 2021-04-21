---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Privacy Serviceトラブルシューティングガイド
topic-legacy: troubleshooting
description: このドキュメントでは、Privacy Serviceに関するよくある質問と、APIでよく発生するエラーに関する情報に回答します。
exl-id: 8afbb065-0f41-4048-9003-a22c0c839717
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '884'
ht-degree: 42%

---

# [!DNL Privacy Service] トラブルシューティングガイド

Adobe Experience Platform[!DNL Privacy Service]は、会社が顧客データのプライバシー要求を管理するのに役立つRESTful APIとユーザーインターフェイスを提供します。 [!DNL Privacy Service]を使用すると、個人または個人の顧客データにアクセスして削除する要求を送信でき、組織や法的プライバシーに関する規制への自動コンプライアンスを容易に行うことができます。

このドキュメントでは、[!DNL Privacy Service]に関するよくある質問と、APIでよく発生するエラーに関する情報を提供します。

## API でプライバシーリクエストをおこなう場合、ユーザーとユーザー ID の違いは何ですか？ {#user-ids}

API で新しいプライバシージョブを作成するには、リクエストの JSON ペイロードに、プライバシーリクエストが適用される、各リストに固有の情報をする `users` 配列を含める必要があります。`users` 配列内の各項目は、`key` 値で識別される特定のユーザーを表すオブジェクトです。

次に、各ユーザーオブジェクト（または `key`）には独自の `userIDs` 配列が含まれます。この配列は、**その1 人のユーザーのために**&#x200B;特定の ID 値をリストします。

以下の `users` 配列の例について考えてみましょう。

```json
"users": [
  {
    "key": "DavidSmith",
    "action": ["access"],
    "userIDs": [
      {
        "namespace": "email",
        "value": "dsmith@acme.com",
        "type": "standard"
      }
    ]
  },
  {
    "key": "user12345",
    "action": ["access", "delete"],
    "userIDs": [
      {
        "namespace": "email",
        "value": "ajones@acme.com",
        "type": "standard"
      },
      {
        "namespace": "ECID",
        "type": "standard",
        "value":  "443636576799758681021090721276",
        "isDeletedClientSide": false
      }
    ]
  }
]
```

この配列には 2 つのオブジェクトが含まれ、それぞれの `key` 値（「DavidSmith」と「user12345」）で識別される個々のユーザーを表します。「DavidSmith」には 1 つの ID（電子メールアドレス）しかありませんが、「user12345」には 2 つ（電子メールアドレスと ECID）があります。

ユーザID情報の提供について詳しくは、[プライバシーリクエストの ID データ](identity-data.md)に関するガイドを参照してください。


## [!DNL Privacy Service]を使用して、誤って[!DNL Platform]に送られたデータをクリーンアップできますか。

Adobeでは、誤って製品に送信されたデータの消去に[!DNL Privacy Service]を使用することはサポートされていません。 [!DNL Privacy Service] は、お客様が、データ主体（または消費者）のアクセスや削除のリクエストに対する義務を果たすのを支援するよう設計されています。これらのリクエストは時間に依存し、該当するプライバシー法に従っておこなわれます。データの件名/コンシューマーアクセスまたは削除リクエスト以外のリクエストの送信は、すべての[!DNL Privacy Service]顧客に影響し、[!DNL Privacy Service]が適切な法的日程をサポートする機能に影響します。

PII やデータの問題をなくすための取り組みのレベルを調整および提供するには、担当のアカウントマネージャー（CDM）にお問い合わせください。

## プライバシーリクエストやジョブのステータスに関する情報び取得方法を教えてください。

[!DNL Privacy Service] APIまたはユーザーインターフェイスを使用して、特定のジョブに関する詳細を取得できます。

### API の使用

[!DNL Privacy Service] APIを使用して特定のジョブのステータスを取得するには、リクエストパスでジョブのIDを使用して、ルート(`GET /`)エンドポイントにリクエストを行います。 詳しくは、『 開発者ガイド』で、[ジョブのステータスの確認](api/privacy-jobs.md#check-the-status-of-a-job)に関する節を参照してください。[!DNL Privacy Service]

### UI の使用

すべてのアクティブなジョブ要求は、[!DNL Privacy Service] UIダッシュボードの&#x200B;**[!UICONTROL ジョブ要求]**&#x200B;ウィジェットに一覧表示されます。 各ジョブリクエストのステータスが「**[!UICONTROL ステータス]**」列に表示されます。UI でのジョブリクエストの表示について詳しくは、[Privacy Service のユーザーガイド](ui/user-guide.md)を参照してください。

## 完了したプライバシージョブの結果をダウンロードする方法を教えてください。

[!DNL Privacy Service] APIとユーザーインターフェイスは共に、完了したジョブの結果をZIP形式でダウンロードする方法を提供します。

### API の使用

リクエストパスで結果をダウンロードするジョブの ID を使用して、 API のルート（`GET /`）エンドポイントにリクエストを送信します。[!DNL Privacy Service]ジョブのステータスが完了した場合、API は応答本文に `downloadURL` 属性を含めます。この属性には、ブラウザーのアドレスバーに貼り付けて ZIP ファイルをダウンロードできる URL が含まれます。

詳しくは、『 開発者ガイド』で、[ID によるジョブの検索](api/privacy-jobs.md#check-the-status-of-a-job)に関する節を参照してください。[!DNL Privacy Service]

### UI の使用

[!DNL Privacy Service] UIダッシュボードで、**ジョブリクエスト**&#x200B;ウィジェットからダウンロードするジョブを探します。 ジョブのIDを選択して、ジョブの詳細ページを開きます。 右上隅の「**ダウンロード**」を選択して、ZIPファイルをダウンロードします。 詳細な手順については、『[Privacy Service ユーザーガイド](ui/user-guide.md)』を参照してください。

## 一般的なエラーメッセージ

次の表に、[!DNL Privacy Service]の一般的なエラーの概要と、それぞれの問題の解決に役立つ説明を示します。

| エラーメッセージ | 説明 |
| --- | --- |
| ユーザーIDが見つかりませんでした。 | 要求で指定された一部のユーザーIDが見つからず、スキップされました。 リクエストペイロードで正しい名前空間とID値を使用していることを確認します。 詳しくは、[IDデータ](./identity-data.md)の提供に関するドキュメントを参照してください。 |
| 無効な名前空間 | ユーザーIDに対して入力されたID名前空間が無効でした。 受け入れられる名前空間のリストについては、[!DNL Privacy Service]デベロッパーガイドの付録](./api/appendix.md#standard-namespaces)の[標準ID名前空間の節を参照してください。 カスタム名前空間を使用する場合は、IDの`type`プロパティを「custom」に設定していることを確認してください。 |
| 部分的に完了 | ジョブは正常に完了しましたが、一部のデータが指定された要求に適用されず、スキップされました。 |
| データが必要な形式ではありません。 | 指定されたアプリケーションの1つ以上のデータ値が正しく形式設定されていません。 ジョブの詳細を確認し、詳細を確認します。 |
| IMS組織がプロビジョニングされていません。 | このメッセージは、IMS組織が[!DNL Privacy Service]用にプロビジョニングされていない場合に発生します。 詳細については、管理者に問い合わせてください。 |
| アクセスと権限が必要です。 | [!DNL Privacy Service]を使用するには、アクセスと権限が必要です。 アクセス権を取得するには、管理者に問い合わせてください。 |
| アクセスデータのアップロードとアーカイブで問題が発生しました。 | このエラーが発生した場合は、アクセスデータを再度アップロードして、もう一度お試しください。 |
| 現在のドキュメントレート制限を超えたワークロードです。 | このエラーが発生した場合は、送信率を下げてから、もう一度お試しください。 |

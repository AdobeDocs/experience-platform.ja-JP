---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Service に関する FAQ
topic: troubleshooting
translation-type: tm+mt
source-git-commit: 28b733a16b067f951a885c299d59e079f0074df8
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 47%

---


# [!DNL Privacy Service] トラブルシューティングガイド

Adobe Experience Platform [!DNL Privacy Service] provides a RESTful API and user interface to help companies manage customer data privacy requests. With [!DNL Privacy Service], you can submit requests to access and delete private or personal customer data, facilitating automated compliance with organizational and legal privacy regulations.

このドキュメントでは、に関するよくある質問 [!DNL Privacy Service]と、APIでよく発生するエラーに関する情報を提供します。

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


## Can I use [!DNL Privacy Service] to clean up data that was accidentally sent to [!DNL Platform]?

Adobe does not support using [!DNL Privacy Service] for clearing out data that was accidentally submitted to a product. [!DNL Privacy Service] は、お客様が、データ主体（または消費者）のアクセスや削除のリクエストに対する義務を果たすのを支援するよう設計されています。これらのリクエストは時間に依存し、該当するプライバシー法に従っておこなわれます。Submission of requests which are not data-subject/consumer access or delete requests impacts all [!DNL Privacy Service] customers and the ability for [!DNL Privacy Service] to support the appropriate legal timelines.

PII やデータの問題をなくすための取り組みのレベルを調整および提供するには、担当のアカウントマネージャー（CDM）にお問い合わせください。

## プライバシーリクエストやジョブのステータスに関する情報び取得方法を教えてください。

You can retrieve details about a particular job by using the [!DNL Privacy Service] API or user interface.

### API の使用

To retrieve the status of a particular job using the [!DNL Privacy Service] API, make a request to the root (`GET /`) endpoint, using the job&#39;s ID in the request path. 詳しくは、『 開発者ガイド』で、[ジョブのステータスの確認](api/privacy-jobs.md#check-the-status-of-a-job)に関する節を参照してください。[!DNL Privacy Service]

### UI の使用

All active job requests are listed in the **[!UICONTROL Job Requests]** widget on the [!DNL Privacy Service] UI dashboard. 各ジョブリクエストのステータスが「**[!UICONTROL ステータス]**」列に表示されます。UI でのジョブリクエストの表示について詳しくは、[Privacy Service のユーザーガイド](ui/user-guide.md)を参照してください。

## 完了したプライバシージョブの結果をダウンロードする方法を教えてください。

The [!DNL Privacy Service] API and user interface both provide methods for downloading the results of completed jobs in ZIP format.

### API の使用

リクエストパスで結果をダウンロードするジョブの ID を使用して、 API のルート（`GET /`）エンドポイントにリクエストを送信します。[!DNL Privacy Service]ジョブのステータスが完了した場合、API は応答本文に `downloadURL` 属性を含めます。この属性には、ブラウザーのアドレスバーに貼り付けて ZIP ファイルをダウンロードできる URL が含まれます。

詳しくは、『 開発者ガイド』で、[ID によるジョブの検索](api/privacy-jobs.md#check-the-status-of-a-job)に関する節を参照してください。[!DNL Privacy Service]

### UI の使用

On the [!DNL Privacy Service] UI dashboard, find the job you want to download from the **Job Requests** widget. ジョブの ID をクリックして、ジョブの詳細ページを開きます。右上隅の「**ダウンロード**」をクリックして、ZIP ファイルをダウンロードします。詳細な手順については、『[Privacy Service ユーザーガイド](ui/user-guide.md)』を参照してください。

## 一般的なエラーメッセージ

次の表に、の一般的なエラーの概要と、それぞれの問題の解決に役立つ [!DNL Privacy Service]説明を示します。

| エラーメッセージ | 説明 |
| --- | --- |
| ユーザーIDが見つかりませんでした。 | 要求で指定された一部のユーザーIDが見つからず、スキップされました。 リクエストペイロードで正しい名前空間とID値を使用していることを確認します。 詳しくは、IDデータの [提供に関するドキュメント](./identity-data.md) （英語）を参照してください。 |
| 無効な名前空間 | ユーザーIDに対して入力されたID名前空間が無効でした。 受け入れられる名前空間のリストについては、『 [開発者ガイド付録』の「](./api/appendix.md#standard-namespaces) 標準ID名前空間 [!DNL Privacy Service] 」の節を参照してください。 カスタム名前空間を使用する場合は、IDのプロパティを「custom」に設定しているこ `type` とを確認してください。 |
| 部分的に完了 | ジョブは正常に完了しましたが、一部のデータが指定された要求に適用されず、スキップされました。 |
| データが必要な形式ではありません。 | 指定されたアプリケーションの1つ以上のデータ値が正しく形式設定されていません。 ジョブの詳細を確認し、詳細を確認します。 |
| IMS組織がプロビジョニングされていません。 | このメッセージは、IMS組織がに対してプロビジョニングされていない場合に発生し [!DNL Privacy Service]ます。 詳細については、管理者に問い合わせてください。 |
| アクセスと権限が必要です。 | を使用するには、アクセスと権限が必要で [!DNL Privacy Service]す。 アクセス権を取得するには、管理者に問い合わせてください。 |
| アクセスデータのアップロードとアーカイブで問題が発生しました。 | このエラーが発生した場合は、アクセスデータを再度アップロードして、もう一度お試しください。 |
| 現在のドキュメントレート制限を超えたワークロードです。 | このエラーが発生した場合は、送信率を下げてから、もう一度お試しください。 |
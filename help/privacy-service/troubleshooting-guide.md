---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy ServiceFAQ
topic: troubleshooting
translation-type: tm+mt
source-git-commit: 5b32c1955fac4f137ba44e8189376c81cdbbfc40
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---


# [!DNL Privacy Service] トラブルシューティングガイド

Adobe Experience Platform [!DNL Privacy Service] は、会社が顧客データのプライバシーリクエストを管理するのに役立つRESTful APIおよびユーザーインターフェイスを提供します。 ま [!DNL Privacy Service]た、個人または個人の顧客データにアクセスして削除する要求を送信でき、組織や法的プライバシーに関する規制への自動コンプライアンスが容易になります。

このドキュメントでは、に関するよくある質問 [!DNL Privacy Service]と、APIでよく発生するエラーに関する情報を提供します。

## APIでプライバシーリクエストを行う場合、ユーザーとユーザーIDの違いは何ですか。 {#user-ids}

APIで新しいプライバシージョブを作成するには、リクエストのJSONペイロードに、プライバシーリクエストが適用される各ユーザーに対して特定のリストを含む `users` 配列を含める必要があります。 配列内の各項目は、 `users` 値で識別される特定のユーザーを表すオブジェクトで `key` す。

次に、各ユーザーオブジェクト(または `key`)には、それぞれ独自の `userIDs` 配列が含まれます。 この配列は、その特定 **のユーザーに固有のID値をリストします**。

Consider the following example `users` array:

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

この配列には2つのオブジェクトが含まれ、それぞれの `key` 値で識別される個々のユーザー（「DavidSmith」と「user12345」）を表します。 「DavidSmith」には1つのリストID（電子メールアドレス）しかありませんが、「user12345」には2つのID（電子メールアドレスとECID）があります。

ユーザーID情報の提供について詳しくは、プライバシー要求の [IDデータに関するガイドを参照してください](identity-data.md)。


## を使用して、誤って送信 [!DNL Privacy Service] されたデータをクリーンアップできま [!DNL Platform]すか。

誤って製品に送信されたデータ [!DNL Privacy Service] の消去の使用は、アドビではサポートしていません。 [!DNL Privacy Service] は、データ件名（または消費者）へのアクセスや削除の要求に対する義務を満たすのに役立つように設計されています。 これらの要求は時間に依存し、適用されるプライバシー法に関するものです。 データの件名/コンシューマーアクセスまたは削除のリクエスト以外のリクエストの送信は、すべての [!DNL Privacy Service] お客様に影響を与え、の適切な法的日程 [!DNL Privacy Service] をサポートする機能に影響します。

PIIやデータの問題を取り除くための作業レベルを、アカウントマネージャ(CDM)に問い合わせて調整し、ご連絡ください。

## プライバシーの要請や仕事の状態に関する情報を取得する方法を教えてください。

特定のジョブに関する詳細を取得するには、 [!DNL Privacy Service] APIまたはユーザーインターフェイスを使用します。

### APIの使用

APIを使用して特定のジョブのステータスを取得するには、リクエストパスでジョブのIDを使用して、ルート( [!DNL Privacy Service]`GET /`)エンドポイントにリクエストを行います。 詳しくは、『 [開発者ガイド』のジョブのステータスの](api/privacy-jobs.md#check-the-status-of-a-job) チェックに関する節を参照してください [!DNL Privacy Service] 。

### UIの使用

すべてのアクティブなジョブ要求は、 **[!UICONTROL UIダッシュボードの]** ジョブ要求 [!DNL Privacy Service] Widgetに一覧表示されます。 各ジョブリクエストのステータスが「 **[!UICONTROL ステータス]** 」列に表示されます。 UIでのジョブ要求の表示について詳しくは、 [Privacy Serviceユーザーガイドを参照してください](ui/user-guide.md)。

## 完了したプライバシージョブの結果をダウンロードするにはどうしますか？

APIとユーザーインターフェイスは共に、完了したジョブの結果をZIP形式でダウンロードする方法を提供します。 [!DNL Privacy Service]

### APIの使用

結果を要求パスでダウンロードするジョブのIDを使用して、`GET /`APIのルート( [!DNL Privacy Service] )エンドポイントにリクエストを行います。 ジョブのステータスが完了した場合、APIは応答本文に `downloadURL` 属性を含めます。 この属性にはURLが含まれ、このURLをブラウザーのアドレスバーに貼り付けてZIPファイルをダウンロードできます。

詳しくは、 [開発者ガイドのIDによるジョブの](api/privacy-jobs.md#check-the-status-of-a-job)[!DNL Privacy Service] 検索に関する節を参照してください。

### UIの使用

UI [!DNL Privacy Service] ダッシュボードで、 **ジョブ要求ウィジェットからダウンロードするジョブを探します** 。 ジョブのIDをクリックして、 _ジョブの詳細_ ページを開きます。 ここから、右上隅の「 **ダウンロード** 」をクリックしてZIPファイルをダウンロードします。 詳細な手順については、『 [Privacy Serviceユーザガイド](ui/user-guide.md) 』を参照してください。

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
---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy Serviceトラブルシューティングガイド
topic-legacy: troubleshooting
description: このドキュメントでは、Privacy Serviceに関するよくある質問と、API でよく発生するエラーに関する情報を提供します。
exl-id: 8afbb065-0f41-4048-9003-a22c0c839717
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '884'
ht-degree: 43%

---

# [!DNL Privacy Service] トラブルシューティングガイド

Adobe Experience Platform [!DNL Privacy Service] は、企業が顧客データのプライバシーリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスを提供します。 [!DNL Privacy Service] を使用すると、組織や法的なプライバシー規制への自動コンプライアンスを促進し、非公開または個人の顧客データに対するアクセスおよび削除のリクエストを送信できます。

このドキュメントでは、[!DNL Privacy Service] に関するよくある質問と、API でよく発生するエラーに関する情報を提供します。

## API でプライバシーリクエストをおこなう場合、ユーザーとユーザー ID の違いは何ですか？  {#user-ids}

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


## [!DNL Privacy Service] を使用して、[!DNL Platform] に誤って送信されたデータをクリーンアップすることはできますか？

Adobeは、誤って製品に送信されたデータの消去に [!DNL Privacy Service] を使用することをサポートしていません。 [!DNL Privacy Service] は、お客様が、データ主体（または消費者）のアクセスや削除のリクエストに対する義務を果たすのを支援するよう設計されています。これらのリクエストは時間に依存し、該当するプライバシー法に従っておこなわれます。データ主体/消費者以外のアクセス要求や削除要求の送信は、すべての [!DNL Privacy Service] のお客様と、[!DNL Privacy Service] が適切な法的日程をサポートする機能に影響を与えます。

PII やデータの問題をなくすための取り組みのレベルを調整および提供するには、担当のアカウントマネージャー（CDM）にお問い合わせください。

## プライバシーリクエストやジョブのステータスに関する情報び取得方法を教えてください。

[!DNL Privacy Service] API またはユーザーインターフェイスを使用して、特定のジョブに関する詳細を取得できます。

### API の使用

[!DNL Privacy Service] API を使用して特定のジョブのステータスを取得するには、リクエストパスでジョブの ID を使用して、ルート (`GET /`) エンドポイントにリクエストを送信します。 詳しくは、『 開発者ガイド』で、[ジョブのステータスの確認](api/privacy-jobs.md#check-the-status-of-a-job)に関する節を参照してください。[!DNL Privacy Service]

### UI の使用

すべてのアクティブなジョブリクエストは、[!DNL Privacy Service] UI ダッシュボードの **[!UICONTROL ジョブリクエスト]** ウィジェットに表示されます。 各ジョブリクエストのステータスが「**[!UICONTROL ステータス]**」列に表示されます。UI でのジョブリクエストの表示について詳しくは、[Privacy Service のユーザーガイド](ui/user-guide.md)を参照してください。

## 完了したプライバシージョブの結果をダウンロードする方法を教えてください。

[!DNL Privacy Service] API とユーザーインターフェイスの両方で、完了したジョブの結果を ZIP 形式でダウンロードする方法が用意されています。

### API の使用

リクエストパスで結果をダウンロードするジョブの ID を使用して、 API のルート（`GET /`）エンドポイントにリクエストを送信します。[!DNL Privacy Service]ジョブのステータスが完了した場合、API は応答本文に `downloadURL` 属性を含めます。この属性には、ブラウザーのアドレスバーに貼り付けて ZIP ファイルをダウンロードできる URL が含まれます。

詳しくは、『 開発者ガイド』で、[ID によるジョブの検索](api/privacy-jobs.md#check-the-status-of-a-job)に関する節を参照してください。[!DNL Privacy Service]

### UI の使用

[!DNL Privacy Service] UI ダッシュボードで、**ジョブリクエスト** ウィジェットからダウンロードするジョブを探します。 ジョブの ID を選択して、ジョブの詳細ページを開きます。 右上隅の「**ダウンロード**」を選択して、ZIP ファイルをダウンロードします。 詳細な手順については、『[Privacy Service ユーザーガイド](ui/user-guide.md)』を参照してください。

## 一般的なエラーメッセージ

次の表に、[!DNL Privacy Service] の一般的なエラーと、それぞれの問題の解決に役立つ説明を示します。

| エラーメッセージ | 説明 |
| --- | --- |
| ユーザー ID が見つかりませんでした。 | 要求で指定された一部のユーザー ID が見つからず、スキップされました。 リクエストペイロードで正しい名前空間と ID 値を使用していることを確認します。 詳しくは、[ID データの提供 ](./identity-data.md) に関するドキュメントを参照してください。 |
| 無効な名前空間 | ユーザー ID に指定された ID 名前空間が無効でした。 受け入れられる名前空間のリストについては、『[!DNL Privacy Service] 開発者ガイドの付録』の「[ 標準 ID 名前空間 ](./api/appendix.md#standard-namespaces)」の節を参照してください。 カスタム名前空間を使用する場合は、ID の `type` プロパティを「custom」に設定します。 |
| 部分的に完了 | ジョブは正常に完了しましたが、一部のデータは指定されたリクエストに適用できず、スキップされました。 |
| データが必要な形式ではありません。 | 指定されたアプリケーションの 1 つ以上のデータ値の形式が正しくありません。 詳細については、ジョブの詳細を確認してください。 |
| IMS 組織がプロビジョニングされていません。 | このメッセージは、IMS 組織が [!DNL Privacy Service] 用にプロビジョニングされていない場合に発生します。 詳しくは、管理者にお問い合わせください。 |
| アクセス権と権限が必要です。 | [!DNL Privacy Service] を使用するには、アクセス権と権限が必要です。 アクセス権を取得するには、管理者に問い合わせてください。 |
| アクセスデータのアップロードとアーカイブで問題が発生しました。 | このエラーが発生した場合は、アクセスデータを再度アップロードして、もう一度やり直してください。 |
| 現在のドキュメントレート制限を超えたワークロードです。 | このエラーが発生した場合は、送信率を下げて、もう一度やり直してください。 |

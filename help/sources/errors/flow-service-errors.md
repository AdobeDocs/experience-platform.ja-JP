---
title: フローサービスエラーメッセージ
description: ソースに対してフローサービスを使用する際に発生する可能性のあるエラーメッセージについて説明します。
exl-id: af79c547-25d0-459a-8de7-eb14206a8694
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '1698'
ht-degree: 5%

---

# フローサービスのエラーメッセージ

フローサービスは、Platform 内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスにはユーザーインターフェイスおよび RESTful API が用意されており、様々なデータプロバイダーへのソース接続を簡単に設定できます。これらのソース接続を使用すると、サードパーティ製システムの認証、データ取り込み時間の設定、データ取り込みスループットの管理を行うことができます。

このドキュメントでは、フローサービスに関するエラーメッセージ、説明、推奨される解決策のカタログを提供します。

## フローサービスの内部検証エラー

次の表に、フローサービスの内部検証に関するエラーの概要を示します。

| エラーコード | タイトル | 詳細なメッセージ |
| --- | --- | --- |
| `1100-400` | 無効なリクエスト | リクエストを処理できませんでした。 フロープロバイダーからのエラー：この操作に対する権限がありません。 |
| `1101-404` | リソースが見つかりません | リクエストされたリソースが見つかりません。 フロープロバイダーからのエラー：指定された ID のリソースは存在しません。 |
| `1102-500` | 内部エラー | 内部エラーが発生しました。 再試行してください。問題が解決しない場合は、カスタマーサポートにお問い合わせください。 |
| `1103-503` | サービス利用不可 | サービスは一時的に利用できなくなっています。 再試行してください。問題が解決しない場合は、カスタマーサポートにお問い合わせください。 |
| `1104-504` | ゲートウェイタイムアウト | ゲートウェイのタイムアウトが発生しました。 再試行してください。問題が解決しない場合は、カスタマーサポートにお問い合わせください。 |
| `1400-500` | 内部エラー | 内部エラーが発生しました。 再試行してください。問題が解決しない場合は、カスタマーサポートにお問い合わせください。 |
| `1401-400` | 無効なリクエスト | 同じリクエスト内で limit パラメーターと count パラメーターを同時に指定することはできません。 制限またはカウントパラメーターのみを指定して、もう一度試してください。 |
| `1402-400` | 無効なリクエスト | アクション「finalize」は、プロバイダーリクエストに対してのみサポートされます。 |
| `1403-400` | ヘッダーがありません | リクエストにヘッダー「If-Match」がありません。 ヘッダーを指定して、もう一度試してください。 |
| `1404-412` | バージョンが一致しません | 指定されたバージョン「v1」は、エンティティ「cc01fc2c-0000-0200」の現在のバージョンと一致しません。 提供されたバージョンがエンティティの現在のバージョンと一致していることを確認して、もう一度試してください。 |
| `1405-400` | 無効なリクエスト | リクエスト本文を null または空にすることはできません。 リクエストの本文を指定して、もう一度試してください。 |
| `1406-400` | 無効なリクエスト | サブ操作「progress」を他のサブ操作に適用することはできません。 サブ操作リストを更新し、再試行してください。 |
| `1407-400` | 無効なリクエスト | 失敗したステータスは、 `progress` subOp. を削除してください `progress` subOp を使用するか、status=&#39;inProgress&#39;を使用して、もう一度試してください。 |
| `1408-400` | 無効なリクエスト | 完了または失敗の状態の場合、完了率は 100 にする必要があります。 完了率を更新し、再度お試しください。 |
| `1409-400` | 無効なリクエスト | 現在の状態で有効になっている最終処理に対して操作「finalize」を適用することはできません。 操作を更新し、再度お試しください。 |
| `1410-400` | 無効なリクエスト | `State` は、フロー作成リクエストでは許可されていません。 |
| `1411-400` | 無効なリクエスト | entityId を取得できません。 無効なリクエストをパス「baseConnections/123」で受信しました。 |
| `1412-400` | 無効なリクエスト | リクエストのマッピングバージョンが無効です。 正しいマッピングバージョンを指定してください。 |
| `1413-400` | 無効なリクエスト | 指定された仕様 ID が無効です。 有効な仕様 ID を指定して、もう一度やり直してください。 |
| `1414-400` | 無効なリクエスト | 接続の接続仕様を更新できません。 |
| `1415-400` | 無効なリクエスト | 接続仕様 ID ba6e206f-f233-ab16 の認証仕様「authConnection」が見つかりませんでした。 |
| `1416-400` | 無効なリクエスト | 削除または更新は、有効、無効、または初期化状態の接続でのみ適用できます。 |
| `1417-400` | 無効なリクエスト | の接続での削除または更新 `initializing` 状態は UserToken では許可されていません。 |
| `1418-400` | 無効なリクエスト | ID 35dcaad3-122a-4278 のベース接続は、1 つ以上のフローでベース接続が使用されているので、削除できません。 ベース接続を削除する前に、対応するフローが削除されていることを確認してください。 |
| `1419-400` | 無効なリクエスト | ID 45d90285d2d249acb87a72a2f12f7401、バージョン 0 のマッピングの検証中にエラーが発生しました。 これは、マッピングされたフィールドに対する権限が不十分なためです。 マッピングを確認するか、管理者にお問い合わせください。 |
| `1420-400` | 無効なリクエスト | 現在の無効化状態は更新できません。 |
| `1421-400` | 無効なリクエスト | 現在の状態の更新を遷移できません。 |
| `1422-400` | 無効なリクエスト | 無効にするアクションは、現在の状態 {state} に適用できません。 アクションを更新し、再度お試しください。 |
| `1423-400` | 無効なリクエスト | ConnectionSpecFiltering で未処理のフィールド baseSpec が提供されました。 フィールド {field} を更新して、もう一度試してください。 |
| `1424-400` | 無効なリクエスト | OrderBy は、クロスサンドボックスクエリではサポートされていません。 |
| `1425-400` | 無効なリクエスト | 91ac5a2c0eb のマッピングで、ターゲットデータセット 64ef1a3c0ef のスキーマとスキーマのマッチング中にエラーが発生しました。 マッピングとターゲットのデータセットの両方で、同じ ID とバージョンのスキーマを使用する必要があります。 |
| `1426-400` | 無効なリクエスト | ユーザートークンは、接続仕様を作成または更新する権限がありません。 ユーザートークンが認証されていることを確認し、もう一度試してください。 |
| `1427-400` | 無効なリクエスト | ユーザートークンは、フロー実行を作成/更新する権限がありません。 ユーザートークンが認証されていることを確認し、もう一度試してください。 |
| `1428-400` | 無効なリクエスト | ID aa6a206f-f233-4c2d の先行フロー実行が見つかりません。 |
| `1429-400` | 無効なリクエスト | ID aa6a206f-f233-4c2d の先行フローが見つかりません。 |
| `1430-400` | 無効なリクエスト | ID aa6a206f-f233-4c2d のフローが見つかりません。 |
| `1431-400` | 無効なリクエスト | ポリシーでは空の配列「fields」を使用できません。 |
| `1432-400` | 無効なリクエスト | 指定した並べ替え順は正しくありません。前に+を付け、降順に — を付けます。 |
| `1433-400` | 無効なリクエスト | 間違った field= #idが orderBy に渡されました。例： +name、-name、name。 |
| `1434-400` | 無効なリクエスト | サポートされていない操作「移動」を受け取りました。 サポートされている操作の種類の 1 つを使用して、もう一度試してください。 |
| `1435-400` | 無効なリクエスト | サポートされていないサブアクション「reverse」を受信しました。 サポートされているサブアクションタイプの 1 つを使用して、もう一度試してください。 |
| `1436-400` | 無効なリクエスト | 操作の有効化に対して、サブ操作 PROGRESS を受け取りました。 |
| `1437-400` | 無効なリクエスト | サポートされていないステータス値「started」を受け取りました。 ステータス値を更新し、再度お試しください。 |
| `1438-400` | 無効なリクエスト | サポートされていない集計操作「average」を受信しました。 |
| `1439-400` | リソースが見つかりません | リクエストされたフローリソース 3f4ae131-b384-4e73 が見つかりません。 再試行する前に、リソース ID を確認してください。 |
| `1440-400` | 無効なリクエスト | オペレーター「～」が実装されていません。 |
| `1441-400` | 無効なリクエスト | 集計関数「average」が実装されていません。 |
| `1442-400` | 無効なリクエスト | フィールド ID で集計を使用できません。 |
| `1443-400` | 無効なリクエスト | 演算子「&lt;=」は複数の値に使用できません。 |
| `1444-400` | 無効なリクエスト | フロー 3f4ae131-b384-4e73 は、予期された状態ではありません。 |
| `1445-400` | 無効なリクエスト | PATCH接続機能が有効になっていません。 |
| `1446-400` | 無効なリクエスト | フロー ID を null または空にすることはできません。 フロー ID を更新し、再度お試しください。 |
| `1447-400` | 無効なリクエスト | ID aa6a206f-f233-4c2d を含むアクティブフローが見つかりました。 |
| `1448-400` | 無効なリクエスト | 演算子 >=は、フィールド ID ではサポートされていません。 |
| `1449-400` | 無効なリクエスト | フィルターパラメーターのフィールド接続が無効です。 |
| `1450-400` | 無効なリクエスト | 無効な演算子です。> （フィルターパラメーター内） |
| `1451-400` | 無効なリクエスト | クエリパラメーターに無効な params testParam が指定されました。 |
| `1452-400` | 無効なリクエスト | フィールド createdAt の値1676643256、1676643210が無効です。 |
| `1453-400` | 無効なリクエスト | クエリパラメーターで無効なクエリ値 createdAt==が指定されました。 |
| `1454-400` | 無効なリクエスト | 未処理のフィルター値のタイプが指定されました。 |
| `1455-400` | 無効なリクエスト | パッチの手順の検証中にエラーが発生しました：フィールド「keyWhichDoesNotExist」がありません。 |
| `1456-400` | 無効なリクエスト | 状態の削除の最終処理が有効な値ではありません。 使用できる値は次のとおりです。 [有効、無効、初期化]. |
| `1457-400` | 無効なリクエスト | 状態の削除/最終処理では、操作の更新を適用できません。 |
| `1458-400` | 無効なリクエスト | 状態の削除/最終処理では、操作の削除を適用できません。 |

{style="table-layout:auto"}


## 認証用のユーザートークンの検証エラー

次の表に、認証用のユーザートークンの検証に関するエラーの概要を示します

| エラーコード | タイトル | 詳細なメッセージ |
| --- | --- | --- |
| `2000-401` | 無効な認証トークン | 認証トークンはこの組織へのアクセス権を持っていないか、組織が存在しません。 組織が存在することを確認するか、管理者に連絡してアクセス権を取得してください。 |
| `2001-401` | ヘッダーがないか空です | ヘッダー x-gw-ims-org-id がないか、空です。 ヘッダー値を更新し、再度お試しください。 |
| `2002-401` | ヘッダーがありません | リクエストにヘッダー x-gw-ims-org-id がありません。 ヘッダー値を更新し、再度お試しください。 |
| `2100-404` | サンドボックスが見つかりません | 「dev」という名前のサンドボックスが見つかりませんでした。 サンドボックス名が正しいことを確認して、もう一度試してください。 |
| `2101-404` | サンドボックスが見つかりません | 「dev」という名前のサンドボックスが見つかりませんでした。 サンドボックス管理 API からのエラー：「dev」という名前のサンドボックスが存在しません。 リソースが存在するかどうかを確認してください。 |
| `2102-500` | 名前によるサンドボックスの取得エラー | 「dev」という名前のサンドボックスを取得する際に問題が発生しました。 サンドボックス管理 API からのエラー：問題が発生しました。 再試行してください。 |
| `2103-404` | サンドボックスが見つかりません | ID が 8da3ef09-b469-404a で、開発名が dev のサンドボックスが見つかりませんでした。 ID とサンドボックス名の値が正しいことを確認して、もう一度試してください。 |
| `2104-500` | 識別子によるサンドボックスの取得エラー | ID が「8da3ef09-b469-404a」で「dev」という名前のサンドボックスを取得する際に問題が発生しました。 サンドボックス管理 API からのエラー：問題が発生しました。 再試行してください。 |
| `2105-400` | ヘッダーがありません | リクエストにヘッダー x-sandbox-name がありません。 リクエストにヘッダーを追加して、もう一度試してください。 |
| `2106-404` | デフォルトのサンドボックスの取得エラー | デフォルトのサンドボックス情報が見つかりませんでした。 |
| `2107-500` | デフォルトのサンドボックスの取得エラー | デフォルトのサンドボックスの取得中に問題が発生しました。 サンドボックス管理 API からのエラー：問題が発生しました。 再試行してください。 |
| `2108-500` | アクティブなサンドボックスの取得エラー | アクティブなサンドボックスの取得中に問題が発生しました。 サンドボックス管理 API からのエラー：問題が発生しました。 再試行してください。 |
| `2110-400` | 許可されていないヘッダー | ヘッダー x-sandbox-id はユーザートークンでは使用できません。 ヘッダーを更新し、再度お試しください。 |
| `2111-403` | 値が制限されたヘッダー | 値*を含むヘッダー x-sandbox-name は、ユーザートークンで制限されています。 ヘッダーと値を更新し、再度お試しください。 |
| `2112-400` | 異なる値を持つヘッダーは許可されていません | ヘッダー x-sandbox-name と x-sandbox-id の両方に、クロスサンドボックスクエリの値*が必要です。 ヘッダーを更新し、再度お試しください。 |
| `2113-400` | 値を持つヘッダーは許可されていません | 値*を持つヘッダー x-sandbox-id と x-sandbox-name は、リクエストでは許可されません。 ヘッダーを更新し、再度お試しください。 |
| `2114-400` | 空のトークン | 空のトークンが指定されました。 トークンを指定して、もう一度やり直してください。 |
| `2115-400` | 無効なトークン | 無効なトークンが提供されました。 有効なトークンを指定して、もう一度やり直してください。 |
| `2116-500` | 内部エラー | スコープ解決のための bearer トークンのオフラインデコード中に内部エラーが発生しました。 |
| `2117-500` | 内部エラー | ユーザーの権限の確認中に内部エラーが発生しました。 再試行してください。問題が解決しない場合は、カスタマーサポートにお問い合わせください。 |
| `2118-403` | 権限がありません | 権限の書き込みがありません。 管理者に連絡して権限を解決してから、もう一度やり直してください。 |
| `2119-400` | クエリパラメーターがありません | リクエストコンテキストに、必要なクエリパラメーター specId が含まれていません。 見つからないクエリパラメーターを指定して、もう一度試してください。 |
| `2120-403` | 禁止されています | この操作を実行する権限がありません。 管理者に連絡して権限を解決してから、もう一度やり直してください。 |
| `2121-403` | 禁止されています | 権限管理は、リソースのエンタープライズソースで拒否されました。 管理者に連絡して権限を解決してから、もう一度やり直してください。 |
| `2200-500` | 外部サービスリクエストエラー | スキーマの検証用にカタログサービスを呼び出す際に問題が発生しました。 |
| `2250-500` | 外部サービスリクエストエラー | Data Prep サービスを呼び出してマッピングを検証する際に問題が発生しました。 |

{style="table-layout:auto"}

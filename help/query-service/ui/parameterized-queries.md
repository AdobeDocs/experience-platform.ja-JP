---
title: パラメーター化クエリ
description: Adobe Experience Platform UI でパラメーター化クエリを使用する方法を説明します。
exl-id: 5c5ac691-5e29-4262-ba53-84dcc56e744f
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '681'
ht-degree: 13%

---

# パラメーター化クエリ {#parameterized-queries}

>[!CONTEXTUALHELP]
>id="platform_queryService_queryEditor_parameterizedQueries"
>title="パラメーター化クエリ"
>abstract="パラメーター化クエリを使用して、実行時にパラメーター値を追加します。これにより、動的データを操作し、様々なユースケースでクエリを再利用できます。`'$'` の序文を使用して、テキストエディターでクエリにクエリパラメーターを入力します。次に、エディターの下にある「クエリパラメーター」セクションで、キーの値を追加します。"

クエリ サービス では、クエリエディター でのパラメーター化クエリの使用がサポートされています。 パラメーター化されたクエリでは、パラメーターのプレースホルダーを使用して、実行時にパラメーター値を追加できるようになりました。 プレースホルダーを使用すると、ステートメントが実行されるまで値が何であるかわからない動的データを操作できます。 また、クエリを事前に準備し、同様の目的で再利用することもできます。 クエリを再利用すると、ユースケースごとに個別の SQL クエリを作成しなくなるため、かなりの労力を節約できます。

## 前提条件

このガイドを続ける前に、 [クエリエディター UI ガイド](./user-guide.md)をお読みください。 クエリエディターガイドでは、Experience Platform ユーザーインターフェイス内でカスタマーエクスペリエンス（顧客体験）データのクエリを記述、検証および実行する方法について詳しく説明しています。

>[!NOTE]
>
>Adobe Experience Platform UI 内では、パラメーター化クエリは、インラインテンプレートの親レベルでのみサポートされます。 つまり、パラメーター化されたクエリは、元のテンプレートで使用された場合にのみ機能します。子テンプレートは静的テンプレートでなければならず、動的パラメーターを持つことはできません。 詳しくは、[ インラインテンプレートのドキュメント ](../key-concepts/inline-templates.md) を参照してください。

## パラメーター化クエリ構文 {#syntax}

パラメーター化されたクエリでは、形式 `'$YOUR_PARAMETER_NAME'` が使用され、ドット表記を使用して連結できます。 パラメーター化されたクエリを使用するSQL 文の例を以下に示します。

```sql
INSERT INTO
   $Database_Name.Schema_Name.adwh_lkup_process_delta_log
   (process_name, merge_policy_id, process_status, process_date, create_ts, change_ts)
SELECT
   '$Table_Process_Name' process_name,
   hash('$Merge_PolicyID') merge_policy_id,
   '$process_status' process_status,
   to_date('$date_key') process_date,
   CURRENT_TIMESTAMP create_ts,
   CURRENT_TIMESTAMP change_ts;
```

## パラメーター化されたクエリ作成 {#create}

UIにパラメーター化されたクエリを作成するには、クエリエディターに移動します。 詳細については、 [クエリエディターへのアクセス](./user-guide.md#accessing-query-editor) のセクションを参照してください。

`'$'` の序文を使用して、テキストエディターでクエリにクエリパラメーターを入力します。次へ、**[!UICONTROL Query parameters]**&#x200B;の横にある[!UICONTROL Console]タブを選択し、キーの欠損値を追加します。必要なキーのいずれかに値を追加しないと、クエリを実行できません。 アラートアイコン(![アラートアイコン。](/help/images/icons/alert.png))が表示されます。「クエリーパラメーター」セクションで、空の [!UICONTROL Value] 入力フィールドの隣に表示されます。

>[!NOTE]
>
>クエリでパラメーターが指定されない場合でも、クエリエディター内で不要なパラメーターを入力できます。 クエリエディターでは、不要なキーと値のペアはすべて無視され、クエリの実行や結果には影響しません。

![ パラメーター化されたクエリと「クエリパラメーター」セクションがハイライト表示されたクエリエディター。](../images/ui/parameterized-queries/parameterized-query.png)

>[!TIP]
>
>タブを [!UICONTROL Query parameters] から [!UICONTROL Console] に変更して、クエリのコンソール出力を表示します。

## クエリログの詳細を使用してパラメーター値を確認 {#check-parameter-values}

使用される値は永続的ではないため、テンプレート内にパラメーターを保存することはできません。 ただし、 [!UICONTROL Query log details] ページを確認して、クエリ実行で使用されるパラメーター値を見つけることができます。 この場合、ログは、クエリがパラメーター化されたクエリであったことを示しません。 使用されている値を見つける方法については[](./query-logs.md)クエリログのドキュメントを参照してください。

![クエリ ログは、詳細セクションで強調表示されているパラメーター化されたクエリの SQL と共に表示されます。](../images/ui/parameterized-queries/parameterized-query-logs.png)

<!-- improve screenshot above ^ I am waiting for a scheduled run to complete -->

## パラメーター化されたクエリスケジュール {#schedule}

パラメーター値は、パラメーター化されたクエリをスケジュールするときに保存されます。 パラメータ化されたクエリをスケジュールするには、ガイド [クエリスケジュールを作成する](./query-schedules.md#create-schedule)で説明されているように、スケジュールされたクエリを作成する一般的なプロセスフォローする、クエリ実行で使用するパラメータ値を入力します。 このUIセクションは、パラメーター化されたクエリに対してのみ表示されます。 詳しくは、[ スケジュールされたパラメーター化クエリのパラメーターの設定 ](./query-schedules.md#set-parameters) の節を参照してください。

>[!TIP]
>
>クエリサービスは、パラメーター化されたクエリを使用して、準備済みステートメントをサポートします。 使用される SQL 構文について詳しくは、[ 準備文構文ガイド ](../sql/prepared-statements.md) を参照してください。

## 次の手順

このドキュメントでは、Adobe Experience Platform UI でクエリをパラメーター化し、スケジュールされたクエリの実行で使用する方法について説明しました。 また、このドキュメントでは、クエリの実行で使用されるパラメーター値のログを確認する方法についても重点的に説明しました。

次へ、 [スケジュールされたクエリの監視](./monitor-queries.md) に関するガイドを読み、Experience Platform UIを通じてすべてのクエリジョブのステータスをよりよく理解することをお勧めします。

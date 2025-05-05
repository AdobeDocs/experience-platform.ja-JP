---
title: パラメーター化クエリ
description: Adobe Experience Platform UI でパラメーター化クエリを使用する方法を説明します。
exl-id: 5c5ac691-5e29-4262-ba53-84dcc56e744f
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 13%

---

# パラメーター化クエリ {#parameterized-queries}

>[!CONTEXTUALHELP]
>id="platform_queryService_queryEditor_parameterizedQueries"
>title="パラメーター化クエリ"
>abstract="パラメーター化クエリを使用して、実行時にパラメーター値を追加します。これにより、動的データを操作し、様々なユースケースでクエリを再利用できます。`'$'` の序文を使用して、テキストエディターでクエリにクエリパラメーターを入力します。次に、エディターの下にある「クエリパラメーター」セクションで、キーの値を追加します。"

クエリサービスでは、クエリエディターでのパラメーター化クエリの使用がサポートされています。 パラメーター化クエリで、パラメーターのプレースホルダーを使用し、実行時にパラメーター値を追加できるようになりました。 プレースホルダーを使用すると、ステートメントが実行されるまで値が不明な動的データを操作できます。 また、事前にクエリを準備して、同様の目的で再利用することもできます。 クエリを再利用すると、ユースケースごとに個別の SQL クエリを作成しなくて済むので、かなりの労力を節約できます。

## 前提条件

このガイドを進める前に、[ クエリエディター UI ガイド ](./user-guide.md) を参照してください。 クエリエディターガイドでは、Experience Platform ユーザーインターフェイス内でカスタマーエクスペリエンス（顧客体験）データのクエリを記述、検証および実行する方法について詳しく説明しています。

>[!NOTE]
>
>Adobe Experience Platform UI 内では、パラメーター化クエリは、インラインテンプレートの親レベルでのみサポートされます。 つまり、パラメーター化されたクエリは、元のテンプレートで使用された場合にのみ機能します。子テンプレートは静的テンプレートでなければならず、動的パラメーターを持つことはできません。 詳しくは、[ インラインテンプレートのドキュメント ](../key-concepts/inline-templates.md) を参照してください。

## パラメーター化クエリ構文 {#syntax}

パラメーター化クエリは、形式 `'$YOUR_PARAMETER_NAME'` を使用し、ドット表記を使用して連結できます。 パラメーター化クエリを使用する SQL 文の例を以下に示します。

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

## パラメーター化クエリの作成 {#create}

UI でパラメーター化されたクエリを作成するには、クエリエディターに移動します。 詳しくは、[ クエリエディターへのアクセス ](./user-guide.md#accessing-query-editor) の節を参照してください。

`'$'` の序文を使用して、テキストエディターでクエリにクエリパラメーターを入力します。次に、「[!UICONTROL &#x200B; コンソール &#x200B;]&#x200B;**の横にある「**&#x200B;[!UICONTROL &#x200B; クエリパラメーター &#x200B;]」タブを選択し、キーに欠落している値を追加します。 必要なキーに値を追加しない場合、クエリは実行できません。 アラートアイコン（![ アラートアイコン。](/help/images/icons/alert.png)）が「クエリパラメーター」セクション内の空の [!UICONTROL &#x200B; 値 &#x200B;] 入力フィールドの隣に表示されます。

>[!NOTE]
>
>クエリでパラメーターが指定されない場合でも、クエリエディター内で不要なパラメーターを入力できます。 クエリエディターでは、不要なキーと値のペアはすべて無視され、クエリの実行や結果には影響しません。

![ パラメーター化されたクエリと「クエリパラメーター」セクションがハイライト表示されたクエリエディター。](../images/ui/parameterized-queries/parameterized-query.png)

>[!TIP]
>
>タブを [!UICONTROL &#x200B; クエリパラメーター &#x200B;] から [!UICONTROL &#x200B; コンソール &#x200B;] に変更して、クエリのコンソール出力を表示します。

## クエリログの詳細を使用してパラメーター値を確認 {#check-parameter-values}

使用される値は永続的ではないので、テンプレート内にパラメーターを保存することはできません。 ただし、「[!UICONTROL &#x200B; クエリログの詳細 &#x200B;] ページを確認すると、クエリの実行で使用されるパラメーター値を確認できます。 この場合、ログはクエリがパラメーター化クエリだったことを示しません。 使用される値を見つける手順については、[ クエリログドキュメント ](./query-logs.md) を参照してください。

![ 詳細セクションでハイライト表示されたパラメーター化クエリの SQL を含むクエリログビュー。](../images/ui/parameterized-queries/parameterized-query-logs.png)

<!-- improve screenshot above ^ I am waiting for a scheduled run to complete -->

## パラメーター化クエリのスケジュール設定 {#schedule}

パラメーター値は、パラメーター化クエリをスケジュールする際に保存されます。 パラメーター化されたクエリをスケジュールするには、ガイド [ クエリスケジュールの作成 ](./query-schedules.md#create-schedule) の説明に従って、スケジュールされたクエリを一般的なプロセスで作成し、クエリの実行で使用するパラメーター値を入力します。 この UI セクションは、パラメーター化クエリに対してのみ表示されます。 詳しくは、[ スケジュールされたパラメーター化クエリのパラメーターの設定 ](./query-schedules.md#set-parameters) の節を参照してください。

>[!TIP]
>
>クエリサービスは、パラメーター化されたクエリを使用して、準備済みステートメントをサポートします。 使用される SQL 構文について詳しくは、[ 準備文構文ガイド ](../sql/prepared-statements.md) を参照してください。

## 次の手順

このドキュメントでは、Adobe Experience Platform UI でクエリをパラメーター化し、スケジュールされたクエリの実行で使用する方法について説明しました。 また、このドキュメントでは、クエリの実行で使用されるパラメーター値のログを確認する方法についても重点的に説明しました。

次に、[ スケジュール済みクエリの監視 ](./monitor-queries.md) に関するガイドを読み、Experience Platform UI を使用してすべてのクエリジョブのステータスをより深く理解することをお勧めします。

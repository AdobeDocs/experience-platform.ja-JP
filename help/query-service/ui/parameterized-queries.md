---
title: パラメーター化クエリ
description: Adobe Experience Platform UI でパラメーター化クエリを使用する方法について説明します。
source-git-commit: 4fc94fc39fa09756a440b5e532330cd310dd96d2
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 2%

---

# パラメーター化されたクエリ（限定的なリリース） {#parameterized-queries}

>[!CONTEXTUALHELP]
>id="platform_queryService_queryEditor_parameterizedQueries"
>title="パラメーター化クエリ"
>abstract="パラメーター化クエリを使用して、実行時にパラメーター値を追加します。 これにより、動的データを操作し、様々な使用例でクエリを再利用できます。 以下を使用します。 `'$'` 「 」を入力し、テキストエディターでクエリにクエリパラメーターを入力します。 次に、エディターの下にある「クエリパラメーター」セクションで、キーの値を追加します。"

>[!IMPORTANT]
>
>パラメータ化されたクエリ UI 機能は、 **限定リリースのみ** およびは、すべての顧客が利用できるわけではありません。

クエリサービスでは、クエリエディターでのパラメーター化されたクエリの使用がサポートされています。 パラメーター化されたクエリで、パラメーターのプレースホルダーを使用して、実行時にパラメーター値を追加できるようになりました。 プレースホルダーを使用すると、ステートメントが実行されるまでの値が不明な動的データを操作できます。 また、クエリを事前に準備し、同様の目的で再利用することもできます。 クエリを再利用すると、各使用例に対して個別の SQL クエリを作成する手間が省けます。

## 前提条件

このガイドを続行する前に、 [クエリエディター UI ガイド](./user-guide.md). クエリエディターガイドでは、Experience Platformユーザーインターフェイス内で顧客体験データのクエリを記述、検証、実行する方法に関する詳細情報を提供します。

>[!NOTE]
>
>Adobe Experience Platform UI 内では、パラメーター化されたクエリは、インラインテンプレートの親レベルでのみサポートされます。 つまり、パラメーター化されたクエリは、元のテンプレートで使用された場合にのみ機能します。子テンプレートは、静的テンプレートである必要があり、動的パラメーターを持つことはできません。 詳しくは、 [インラインテンプレートドキュメント](../essential-concepts/inline-templates.md) を参照してください。

## パラメーター化されたクエリ構文 {#syntax}

パラメーター化されたクエリは形式を使用します `'$YOUR_PARAMETER_NAME'` とは、ドット表記を使用して連結できます。 パラメータ化されたクエリを使用する SQL 文の例を次に示します。

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

UI でパラメーター化されたクエリを作成するには、クエリエディターに移動します。 詳しくは、 [クエリエディターへのアクセス](./user-guide.md#accessing-query-editor) を参照してください。

以下を使用します。 `'$'` 「 」を入力し、テキストエディターでクエリにクエリパラメーターを入力します。 次に、見つからないキーの値を [!UICONTROL クエリパラメーター] 」セクションをクリックします。 必要なキーのいずれかに値を追加しなかった場合、クエリは実行できません。 アラートアイコン (![アラートアイコン。](../images/ui/parameterized-queries/alert-icon.png)) は、空の場合は横の「クエリパラメーター」セクションに表示されます [!UICONTROL 値] 入力フィールド。

![パラメーター化されたクエリと「クエリパラメーター」セクションがハイライト表示されたクエリエディター。](../images/ui/parameterized-queries/parameterized-query.png)

>[!TIP]
>
>タブの変更元 [!UICONTROL クエリパラメーター] から [!UICONTROL コンソール] をクリックして、クエリのコンソール出力を表示します。

パラメーターを削除し、既に実行された後にクエリを再実行しようとすると、エラーメッセージが [!UICONTROL クエリパラメーター] 」セクションに移動して、警告を表示します。

>[!NOTE]
>
>クエリがパラメータを取らない場合でも、クエリエディタ内で不要なパラメータを入力できます。 クエリエディターは、不要なキーと値のペアをすべて無視し、クエリの実行や結果には影響しません。

![値が空のフィールドとクエリーパラメーターのエラーが強調表示されたクエリーエディター。](../images/ui/parameterized-queries/query-parameter-error.png)

## クエリログの詳細を使用してパラメーター値を確認 {#check-parameter-values}

使用した値は永続的ではないので、テンプレート内にパラメーターを保存することはできません。 ただし、 [!UICONTROL クエリログの詳細] ページを開き、クエリの実行で使用されるパラメーター値を検索します。 この場合、ログはクエリがパラメーター化されたクエリであることを示しません。 詳しくは、 [クエリログドキュメント](./query-logs.md) を参照してください。

![クエリログビューでは、詳細セクションでパラメータ化されたクエリの SQL が強調表示されます。](../images/ui/parameterized-queries/parameterized-query-logs.png)

<!-- improve screenshot above ^ I am waiting for a scheduled run to complete -->

## パラメーター化クエリのスケジュール設定 {#schedule}

パラメータ値は、パラメータ化クエリをスケジュールする際に保存されます。 パラメーター化されたクエリをスケジュールするには、次のガイドに従って、通常の手順に従ってスケジュール済みクエリを作成します。 [クエリスケジュールの作成](./query-schedules.md#create-schedule)次に、クエリの実行で使用するパラメーター値を入力します。 この UI セクションは、パラメーター化されたクエリに対してのみ表示されます。 詳しくは、 [スケジュール済みのパラメーター化クエリのパラメーターの設定](./query-schedules.md#set-parameters) を参照してください。

>[!TIP]
>
>クエリサービスは、パラメーター化されたクエリを使用して準備済み文をサポートします。 詳しくは、 [準備済み文構文ガイド](../sql/prepared-statements.md) を参照してください。

## 次の手順

このドキュメントでは、Adobe Experience Platform UI でクエリをパラメータ化し、スケジュールされたクエリの実行で使用する方法を学びました。 また、クエリの実行で使用されるパラメーター値をログで確認する方法も強調されました。

次に、に関するガイドを読むことをお勧めします。 [スケジュール済みクエリの監視](./monitor-queries.md) を使用すると、Platform UI を通じてすべてのクエリジョブのステータスをより深く理解できます。

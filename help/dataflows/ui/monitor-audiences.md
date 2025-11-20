---
description: Experience Platform ユーザーインターフェイスを使用して、セグメント化中にデータフローを監視する方法を説明します。
title: UI でのオーディエンスのデータフローの監視
type: Tutorial
exl-id: 32fd2ba1-0ff0-4ea7-8d55-80d53eebc02f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1710'
ht-degree: 7%

---

# UI でのオーディエンスのデータフローの監視

セグメント化サービスを使用すると、セグメント定義またはその他のソースを使用して、[!DNL Real-Time Customer Profile] データからオーディエンスを作成できます。 Experience Platformは、ソースから宛先へのデータフローを透過的に追跡するデータフローを提供します。

監視ダッシュボードを使用して、データのセグメント化のステータスなど、オーディエンス内のデータのアクティビティを視覚的に表示します。 Experience Platform ユーザーインターフェイスを使用してデータのセグメント化を監視し、オーディエンスのアクティベーション、評価および書き出しジョブのステータスを追跡できる、監視ダッシュボードを使用する方法について詳しくは、チュートリアルをお読みください。

## はじめに {#getting-started}

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [ データフロー ](../home.md)：データフローは、Experience Platform間でデータを移動するデータジョブを表します。 データフローは異なるサービスをまたいで設定され、ソースコネクタからターゲットデータセット、[!DNL Identity] および [!DNL Profile]、[!DNL Destinations] へとデータを移動できます。
   - [データフロー実行](../../sources/notifications.md)：データフロー実行は、選択したデータフローの頻度設定に基づいて繰り返しスケジュールされたジョブです。
- [ セグメント化 ](../../segmentation/home.md)：セグメント化によって、リアルタイム顧客プロファイルデータからオーディエンスを作成できます。
   - [ アクティベーションジョブ ](../../destinations/ui/activation-overview.md)：アクティベーションジョブを使用して、指定した宛先に対してオーディエンスをアクティベートします。
   - [ 評価ジョブ ](../../segmentation/tutorials/evaluate-a-segment.md#evaluate-a-segment)：評価ジョブは、オーディエンスを評価する非同期プロセスです。
   - [ 書き出しジョブ ](../../segmentation/api/export-jobs.md)：書き出しジョブは、オーディエンスメンバーをデータセットに保持するために使用される非同期プロセスです。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

## オーディエンス監視ダッシュボード {#monitoring-audiences-dashboard}

>[!CONTEXTUALHELP]
>id="platform_monitoring_segments"
>title="オーディエンス"
>abstract="オーディエンスビューには、アクティベーションおよび評価ジョブについての詳細情報を含む、すべての組織のオーディエンスに関する情報が表示されます。"

**[!UICONTROL Audiences]** ダッシュボードにアクセスするには、左側のナビゲーションで「**[!UICONTROL Monitoring]**」を選択します。 **[!UICONTROL Monitoring]** ページで、**[!UICONTROL Audiences]** カードを選択します。

![ オーディエンスカード。 前回の評価ジョブおよび前回の書き出しジョブに関する情報が表示されます。](../assets/ui/monitor-audiences/audience-card.png)

メインの **[!UICONTROL Audiences]** ダッシュボードでは、**[!UICONTROL Audiences]** カードに最後の評価ジョブのステータスと日付および最後のエクスポートジョブが表示されます。

ダッシュボード自体には、オーディエンスとセグメント化ジョブの両方の指標が含まれています。 デフォルトでは、ダッシュボードには過去 24 時間のオーディエンス指標が表示されます。 セグメント化ジョブビューについて詳しくは、[ セグメント化ジョブの監視 ](#monitoring-segmentation-jobs-dashboard) を参照してください。

>[!IMPORTANT]
>
>現在、オーディエンスの監視ダッシュボードでは、[ バッチ（ファイルベース）の宛先に対してアクティブ化されたオーディエンス ](../../destinations/destination-types.md#file-based) みがサポートされています。

![オーディエンスダッシュボード。組織やサンドボックスの様々なオーディエンスに関する情報が表示されます。](../assets/ui/monitor-audiences/audience-dashboard.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Audience name]** | オーディエンスの名前。 |
| **[!UICONTROL Data type]** | オーディエンスのデータタイプ。 使用可能な値は、**[!UICONTROL Customer]**、**[!UICONTROL Account]**、**[!UICONTROL Prospect]** です。 カードのリボンの上にある [!UICONTROL Data type] フィルターを使用すると、指定したデータタイプのオーディエンスを表示できます。 |
| **[!UICONTROL Last evaluation timestamp]** | オーディエンスの最後の評価ジョブが実行された日時。 |
| **[!UICONTROL Last evaluation status]** | オーディエンスの前回の評価ジョブのステータス。 使用可能な値は、**[!UICONTROL Success]**、**[!UICONTROL No runs]**、**[!UICONTROL Failed]** です。 |
| **[!UICONTROL Last evaluation method]** | オーディエンスの評価方法。 バッチセグメント化のみがサポートされるので、取り得る値は **[!UICONTROL Batch]** のみです。 |
| **[!UICONTROL Last evaluation profiles]** | オーディエンスの最後の評価ジョブで評価されたプロファイルの数。 |
| **[!UICONTROL Last activation timestamp]** | オーディエンスの最後のアクティベーションジョブが実行された日時。 |
| **[!UICONTROL Last activation status]** | オーディエンスの前回のアクティベーションジョブのステータス。 使用可能な値は、**[!UICONTROL Success]**、**[!UICONTROL No runs]**、**[!UICONTROL Failed]** です。 |
| **[!UICONTROL Last activation identities]** | オーディエンスの最後のアクティベーションジョブでアクティブ化された ID の数。 |
| **[!UICONTROL Last activation destination]** | オーディエンスの最後のアクティベーションジョブがアクティベートされた宛先の名前。 |

フィルターアイコン（![ フィルターアイコンを選択すると、結果を特定のオーディエンスにフィルタリングし、そのセグメント化ジョブを表示できます。](/help/images/icons/filter-add.png)）に設定します。 セグメント化ジョブは時系列で並べ替えられ、最新のセグメント化ジョブが最初に表示されます。

![ フィルターアイコンがハイライト表示されています。 これを選択すると、指定したオーディエンスのセグメント化ジョブを表示できます。](../assets/ui/monitor-audiences/filter-audience.png)

フィルタリングされたオーディエンスダッシュボードが表示されます。 **[!UICONTROL Audiences]** カードには、最後の評価ジョブのステータスと日付、および最後のアクティベーションジョブの日付が表示されます。

![ オーディエンスカード。 前回の評価ジョブと前回のアクティベーションジョブに関する情報が表示されます。](../assets/ui/monitor-audiences/specified-audience-card.png)

ダッシュボード自体には、最後の評価およびアクティベーションジョブの時間とステータス、オーディエンス評価のプロファイル数を示すグラフ、および実行されたセグメント化ジョブの指標が表示されます。 デフォルトでは、ダッシュボードには、過去 24 時間のセグメント化ジョブ指標が表示されます。

![ フィルタリングされたオーディエンスダッシュボード。 このオーディエンスに対して実行された様々なセグメント化ジョブに関する情報が表示されます。](../assets/ui/monitor-audiences/filter-audience.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Job start]** | セグメント化ジョブが開始した日時。 |
| **[!UICONTROL Type]** | セグメント化ジョブのタイプを示します。 サポートされているジョブタイプは **アクティブ化** ジョブと **評価** ジョブの 2 つです。 |
| **[!UICONTROL Job complete]** | セグメント化ジョブが完了した日時。 |
| **[!UICONTROL Processing time]** | セグメント化ジョブが完了するまでにかかった時間。 |
| **[!UICONTROL Job status]** | セグメント化ジョブのステータス。 サポートされる値は、**[!UICONTROL Success]**、**[!UICONTROL In Progress]**、**[!UICONTROL Failed]** です。 |
| **[!UICONTROL Profile count]** | セグメント化ジョブが評価しているプロファイルの数。 各ユーザーには、一意のプロファイルが必要です。 |
| **[!UICONTROL Identity activated]** | セグメント化ジョブがアクティブ化している ID の数。 各プロファイルには複数の ID を設定できます。 例えば、プロファイルには、ID としてメール、電話番号、ロイヤルティ番号を含めることができます。 |
| **[!UICONTROL Destination name]** | セグメント化ジョブをアクティブ化する宛先の名前。 |

フィルターアイコン（![ フィルターアイコンを選択すると、特定のセグメント化ジョブをさらにフィルタリングして、その詳細を確認できます。](/help/images/icons/filter.png)）に設定します。 フィルタリングできるセグメント化ジョブには、アクティベーションジョブと評価ジョブの 2 種類があります。

### アクティベーションジョブの詳細 {#activation-job-details}

アクティベーションジョブのデータフロー実行の詳細ページには、セグメント化ジョブに関連する実行の指標、データフロー実行エラー、オーディエンスに関する情報が表示されます。 アクティベーションジョブは、指定した宛先に対してオーディエンスをアクティベートするために使用されます。

![ アクティベーションジョブダッシュボード。 このオーディエンスに対して実行された様々なセグメント化ジョブに関する情報が表示されます。](../assets/ui/monitor-audiences/activation-job-dashboard.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Profiles received]** | アクティベーションフローで受信したプロファイルの合計数。 |
| **[!UICONTROL Identities activated]** | 受信したプロファイルに基づいて、宛先に対して正常にアクティブ化された ID の合計数です。 |
| **[!UICONTROL Identities excluded]** | 受信したプロファイルに基づいて、宛先に対してアクティブ化するために除外された ID の合計数です。 これらの ID は、属性が見つからない、または同意違反が原因で除外される可能性があります。 |
| **[!UICONTROL Size of data]** | アクティブ化するデータフローのサイズ。 |
| **[!UICONTROL Total files]** | データフローでアクティブ化されているファイルの合計数です。 |
| **[!UICONTROL Status]** | アクティベーションジョブの現在のステータス。 |
| **[!UICONTROL Dataflow run start]** | アクティベーションジョブが開始した日時。 |
| **[!UICONTROL Dataflow run end]** | アクティベーションジョブが終了した日時。 |
| **[!UICONTROL Dataflow run ID]** | 現在のアクティベーションジョブの ID。 |
| **[!UICONTROL IMS org ID]** | アクティベーションジョブが属する組織の ID。 |
| **[!UICONTROL Destination name]** | データがアクティブ化されている宛先の名前。 |

「オーディエンス」セクションには、アクティベーションジョブの一部としてアクティベートされたオーディエンスのリストが表示されます。

![ アクティベーションジョブダッシュボード。 失敗または除外された ID に関する情報がハイライト表示されます。](../assets/ui/monitor-audiences/activation-job-audiences.png)

オーディエンスセクションでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Name]** | アクティブ化されたオーディエンスの名前。 |
| **[!UICONTROL Identities activated]** | 受信したプロファイルに基づいて、宛先に対して正常にアクティブ化された ID の合計数です。 |
| **[!UICONTROL Identities excluded]** | 受信したプロファイルに基づいて、宛先に対してアクティブ化するために除外された ID の合計数です。 これらの ID は、属性の欠如または同意違反が原因で除外される可能性があります。 |
| **[!UICONTROL Last dataflow run status]** | そのオーディエンスに対して実行された前回のアクティベーションジョブのステータス。 |
| **[!UICONTROL Last dataflow run date]** | そのオーディエンスに対して実行された最後のアクティベーションジョブの日時。 |

さらに、データフロー実行エラーに関する詳細を表示できます。 「データフロー実行エラー」セクションでは、失敗した ID と除外された ID の両方を表示できます。 「エラー」セクションには、エラーコードに関する詳細と、失敗または除外された ID の数が含まれます。

![ アクティベーションジョブダッシュボード。 失敗または除外された ID に関する情報がハイライト表示されます。](../assets/ui/monitor-audiences/activation-job-errors.png)

### 評価ジョブの詳細 {#evaluation-job-details}

評価ジョブのデータフロー実行の詳細ページには、セグメント化ジョブに関連する実行の指標とオーディエンスに関する情報が表示されます。

![ 評価ジョブのダッシュボード。 オーディエンスの評価ジョブに関する情報が表示されます。](../assets/ui/monitor-audiences/evaluation-job-details.png)

このダッシュボードビューでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Total profiles]** | 評価中のプロファイルの合計数。 |
| **[!UICONTROL Status]** | 評価ジョブのステータス。 評価ジョブの可能なステータスには、**[!UICONTROL Success]** と **[!UICONTROL Failed]** があります。 |
| **[!UICONTROL Job start]** | 評価ジョブが開始された日時。 |
| **[!UICONTROL Job end]** | 評価ジョブが終了した日時。 |
| **[!UICONTROL Job type]** | セグメント化ジョブのタイプ。 この場合、それは常に **[!UICONTROL Segment evaluation]** しい仕事になります。 |
| **[!UICONTROL Evaluation type]** | 実行されている評価のタイプ。 **[!UICONTROL Batch]** または **[!UICONTROL Streaming]** のいずれかを指定できます。 |
| **[!UICONTROL Job ID]** | 評価ジョブの ID。 |
| **[!UICONTROL IMS org ID]** | 評価ジョブが属する組織の ID。 |
| **[!UICONTROL Audience name]** | 評価されるオーディエンスの名前。 |
| **[!UICONTROL Audience ID]** | 評価対象のオーディエンス ID。 |

「[!UICONTROL Audiences]」セクションには、評価ジョブの一部として評価されているオーディエンスのリストが表示されます。 検索バーを使用して、名前でオーディエンスのリストをフィルタリングできます。

>[!IMPORTANT]
>
>このダッシュボードビューは現在、最大 800 個のオーディエンス指標をサポートしています。

[!UICONTROL Audiences] セクションでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Name]** | 評価されるオーディエンスの名前。 |
| **[!UICONTROL Profile count]** | 評価されているプロファイルの数。 |

## セグメント化ジョブ監視ダッシュボード {#monitoring-segmentation-jobs-dashboard}

>[!CONTEXTUALHELP]
>id="platform_monitoring_segment_jobs"
>title="セグメント化ジョブ"
>abstract="セグメント化ジョブビューには、すべてのオーディエンスに対する評価および書き出しジョブに関する情報が表示されます。"

**[!UICONTROL Segmentation Jobs]** ダッシュボードにアクセスするには、**[!UICONTROL Segmentation jobs]** ダッシュボードで「[!UICONTROL Audiences]」を選択します。 [!UICONTROL Monitoring] ダッシュボードには、評価ジョブとエクスポートジョブに関する指標と情報が含まれています。

>[!NOTE]
>
>オーディエンスごとの監視では、**セグメント化評価ジョブ** のみがサポートされています。 セグメント化の書き出しジョブでは、組織レベルの監視のみがサポートされます。

![ セグメント化ジョブの監視ダッシュボードが表示されます。 オーディエンスとセグメント化ジョブを切り替える切替スイッチがハイライト表示されます。](../assets/ui/monitor-audiences/segmentation-jobs-dashboard.png)

[!UICONTROL Segmentation Jobs] ダッシュボードを使用して、プロファイルの評価と書き出しが時間通りに発生し、例外なく発生するかどうかを把握します。これにより、宛先アクティベーションのダウンストリームサービスに、最新の評価済みプロファイルデータを含めることができます。

セグメント化ジョブでは、次の指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| **[!UICONTROL Segmentation job]** | セグメント化ジョブの名前を示します。 |
| **[!UICONTROL Type]** | セグメント化ジョブのタイプ（書き出しまたは評価）を示します。 どちらの場合も、セグメント化ジョブは組織に属するオーディエンス **すべて** を評価または書き出します。 エクスポートジョブについて詳しくは、[ エクスポートジョブエンドポイント ](../../segmentation/api/export-jobs.md) に関するガイドを参照してください。 評価ジョブについて詳しくは、[ セグメント定義の評価 ](../../segmentation/tutorials/evaluate-a-segment.md#evaluate-a-segment) に関するチュートリアルを参照してください。 |
| **[!UICONTROL Job start]** | セグメント化ジョブが開始した日時。 |
| **[!UICONTROL Job end]** | セグメント化ジョブが完了した日時。 |
| **[!UICONTROL Status]** | 完了したジョブのステータス。 セグメント化ジョブの可能なステータスには、成功または失敗が含まれます。 |

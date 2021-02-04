---
source-git-commit: b9816767556b9d50cf2ff5268816d1de6b85fc63
workflow-type: tm+mt
translation-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---
# Gen2へのAdobe Experience PlatformData Lakeの移行

Adobe Experience PlatformはGen2 Data Lakeに移行中です。 これは、地域レプリケーション、役割に基づく細かいアクセス制御(RBAC)、より高度な拡張など、プラットフォームユーザーにメリットを提供する新世代のデータレークです。

## ユーザーへの影響

AdobeがData LakeをGen1からGen2に移行する間、ユーザーはData Lakeから&#x200B;**read**&#x200B;を実行できますが、**write**&#x200B;がData Lakeに送り込む機能はすべて影響を受けます。 次に、影響を受ける機能のリストを示します。

- **ソース**:ソースからのデータや様々なデータ取り込みワークフローのデータは遅延します。移行が完了すると、ユーザーにデータが表示されます。
- **クエリサービス**:ユーザーはクエリを実行できますが、クエリの出力をデータセットに書き込むことはできません。
- **リアルタイム顧客プロファイル**:バッチインジェストによってプロファイルストアに取り込まれたデータは、 **** 移行中は利用できません。ただし、**ストリーミング**&#x200B;インジェストによって取り込まれたデータは、移行中に利用できます。 また、移行中はプロファイルエクスポートを使用できません。
- **Data Science Workspace**:Data Science Workspaceからの書き込みは失敗します。
- **Segmentation Service**:バッチセグメン **** トから派生したオーディエンスは、移行中にアクティブ化できません。**ストリーミング**&#x200B;セグメントから派生したオーディエンスは影響を受けません。
- **Customer Journey Analytics**:バッチがData Lakeに取り込まれないため、Customer Journey Analyticsレポートのデータは古く、移行中は更新されない場合があります。

## プラットフォームユーザーとの通信

Adobeは、移行の影響について詳しく説明し、特定のIMS組織の移行日時を確認するために、システム管理者に連絡します。
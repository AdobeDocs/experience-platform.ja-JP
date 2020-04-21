---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: オーディエンスマネージャのマッピングフィールド
topic: overview
translation-type: tm+mt
source-git-commit: 53fb7ea201ed9361584d24c8bd2ad10edd9f3975

---


# オーディエンスマネージャーのマッピングフィールド

次の表に、Adobe Adobe Data Managerデータ(リアルタイム、オンボード、プロファイルの各データ)内のフィールドと、対応するXDMフィールドとのマッピングを示します。

各XDMフィールドについて詳し [くは、](../../../../xdm/schema/field-dictionary.md) XDMフィールドディクショナリを参照してください。

## リアルタイムデータ

タイプ：リアルタイムデータ

| リアルタイムデータフィールド | XDMフィールド |
| --- | --- |
| `requestIds[]` | `ExperienceEvent.identityMap["ECID"]` |
| `requestIds[]` | `ExperienceEvent.endUserIds` - *endUserIdsに存在する名前空間と最初の値のみ。* |
| `primaryDeviceId` | `ExperienceEvent.identityMap["CORE"]` |
| `primaryDeviceId` | ExperienceEvent.endUserIds - endUserIdsに *存在し、最初の値のみの名前空間の場合のみ。* |
| `trait[] ` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |
| `segments[]` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `mergeRules[]` | `ExperienceEvent.profileStitch[]` |
| `timestamps` | `ExperienceEvent.timeStamp` |
| `deviceMetadata` | `ExperienceEvent.device` <ul><li>primaryHardwareType→タイプ</li><li>製造元→製造元</li><li>marketingName→モデル</li><li>modelNumber→モデル</li></ul> |
| `location` | `ExperienceEvent.placeContext.geo` <ul><li>d_country → countryCode</li><li>d_state→ stateProvince</li><li>d_city→ city</li><li>d_postal→郵便番号</li><li>d_lat→緯度</li><li>d_longitude →経度</li></ul> |
| `request_user_agent` | `ExperienceEvent.environment.browserDetails` <ul><li>h_user-agent→ userAgent</li><li>h_accept-language → acceptLanguage</li></ul> |
| `client_ip` | `ExperienceEvent.environment` <ul><li>d_os_name→ OS名 </li><li>d_os_version→ os_version</li></ul> |
| `Signals` | ExperienceEvent.signals |

## 受信デー **タ（廃止）**

タイプ：ExperienceEvent

| 受信フィールド | XDMフィールド |
| --- | --- |
| `uuid` | `ExperienceEvent.identityMap[<ID Type>]` |
| `deviceIds` | `ExperienceEvent.identityMap["CORE"] And calculated ECIDs  ExperienceEvent.identityMap["ECID"]` |
| `signals` | `ExperienceEvent.signals` |
| `b_time` | `ExperienceEvent.timeStamp` |
| `overwrite` | `overwriteTraits` |

>[!NOTE] 受信フィールドは、今後のリリースで廃止される予定です。

## プロファイルデータ

タイプ：プロファイルXDM

| プロファイル場 | XDMフィールド |
| --- | --- |
| `ids` | `identityMap` |
| `smem` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `tmem` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |

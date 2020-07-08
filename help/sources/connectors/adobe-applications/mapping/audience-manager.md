---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Audience Managerマッピングフィールド
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---


# Audience Managerマッピングフィールド

以下の表に、Adobe Audience Managerデータ(リアルタイム、オンボード、プロファイルデータ)内のフィールドと、対応するXDMフィールドのマッピングを示します。

各XDMフィールドについて詳しくは、 [XDMフィールドディクショナリ](../../../../xdm/schema/field-dictionary.md) (XDM)を参照してください。

## リアルタイムデータ

タイプ： リアルタイムデータ

| リアルタイムデータフィールド | XDMフィールド |
| --- | --- |
| `requestIds[]` | `ExperienceEvent.identityMap["ECID"]` |
| `requestIds[]` | `ExperienceEvent.endUserIds` - *endUserIdsに存在し、最初の値にのみ存在する名前空間のみ。* |
| `primaryDeviceId` | `ExperienceEvent.identityMap["CORE"]` |
| `primaryDeviceId` | ExperienceEvent.endUserIds - endUserIdsに存在し、最初の値のみに存在する名前空間 *のみ。* |
| `trait[] ` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |
| `segments[]` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `mergeRules[]` | `ExperienceEvent.profileStitch[]` |
| `timestamps` | `ExperienceEvent.timeStamp` |
| `deviceMetadata` | `ExperienceEvent.device` <ul><li>primaryHardwareType→タイプ</li><li>製造元→製造元</li><li>marketingName→モデル</li><li>modelNumber→モデル</li></ul> |
| `location` | `ExperienceEvent.placeContext.geo` <ul><li>d_country→ countryCode</li><li>d_state→ stateProvince</li><li>d_city→市区町村</li><li>d_postal→ postalCode</li><li>d_lat→緯度</li><li>d_longitude→ longitude</li></ul> |
| `request_user_agent` | `ExperienceEvent.environment.browserDetails` <ul><li>h_user-agent→ userAgent</li><li>h_accept-language→ acceptLanguage</li></ul> |
| `client_ip` | `ExperienceEvent.environment` <ul><li>d_os_name→ os名 </li><li>d_os_version→ os_version</li></ul> |
| `Signals` | ExperienceEvent.signals |

## 受信データ **（廃止）**

タイプ： ExperienceEvent

| 受信フィールド | XDMフィールド |
| --- | --- |
| `uuid` | `ExperienceEvent.identityMap[<ID Type>]` |
| `deviceIds` | `ExperienceEvent.identityMap["CORE"] And calculated ECIDs  ExperienceEvent.identityMap["ECID"]` |
| `signals` | `ExperienceEvent.signals` |
| `b_time` | `ExperienceEvent.timeStamp` |
| `overwrite` | `overwriteTraits` |

>[!NOTE]
>
>今後のリリースで、Inbound Fieldsは非推奨となる予定です。

## プロファイルデータ

タイプ： プロファイルXDM

| プロファイルフィールド | XDMフィールド |
| --- | --- |
| `ids` | `identityMap` |
| `smem` | `ExperienceEvent.segmentMemberships["AAMSegments"]` |
| `tmem` | `ExperienceEvent.segmentMemberships["AAMTraits"]` |

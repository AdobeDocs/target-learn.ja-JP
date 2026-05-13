---
title: データプロバイダーを使用してサードパーティデータを統合する方法
description: このチュートリアルでは、データプロバイダーにユーザーを紹介します。 データプロバイダー機能を使用して、サードパーティからAdobe Targetにデータを簡単に渡す方法について説明します。
role: User, Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: feature video
kt: null
author: Daniel Wright
exl-id: 1892136e-14e3-4e52-8b1f-aee806d2f83a
TQID: https://experienceleague.adobe.com/XiUlJGHSFVxAMqdl6Y7hK9PoXOgiiUI43vrFeAj2Rpo
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: c0b4abf2d4ead4d58a8db6e8970857b7b50dbe5c
workflow-type: tm+mt
source-wordcount: 195
ht-degree: 16%

---

# データプロバイダーを利用して、サードパーティデータをAdobe Targetに統合する

[!UICONTROL Data Providers]は、サードパーティからTargetにデータを簡単に渡すことができる機能です。  サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。 このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22349/?quality=12)

## データプロバイダーの活用方法

1. 実装エキスパートは、at.js （またはat.jsのライブラリヘッダーセクション）の前にコードを追加し、サードパーティへのAPI呼び出しを行い、応答を解析し、応答から名前と値のペアを指定して[!DNL Target]に送信します。
1. at.jsはフリッカーを管理し、名前と値のペアをカスタムパラメーターとしてグローバル Target リクエストに含めます。
1. マーケターは、これらのカスタムパラメーターに基づいて、[!DNL Target] インターフェイスでオーディエンスを構築します。
1. マーケターは、これらのオーディエンスを、エクスペリエンス、アクティビティ、指標のターゲット設定やオーディエンスのレポート作成に使用します。

>[!NOTE]
>
>[!UICONTROL Data Providers]にはat.js 1.3以降が必要です

## サポートマテリアル

* [at.jsおよびAdobe Targetでのデータプロバイダーの実装](implement-data-providers-to-integrate-third-party-data.md)

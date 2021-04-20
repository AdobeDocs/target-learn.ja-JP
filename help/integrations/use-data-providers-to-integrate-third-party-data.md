---
title: データプロバイダーを使用したサードパーティデータの統合方法
description: このチュートリアルでは、データプロバイダに関するユーザを紹介します。 データプロバイダー機能を使用して、サードパーティからAdobe Targetに簡単にデータを渡す方法を説明します。
role: Business Practitioner, Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: feature video
kt: null
thumbnail: null
author: Daniel Wright
translation-type: tm+mt
source-git-commit: b89732fcca0be8bffc6e580e4ae0e62df3c3655d
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 22%

---


# データプロバイダーを使用して、サードパーティのデータをAdobe Targetに統合する

[!UICONTROL データプロバイダーは、サードパーティから Target へデータを簡単に渡すことのできる機能です。]サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22349/?quality=12)

## データプロバイダーの使用方法

1. 実装のエキスパートが、at.jsの前（またはat.jsのライブラリヘッダーセクション）にコードを追加します。このコードは、サードパーティへのAPI呼び出しを行い、応答を解析し、応答から名前と値のペアで指定して[!DNL Target]に送信します。
1. at.jsはちらつきを管理し、名前と値のペアをカスタムパラメーターとしてグローバルターゲットリクエストに含めます。
1. マーケティング担当者は、これらのカスタムパラメーターに基づいて[!DNL Target]インターフェイスでオーディエンスを構築します。
1. マーケティング担当者は、これらのオーディエンスを使用して、レポートオーディエンスだけでなく、エクスペリエンス、アクティビティおよび指標もターゲットします。

>[!NOTE]
>
>[!UICONTROL データ] プロバイダーではat.js 1.3以降が必要

## サポート資料

* [at.jsおよびAdobe Targetでのデータプロバイダーの実装](implement-data-providers-to-integrate-third-party-data.md)
* [データプロバイダードキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html#data-providers)
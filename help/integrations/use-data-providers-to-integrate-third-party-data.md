---
title: データプロバイダーを使用したサードパーティデータの統合方法
description: このチュートリアルでは、データプロバイダーのユーザーを紹介します。 データプロバイダー機能を使用して、サードパーティからAdobe Targetにデータを簡単に渡す方法を説明します。
role: User, Developer
level: Experienced
topic: Personalization, Integrations
feature: Implementation, Integrations, APIs/SDKs
doc-type: feature video
kt: null
author: Daniel Wright
exl-id: 1892136e-14e3-4e52-8b1f-aee806d2f83a
source-git-commit: 80208b3ecbc0d627d2afe72f882e91c9800d2726
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 16%

---

# データプロバイダーを使用したサードパーティデータのAdobe Targetへの統合

[!UICONTROL Data Providers] は、サードパーティから Target にデータを簡単に渡せる機能です。  サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22349/?quality=12)

## データプロバイダーの使用方法

1. 実装エキスパートは、API 呼び出しをサードパーティに対して行う at.js （または at.js のライブラリヘッダーセクション）の前にコードを追加し、応答を解析して、応答から名前と値のペアを使用して [!DNL Target] に送信するように指定します。
1. at.js は、ちらつきを管理し、名前と値のペアをカスタムパラメーターとしてグローバル Target リクエストに含めます。
1. マーケターは、これらのカスタムパラメーターに基づいて、[!DNL Target] インターフェイスでオーディエンスを作成します。
1. マーケターは、これらのオーディエンスを使用して、エクスペリエンス、アクティビティおよび指標をターゲット設定し、オーディエンスをレポートします。

>[!NOTE]
>
>[!UICONTROL Data Providers] には at.js 1.3 以降が必要です

## サポート資料

* [at.js とAdobe Targetへのデータプロバイダーの実装](implement-data-providers-to-integrate-third-party-data.md)

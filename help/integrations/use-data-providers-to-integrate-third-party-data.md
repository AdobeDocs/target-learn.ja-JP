---
title: データプロバイダーを使用して、サードパーティのデータをAdobe Targetに統合する
seo-title: データプロバイダーを使用して、サードパーティのデータをAdobe Targetに統合する
description: データプロバイダーは、サードパーティから Target へデータを簡単に渡すことのできる機能です。サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。
audience: marketer
difficulty: 5
author: Daniel Wright
doc-type: use
activity-type: feature-video
translation-type: tm+mt
source-git-commit: b331bb29c099bd91df27300ebe199cafa12516db
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 40%

---


# データプロバイダーを使用して、サードパーティのデータをAdobe Targetに統合する

[!UICONTROL データプロバイダーは、サードパーティから Target へデータを簡単に渡すことのできる機能です。]サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22349/?quality=12)

## データプロバイダーの使用方法

1. 実装のエキスパートが、at.jsの前（またはat.jsのライブラリヘッダーセクション）にコードを追加します。このコードは、サードパーティへのAPI呼び出しを行い、応答を解析し、送信先の応答から名前と値のペアで指定し [!DNL Target]ます。
1. at.jsはちらつきを管理し、名前と値のペアをカスタムパラメーターとしてグローバルTargetリクエストに含めます。
1. マーケティング担当者は、これらのカスタムパラメーターに基づいて、インター [!DNL Target] フェイス内でオーディエンスを作成します。
1. マーケティング担当者は、これらのオーディエンスを使用して、レポートオーディエンスだけでなく、エクスペリエンス、アクティビティおよび指標もターゲットします。

>[!NOTE]
>
>[!UICONTROL データプロバイダー] にはat.js 1.3以降が必要

## サポート資料

* [at.jsおよびAdobe Targetでのデータプロバイダーの実装](implement-data-providers-to-integrate-third-party-data.md)
* [データプロバイダードキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html#data-providers)
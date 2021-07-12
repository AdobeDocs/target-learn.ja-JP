---
title: データプロバイダーを使用してサードパーティデータを統合する方法
description: このチュートリアルでは、データプロバイダーに関するユーザーを紹介します。 データプロバイダー機能を使用して、サードパーティからAdobe Targetにデータを簡単に渡す方法を説明します。
role: User, Developer
level: Experienced
topic: パーソナライゼーション、統合
feature: 実装、統合、API/SDK
doc-type: feature video
kt: null
thumbnail: null
author: Daniel Wright
exl-id: 1892136e-14e3-4e52-8b1f-aee806d2f83a
source-git-commit: ee9aac0144e35abf32c5d8eafe10a013bf30d8d3
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 22%

---

# データプロバイダーを使用してサードパーティデータをAdobe Targetに統合する

[!UICONTROL データプロバイダーは、サードパーティから Target へデータを簡単に渡すことのできる機能です。]サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22349/?quality=12)

## データプロバイダーの使用方法

1. 実装の専門家は、at.jsの前（またはat.jsの「ライブラリヘッダー」セクション）にコードを追加し、API呼び出しをサードパーティに対しておこない、応答を解析し、応答から[!DNL Target]に送信する名前と値のペアを指定します。
1. at.jsはちらつきを制御し、名前と値のペアをカスタムパラメーターとしてグローバルTargetリクエストに含めます。
1. マーケターは、これらのカスタムパラメーターに基づいて、[!DNL Target]インターフェイスでオーディエンスを構築します。
1. マーケターは、これらのオーディエンスを使用して、エクスペリエンス、アクティビティ、指標のターゲット設定やレポート用オーディエンスのターゲット設定をおこないます。

>[!NOTE]
>
>[!UICONTROL デー] タプロバイダーにはat.js 1.3以降が必要です。

## サポート資料

* [at.jsとAdobe Targetでのデータプロバイダーの実装](implement-data-providers-to-integrate-third-party-data.md)
* [データプロバイダーのドキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html#data-providers)

---
title: データプロバイダーを使用してサードパーティデータを統合する方法
description: このチュートリアルでは、データプロバイダーに対するユーザーを紹介します。 データプロバイダー機能を使用して、サードパーティからAdobe Targetにデータを簡単に渡す方法を説明します。
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
source-wordcount: '195'
ht-degree: 25%

---

# データプロバイダーを使用してサードパーティデータをAdobe Targetに統合する

[!UICONTROL データプロバイダーは、サードパーティから Target へデータを簡単に渡すことのできる機能です。]サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

>[!VIDEO](https://video.tv.adobe.com/v/22349/?quality=12)

## データプロバイダーの使用方法

1. 実装の専門家は、at.js（または at.js の「ライブラリヘッダー」セクション）の前に、サードパーティへの API 呼び出しをおこなうコードを追加し、応答を解析して、に送信する応答の名前と値のペアで指定します [!DNL Target].
1. at.js はちらつきを制御し、名前と値のペアをカスタムパラメーターとしてグローバル Target リクエストに含めます。
1. マーケターが [!DNL Target] インターフェイスを作成します。
1. マーケターは、これらのオーディエンスを使用して、エクスペリエンス、アクティビティおよび指標をターゲット設定し、レポート用オーディエンスをターゲット設定します。

>[!NOTE]
>
>[!UICONTROL データプロバイダー] には at.js 1.3 以降が必要です。

## サポート資料

* [at.js とAdobe Targetでのデータプロバイダーの実装](implement-data-providers-to-integrate-third-party-data.md)

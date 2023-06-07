---
title: レスポンストークンと at.js カスタムイベントの使用方法
description: レスポンストークンと at.js カスタムイベントを使用して、Target とサードパーティシステムの間でプロファイル情報を共有する方法について説明します。
role: Developer
level: Experienced
topic: Personalization, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: d6ce5367-a453-4e6c-8545-9fa676977f04
source-git-commit: 80208b3ecbc0d627d2afe72f882e91c9800d2726
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 3%

---

# Adobe Targetでのレスポンストークンと at.js カスタムイベントの使用

レスポンストークンおよび `at.js` カスタムイベントを使用すると、 [!DNL Target] をサードパーティ製システムに追加する必要があります。 内の任意のオブジェクト [!DNL Target] カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなどの訪問者プロファイルを [!DNL Target] の応答を参照してください。この応答では、カスタム JavaScript を使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

## レスポンストークンと at.js カスタムイベントの使用方法

1. 必要なデータの決定元 [!DNL Target]
1. 設定/レスポンストークン画面で切り替えを反転して、必要なデータのレスポンストークンをオンにします。
1. 使用する必要があるイベントリスナーを決定します
1. Adobe Targetイベントをリッスンし、レスポンストークンを読み取り、統合に必要な操作を行うために必要な JavaScript を記述します
1. 「Load Target」アクションの後に、Launch のカスタムコードアクションを使用してイベントリスナー JavaScript をデプロイするか、セットアップ/実装画面で at.js の「ライブラリフッター」セクションに追加して、新しい at.js ファイルを保存します。
1. 統合の QA と公開

## その他のリソース

* [Adobe TargetでのExperience Cloud Debuggerの使用](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [レスポンストークンのドキュメント](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=en)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)

---
title: で A4T レポートを設定する方法 [!DNL Analysis Workspace] 対象 [!UICONTROL 自動配分] アクティビティ
description: で A4T レポートを設定する方法を教えてください。 [!DNL Analysis Workspace] を実行すると、期待した結果が得られます。 [!UICONTROL 自動配分] アクティビティ。
role: User
badgeBeta: label="Beta" type="Informative" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html#beta newtab=true" tooltip="What are Target Beta release features?"
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 7d53adce-cc05-4754-9369-9cc1763a9450
source-git-commit: b29362ea45196d09dcbfbceeaaed5bc20467ea16
workflow-type: tm+mt
source-wordcount: '1083'
ht-degree: 0%

---

# での A4T レポートの設定 [!DNL Analysis Workspace] 対象 [!DNL Auto-Allocate] アクティビティ

An [!DNL Auto-Allocate] 「 」アクティビティでは、2 つ以上のエクスペリエンスの中から勝者を特定し、テストの実施と学習が続く間に、自動的にその勝者に配分するトラフィックを増やします。 この [!UICONTROL Analytics for Target] (A4T) 統合： [!UICONTROL 自動配分] レポートデータを [!DNL Adobe Analytics]を使用すると、 [!DNL Analytics].

豊富な分析機能は [!DNL Adobe Analytics] [!DNL Analysis Workspace]( デフォルトの **[!UICONTROL Analytics for Target]** 正しく解釈するには、パネルが必要です [!DNL Auto-Allocate] アクティビティ、 [最適化基準](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html#supported){target=_blank}.

このチュートリアルでは、分析に推奨される変更について説明します [!DNL Auto-Allocate] アクティビティ [!DNL Analysis Workspace]. 主な概念は次のとおりです。

* [!UICONTROL 訪問者] は、常に [!DNL Auto-Allocate] アクティビティ。
* 指標が [!DNL Adobe Analytics] 指標を使用する場合、コンバージョン率に適した分子は、アクティビティの設定時に選択した最適化条件のタイプに応じて異なります。
   * 「個別訪問者コンバージョン率の最大化」の最適化基準では、コンバージョン率が算出されます。このコンバージョン率の分子は、指標の正の値を持つ個別訪問者の数です。
   * 「訪問者ごとの指標値の最大化」には、コンバージョン率があります。このコンバージョン率の分子は、 [!DNL Adobe Analytics]. これは、デフォルトで **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace].
* 最適化指標が [!DNL Target] 定義済みのコンバージョン指標、デフォルト **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace] はパネルの設定を処理します。
* すべて [!UICONTROL 自動配分] 以前に作成されたアクティビティ [!DNL Target Standard/Premium] 23.3.1 リリース（2023 年 3 月 31 日） [!DNL Analytics Workspace] および [!DNL Target] 同じ値を示す [!UICONTROL 信頼性].

   すべて [!UICONTROL 自動配分] 2023 年 3 月 30 日以降に作成されたアクティビティ。 [!DNL Analysis Workspace] 反映しない [～が使うより保守的な統計 [!UICONTROL 自動配分]](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html#section_98388996F0584E15BF3A99C57EEB7629){target=_blank} これらのアクティビティが *両方* 次の条件を満たす場合：

   * [!DNL Analytics] レポートソースとして (A4T)
   * [!DNL Analytics] 最適化指標

   If *両方* これらの条件が存在する場合は、信頼性指標を A4T パネルから削除する必要があります。 代わりに、 [!DNL Target] レポート。

## 用の A4T の作成 [!DNL Auto-Allocate] パネル内 [!DNL Analysis Workspace]

用の A4T を作成するには、以下を実行します。 [!DNL Auto-Allocate] レポートは **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace]、以下に示すように。 次に、以下の選択を行います。

1. **[!UICONTROL コントロールエクスペリエンス]**:任意のエクスペリエンスを選択できます。
2. **[!UICONTROL 指標の標準化]**:訪問者を選択します。 [!DNL Auto-Allocate] は常に個別訪問者ごとにコンバージョン率を標準化します。
3. **[!UICONTROL 成功指標]**:アクティビティの作成時に使用したのと同じ指標を選択します。 これが [!DNL Target] 定義済みのコンバージョン指標、「 **アクティビティコンバージョン**. それ以外の場合は、 [!DNL Adobe Analytics] 指標を使用しています。

![[!UICONTROL Analytics for Target] パネル設定 [!DNL Auto-Allocate] アクティビティ。](assets/AAFigure1.png)

*図 1: [!UICONTROL Analytics for Target] パネル設定 [!DNL Auto-Allocate] アクティビティ。*

>[!NOTE]
>
> 事前設計された **[!UICONTROL Analytics for Target]** パネルを使用します。 [!DNL Adobe Target].

## [!DNL Target] [!UICONTROL コンバージョン] 指標または [!DNL Analytics] 「訪問者あたりの指標値を最大化」最適化条件を使用する指標

デフォルトの A4T パネルが [!DNL Auto-Allocate] 目標指標が以下のいずれかに該当するアクティビティ [!DNL Target] コンバージョンまたは [!DNL Analytics] 指標と最適化基準「訪問者あたりの指標値を最大化」を使用

このパネルの例は、 [!UICONTROL 売上高] 指標を使用します。ここで、「訪問者あたりの指標値の最大化」が、アクティビティ作成時の最適化条件として選択されています。 前述のように、 [!DNL Auto-Allocate] では、 **[!UICONTROL Analytics for Target]** パネル。 Adobeでは、A4T パネルおよび関連する上昇率指標（下限と上限の指標）から信頼性指標を削除することをお勧めします。 代わりに、 [!DNL Target] レポート。

![[!UICONTROL Target の Analytics — 自動配分レポート] パネル](assets/AAFigure2.png)

*図 2:の推奨レポート [!DNL Auto-Allocate] アクティビティ [!DNL Analytics] 指標「訪問者あたりの指標値の最適化」の基準。 これらのタイプの指標の [!DNL Target] 定義済みのコンバージョン指標、デフォルト&#x200B;**[!UICONTROL Analytics for Target]**パネル内 [!DNL Analysis Workspace] を使用できます。*

## [!DNL Analytics] 指標に「個別訪問者コンバージョン率を最大化」最適化条件が設定されている

実行時に [!DNL Adobe Analytics] 指標は、 *個別訪問者コンバージョン率の最大化*、デフォルト **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace] を変更する必要があります。

成功指標は、コンバージョン指標が肯定的だった個別訪問者の数になりました。 これは、指標の正の値を持つヒットをフィルタリングするセグメントを作成することで実現できます。 次のように、このセグメントを作成します。

1. を選択します。 **[!UICONTROL コンポーネント]** > **[!UICONTROL セグメントを作成]** オプション [!DNL Analysis Workspace] ツールバー。
1. アクティビティ作成時に使用する指標を左側のパネルから **[!UICONTROL 定義]** 」ボックスに入力します。
1. 指標の値を選択します。 **より大きい** 0 の数値。
1. 次の **[!UICONTROL 次を含む]** ドロップダウンで、「 **[!UICONTROL 訪問者]**.
1. セグメントに適切な名前を付けます。

セグメントの作成例を次の図に示します。この図で、 [!UICONTROL 正の売上高を持つ訪問者].

![[!UICONTROL 正の売上高を持つ訪問者] セグメント内 [!DNL Analysis Workspace]](assets/AAFigure3.png)

*図 3:のセグメント作成 [!DNL Adobe Analytics] 最適化条件が「」と等しい指標[!UICONTROL 個別訪問者コンバージョン率の最大化].&quot; この例では、指標は [!UICONTROL 売上高]を最適化する目標は、正の売上高を持つ訪問者数を最大化することです。*

適切なセグメントが作成された後、デフォルトの  **[!UICONTROL Analytics for Target]** パネル内 [!DNL Analysis Workspace] を変更できます。

1. 秒を追加 **実訪問者数** 既存の指標と並行した指標 [!UICONTROL 訪問者] 指標列を使用します。
2. 最初の列の下に新しく作成したセグメントをドラッグし、図 4 のようなパネルを作成します。 違いに注意してください。正の売上高を持つユニーク訪問者の数は、各エクスペリエンスに割り当てられたユニーク訪問者の合計数の一部です。

   ![図 4.png](assets/AAFigure4.png)

   *図 4:フィルター [!UICONTROL 実訪問者数] 新しく作成したセグメント別*

3. コンバージョン率は、 [迅速に計算される](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/components/calculated-metrics/quick-calculated-metrics-in-analysis-workspace.html) 1 列目と 2 列目の両方をハイライト表示し、右クリックして、選択 **[!UICONTROL 選択から指標を作成]** > **[!UICONTROL 除算]**.

   次の図に示すように、デフォルトのコンバージョン率は削除し、この新しい計算指標に置き換える必要があります。 新しく作成した計算指標を編集して **[!UICONTROL 形式]** > **[!UICONTROL 割合]** 図のように、小数点以下 2 桁までです。

   ![図 5.png](assets/AAFigure5.png)

   *図 5:最終 [!UICONTROL 自動配分] 2 値化された売上高コンバージョン指標のコンバージョン率を示すパネル*

## まとめ

このチュートリアルの手順では、を正しく設定する方法を示しました [!DNL Analysis Workspace] 表示する [!UICONTROL 自動配分] レポートデータ。

まとめると、次のようになります。

* 指標が [!DNL Target] 定義済みのコンバージョン指標または [!DNL Adobe Analytics] 指標と最適化基準「訪問者あたりの指標値を最大化」を使用する場合は、訪問者を標準化指標として設定したデフォルトのワークスペースパネルを使用する必要があります。
* 指標が [!DNL Adobe Analytics] 指標と最適化基準「個別訪問者コンバージョン率を最大化」を使用する場合は、指標が正の数の訪問者の割合として定義されたコンバージョン率を使用する必要があります。 これは、 [!UICONTROL 個別訪問者] 指標。

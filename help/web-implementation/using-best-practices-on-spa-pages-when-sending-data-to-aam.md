---
title: データをAAMに送信する際のSPA ページでのベストプラクティスの使用
description: 単一ページアプリケーション（SPA）からAdobe Audience Manager（AAM）にデータを送信する際のベストプラクティスについて説明します。 この記事では、推奨される実装方法であるExperience Platformタグの使用に焦点を当てています。
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d4874d9f6d7a36bb81ac183eb8b853d893822ae0
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---

# データをAAMに送信する際のSPA ページでのベストプラクティスの使用 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

このドキュメントでは、シングルページアプリケーション（SPA）からAdobe Audience Manager（AAM）にデータを送信するためのベストプラクティスをいくつか説明します。 この記事では、推奨される実装方法である [!UICONTROL Experience Platform tags] の使用に焦点を当てています。

## 最初のメモ

* 以下の項目は、Platform タグを使用してサイトに実装していることを前提としています。 Platform タグを使用していない場合でも考慮事項は存在しますが、実装方法に合わせて調整する必要があります。
* SPAはそれぞれ異なるため、要件に合わせて以下の項目を微調整する必要が生じる場合がありますが、Adobeでは、SPA ページからAudience Managerにデータを送信する際に考慮する必要がある、いくつかのベストプラクティスを紹介したいと考えています。

## Experience Platformタグ（以前の Launch）でのSPAおよびAAMの操作に関する簡単な図{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![ タグでの aam 用 spa](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>前述のように、これは Platform タグを使用したAdobe Audience Manager実装（Adobe Analyticsなし）で、SPA ページがどのように処理されるかを簡単に示した図です。 ご覧のように、これは非常に簡単です。大きな決定は、ビューの変更（またはアクション）を Platform タグにどのように伝えるかです。

## SPA ページからのタグのトリガー {#triggering-launch-from-the-spa-page}

Platform タグでルールをトリガーする（したがってAudience Managerにデータを送信する）最も一般的な方法は、次の 2 つです。

* JavaScript カスタムイベントの設定（Adobe Analyticsを使用した例 [ こちら ](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) を参照）
* [!UICONTROL Direct Call Rule] の使用

このAudience Managerの例では、Platform タグで [!UICONTROL Direct Call rule] を使用して、Audience Managerに入るヒットをトリガーします。 次の節で説明するように、[!UICONTROL Data Layer] を新しい値に設定すると、Platform タグの [!UICONTROL Data Element] で取得できるので便利です。

## デモページ {#demo-page}

次に、SPA ページで行うように、データレイヤー内の値を変更してAudience Managerに送信する方法を示す小さなページを示します。 この機能をモデル化して、より詳細な変更が必要になるようにすることができます。 このデモページは [ こちら ](https://aam.enablementadobe.com/SPA-Launch.html) にあります。

## データレイヤーの設定 {#setting-the-data-layer}

前述のように、新しいコンテンツがページに読み込まれるとき、またはユーザーがサイトでアクションを実行するとき、データレイヤーは、Platform タグが呼び出されて [!UICONTROL rules] を実行する前に、ページの先頭に動的に設定される必要があります。これにより、Platform タグはデータレイヤーから新しい値を取得し、Audience Managerにプッシュできます。

上記のデモサイトに移動し、ページソースを確認すると、次のように表示されます。

* データレイヤーが、Platform タグを呼び出す前の、ページの先頭にあります
* シミュレートされたSPA リンクのJavaScriptが [!UICONTROL Data Layer] を変更してから、Platform タグを呼び出します（`_satellite.track()` 呼び出し）。 この [!UICONTROL Direct Call Rule] の代わりにJavaScript カスタムイベントを使用した場合、レッスンは同じです。 最初に [!DNL data layer] を変更し、次に Platform タグを呼び出します。

>[!VIDEO](https://video.tv.adobe.com/v/38109/?quality=12&captions=jpn)

## その他のリソース {#additional-resources}

* [AdobeフォーラムでのSPA ディスカッション ](https://forums.adobe.com/thread/2451022)
* [Platform タグへのSPAの実装方法を示すリファレンスアーキテクチャのサイト ](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [Adobe AnalyticsでSPAをトラッキングする際のベストプラクティスの使用 ](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [ この記事で使用するデモサイト ](https://aam.enablementadobe.com/SPA-Launch.html)

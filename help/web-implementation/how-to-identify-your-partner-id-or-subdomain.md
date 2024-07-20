---
title: パートナー ID またはサブドメインの識別方法
description: 一部のExperience Cloud機能を実装する際にパートナー ID またはサブドメインを特定する方法と、Audience ManagerUI でこの ID を取得できる場所について説明します。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Audience Managerサブドメインの識別方法 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

一部のExperience Cloud機能を実装する場合は、Audience Manager`Subdomain` ードが何であるかを知る必要があります（`client ID` または `Partner ID` とも呼ばれます）。 このビデオでは、Audience ManagerUI でこの情報を取得できる 2 つの場所を説明します。

## 結末を捨てて… {#giving-away-the-ending}

この短いビデオを視聴せずに飛び込んで見つけたい場合は、UI の次の 2 つの場所で `Partner Subdomain` を検索できます。

1. [!UICONTROL rule-based] 特性を既に作成している場合は、「**[!UICONTROL Get Trait URL]**」をクリックします
   [!UICONTROL Get Trait URL] は、そのフォルダー内の特性リスト内の特性の横にあり、URL は URL にサブドメインを含みます。
1. **[!UICONTROL Tools]**/**[!UICONTROL Tags]** インターフェイスに移動して、コンテナの **[!UICONTROL Get code]** をクリックすると、サブドメインは Akamai 行の末尾に向けられます

これらのクイックリファレンスですばやく見つからない場合、このビデオは短時間のコミットメントです。 :）

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>Adobe Experience Cloudの各カスタマーには数値の ID が割り当てられますが、これは多くの場合、「PID」やパートナー ID と呼ばれます。 この ID は、この記事とビデオで説明している ID ではありません。 代わりに、「パートナーサブドメイン」（パートナー ID と呼ばれることもあります）は、通常、クライアント名のバージョンであり、データの送信先のサーバーのサブドメインです。 例えば、会社が「ボブのノブ」（すべてのものがドアノブで、もちろん笑）の場合、パートナースブドメインは「bobsknobs」である可能性が高くなりますが、「PID」は「12345」のようなものです。 通常は PID を知る必要はありませんが、Audience Manager実装を設定するためには、サブドメインを知ることが重要です。
>

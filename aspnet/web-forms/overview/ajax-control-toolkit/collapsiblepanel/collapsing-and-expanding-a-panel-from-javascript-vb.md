---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: "折りたたみと展開の JavaScript (VB) からパネル |Microsoft ドキュメント"
author: wenz
description: "ASP.NET AJAX コントロールのツールキットで CollapsiblePanel コントロール パネルを拡張し、その内容を折りたたんだり、展開する機能を提供する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adca6771042cad71139977496f985cb8dac63aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="d93f1-103">折りたたみモードと JavaScript (VB) からのパネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="d93f1-104">によって[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d93f1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d93f1-105">[コードをダウンロードする](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d93f1-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="d93f1-106">ASP.NET AJAX コントロールのツールキットで CollapsiblePanel コントロールは、パネルを拡張し、その内容を折りたたむし、再度展開する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="d93f1-107">これら 2 つのアクションは、カスタム JavaScript コードからトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d93f1-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="d93f1-108">概要</span><span class="sxs-lookup"><span data-stu-id="d93f1-108">Overview</span></span>

<span data-ttu-id="d93f1-109">ASP.NET AJAX コントロールのツールキットで CollapsiblePanel コントロールは、パネルを拡張し、その内容を折りたたむし、再度展開する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="d93f1-110">これら 2 つのアクションは、カスタム JavaScript コードからトリガーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="d93f1-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d93f1-111">手順</span><span class="sxs-lookup"><span data-stu-id="d93f1-111">Steps</span></span>

<span data-ttu-id="d93f1-112">まず、新しい ASP.NET ページを作成し、含める、`ScriptManager`内、`<form>`要素。</span><span class="sxs-lookup"><span data-stu-id="d93f1-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="d93f1-113">これは、コントロールのツールキットに必要な ASP.NET AJAX ライブラリをロードします。</span><span class="sxs-lookup"><span data-stu-id="d93f1-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="d93f1-114">次に、展開/折りたたみ効果がわかるようにいくつかのテキストで、パネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="d93f1-115">わかりますパネルは、ここで示すようになり、基本的には、背景色とパネルの幅を定義します)、CSS クラスを参照します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="d93f1-116">`CollapsiblePanelExtender`コントロールに必要な`TargetControlID`属性のツールキットが認識するパネルを折りたたむか、要求時に展開できるようにします。</span><span class="sxs-lookup"><span data-stu-id="d93f1-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="d93f1-117">残念ながら、エクステンダー現在公開しない特定の API、パネルを展開または折りたたみが、一部の文書化されていないメソッドは操作を行います。</span><span class="sxs-lookup"><span data-stu-id="d93f1-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="d93f1-118">まずを折りたたんだり展開したり、パネルの内容を展開するには、クライアント側 JavaScript をトリガーし、ページに、3 つの HTML ボタンを追加します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="d93f1-119">クライアント側の JavaScript コードで (の使用を開始`<script type="text/javascript">`) では、`$find()`メソッドを使用する必要があるアクセスに、`CollapsiblePanelExtender`です。</span><span class="sxs-lookup"><span data-stu-id="d93f1-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="d93f1-120">`$find("cpe")`参照を返します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="d93f1-121">上から、特定のメソッドは、前のタスクを解決します。</span><span class="sxs-lookup"><span data-stu-id="d93f1-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="d93f1-122">メソッド、パネルが呼び出されます (展開) を開くため`_doOpen()`; 次のコードを実装、`doOpen()`関数は、最初のボタンがクリックされたときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d93f1-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="d93f1-123">閉じる、または、パネルを折りたたみ、`_doClose()`メソッドを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d93f1-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="d93f1-124">したがって、ユーザーは、2 番目のボタンをクリックすると、次の JavaScript コードが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d93f1-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="d93f1-125">3 番目のボタンは、パネルの状態を切り替えます: から折りたたまれているを展開すると、その逆です。</span><span class="sxs-lookup"><span data-stu-id="d93f1-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="d93f1-126">`CollapsiblePanelExtender`公開、`toggle()`ようメソッド: パネルの状態を反転させます。</span><span class="sxs-lookup"><span data-stu-id="d93f1-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="d93f1-127">ただしも別の方法 (によって内部的に使用される、`toggle()`メソッド):`get_Collapsed()`のメソッド、`CollapsiblePanelExtender()`パネルが折りたたまれているかどうかどうかがわかります。</span><span class="sxs-lookup"><span data-stu-id="d93f1-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="d93f1-128">この関数の戻り値は、パネルが展開されているいずれか (`_doOpen()`メソッド) か、折りたたんだ状態 (`_doClose()`) メソッド。</span><span class="sxs-lookup"><span data-stu-id="d93f1-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="d93f1-129">[![3 番目のボタン、パネルの状態を変更する: から拡張とバックのために折りたたむ](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d93f1-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="d93f1-130">3 番目のボタン、パネルの状態を変更する: から拡張とバックのために折りたたむ ([フルサイズのイメージを表示するをクリックして](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d93f1-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d93f1-131">前へ</span><span class="sxs-lookup"><span data-stu-id="d93f1-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
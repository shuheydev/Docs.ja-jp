---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: "FTP クライアント (c#) を使用して、サイトを展開する |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET アプリケーションを配置する最も簡単な方法では、開発環境から運用環境に必要なファイルを手動でコピーします。 Thi しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e4af20fa1fecd1f363e979023b41203096d64ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-an-ftp-client-c"></a><span data-ttu-id="529e8-104">FTP クライアント (c#) を使用して、サイトを展開します。</span><span class="sxs-lookup"><span data-stu-id="529e8-104">Deploying Your Site Using an FTP Client (C#)</span></span>
====================
<span data-ttu-id="529e8-105">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="529e8-105">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="529e8-106">[コードをダウンロードする](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)</span><span class="sxs-lookup"><span data-stu-id="529e8-106">[Download Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) or [Download PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)</span></span>

> <span data-ttu-id="529e8-107">ASP.NET アプリケーションを配置する最も簡単な方法では、開発環境から運用環境に必要なファイルを手動でコピーします。</span><span class="sxs-lookup"><span data-stu-id="529e8-107">The simplest way to deploy an ASP.NET application is to manually copy the necessary files from the development environment to the production environment.</span></span> <span data-ttu-id="529e8-108">このチュートリアルでは、FTP クライアントを使用して、web ホスト プロバイダーをデスクトップからファイルを取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="529e8-108">This tutorial shows how to use an FTP client to get the files from your desktop to the web host provider.</span></span>


## <a name="introduction"></a><span data-ttu-id="529e8-109">はじめに</span><span class="sxs-lookup"><span data-stu-id="529e8-109">Introduction</span></span>

<span data-ttu-id="529e8-110">前のチュートリアルには、いくつかの ASP.NET ページ、マスター ページ、カスタム ベースで構成されますが、単純なの書籍レビュー ASP.NET web アプリケーションが導入された`Page`クラス、イメージの数と、次の 3 つの CSS スタイル シートです。</span><span class="sxs-lookup"><span data-stu-id="529e8-110">The previous tutorial introduced a simple Book Review ASP.NET web application, which is comprised of a handful of ASP.NET pages, a master page, a custom base `Page` class, a number of images, and three CSS style sheets.</span></span> <span data-ttu-id="529e8-111">この時点で、アプリケーションがアクセスできるだれでも、インターネットへの接続に、web ホスト プロバイダーには、このアプリケーションを展開する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="529e8-111">We are now ready to deploy this application to a web host provider, at which point the application will be accessible to anyone with a connection to the Internet!</span></span>


<span data-ttu-id="529e8-112">話し合いから、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-cs.md)チュートリアルでは、web ホスト プロバイダーにコピーする必要があるファイルがわかります。</span><span class="sxs-lookup"><span data-stu-id="529e8-112">From our discussions in the [*Determining What Files Need to Be Deployed*](determining-what-files-need-to-be-deployed-cs.md) tutorial, we know what files need to be copied to the web host provider.</span></span> <span data-ttu-id="529e8-113">(再呼び出しがどのようなファイルがコピーされるによって異なるかどうか、アプリケーションが明示的にまたは自動的にコンパイルします。)しかし、どうやってファイル (デスクトップ、) の開発環境から運用環境 (web ホスト プロバイダーによって管理されている web サーバー) まででしょうか。</span><span class="sxs-lookup"><span data-stu-id="529e8-113">(Recall that what files are copied depends on whether your application is explicitly or automatically compiled.) But how do we get the files from the development environment (our desktop) up to the production environment (the web server managed by the web host provider)?</span></span> <span data-ttu-id="529e8-114">[ **F** ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol)ネットワーク経由で 1 台のコンピューターから別のファイルをコピーするための一般的に使用されるプロトコルします。</span><span class="sxs-lookup"><span data-stu-id="529e8-114">The [**F** ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) is a commonly used protocol for copying files from one machine to another over a network.</span></span> <span data-ttu-id="529e8-115">FrontPage Server Extensions (FPSE) こともできます。</span><span class="sxs-lookup"><span data-stu-id="529e8-115">Another option is FrontPage Server Extensions (FPSE).</span></span> <span data-ttu-id="529e8-116">このチュートリアルは、スタンドアロンの FTP クライアント ソフトウェアを使用して、必要なファイル、開発環境から運用環境を展開するについて説明します。</span><span class="sxs-lookup"><span data-stu-id="529e8-116">This tutorial focuses on using stand-alone FTP client software to deploy the necessary files from the development environment to the production environment.</span></span>

> [!NOTE]
> <span data-ttu-id="529e8-117">Visual Studio には、FTP; 経由で web サイトを発行するためのツールが含まれています。次のチュートリアルを見て FPSE を使用するツールと同様に、これらのツールについて説明します。</span><span class="sxs-lookup"><span data-stu-id="529e8-117">Visual Studio includes tools for publishing websites via FTP; these tools, as well as a look at tools that use FPSE, are covered in the next tutorial.</span></span>


<span data-ttu-id="529e8-118">必要な FTP を使用してファイルをコピーする、 *FTP クライアント*開発環境でします。</span><span class="sxs-lookup"><span data-stu-id="529e8-118">To copy the files using FTP we need an *FTP client* on the development environment.</span></span> <span data-ttu-id="529e8-119">FTP クライアントが実行しているコンピューターにインストールされているコンピューターからファイルをコピーするように設計されたアプリケーション、 *FTP サーバー*です。</span><span class="sxs-lookup"><span data-stu-id="529e8-119">An FTP client is an application that is designed to copy files from the computer it's installed to a computer that is running an *FTP server*.</span></span> <span data-ttu-id="529e8-120">(Web ホスト プロバイダーはほとんどの FTP を介したファイル転送をサポートする場合は、FTP サーバー、web サーバーで実行されている。)FTP クライアント アプリケーションの数は使用できます。</span><span class="sxs-lookup"><span data-stu-id="529e8-120">(If your web host provider supports file transfers via FTP, as most do, then there is an FTP server running on their web servers.) There are a number of FTP client applications available.</span></span> <span data-ttu-id="529e8-121">Web ブラウザーは、FTP クライアントとしても倍精度浮動小数点ことができます。</span><span class="sxs-lookup"><span data-stu-id="529e8-121">Your web browser can even double as an FTP client.</span></span> <span data-ttu-id="529e8-122">お気に入りの FTP クライアントとは、このチュートリアルを使用した 1 つは[FileZilla](http://filezilla-project.org/)Windows、Linux、および mac コンピューターで利用可能な無料、オープン ソースの FTP クライアント。</span><span class="sxs-lookup"><span data-stu-id="529e8-122">My favorite FTP client, and the one I will be using for this tutorial, is [FileZilla](http://filezilla-project.org/), a free, open-source FTP client that is available for Windows, Linux, and Macs.</span></span> <span data-ttu-id="529e8-123">任意の FTP クライアントは機能、ただし、ためお気軽に最も使いやすいはどのようなクライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="529e8-123">Any FTP client will work, though, so feel free to use whatever client you are most comfortable with.</span></span>

<span data-ttu-id="529e8-124">に沿ってしている場合は必要がありますにアカウントを作成する前に、web ホスト プロバイダーを使用できますこのチュートリアルまたは完了以降。</span><span class="sxs-lookup"><span data-stu-id="529e8-124">If you are following along you will need to create an account with a web host provider before you can complete this tutorial or subsequent ones.</span></span> <span data-ttu-id="529e8-125">述べたように前のチュートリアルでは、さまざまな価格、機能、およびサービスの品質と web ホスト プロバイダー会社などがあります。</span><span class="sxs-lookup"><span data-stu-id="529e8-125">As noted in the previous tutorial, there are a gaggle of web host provider companies with a wide spectrum of prices, features, and quality of service.</span></span> <span data-ttu-id="529e8-126">このチュートリアルの系列が使用する[Discount ASP.NET](http://discountasp.net) web ホストとして、プロバイダーができますに従って、web ホスト プロバイダーでは、サイトが開発された ASP.NET バージョンをサポートしている限り、します。</span><span class="sxs-lookup"><span data-stu-id="529e8-126">For this tutorial series I will be using [Discount ASP.NET](http://discountasp.net) as my web host provider, but you can follow along with any web host provider as long as they support the ASP.NET version your site is developed in.</span></span> <span data-ttu-id="529e8-127">(これらのチュートリアル使用して作成された ASP.NET 3.5。)また、ファイル、web ホスト プロバイダーをコピーしているため FTP を使用して、このチュートリアルと将来のでは、web ホスト プロバイダーが、web サーバーに対する FTP アクセスをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="529e8-127">(These tutorials were created using ASP.NET 3.5.) Also, because we will be copying the files to the web host provider using FTP in this tutorial and in future ones it is imperative that your web host provider supports FTP access to their web servers.</span></span> <span data-ttu-id="529e8-128">ほぼすべての web ホスト プロバイダーは、この機能を提供しますが、サインアップする前に再確認してください。</span><span class="sxs-lookup"><span data-stu-id="529e8-128">Virtually all web host providers offer this feature, but you should double-check before signing up.</span></span>

## <a name="deploying-the-book-review-web-application-project"></a><span data-ttu-id="529e8-129">Book レビュー Web アプリケーション プロジェクトを配置します。</span><span class="sxs-lookup"><span data-stu-id="529e8-129">Deploying the Book Review Web Application Project</span></span>

<span data-ttu-id="529e8-130">Web アプリケーションの書籍の確認の 2 つのバージョンがあることに注意してください: 1 つの Web アプリケーション プロジェクト モデル (BookReviewsWAP) およびその他の Web サイト プロジェクト モデル (BookReviewsWSP) を使用してを使用して実装します。</span><span class="sxs-lookup"><span data-stu-id="529e8-130">Recall that there are two versions of the Book Review web application: one implemented using the Web Application Project model (BookReviewsWAP) and the other using the Web Site Project model (BookReviewsWSP).</span></span> <span data-ttu-id="529e8-131">プロジェクトの種類は、自動的にまたは明示的に、サイトがコンパイルされ、そのコンパイル モデルがどのようなファイルを配置する必要がありますを指定するかどうかに影響します。</span><span class="sxs-lookup"><span data-stu-id="529e8-131">The project type influences whether the site is compiled automatically or explicitly, and that compilation model dictates what files need to be deployed.</span></span> <span data-ttu-id="529e8-132">その結果、見ていきます BookReviewsWAP と BookReviewsWSP プロジェクトを個別に、展開する、BookReviewsWAP で開始します。</span><span class="sxs-lookup"><span data-stu-id="529e8-132">Consequently, we will examine deploying the BookReviewsWAP and BookReviewsWSP projects separately, starting with the BookReviewsWAP.</span></span> <span data-ttu-id="529e8-133">行っていない既にこれら 2 つの ASP.NET アプリケーションをダウンロードしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="529e8-133">Take a moment to download these two ASP.NET applications if you have not done so already.</span></span>

<span data-ttu-id="529e8-134">移動して BookReviewsWAP プロジェクトを起動して、`BookReviewsWAP`フォルダーとをダブルクリックすると、`BookReviewsWAP.sln`ファイル。</span><span class="sxs-lookup"><span data-stu-id="529e8-134">Launch the BookReviewsWAP project by navigating to the `BookReviewsWAP` folder and double-clicking the `BookReviewsWAP.sln` file.</span></span> <span data-ttu-id="529e8-135">プロジェクトを配置する前に、ビルドのソース コードを変更するが、コンパイルされたアセンブリに含まれていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-135">Before deploying the project it is important to build it to ensure that any changes to the source code are included in the compiled assembly.</span></span> <span data-ttu-id="529e8-136">プロジェクトをビルドするビルド メニューを開き、BookReviewsWAP のビルド メニュー オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="529e8-136">To build the project go to the Build menu and choose the Build BookReviewsWAP menu option.</span></span> <span data-ttu-id="529e8-137">1 つのアセンブリに、プロジェクト内のソース コードをコンパイルこの`BookReviewsWAP.dll`に配置されている、`Bin`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="529e8-137">This compiles the source code in the project into a single assembly, `BookReviewsWAP.dll`, which is placed in the `Bin` folder.</span></span>

<span data-ttu-id="529e8-138">必要なファイルを展開する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="529e8-138">We are now ready to deploy the necessary files!</span></span> <span data-ttu-id="529e8-139">FTP クライアントを起動し、web ホスト プロバイダーの web サーバーに接続します。</span><span class="sxs-lookup"><span data-stu-id="529e8-139">Launch your FTP client and connect to the web server at your web host provider.</span></span> <span data-ttu-id="529e8-140">(Web ホスト会社にサインアップするときに、FTP サーバーに接続する方法に関する情報を電子メールで送信されます。 これには、FTP サーバーだけでなく、ユーザー名とパスワードのアドレスが含まれます) です。</span><span class="sxs-lookup"><span data-stu-id="529e8-140">(When you sign up with a web hosting company they will e-mail you information on how to connect to the FTP server; this includes the address for the FTP server as well as a username and password.)</span></span>

<span data-ttu-id="529e8-141">デスクトップから、次のファイルを web ホスト プロバイダーのルート web サイト フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="529e8-141">Copy the following files from your desktop to the root website folder at your web host provider.</span></span> <span data-ttu-id="529e8-142">ホストしている場合、web サーバーに FTP、web のプロバイダーは、web サイトのルート ディレクトリにあります。</span><span class="sxs-lookup"><span data-stu-id="529e8-142">When you FTP into the web server at the web host provider you are likely at the root website directory.</span></span> <span data-ttu-id="529e8-143">ただし、一部の web ホスト プロバイダーがという名前のサブフォルダーをある`www`または`wwwroot`web サイトのファイルのルート フォルダーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="529e8-143">However, some web host providers have a subfolder named `www` or `wwwroot` that serves as the root folder for your website files.</span></span> <span data-ttu-id="529e8-144">最後に、ファイルを FTPing ときにする必要があります、実稼働環境に対応するフォルダー構造を作成する、`Bin`フォルダー、`Fiction`フォルダー、`Images`フォルダーというようにします。</span><span class="sxs-lookup"><span data-stu-id="529e8-144">Finally, when FTPing the files you may need to create the corresponding folder structure on the production environment - the `Bin` folder, the `Fiction` folder, the `Images` folder, and so on.</span></span>

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- <span data-ttu-id="529e8-145">完全な内容、`Styles`フォルダー</span><span class="sxs-lookup"><span data-stu-id="529e8-145">The complete contents of the `Styles` folder</span></span>
- <span data-ttu-id="529e8-146">完全な内容、`Images`フォルダー (とそのサブフォルダー `BookCovers`)</span><span class="sxs-lookup"><span data-stu-id="529e8-146">The complete contents of the `Images` folder (and its subfolder, `BookCovers`)</span></span>
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

<span data-ttu-id="529e8-147">図 1 は、必要なファイルをコピーした後、FileZilla を示します。</span><span class="sxs-lookup"><span data-stu-id="529e8-147">Figure 1 shows FileZilla after the necessary files have been copied.</span></span> <span data-ttu-id="529e8-148">FileZilla は、左と右上のリモート コンピューター上のファイルで、ローカル コンピューター上のファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="529e8-148">FileZilla displays the files on the local computer on the left and the files on the remote computer on the right.</span></span> <span data-ttu-id="529e8-149">図 1 に示す ASP.NET ソース コード ファイルなど`About.aspx.cs`、ローカル コンピューター (開発環境) 上にあるが、コード ファイルを使用する場合に展開する必要がないために、web ホスト プロバイダー (運用環境) にコピーされていません。明示的なコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="529e8-149">As Figure 1 shows, the ASP.NET source code files, such as `About.aspx.cs`, are on the local computer (the development environment) but were not copied to the web host provider (the production environment) because code files do not need to be deployed when using explicit compilation.</span></span>

> [!NOTE]
> <span data-ttu-id="529e8-150">これらは無視されて、ソース コード ファイルの実稼働サーバー上で害はありません。</span><span class="sxs-lookup"><span data-stu-id="529e8-150">There is no harm in having the source code files on the production server, as they are ignored.</span></span> <span data-ttu-id="529e8-151">ASP.NET は、これらは、web サイトへの訪問者にアクセスできない場合でも、ソース コード ファイルは、実稼働サーバー上に存在できるように、既定ではソース コード ファイルへの HTTP 要求が禁止されます。</span><span class="sxs-lookup"><span data-stu-id="529e8-151">ASP.NET forbids HTTP requests to source code files by default so that even if the source code files are present on the production server they are inaccessible to visitors to your website.</span></span> <span data-ttu-id="529e8-152">(を参照してくださいしようとしているユーザーの場合は、`http://www.yoursite.com/Default.aspx.cs`これらの種類のファイルを含むことを説明するエラー ページが表示されます`.cs`ファイルでは禁止されています)。</span><span class="sxs-lookup"><span data-stu-id="529e8-152">(That is, if a user tries to visit `http://www.yoursite.com/Default.aspx.cs` they will get an error page that explains that these types of files - `.cs` files - are forbidden.)</span></span>


<span data-ttu-id="529e8-153">[![FTP クライアントを使用して、デスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーするには](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="529e8-153">[![Use an FTP Client to Copy the Necessary Files from Your Desktop to the Web Server at the Web Host Provider](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)</span></span>

<span data-ttu-id="529e8-154">**図 1**: FTP クライアントを使用して、自分のデスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーする ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="529e8-154">**Figure 1**: Use an FTP Client to Copy the Necessary Files from Your Desktop to the Web Server at the Web Host Provider ([Click to view full-size image](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))</span></span>


<span data-ttu-id="529e8-155">サイトを展開した後すぐサイトをテストします。</span><span class="sxs-lookup"><span data-stu-id="529e8-155">After deploying your site take a moment to test the site.</span></span> <span data-ttu-id="529e8-156">ドメイン名を購入して、DNS の設定を構成したかどうか、適切にドメイン名を入力して、サイトにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="529e8-156">If you have purchased a domain name and configured the DNS settings properly, you can visit your site by entering your domain name.</span></span> <span data-ttu-id="529e8-157">代わりに、web ホスト プロバイダー必要がありますが付属する、サイトの URL を次のように*accountname*.*webhostprovider*.com または*webhostprovider*.com/*accountname*です。</span><span class="sxs-lookup"><span data-stu-id="529e8-157">Alternatively, your web host provider should have supplied you with a URL to your site, which will look something like *accountname*.*webhostprovider*.com or *webhostprovider*.com/*accountname*.</span></span> <span data-ttu-id="529e8-158">たとえば、ASP.NET の割引のマイ アカウントの URL は:`http://httpruntime.web703.discountasp.net`です。</span><span class="sxs-lookup"><span data-stu-id="529e8-158">For example, the URL for my account on Discount ASP.NET is: `http://httpruntime.web703.discountasp.net`.</span></span>

<span data-ttu-id="529e8-159">図 2 は、配置の書評サイトを示します。</span><span class="sxs-lookup"><span data-stu-id="529e8-159">Figure 2 shows the deployed Book Reviews site.</span></span> <span data-ttu-id="529e8-160">いる表示されて Discount ASP に注意してください。NET のサーバーで`http://httpruntime.web703.discountasp.net`です。</span><span class="sxs-lookup"><span data-stu-id="529e8-160">Note that I am viewing it on Discount ASP.NET's servers, at `http://httpruntime.web703.discountasp.net`.</span></span> <span data-ttu-id="529e8-161">この時点で自分の web サイトを表示、インターネットへの接続を持つすべてのユーザーです。</span><span class="sxs-lookup"><span data-stu-id="529e8-161">At this point in time anyone with a connection to the Internet could view my website!</span></span> <span data-ttu-id="529e8-162">予想、サイトの外観や開発環境でテストする場合と同様に動作します。</span><span class="sxs-lookup"><span data-stu-id="529e8-162">As we'd expect, the site looks and behaves just as it did when testing it in the development environment.</span></span>

> [!NOTE]
> <span data-ttu-id="529e8-163">正しいファイルのセットを展開していることを確認するすぐ、アプリケーションを表示するときにエラーが発生する場合。</span><span class="sxs-lookup"><span data-stu-id="529e8-163">If you get an error when viewing your application take a moment to ensure that you deployed the correct set of files.</span></span> <span data-ttu-id="529e8-164">次に、エラー メッセージがわかるかどうか、問題についての手がかりを確認します。</span><span class="sxs-lookup"><span data-stu-id="529e8-164">Next, check the error message to see if it reveals any clues as to the problem.</span></span> <span data-ttu-id="529e8-165">次に、web ホスト会社のヘルプデスクを有効にしたり、適切なフォーラムに質問を投稿、 [ASP.NET フォーラム](https://forums.asp.net/)です。</span><span class="sxs-lookup"><span data-stu-id="529e8-165">Following that, you can turn to your web host company's helpdesk or post your question to the appropriate forum at the [ASP.NET Forums](https://forums.asp.net/).</span></span>


<span data-ttu-id="529e8-166">[![本のレビュー サイトは、インターネット接続を持つユーザーにアクセスできるようになりました](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="529e8-166">[![The Book Reviews Site is Now Accessible to Anyone with an Internet Connection](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)</span></span>

<span data-ttu-id="529e8-167">**図 2**: Book レビュー サイトは、インターネット接続を持つユーザーにアクセスできるようになりました ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="529e8-167">**Figure 2**: The Book Reviews Site is Now Accessible to Anyone with an Internet Connection ([Click to view full-size image](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))</span></span>


## <a name="deploying-the-book-review-web-site-project"></a><span data-ttu-id="529e8-168">Book レビューの Web サイト プロジェクトを配置します。</span><span class="sxs-lookup"><span data-stu-id="529e8-168">Deploying the Book Review Web Site Project</span></span>

<span data-ttu-id="529e8-169">BookReviewsWSP Web サイト プロジェクトなど、自動コンパイルを使用する ASP.NET アプリケーションを展開するときに、コンパイルされたアセンブリではありません、`Bin`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="529e8-169">When deploying an ASP.NET application that uses automatic compilation, such as the BookReviewsWSP Web Site Project, there is no compiled assembly in the `Bin` folder.</span></span> <span data-ttu-id="529e8-170">その結果、web アプリケーションのソース コード ファイルを実稼働環境に展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-170">As a result, the web application's source code files must be deployed to the production environment.</span></span> <span data-ttu-id="529e8-171">このプロセスについて説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="529e8-171">Let's walk through this process.</span></span>

<span data-ttu-id="529e8-172">ように Web アプリケーション プロジェクトには、最初のビルド アプリケーションを展開する前にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="529e8-172">As with the Web Application Project it's wise to first Build the application before deploying it.</span></span> <span data-ttu-id="529e8-173">Web サイト プロジェクトのビルド、アセンブリが作成されることはできません、間は、コンパイル時のエラー ページの確認はします。</span><span class="sxs-lookup"><span data-stu-id="529e8-173">While building a Web Site Project does not create an assembly, it does check for any compile-time errors in the page.</span></span> <span data-ttu-id="529e8-174">検出して、サイトへの訪問者を持つのではなく、これらのエラーの検索を開始するより強力な!</span><span class="sxs-lookup"><span data-stu-id="529e8-174">Better to find these errors now rather than having a visitor to your site discover them for you!</span></span>

<span data-ttu-id="529e8-175">プロジェクトを正常に作成した後は、web ホスト プロバイダーのルート web サイト フォルダーに、次のファイルをコピーするのに、FTP クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="529e8-175">Once you've successfully built the project, use your FTP client to copy the following files to the root website folder at your web host provider.</span></span> <span data-ttu-id="529e8-176">実稼働環境に対応するフォルダー構造を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-176">You may need to create the corresponding folder structure on the production environment.</span></span>

> [!NOTE]
> <span data-ttu-id="529e8-177">プロジェクトは、まだ BookReviewsWSP プロジェクトの配置を試したい BookReviewsWAP が既に展開されている場合は、まず BookReviewsWAP を展開するときにアップロードされた web サーバー上のファイルをすべて削除し、BookReviewsWSP のファイルを展開します。</span><span class="sxs-lookup"><span data-stu-id="529e8-177">If you already deployed the BookReviewsWAP project but still want to try deploying the BookReviewsWSP project, first delete all of the files on the web server that were uploaded when deploying BookReviewsWAP and then deploy the files for BookReviewsWSP.</span></span>


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- <span data-ttu-id="529e8-178">完全な内容、`Styles`フォルダー</span><span class="sxs-lookup"><span data-stu-id="529e8-178">The complete contents of the `Styles` folder</span></span>
- <span data-ttu-id="529e8-179">完全な内容、`Images`フォルダー (とそのサブフォルダー `BookCovers`)</span><span class="sxs-lookup"><span data-stu-id="529e8-179">The complete contents of the `Images` folder (and its subfolder, `BookCovers`)</span></span>
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

<span data-ttu-id="529e8-180">図 3 は、必要なファイルをコピーした後、FileZilla を示します。</span><span class="sxs-lookup"><span data-stu-id="529e8-180">Figure 3 shows FileZilla after copying up the necessary files.</span></span> <span data-ttu-id="529e8-181">ご覧のように、ASP.NET ソース コード ファイルなど`About.aspx.cs`は、コード ファイルが自動を使用するときに、展開する必要があるため、ローカル コンピューター (開発環境) と web ホスト プロバイダー (運用環境) の両方に存在コンパイルします。</span><span class="sxs-lookup"><span data-stu-id="529e8-181">As you can see, the ASP.NET source code files, such as `About.aspx.cs`, are present on both the local computer (the development environment) and the web host provider (the production environment) because code files need to be deployed when using automatic compilation.</span></span>


<span data-ttu-id="529e8-182">[![FTP クライアントを使用して、デスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーするには](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="529e8-182">[![Use an FTP Client to Copy the Necessary Files from Your Desktop to the Web Server at the Web Host Provider](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)</span></span>

<span data-ttu-id="529e8-183">**図 3**: FTP クライアントを使用して、自分のデスクトップからの Web サーバーで Web ホスト プロバイダーに必要なファイルをコピーする ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="529e8-183">**Figure 3**: Use an FTP Client to Copy the Necessary Files from Your Desktop to the Web Server at the Web Host Provider ([Click to view full-size image](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))</span></span>


<span data-ttu-id="529e8-184">ユーザー エクスペリエンスは、アプリケーションのコンパイル モデルの影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="529e8-184">The user experience is not affected by the application's compilation model.</span></span> <span data-ttu-id="529e8-185">同じ ASP.NET ページにアクセスし、外観や Web アプリケーション プロジェクトのモデルまたは Web サイト プロジェクトのモデルを使用して、web サイトが作成されたかどうか、同じ動作です。</span><span class="sxs-lookup"><span data-stu-id="529e8-185">The same ASP.NET pages are accessible and they look and behave the same whether the website was created using the Web Application Project model or the Web Site Project model.</span></span>

### <a name="updating-a-web-application-on-production"></a><span data-ttu-id="529e8-186">実稼働環境で Web アプリケーションの更新</span><span class="sxs-lookup"><span data-stu-id="529e8-186">Updating a Web Application on Production</span></span>

<span data-ttu-id="529e8-187">Web アプリケーションの開発と展開は、1 回限りのプロセスではありません。</span><span class="sxs-lookup"><span data-stu-id="529e8-187">Web application development and deployment are not a one-time process.</span></span> <span data-ttu-id="529e8-188">たとえば、書籍レビュー web サイトを作成するときにさまざまなページに構築され、パーソナル コンピューター (開発環境) に付属のコードを記述しました。</span><span class="sxs-lookup"><span data-stu-id="529e8-188">For example, when creating the Book Review website I built the various pages and wrote the accompanying code on my personal computer (the development environment).</span></span> <span data-ttu-id="529e8-189">特定の安定した状態に達するとを展開したアプリケーション サイトにアクセスし、レビューを読み他のユーザーができるようにします。</span><span class="sxs-lookup"><span data-stu-id="529e8-189">After reaching a certain stable state, I deployed my application so that others could visit the site and read my reviews.</span></span> <span data-ttu-id="529e8-190">展開は、このサイトで自分の開発の最後をマークしません。</span><span class="sxs-lookup"><span data-stu-id="529e8-190">But deployment does not mark the end of my development on this site.</span></span> <span data-ttu-id="529e8-191">複数の書評を追加または my レート ブックへの訪問者を許可するなどの新機能を実装または独自のコメントを残す場合があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-191">I may add more book reviews or implement new features, such as allowing my visitors to rate books or leave their own comments.</span></span> <span data-ttu-id="529e8-192">このような拡張機能の開発環境で開発し、完了すると、展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-192">Such enhancements would be developed on the development environment and, when completed, would need to be deployed.</span></span> <span data-ttu-id="529e8-193">開発と配置、そのためが循環的です。</span><span class="sxs-lookup"><span data-stu-id="529e8-193">Development and deployment, therefore, are cyclical.</span></span> <span data-ttu-id="529e8-194">アプリケーションを開発して、展開するとします。</span><span class="sxs-lookup"><span data-stu-id="529e8-194">You develop an application and then deploy it.</span></span> <span data-ttu-id="529e8-195">サイトがライブときに、実稼働環境での新機能が追加され、アプリケーションを再デプロイする必要のある時間の経過と共にバグが修正されています。</span><span class="sxs-lookup"><span data-stu-id="529e8-195">While the site is live and in production, new features are added and bugs are fixed over time, which necessitates re-deploying the application.</span></span> <span data-ttu-id="529e8-196">同様に続きます。</span><span class="sxs-lookup"><span data-stu-id="529e8-196">And so on and so on.</span></span>

<span data-ttu-id="529e8-197">期待どおり、のみを追加または変更されたファイルをコピーする必要がある web アプリケーションを再デプロイするときにします。</span><span class="sxs-lookup"><span data-stu-id="529e8-197">As you might expect, when re-deploying a web application you only need to copy new and changed files.</span></span> <span data-ttu-id="529e8-198">サーバーまたはクライアント側 (行っても問題はありませんが) ファイルをサポートしてか変更していないページを再配置する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="529e8-198">There's no need to re-deploy unchanged pages or server- or client-side support files (although there's no harm in doing so).</span></span>

> [!NOTE]
> <span data-ttu-id="529e8-199">または、新しい ASP.NET ページをプロジェクトに追加するコードに関連する変更を加えるたびにアセンブリを更新する、プロジェクトをリビルドする必要がありますを明示的なコンパイルを使用する場合に留意すること、`Bin`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="529e8-199">One thing to keep in mind when using explicit compilation is that anytime you add a new ASP.NET page to the project or make any code-related changes, you need to rebuild your project, which updates the assembly in the `Bin` folder.</span></span> <span data-ttu-id="529e8-200">したがって、(、他の新規および更新内容と共に) の運用上の web アプリケーションを更新するときに、実稼働環境にこの更新されたアセンブリをコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-200">Consequently, you'll need to copy this updated assembly to production when updating a web application on production (along with the other new and updated content).</span></span>


<span data-ttu-id="529e8-201">またに変更されているいずれかを理解、`Web.config`または内のファイル、`Bin`ディレクトリが停止し、web サイトのアプリケーション プールを再起動します。</span><span class="sxs-lookup"><span data-stu-id="529e8-201">Also understand that any changes to the `Web.config` or the files in the `Bin` directory stops and restarts the website's Application Pool.</span></span> <span data-ttu-id="529e8-202">使用して、セッション状態に格納されている場合、`InProc`モード (既定)、サイトの訪問者は、これらのキー ファイルが変更されるたびに、セッション状態に失われます。</span><span class="sxs-lookup"><span data-stu-id="529e8-202">If your session state is stored using the `InProc` mode (the default) then your site's visitors will lose their session state whenever these key files are modified.</span></span> <span data-ttu-id="529e8-203">この危険を回避することを検討してセッションを使用して格納する、`StateServer`または`SQLServer`モード。</span><span class="sxs-lookup"><span data-stu-id="529e8-203">To avoid this pitfall, consider storing session using the `StateServer` or `SQLServer` modes.</span></span> <span data-ttu-id="529e8-204">このトピックの詳細については読み取る[セッション状態モード](https://msdn.microsoft.com/en-us/library/ms178586.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="529e8-204">For more information on this topic read [Session-State Modes](https://msdn.microsoft.com/en-us/library/ms178586.aspx).</span></span>

<span data-ttu-id="529e8-205">最後に、アプリケーションの再配置する実行できる任意の場所は数秒から実稼働環境にコピーする必要のあるファイルのサイズと数によっては、数分に注意してください。</span><span class="sxs-lookup"><span data-stu-id="529e8-205">Finally, keep in mind that re-deploying an application can take anywhere from a few seconds to several minutes, depending on the number and size of files that need to be copied to the production environment.</span></span> <span data-ttu-id="529e8-206">この期間中にサイトを訪問ユーザー可能性がありますが発生するエラーや不適切な動作をします。</span><span class="sxs-lookup"><span data-stu-id="529e8-206">During this time users visiting your site may experience errors or odd behavior.</span></span> <span data-ttu-id="529e8-207">「無効にできます」アプリケーション全体という名前のページを追加することによって`App_Offline.htm`をユーザーに説明する、アプリケーションのルート ディレクトリに、サイトのメンテナンス (など) がダウンしてがなることをバックアップ直後。</span><span class="sxs-lookup"><span data-stu-id="529e8-207">You can "turn off" your entire application by adding a page named `App_Offline.htm` to your application's root directory that explains to your users that the site is down for maintenance (or whatever) and will be back up shortly.</span></span> <span data-ttu-id="529e8-208">ときに、`App_Offline.htm`ファイルが存在、ASP.NET ランタイムがそのページにすべての着信要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="529e8-208">When the `App_Offline.htm` file is present, the ASP.NET runtime redirects all incoming requests to that page.</span></span>

## <a name="summary"></a><span data-ttu-id="529e8-209">概要</span><span class="sxs-lookup"><span data-stu-id="529e8-209">Summary</span></span>

<span data-ttu-id="529e8-210">Web アプリケーションを配置するには、開発環境から運用環境に必要なファイルをコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="529e8-210">Deploying a web application entails copying the necessary files from the development environment to the production environment.</span></span> <span data-ttu-id="529e8-211">ファイルがネットワーク経由で転送する最も一般的な方法は、ファイル転送プロトコル (FTP)、およびほとんどの web ホスト プロバイダーは、web サーバーに対する FTP アクセスをサポートします。</span><span class="sxs-lookup"><span data-stu-id="529e8-211">The most common means by which files are transferred over a network is the File Transfer Protocol (FTP), and most web host providers support FTP access to their web servers.</span></span> <span data-ttu-id="529e8-212">このチュートリアルでは、FTP クライアントを使用して、web サーバーに必要なファイルを配置する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="529e8-212">In this tutorial we saw how to use an FTP client to deploy the needed files to the web server.</span></span> <span data-ttu-id="529e8-213">展開した後、web サイトが参照できますすべてのユーザー接続でインターネットに!</span><span class="sxs-lookup"><span data-stu-id="529e8-213">Once deployed, the website can be visited by anyone with a connection to the Internet!</span></span>

<span data-ttu-id="529e8-214">満足プログラミング!</span><span class="sxs-lookup"><span data-stu-id="529e8-214">Happy Programming!</span></span>

### <a name="further-reading"></a><span data-ttu-id="529e8-215">関連項目</span><span class="sxs-lookup"><span data-stu-id="529e8-215">Further Reading</span></span>

<span data-ttu-id="529e8-216">このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。</span><span class="sxs-lookup"><span data-stu-id="529e8-216">For more information on the topics discussed in this tutorial, refer to the following resources:</span></span>

- [<span data-ttu-id="529e8-217">アプリ\_Offline.htm と「IE フレンドリ エラー」機能を回避します。</span><span class="sxs-lookup"><span data-stu-id="529e8-217">App\_Offline.htm and Working Around the "IE Friendly Errors" Feature</span></span>](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [<span data-ttu-id="529e8-218">セッション状態モード</span><span class="sxs-lookup"><span data-stu-id="529e8-218">Session-State Modes</span></span>](https://msdn.microsoft.com/en-us/library/ms178586.aspx)

>[!div class="step-by-step"]
<span data-ttu-id="529e8-219">[前へ](determining-what-files-need-to-be-deployed-cs.md)
[次へ](deploying-your-site-using-visual-studio-cs.md)</span><span class="sxs-lookup"><span data-stu-id="529e8-219">[Previous](determining-what-files-need-to-be-deployed-cs.md)
[Next](deploying-your-site-using-visual-studio-cs.md)</span></span>
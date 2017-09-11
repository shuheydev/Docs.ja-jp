---
title: "Razor ビューのコンパイルとプリコンパイル"
author: rick-anderson
description: "MVC Razor ビューのコンパイル、および ASP.NET Core アプリケーションでのプリコンパイルを有効にする方法を説明するリファレンス ドキュメント。"
keywords: "ASP.NET Core、Razor ビューのコンパイル、Razor 事前コンパイル、Razor プリコンパイル"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 1395717341bfcf5441b78633ca3957630ae5d899
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Razor ビューのコンパイル、および ASP.NET Core でのプリコンパイル

によって[Rick Anderson](https://twitter.com/RickAndMSFT)

Razor ビューは、ビューが呼び出されたときに、実行時にコンパイルされます。 ASP.NET 1.1.0 のコアし以降が必要に応じて Razor ビューをコンパイルし、アプリの配置&mdash;プリコンパイルと呼ばれるプロセスです。 ASP.NET Core 2.x プロジェクト テンプレートは、既定では、プリコンパイルを有効にします。

> [!NOTE]
> Razor ビュー プリコンパイル実施する際に使用できません、 [Self-Contained 展開](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd)ASP.NET Core バージョン 2.0.0 で以前のバージョン。

プリコンパイルの考慮事項:

* ビューをプリコンパイルするは、パブリッシュされたバンドル小さいと起動時間が高速になります。
* ビューをプリコンパイルした後、Razor ファイルを編集することはできません。 編集済みのビューは、パブリッシュされたバンドル内に存在することはできません。 

プリコンパイル済みのビューを展開します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

参照をパッケージを含める場合は、プロジェクトは、.NET Framework を対象、 `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

プロジェクトは .NET Core をターゲットの変更の必要はありません。

ASP.NET Core 2.x プロジェクト テンプレートが暗黙的に設定`MvcRazorCompileOnPublish`に`true`既定では、つまりこのノードはから安全に削除できる、 *.csproj*ファイル。 明示的に指定する場合は、設定で害はありません、`MvcRazorCompileOnPublish`プロパティを`true`です。 次*.csproj*サンプルには、この設定が強調表示します。

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

設定`MvcRazorCompileOnPublish`に`true`、パッケージの参照を含めて、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`です。 次*.csproj*サンプルにはこれらの設定が強調表示されます。

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
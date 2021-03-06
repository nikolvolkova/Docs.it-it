---
title: Aggiunta di un modello a un'app di pagine Razor con Visual Studio per Mac
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac
keywords: ASP.NET Core, pagine Razor, Razor, MVC, modello
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 648ecd3a782fa489b727982ce5f7a2087539bf38
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a>Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

* In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.
* Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.
* Nella finestra di dialogo **Nuovo file**:

  * Selezionare **Generale** nel riquadro a sinistra.
  * Selezionare **Classe vuota** nel riquadro centrale.
  * Denominare la classe **Filmato** e selezionare **Nuovo**.

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Fare clic con il pulsante destro del mouse su una riga rossa ondulata, ad esempio `MovieContext` nella riga `services.AddDbContext<MovieContext>(options =>`. Selezionare **Quick Fix > using RazorPagesMovie.Models;** (Correzione rapida > tramite RazorPagesMovie.Models;). Visual studio aggiunge l'istruzione using.

Compilare il progetto per verificare che non ci siano errori.

![Pagina Crea](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacchetti NuGet di Entity Framework Core per le migrazioni

Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*. **Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.

Per modificare un file *.csproj*:

* Selezionare **File** > **Apri** e selezionare il file *csproj*.
* Selezionare **Opzioni**.
* Modificare **Apri con** in **Editor codice sorgente**.

![Modificare il file csproj](model/csproj.png)

Aggiungere il riferimento allo strumento `Microsoft.EntityFrameworkCore.Tools.DotNet` al secondo **\<ItemGroup >**:

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a>Aggiungere i file di pagine/filmati al progetto

* In Visual Studio, fare clic con il pulsante destro del mouse sulla cartella *Pagine* e selezionare **Aggiungi > Aggiungi cartella esistente**.
* Selezionare la cartella *Filmati*.
* Nella finestra di dialogo *Scegli i file da includere nel progetto*, selezionare **Includi tutto**.

L'esercitazione successiva illustra i file creati tramite scaffolding.

>[!div class="step-by-step"]
[Indietro: Introduzione](xref:tutorials/razor-pages-mac/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)

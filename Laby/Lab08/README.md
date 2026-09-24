# Lab8 - Lokalizace

**Zadání:**

- Upravte aplikaci převodník aby podporovala lokalizaci pomocí .NET Resources
- Otestujte slovenské a české rozhraní

## Postup:

1. Otevřte projekt Prevodnik.sln ve složce Laby\Lab08\Start.
1. Do projektu přidejte nový soubor Resources File (.resx), který nazvete **TextyOkna.resx**.
1. Nastavte jeho Access Modifier na Public, aby bylo možné použít vygenerovanou třídu TextyOkna přímo z XAMLu.
1. Do resource souboru přidejte následující texty:

   - NadpisOkna - Převodník
   - CnaF - Celsius na Fahrenhait
   - FnaC - Fahrenhait na Celsius
   - MnaS - Metry na stopy
   - SnaM - Stopy na metry

1. Vytvořte kopii souboru TextyOkna.resx a nazvěte ji **TextyOkna.sk-sk.resx**.
1. Ve slovenském souboru upravte hodnotu NadpisOkna na Prevodník, MnaS na Metre na stopy a SnaM na Stopy na metre. Názvy jednotlivých položek musí zůstat stejné.

### Použití Resources v XAMLu

1. Otevřte soubor MainWindow.xaml.
1. Element Window už obsahuje namespace local odkazující na namespace Prevodnik. Vlastnost Title nastavte na statickou vlastnost NadpisOkna vygenerované třídy TextyOkna.

    ```xml
    Title="{x:Static local:TextyOkna.NadpisOkna}"
    ```

1. U všech čtyř elementů ListBoxItem odstraňte vložený text a vlastnost Content nastavte na odpovídající statickou vlastnost třídy TextyOkna.

    ```xml
    <ListBoxItem IsSelected="True" Selected="AkceCnaF"
                 Content="{x:Static local:TextyOkna.CnaF}" />
    <ListBoxItem Selected="AkceFnaC"
                 Content="{x:Static local:TextyOkna.FnaC}" />
    <ListBoxItem Selected="AkceMnaS"
                 Content="{x:Static local:TextyOkna.MnaS}" />
    <ListBoxItem Selected="AkceSnaM"
                 Content="{x:Static local:TextyOkna.SnaM}" />
    ```

### Přepnutí jazyka aplikace

1. Otevřte soubor App.xaml.cs a přidejte usingy na namespace System.Globalization a System.Threading.
1. Do třídy App přidejte konstruktor a v něm vytvořte instanci CultureInfo pro slovenskou kulturu.
1. Nastavte CurrentCulture i CurrentUICulture aktuálního vlákna.

    ```csharp
    public App()
    {
        CultureInfo localization = new CultureInfo("sk-sk");
        Thread.CurrentThread.CurrentCulture = localization;
        Thread.CurrentThread.CurrentUICulture = localization;
    }
    ```

1. Spusťte aplikaci a ověřte, že se použije slovenský nadpis a texty převodů.
1. Zakomentujte tři řádky v konstruktoru, které kulturu vytvářejí a nastavují, a spusťte aplikaci znovu. Při českém nastavení systému se použijí texty z neutrálního souboru TextyOkna.resx.

Názvy souborů a hodnoty kultury jsou zapsány stejně jako v dodaném řešení. Soubor používá označení sk-sk malými písmeny; CultureInfo tuto variantu přijímá.

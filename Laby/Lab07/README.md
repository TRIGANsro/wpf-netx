# Lab7 - WPF vlákna

**Zadání:**

- Vytvořte aplikaci, která bude v pomocné třídě provádět dlouhý výpočet
- Zajistěte pomocí Task a async/await plnou funkčnost okna i v době provádění výpočtu
- Informujte v ProgressBaru o průběhu výpočtu
- Implementujte možnost zrušení požadavku na výpočet

## Postup:

Ve složce Lab07 není startovací projekt, vytvořte proto nový projekt typu WPF App pro .NET 8 s názvem **Vlakna** a solution nazvěte **VlaknaSL**.

### Vytvoření pomocné třídy

1. Přidejte do projektu class, který nazvete **Pracant**.
1. Do souboru přidejte using pro namespace System.Threading.
1. Do třídy přidejte metodu DlouhaAkce, která bude přijímat číselná data, IProgress\<int> pro hlášení průběhu a CancellationToken pro možnost zrušení.

    ```csharp
    public string DlouhaAkce(int data, IProgress<int> progress, CancellationToken token)
    {
        for (int i = 1; i < 11; i++)
        {
            token.ThrowIfCancellationRequested();
            Thread.Sleep(1000);
            progress?.Report(i * 10);
        }

        return "HOTOVO " + data;
    }
    ```

   Thread.Sleep zajistí, že akce bude trvat alespoň 10 vteřin. CancellationToken je potřeba v průběhu akce pravidelně kontrolovat.

### Vytvoření UI

1. Otevřte soubor MainWindow.xaml.
1. Do Gridu přidejte Label s názvem **InfoLabel**, Button pro spuštění výpočtu, Button pro jeho zrušení a ProgressBar s názvem **HlavniProgress**.
1. Prvnímu Buttonu nastavte Click na metodu **Akce** a druhému na metodu **Zrusit**.
1. ProgressBaru nastavte Minimum na 0 a Maximum na 100.

    ```xml
    <StackPanel Margin="30">
        <Label x:Name="InfoLabel" Content="Připraveno" />
        <ProgressBar x:Name="HlavniProgress" Height="26" Minimum="0" Maximum="100" />
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Center">
            <Button Margin="5" Padding="20,5" Click="Akce">Spustit</Button>
            <Button Margin="5" Padding="20,5" Click="Zrusit">Zrušit</Button>
        </StackPanel>
    </StackPanel>
    ```

### Spuštění a zrušení výpočtu

1. Otevřte soubor MainWindow.xaml.cs a do třídy MainWindow přidejte členský prvek typu CancellationTokenSource.

    ```csharp
    private CancellationTokenSource cancellation;
    ```

1. Metodu Akce označte jako async a vytvořte v ní instanci třídy Pracant, Progress\<int> a CancellationTokenSource.
1. Dlouhou akci spusťte pomocí Task.Run. Díky await zůstane UI vlákno po dobu výpočtu volné.

    ```csharp
    private async void Akce(object sender, RoutedEventArgs e)
    {
        InfoLabel.Content = "Pracuji";
        HlavniProgress.Value = 0;
        cancellation = new CancellationTokenSource();
        var pracant = new Pracant();
        Progress<int> progress = new Progress<int>(x => HlavniProgress.Value = x);

        try
        {
            string vysledek = await Task.Run(() =>
                pracant.DlouhaAkce(100, progress, cancellation.Token));
            InfoLabel.Content = vysledek;
        }
        catch (OperationCanceledException)
        {
            InfoLabel.Content = "ZRUŠENO";
        }
    }
    ```

1. Do metody Zrusit přidejte požadavek na zrušení právě prováděné akce.

    ```csharp
    private void Zrusit(object sender, RoutedEventArgs e)
    {
        cancellation?.Cancel();
    }
    ```

1. Spusťte aplikaci a ověřte, že lze během výpočtu přesouvat a měnit velikost okna, ProgressBar postupně zobrazuje průběh a druhé tlačítko výpočet zruší.

Řešení v repozitáři obsahuje Task a Progress, ale neobsahuje CancellationToken a používá Thread.Sleep(500), takže výpočet trvá přibližně 5 vteřin. Tyto dvě části řešení neodpovídají úplnému zadání na slidu.

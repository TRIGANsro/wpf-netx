# Lab13 - Nastavení aplikace

**Zadání:**

- Přidejte do aplikace Prevodnik uživatelské nastavení barev a hranic
- Vytvořte okno pro změnu, uložení, zrušení a obnovení nastavení
- Po zavření dialogu aktualizujte GopasLabel

## Postup:

1. Otevřte projekt Prevodnik.sln ve složce Laby\Lab13\Start.
1. Otevřte projekt Prevodnik a přidejte do něj nový Settings File s názvem **Settings.settings** do složky Properties.
1. Přidejte dvě nastavení s User scope:

   - Barvicky typu string s výchozí hodnotou green;yellow;red
   - Hranice typu string s výchozí hodnotou 100;500

1. Ověřte, že generátor vytvořil třídu Prevodnik.Properties.Settings a statickou vlastnost Default.

### Vytvoření okna Nastaveni

1. Přidejte do projektu nové WPF Window s názvem **Nastaveni.xaml**.
1. Do elementu Window přidejte namespace prop odkazující na Prevodnik.Properties.

    ```xml
    xmlns:prop="clr-namespace:Prevodnik.Properties"
    ```

1. Do okna přidejte TextBox pro každé nastavení. Text bindujte TwoWay přímo na Settings.Default.

    ```xml
    <Label>Barvy:</Label>
    <TextBox Text="{Binding Barvicky, Source={x:Static prop:Settings.Default}, Mode=TwoWay}" />
    <Label>Hranice</Label>
    <TextBox Text="{Binding Hranice, Source={x:Static prop:Settings.Default}, Mode=TwoWay}" />
    ```

1. Přidejte tři Buttony Ulož, Zruš a Výchozí nastavení. Událost Click nastavte na metody Save, Cancel a Reset. Tlačítku Ulož nastavte IsDefault na True a tlačítku Zruš IsCancel na True.
1. V souboru Nastaveni.xaml.cs implementujte jednotlivé metody.

    ```csharp
    private void Save(object sender, RoutedEventArgs e)
    {
        Properties.Settings.Default.Save();
        this.Close();
    }

    private void Cancel(object sender, RoutedEventArgs e)
    {
        Properties.Settings.Default.Reload();
        this.Close();
    }

    private void Reset(object sender, RoutedEventArgs e)
    {
        Properties.Settings.Default.Reset();
        this.Close();
    }
    ```

### Úprava hlavního okna

1. Otevřte MainWindow.xaml a původní Grid vložte do DockPanelu s vlastností LastChildFill nastavenou na True.
1. Před Grid přidejte Menu umístěné u horního okraje DockPanelu.
1. Menu bude obsahovat položku Hlavni s příkazem Konec a položku Nastavení s příkazem Barvy terče.

    ```xml
    <Menu DockPanel.Dock="Top">
        <MenuItem Header="Hlavni">
            <MenuItem Header="Konec" Click="Konec" />
        </MenuItem>
        <MenuItem Header="Nastavení">
            <MenuItem Header="Barvy terče" Click="Nastaveni" />
        </MenuItem>
    </Menu>
    ```

1. Prvku GopasLabel nastavte x:Name na **SuperLabel**.
1. V souboru MainWindow.xaml.cs přidejte metodu NastavLabel. Ta načte obě hodnoty z Settings.Default do vlastností controlu.

    ```csharp
    private void NastavLabel()
    {
        SuperLabel.Barvy = Properties.Settings.Default.Barvicky;
        SuperLabel.Hranice = Properties.Settings.Default.Hranice;
    }
    ```

1. Metodu NastavLabel zavolejte v konstruktoru hned za InitializeComponent.
1. Přidejte metodu Konec, která zavře hlavní okno.
1. Přidejte metodu Nastaveni, která vytvoří okno Nastaveni, zobrazí ho modálně pomocí ShowDialog a po zavření znovu zavolá NastavLabel.

    ```csharp
    private void Nastaveni(object sender, RoutedEventArgs e)
    {
        Nastaveni nastaveni = new();
        nastaveni.ShowDialog();
        NastavLabel();
    }
    ```

1. Spusťte aplikaci, změňte barvy a hranice a použijte tlačítko Ulož. Po novém spuštění aplikace musí zůstat hodnoty zachovány.
1. Ověřte, že Zruš znovu načte uložené hodnoty a Výchozí nastavení obnoví green;yellow;red a 100;500.

Dodaný GopasLabel neošetřuje neplatné hodnoty. Při ručním zadání neplatné barvy, nečíselné hranice nebo nesprávného počtu hodnot může aplikace skončit výjimkou.

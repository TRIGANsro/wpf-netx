# Lab10 - Custom Control

**Zadání:**

- Vytvořte vlastní control pro zobrazení číselného výsledku převodu
- Vedle hodnoty zobrazte barevný kruhový terč
- Barvu terče určete podle nastavených hranic a barev
- Control použijte v aplikaci Prevodnik

## Postup:

1. Otevřte projekt Prevodnik.sln ve složce Laby\Lab10\Start.
1. Do solution přidejte nový projekt typu WPF Custom Control Library a nazvěte ho **PrevodControls**.
1. Do projektu Prevodnik přidejte Project Reference na projekt PrevodControls.
1. Výchozí třídu vlastního controlu přejmenujte na **GopasLabel** a zajistěte, aby byla potomkem třídy Control.

### Implementace GopasLabel

1. Třídě přidejte attribut TemplatePart. Pojmenovaná část šablony se bude jmenovat PART_Kolecko a bude typu Ellipse.

    ```csharp
    [TemplatePart(Name = "PART_Kolecko", Type = typeof(Ellipse))]
    public class GopasLabel : Control
    ```

1. Ve statickém konstruktoru nastavte DefaultStyleKeyProperty pro typ GopasLabel.

    ```csharp
    static GopasLabel()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(GopasLabel),
            new FrameworkPropertyMetadata(typeof(GopasLabel)));
    }
    ```

1. Přidejte Dependency Property **Hodnota** typu double. Její výchozí hodnota bude 0.0 a callback se bude jmenovat ZmenaHodnoty.
1. V callbacku otestujte typ vstupního objektu a zavolejte metodu NastavBarvu.

    ```csharp
    public static readonly DependencyProperty HodnotaProperty =
        DependencyProperty.Register("Hodnota", typeof(double), typeof(GopasLabel),
            new PropertyMetadata(0.0, ZmenaHodnoty));

    private static void ZmenaHodnoty(DependencyObject d,
        DependencyPropertyChangedEventArgs e)
    {
        if (d is not GopasLabel cil)
            return;
        cil.NastavBarvu();
    }
    ```

1. Stejným způsobem přidejte Dependency Property **Barvy** a **Hranice**, obě typu string. Výchozí hodnoty budou `green;yellow;red` a `100;500`.
1. V callbacku ZmenaBarvy rozdělte nový text podle středníku a jednotlivé barvy převeďte pomocí ColorConverter.ConvertFromString na pole SolidColorBrush.
1. V callbacku ZmenaHranice rozdělte text podle středníku a hodnoty převeďte pomocí double.Parse na pole double.
1. Přidejte pole s výchozími hodnotami a členský prvek pro pojmenovanou část šablony.

    ```csharp
    private Brush[] sadaBarev = { Brushes.Green, Brushes.Yellow, Brushes.Red };
    private double[] sadaHranic = { 100.0, 500.0 };
    private Ellipse Kolecko;
    ```

1. V metodě NastavBarvu projděte hranice a vyberte odpovídající štětec. Pokud už je Kolecko vytvořeno, nastavte jeho Fill.
1. Přepište metodu OnApplyTemplate, pomocí GetTemplateChild získejte PART_Kolecko a zavolejte NastavBarvu.

    ```csharp
    public override void OnApplyTemplate()
    {
        base.OnApplyTemplate();
        Kolecko = GetTemplateChild("PART_Kolecko") as Ellipse;
        NastavBarvu();
    }
    ```

### Definování vzhledu

1. Otevřte soubor Themes\Generic.xaml v projektu PrevodControls.
1. Vytvořte Style pro GopasLabel a v Setteru vlastnosti Template vytvořte ControlTemplate.
1. Do šablony vložte vodorovný StackPanel, Ellipse s názvem PART_Kolecko a Label.
1. Content Labelu bindujte přes RelativeSource TemplatedParent na vlastnost Hodnota.

    ```xml
    <StackPanel Orientation="Horizontal">
        <Ellipse x:Name="PART_Kolecko" Width="20" Height="20" />
        <Label MinWidth="100"
               Content="{Binding RelativeSource={RelativeSource TemplatedParent}, Path=Hodnota}" />
    </StackPanel>
    ```

### Použití controlu v aplikaci

1. Otevřte MainWindow.xaml v projektu Prevodnik a přidejte namespace pro projekt PrevodControls.

    ```xml
    xmlns:pc="clr-namespace:PrevodControls;assembly=PrevodControls"
    ```

1. Původní Label zobrazující výsledek nahraďte prvkem GopasLabel. Vlastnost Hodnota bindujte na VystupniHodnota.

    ```xml
    <pc:GopasLabel Hodnota="{Binding VystupniHodnota}" Grid.Column="2"
                   HorizontalAlignment="Center" VerticalAlignment="Center" />
    ```

1. Otestujte výchozí hranice 100 a 500.
1. Nastavte prvku vlastní hodnoty například `Barvy="blue;green;yellow;red;black"` a `Hranice="0;100;500;2000"` a ověřte změnu barev.

Control v dodaném řešení nekontroluje počet a správnost hodnot v Barvy a Hranice. Pokud zadáte neplatnou barvu, nečíselnou hranici nebo málo barev, aplikace může skončit výjimkou.

# Lab12 - WPF animace

**Zadání:**

- Vytvořte vlastní styl tlačítka podle výchozí šablony WPF
- Při najetí myší tlačítko plynule zvětšete
- Při opuštění tlačítka vraťte jeho původní velikost

## Postup:

Ve složce Lab12 není startovací projekt. Vytvořte nový projekt typu WPF App pro .NET 8 s názvem **AnimaceTlacitka** a solution nazvěte **AnimaceTlacitkaSol**.

### Vytvoření stylu tlačítka

1. Otevřte MainWindow.xaml a do Window.Resources vložte kopii výchozího stylu Buttonu.
1. Styl nazvěte **AnimoButtonStyle** a ponechte v něm výchozí štětce, FocusVisual a ControlTemplate.
1. Do Gridu přidejte Button, nastavte mu šířku 127, výšku 57 a aplikujte vytvořený styl.

    ```xml
    <Button Style="{DynamicResource AnimoButtonStyle}"
            Content="Button" HorizontalAlignment="Center"
            Height="57" VerticalAlignment="Center" Width="127" />
    ```

### Animace rozměrů

1. V ControlTemplate najděte Trigger pro vlastnost IsMouseOver s hodnotou true.
1. Do Trigger.EnterActions přidejte BeginStoryboard se Storyboardem.
1. Do Storyboardu přidejte dvě DoubleAnimation. Obě budou trvat jednu vteřinu a zvětší Width a Height o 20.

    ```xml
    <Trigger.EnterActions>
        <BeginStoryboard>
            <Storyboard>
                <DoubleAnimation Duration="0:0:1" By="20"
                                 Storyboard.TargetProperty="Width" />
                <DoubleAnimation Duration="0:0:1" By="20"
                                 Storyboard.TargetProperty="Height" />
            </Storyboard>
        </BeginStoryboard>
    </Trigger.EnterActions>
    ```

1. Do Trigger.ExitActions přidejte stejné animace s hodnotou By -20.
1. Spusťte aplikaci a ověřte zvětšení tlačítka při najetí myší a návrat po odjetí.

### Animace pomocí transformace

1. V elementu Border uvnitř ControlTemplate nastavte RenderTransformOrigin na 0.5,0.5.
1. Do Border.RenderTransform přidejte TransformGroup. Prvním prvkem v kolekci bude ScaleTransform, za ním SkewTransform, RotateTransform a TranslateTransform.

    ```xml
    <Border.RenderTransform>
        <TransformGroup>
            <ScaleTransform />
            <SkewTransform />
            <RotateTransform />
            <TranslateTransform />
        </TransformGroup>
    </Border.RenderTransform>
    ```

1. Původní animace Width a Height nahraďte animacemi ScaleX a ScaleY prvního prvku TransformGroup.

    ```xml
    <DoubleAnimation Duration="0:0:1" From="1" To="1.5"
        Storyboard.TargetName="border"
        Storyboard.TargetProperty="(UIElement.RenderTransform).(TransformGroup.Children)[0].(ScaleTransform.ScaleX)" />
    <DoubleAnimation Duration="0:0:1" From="1" To="1.5"
        Storyboard.TargetName="border"
        Storyboard.TargetProperty="(UIElement.RenderTransform).(TransformGroup.Children)[0].(ScaleTransform.ScaleY)" />
    ```

1. V ExitActions nastavte obrácenou animaci From 1.5 To 1 pro ScaleX i ScaleY.
1. Spusťte aplikaci a porovnejte obě varianty. Animace transformace nemění rozložení okolních elementů, protože nemění skutečné Width a Height.

Složka Reseni1 obsahuje animaci Width a Height. Složka Reseni2 obsahuje výslednou variantu s RenderTransform a ScaleTransform.

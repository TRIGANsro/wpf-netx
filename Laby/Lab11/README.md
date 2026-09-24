# Lab11 - WPF Behaviors

**Zadání:**

- Vytvořte Attached Property, která umožní vybrané UI elementy přesouvat myší
- Stejnou funkčnost vytvořte pomocí WPF Behavior
- Behavior použijte v XAMLu bez obsluhy událostí v code-behind

## Postup:

Pro Lab11 není v repozitáři startovací projekt ani řešení. Následující postup proto vychází pouze z tématu a API uvedeného v prezentaci wpfnet-11-CZ.pdf. Jako základ použijte nový projekt typu WPF App pro .NET 8; název projektu zvolte libovolný a namespace v ukázkách přizpůsobte projektu.

### Attached Property

1. Do projektu přidejte class **DragBehavior**.
1. Přidejte Attached Property **IsEnabled** typu bool a vytvořte metody GetIsEnabled a SetIsEnabled.
1. Při změně hodnoty připojte nebo odpojte události MouseLeftButtonDown, MouseMove a MouseLeftButtonUp elementu UIElement.
1. Při stisknutí tlačítka myši uložte počáteční pozici, zavolejte CaptureMouse a při pohybu měňte Canvas.Left a Canvas.Top.
1. Při uvolnění tlačítka zavolejte ReleaseMouseCapture.
1. Otevřte MainWindow.xaml, do Gridu vložte Canvas a do něj alespoň dva prvky.
1. Elementu, který chcete přesouvat, nastavte vytvořenou Attached Property na True.

    ```xml
    <Canvas>
        <Button Canvas.Left="40" Canvas.Top="40"
                local:DragBehavior.IsEnabled="True">Přesuň mě</Button>
    </Canvas>
    ```

1. Spusťte aplikaci a ověřte, že lze tlačítko přesouvat a element bez nastavené property zůstává na místě.

### WPF Behavior

1. Pomocí Manage NuGet Packages přidejte do projektu balíček **Microsoft.Xaml.Behaviors.Wpf**. Verzi balíčku prezentace ani projekt neurčují, proto použijte verzi kompatibilní s cílovým frameworkem projektu.
1. Přidejte class **DragAction**, která bude potomkem třídy TriggerAction\<DependencyObject> z namespace Microsoft.Xaml.Behaviors.
1. Přesuňte logiku pro práci s myší z Attached Property do této třídy. Události připojte po připojení behavioru k AssociatedObject a při odpojení je zase odpojte.
1. Do MainWindow.xaml přidejte namespace Behaviors.

    ```xml
    xmlns:i="http://schemas.microsoft.com/xaml/behaviors"
    ```

1. K přesouvanému prvku přidejte Interaction.Triggers a EventTrigger pro událost Loaded. Do triggeru vložte vytvořenou DragAction.

    ```xml
    <Button Canvas.Left="40" Canvas.Top="40">Přesuň mě
        <i:Interaction.Triggers>
            <i:EventTrigger EventName="Loaded">
                <local:DragAction />
            </i:EventTrigger>
        </i:Interaction.Triggers>
    </Button>
    ```

1. Spusťte aplikaci a ověřte stejné chování jako u Attached Property.

Tento lab nelze proti dodanému zdrojovému kódu ověřit, protože složka Laby\Lab11 v repozitáři chybí. Před publikací je potřeba doplnit autorův Start a Reseni nebo potvrdit konkrétní podobu požadovaného behavioru; uvedené zadání je technicky možná varianta odpovídající tématu prezentace, ne rekonstrukce chybějícího řešení.

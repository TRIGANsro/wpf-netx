# Lab9 - 2D grafika

**Zadání:**

- Vytvořte pomocí WPF 2D grafiky obrázek obličeje
- Obrázek zobrazte v tlačítku společně s textem
- Stejný obrázek vytvořte také pomocí elementů Shape na Canvasu

## Postup:

Ve složce Lab09 není startovací projekt, vytvořte proto nový projekt typu WPF App pro .NET 8 s názvem **Obrazek**.

### Vytvoření DrawingGroup

1. Otevřte soubor MainWindow.xaml a do Window.Resources přidejte DrawingGroup s klíčem obrazek.
1. Základ obličeje vytvořte pomocí GeometryDrawing, žlutého štětce, černého pera a EllipseGeometry.

    ```xml
    <DrawingGroup x:Key="obrazek">
        <GeometryDrawing Brush="Yellow">
            <GeometryDrawing.Pen>
                <Pen Brush="Black" Thickness="6" />
            </GeometryDrawing.Pen>
            <GeometryDrawing.Geometry>
                <EllipseGeometry RadiusX="50" RadiusY="50" Center="50,50" />
            </GeometryDrawing.Geometry>
        </GeometryDrawing>
    </DrawingGroup>
    ```

1. Do DrawingGroup přidejte další GeometryDrawing s černým štětcem. Jeho Geometry bude GeometryGroup se dvěma EllipseGeometry pro oči. Jejich středy budou 35,34 a 65,34, RadiusX bude 5 a RadiusY 10.
1. Ústa vytvořte pomocí GeometryDrawing bez výplně. Nastavte jeho Geometry na `M 20,60 Q 50,100 80,60` a Pen na černou barvu, tloušťku 4 a kulaté konce.

    ```xml
    <GeometryDrawing Geometry="M 20,60 Q 50,100 80,60">
        <GeometryDrawing.Pen>
            <Pen Brush="Black" Thickness="4"
                 EndLineCap="Round" StartLineCap="Round" />
        </GeometryDrawing.Pen>
    </GeometryDrawing>
    ```

### Použití obrázku v tlačítku

1. Do Gridu přidejte Button o velikosti 150 x 100.
1. Do Buttonu vložte StackPanel, Image a TextBlock s textem Pokus.
1. Jako Image.Source použijte DrawingImage odkazující na resource obrazek.

    ```xml
    <Button Width="150" Height="100">
        <StackPanel>
            <Image Width="50" Height="50">
                <Image.Source>
                    <DrawingImage Drawing="{StaticResource obrazek}" />
                </Image.Source>
            </Image>
            <TextBlock HorizontalAlignment="Center">Pokus</TextBlock>
        </StackPanel>
    </Button>
    ```

1. Spusťte aplikaci a ověřte zobrazení obrázku i textu v tlačítku.

### Vytvoření obrázku pomocí Shapes

1. Původní Button dočasně zakomentujte.
1. Do Gridu vložte Viewbox a do něj Canvas o velikosti 100 x 100.
1. Do Canvasu přidejte žlutou Ellipse pro obličej, dvě černé Ellipse pro oči a Path pro ústa.

    ```xml
    <Viewbox>
        <Canvas Height="100" Width="100">
            <Ellipse Width="100" Height="100" Fill="Yellow"
                     StrokeThickness="5" Stroke="Black" />
            <Ellipse x:Name="OcickoP" Canvas.Top="24" Canvas.Left="30"
                     Width="10" Height="20" Fill="Black"
                     MouseLeftButtonUp="AkceP" />
            <Ellipse x:Name="OcickoL" Canvas.Top="24" Canvas.Right="30"
                     Width="10" Height="20" Fill="Black" />
            <Path Stroke="Black" StrokeThickness="4"
                  Data="M 20,60 Q 50,100 80,60" />
        </Canvas>
    </Viewbox>
    ```

1. V souboru MainWindow.xaml.cs přidejte členský prvek typu bool a metodu AkceP. Metoda bude po kliknutí střídat černou a červenou výplň pravého oka.

    ```csharp
    private bool barvaP;

    private void AkceP(object sender, MouseButtonEventArgs e)
    {
        OcickoP.Fill = barvaP ? Brushes.Black : Brushes.Red;
        barvaP = !barvaP;
    }
    ```

1. Spusťte aplikaci, změňte velikost okna a ověřte funkci Viewboxu i reakci pravého oka na kliknutí.

Dodaný soubor MainWindow.xaml obsahuje hotovou variantu DrawingGroup. Varianta Canvas a metoda AkceP jsou v řešení zakomentované a slouží jako druhá část cvičení.

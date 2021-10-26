---
title: "Fonts"
description: "This article explains how to specify font information on controls that display text in .NET MAUI apps."
author: davidbritch
ms.author: dabritch
ms.date: 10/19/2021
---

# Fonts

<!-- Sample link (if any), goes here -->

By default, .NET Multi-platform App UI (.NET MAUI) apps use the Open Sans font on each platform. However, this default can be changed, and additional fonts can be registered for use in an app.

All controls that display text define properties that can be set to change font appearance:

- `FontFamily`, of type `string`.
- `FontAttributes`, of type `FontAttributes`, which is an enumeration with three members: `None`, `Bold`, and `Italic`. The default value of this property is `None`.
- `FontSize`, of type `double`.
- `FontAutoScalingEnabled`, of type `bool`, which defines whether an app's UI reflects text scaling preferences set in the operating system. The default value of this property is `true`.

These properties are backed by `BindableProperty` objects, which means that they can be targets of data bindings, and styled.

## Register fonts

True type format (TTF) and open type font (OTF) fonts can be added to your app and referenced by filename or alias, with registration being performed in the `CreateMauiApp` method in the `MauiProgram` class. This is accomplished by invoking the `ConfigureFonts` method on the `MauiAppBuilder` object. Then, on the `IFontCollection` object, call the `AddFont` method to add the required font to your app:

```csharp
using Microsoft.Maui.Controls.Hosting;
using Microsoft.Maui.Hosting;

namespace MyMauiApp
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("Lobster-Regular.ttf", "Lobster");
                });

            return builder.Build();
        }
    }  
}
```

In the example above, the first argument to the `AddFont` method is the font filename, while the second argument represents an optional alias by which the font can be referenced when consuming it.

Any fonts consumed by an app must be placed in the **Resources** > **Fonts** folder of the app's project, with a build action of **MauiFont**. This creates a corresponding item in the app's .csproj file. Alternatively, all fonts in the app can be registered by using a wildcard in the .csproj file:

```xml
<ItemGroup>
   <MauiFont Include="Resources\Fonts\*" />
</ItemGroup>
```

> [!NOTE]
> The `*` wildcard character indicates that all the files within the folder will be treated as being font files. In addition, if you want to include files from sub-folders too, then configure it using additional wildcard characters, for example, `Resources\Fonts\**\*`.

## Consume fonts

Registered fonts can be consumed by setting the `FontFamily` property of a control that displays text to the font name, without the file extension:

```xaml
<!-- Use font name -->
<Label Text="Hello .NET MAUI"
       FontFamily="Lobster-Regular" />
```

Alternatively, it can be consumed by referencing its alias:

```xaml
<!-- Use font alias -->
<Label Text="Hello .NET MAUI"
       FontFamily="Lobster" />
```

The equivalent C# code is:

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello .NET MAUI!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello .NET MAUI!",
    FontFamily = "Lobster"
};
```

## Set font attributes

Controls that display text can set the `FontAttributes` property to specify font attributes:

```xaml
<Label Text="Italics"
       FontAttributes="Italic" />
<Label Text="Bold and italics"
       FontAttributes="Bold, Italic" />
```

The equivalent C# code is:

```csharp
Label label1 = new Label
{
    Text = "Italics",
    FontAttributes = FontAttributes.Italic
};

Label label2 = new Label
{
    Text = "Bold and italics",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};    
```

## Set the font size

Controls that display text can set the `FontSize` property to specify the font size. The `FontSize` property can be set to a `double` value directly, or by a `NamedSize` enumeration value:

```xaml
<Label Text="Font size 24"
       FontSize="24" />
<Label Text="Large font size"
       FontSize="Large" />
```

The equivalent C# code is:

```csharp
Label label1 = new Label
{
    Text = "Font size 24",
    FontSize = 24
};

Label label2 = new Label
{
    Text = "Large font size",
    FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))
};
```

<!-- Todo: Check Device.GetNamedSize survives the cull -->

Alternatively, the `Device.GetNamedSize` method has an override that specifies the second argument as an `Element`:

```csharp
Label myLabel = new Label
{
    Text = "Large font size",
};
myLabel.FontSize = Device.GetNamedSize(NamedSize.Large, myLabel);
```

> [!NOTE]
> The `FontSize` value, when specified as a `double`, is measured in device-independent units. <!--For more information, see [Units of Measurement](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).-->

For more information about named font sizes, see [Understand named font sizes](#understand-named-font-sizes).

## Disable font auto scaling

All controls that display text have font scaling enabled by default, which means that an app's UI reflects text scaling preferences set in the operating system. However, this behavior can be disabled by setting the `FontAutoScalingEnabled` property on text-based control's to `false`:

```xaml
<Label Text="Scaling disabled"
       FontSize="18"
       FontAutoScalingEnabled="False" />
```

This approach is useful when you want to guarantee that text is displayed at a specific size.

> [!NOTE]
> Font auto scaling also works with font icons. For more information, see [Display font icons](#display-font-icons).

<!-- Todo: Breaking change on iOS because all numbered fonts will scale. Disable by implementing IFontManager that inherits from MAUI, and explicitly implement GetFont to influence the font that's returned. -->

## Set font properties per platform

The `OnPlatform` and `On` classes can be used in XAML to set font properties per platform. The example below sets different font families and sizes:

<!-- Todo: UWP refs in code below -->

```xaml
<Label Text="Different font properties on different platforms"
       FontSize="{OnPlatform iOS=20, Android=Medium, UWP=24}">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
            <On Platform="iOS" Value="MarkerFelt-Thin" />
            <On Platform="Android" Value="Lobster-Regular" />
            <On Platform="UWP" Value="ArimaMadurai-Black" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<!-- Todo: Device.RuntimePlatform will most likely be deprecated in favour of Essentials -->

The `Device.RuntimePlatform` property can be used in code to set font properties per platform

```csharp
Label label = new Label
{
    Text = "Different font properties on different platforms"
};

label.FontSize = Device.RuntimePlatform == Device.iOS ? 20 :
    Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : 24;
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular" : "ArimaMadurai-Black";
```

<!-- For more information about providing platform-specific values, see [Provide platform-specific values](~/xamarin-forms/platform/device.md#provide-platform-specific-values). For information about the `OnPlatform` markup extension, see [OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension). -->

## Understand named font sizes

<!-- Todo: Confirm that these sizes are still valid -->

.NET MAUI defines fields in the `NamedSize` enumeration that represent specific font sizes. The following table shows the `NamedSize` members, and their default sizes on iOS, Android, and Windows:

| Member | iOS | Android | Windows |
| --- | --- | --- | --- |
| `Default` | 17 | 14 | 14 |
| `Micro` | 12 | 10 | 15.667 |
| `Small` | 14 | 14 | 18.667 |
| `Medium` | 17 | 17 | 22.667 |
| `Large` | 22 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

The size values are measured in device-independent units. <!-- For more information, see [Units of Measurement](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement). -->

> [!NOTE]
> On iOS and Android, named font sizes will autoscale based on operating system accessibility options. This behavior can be disabled on iOS with a platform-specific. <!-- For more information, see [Accessibility Scaling for Named Font Sizes on iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md). -->

## Display font icons

Font icons can be displayed by .NET MAUI apps by specifying the font icon data in a `FontImageSource` object. This class, which derives from the `ImageSource` class, has the following properties:

- `Glyph` – the unicode character value of the font icon, specified as a `string`.
- `Size` – a `double` value that indicates the size, in device-independent units, of the rendered font icon. The default value is 30. In addition, this property can be set to a named font size.
- `FontFamily` – a `string` representing the font family to which the font icon belongs.
- `Color` – an optional `Color` value to be used when displaying the font icon.

This data is used to create a PNG, which can be displayed by any view that can display an `ImageSource`. This approach permits font icons, such as emojis, to be displayed by multiple views, as opposed to limiting font icon display to a single text presenting view, such as a `Label`.

> [!IMPORTANT]
> Font icons can only currently be specified by their unicode character representation.

The following XAML example has a single font icon being displayed by an `Image` view:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

This code displays an XBox icon, from the Ionicons font family, in an `Image` view. Note that while the unicode character for this icon is `\uf30c`, it has to be escaped in XAML and so becomes `&#xf30c;`. The equivalent C# code is:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

The following screenshot shows several font icons being displayed:

:::image type="content" source="fonts-images/font-image-source.png" alt-text="Screenshot of three font icons.":::
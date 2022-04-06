---
title: "VisualElement legacy color mode on Android"
description: "This article explains how to consume the .NET MAUI Android platform-specific that disables the .NET MAUI legacy color mode."
ms.date: 04/05/2022
---

# VisualElement legacy color mode on Android

Some of the .NET Multi-platform App UI (.NET MAUI) views feature a legacy color mode. In this mode, when the `IsEnabled` property of the view is set to `false`, the view will override the colors set by the user with the default native colors for the disabled state. For backwards compatibility, this legacy color mode remains the default behavior for supported views.

This Android platform-specific disables this legacy color mode, so that colors set on a view by the user remain even when the view is disabled. It's consumed in XAML by setting the `VisualElement.IsLegacyColorModeEnabled` attached property to `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Microsoft.Maui.Controls.PlatformConfiguration.AndroidSpecific;assembly=Microsoft.Maui.Controls">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

[!INCLUDE [docs under construction](~/includes/preview-note.md)]

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Microsoft.Maui.Controls.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Microsoft.Maui.Controls.PlatformConfiguration.Android>().SetIsLegacyColorModeEnabled(false);
```

The `VisualElement.On<Microsoft.Maui.Controls.PlatformConfiguration.Android>` method specifies that this platform-specific will only run on Android. The `VisualElement.SetIsLegacyColorModeEnabled` method, in the `Microsoft.Maui.Controls.PlatformConfiguration.AndroidSpecific` namespace, is used to control whether the legacy color mode is disabled. In addition, the `VisualElement.GetIsLegacyColorModeEnabled` method can be used to return whether the legacy color mode is disabled.

The result is that the legacy color mode can be disabled, so that colors set on a view by the user remain even when the view is disabled:

:::image type="content" source="media/legacy-color-mode/legacy-color-mode-disabled.png" alt-text="Legacy color mode disabled.":::

> [!NOTE]
> When setting a `VisualStateGroup` on a view, the legacy color mode is completely ignored. For more information about visual states, see [Visual states](~/user-interface/visual-states.md).
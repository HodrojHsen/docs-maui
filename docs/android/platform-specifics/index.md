---
title: "Android platform-specifics in .NET MAUI"
description: "Learn how to consume Android platform-specifics in .NET MAUI apps."
ms.date: 04/05/2022
---

# Android platform-specifics

.NET Multi-platform App UI (.NET MAUI) platform-specifics allow you to consume functionality that's only available on a specific platform, without customizing handlers.

[!INCLUDE [docs under construction](~/includes/preview-note.md)]

The following platform-specific functionality is provided for .NET MAUI views, pages, and layouts on Android:

- Controlling the Z-order of visual elements to determine drawing order. For more information, see [VisualElement elevation on Android](visualelement-elevation.md).
- Disabling legacy color mode on a supported `VisualElement`. For more information, see [VisualElement legacy color mode on Android](legacy-color-mode.md).

The following platform-specific functionality is provided for .NET MAUI views on Android:

- Using the default padding and shadow values of Android buttons. For more information, see [Button padding and shadows on Android](button-padding-shadow.md).
- Setting the input method editor options for the soft keyboard for an `Entry`. For more information, see [Entry input method editor options on Android](entry-ime-options.md).
- Enabling a drop shadow on a `ImageButton`. For more information, see [ImageButton drop shadows on Android](imagebutton-drop-shadow.md).
- Enabling fast scrolling in a `ListView`. For more information, see [ListView fast scrolling on Android](listview-fast-scrolling.md).
- Controlling the transition that's used when opening a `SwipeView`. For more information, see [SwipeView swipe transition Mode](swipeview-swipetransitionmode.md).
- Controlling whether a `WebView` can display mixed content. For more information, see [WebView mixed content on Android](webview-mixed-content.md).
- Enabling zoom on a `WebView`. For more information, see [WebView zoom on Android](webview-zoom-controls.md).

The following platform-specific functionality is provided for .NET MAUI cells on Android:

- Enabling `ViewCell` context actions legacy mode, so that the context actions menu is not updated when the selected item in a `ListView` changes. For more information, see [ViewCell context actions on Android](viewcell-context-actions.md).

The following platform-specific functionality is provided for .NET MAUI pages on Android:

- Setting the height of the navigation bar on a `NavigationPage`. For more information, see [NavigationPage bar height on Android](navigationpage-bar-height.md).
- Disabling transition animations when navigating through pages in a `TabbedPage`. For more information, see [TabbedPage page transition animations on Android](tabbedpage-transition-animations.md).
- Enabling swiping between pages in a `TabbedPage`. For more information, see [TabbedPage page swiping on Android](tabbedpage-page-swiping.md).
- Setting the toolbar placement and color on a `TabbedPage`. For more information, see [TabbedPage toolbar placement on android](tabbedpage-toolbar-placement.md).

The following platform-specific functionality is provided for the .NET MAUI `Application` class on Android:

- Setting the operating mode of a soft keyboard. For more information, see [Soft keyboard input mode on Android](soft-keyboard-input-mode.md).
- Disabling the `Disappearing` and `Appearing` page lifecycle events on pause and resume respectively, for applications that use AppCompat. For more information, see [Page lifecycle events on Android](page-lifecycle-events.md).
---
author: Xansky
Description: Provides a checklist to help you ensure that your Universal Windows Platform (UWP) app is accessible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Accessibility checklist
label: Accessibility checklist
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 9580ccc0716b7e9f4ee32ce241b0ac3ee9bf319d

---

# Accessibility checklist



Provides a checklist to help you ensure that your Universal Windows Platform (UWP) app is accessible.

Here we provide a checklist you can use to ensure that your app is accessible.

1.  Set the accessible name (required) and description (optional) for content and interactive UI elements in your app.

    An accessible name is a short, descriptive text string that a screen reader uses to announce a UI element. Some UI elements such as <bpt id="p1">[</bpt><bpt id="p2">**</bpt>TextBlock<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/apps/BR209652)</ept> and <bpt id="p3">[</bpt><bpt id="p4">**</bpt>TextBox<ept id="p4">**</ept><ept id="p3">](https://msdn.microsoft.com/library/windows/apps/BR209683)</ept> promote their text content as the default accessible name; see <bpt id="p5">[</bpt>Basic accessibility information<ept id="p5">](basic-accessibility-information.md#name_from_inner_text)</ept>.

    You should set the accessible name explicitly for images or other controls that do not promote inner text content as an implicit accessible name. You should use labels for form elements so that the label text can be used as a <bpt id="p1">[</bpt><bpt id="p2">**</bpt>LabeledBy<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/apps/Hh759769)</ept> target in the Microsoft UI Automation model for correlating labels and inputs. If you want to provide more UI guidance for users than is typically included in the accessible name, accessible descriptions and tooltips help users understand the UI.

    For more info, see <bpt id="p1">[</bpt>Accessible name<ept id="p1">](basic-accessibility-information.md#accessible_name)</ept> and <bpt id="p2">[</bpt>Accessible description<ept id="p2">](basic-accessibility-information.md)</ept>.

2.  Implement keyboard accessibility:

    * Test the default tab index order for a UI. Adjust the tab index order if necessary, which may require enabling or disabling certain controls, or changing the default values of <bpt id="p1">[</bpt><bpt id="p2">**</bpt>TabIndex<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/apps/BR209461)</ept> on some of the UI elements.
    * Use controls that support arrow-key navigation for composite elements. For default controls, the arrow-key navigation is typically already implemented.
    * Use controls that support keyboard activation. For default controls, particularly those that support the UI Automation <bpt id="p1">[</bpt><bpt id="p2">**</bpt>Invoke<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/apps/BR242582)</ept> pattern, keyboard activation is typically available; check the documentation for that control.
    * Set access keys or implement accelerator keys for specific parts of the UI that support interaction.
    * For any custom controls that you use in your UI, verify that you have implemented these controls with correct <bpt id="p1">[</bpt><bpt id="p2">**</bpt>AutomationPeer<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/apps/BR209185)</ept> support for activation, and defined overrides for key handling as needed to support activation, traversal and access or accelerator keys.

    For more info, see <bpt id="p1">[</bpt>Keyboard interactions<ept id="p1">](https://msdn.microsoft.com/library/windows/apps/Mt185607)</ept>.

3.  Visually verify your UI to ensure that the text contrast is adequate, elements render correctly in the high-contrast themes, and colors are used correctly.

    * Use the system display options that adjust the display's dots per inch (dpi) value, and ensure that your app UI scales correctly when the dpi value changes. (Some users change dpi values as an accessibility option, it's available from <bpt id="p1">**</bpt>Ease of Access<ept id="p1">**</ept>.)
    * Use a color analyzer tool to verify that the visual text contrast ratio is at least 4.5:1.
    * Switch to a high contrast theme and verify that the UI for your app is readable and usable.
    * Ensure that your UI doesn’t use color as the only way to convey information.

    For more info, see <bpt id="p1">[</bpt>High-contrast themes<ept id="p1">](high-contrast-themes.md)</ept> and <bpt id="p2">[</bpt>Accessible text requirements<ept id="p2">](accessible-text-requirements.md)</ept>.

4.  Run accessibility tools, address reported issues, and verify the screen reading experience.

    Use tools such as <bpt id="p1">[</bpt><bpt id="p2">**</bpt>Inspect<ept id="p2">**</ept><ept id="p1">](https://msdn.microsoft.com/library/windows/desktop/Dd318521)</ept> to verify programmatic access, run diagnostic tools such as <bpt id="p3">[</bpt><bpt id="p4">**</bpt>AccChecker<ept id="p4">**</ept><ept id="p3">](https://msdn.microsoft.com/library/windows/desktop/Hh920985)</ept> to discover common errors, and verify the screen reading experience with Narrator.

    For more info, see <bpt id="p1">[</bpt>Accessibility testing<ept id="p1">](accessibility-testing.md)</ept>.

5.  Make sure your app manifest settings follow accessibility guidelines.

6.  Declare your app as accessible in the Windows Store.

    If you implemented the baseline accessibility support, declaring your app as accessible in the Windows Store can help reach more customers and get some additional good ratings.

    For more info, see <bpt id="p1">[</bpt>Accessibility in the Store<ept id="p1">](accessibility-in-the-store.md)</ept>.

<span id="related_topics"/>
## Related topics  
* [Accessibility](accessibility.md)
* [Design for accessibility](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Practices to avoid](practices-to-avoid.md)



<!--HONumber=Jun16_HO4-->



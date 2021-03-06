---
title: Identifying the Input Source for a Scan Event
author: windows-driver-content
description: Identifying the Input Source for a Scan Event
ms.assetid: aaa0bbf4-6866-45d7-8150-c6a74d6c46c1
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Identifying the Input Source for a Scan Event


A *push-scan* operation is a scanning operation that the user initiates from a WIA scanner device instead of from the user interface of a WIA application running on a desktop computer. When the user presses the start-scan button on the device, the application receives a scan event to notify it that the user has requested a scanning operation. In response to this event, the application can perform the push-scan operation in one of the following two ways:

-   If the device supports [auto-configured scanning](auto-configured-scanning.md), the application can request a data transfer from the [auto item](auto-item.md) to acquire an image from the currently selected input source (flatbed, automatic document feeder, or film-scanning adapter). In response, the device automatically configures its scan settings (excluding the few properties that can be configured only by the application, which are described in [WIA Properties Supported by an Auto Item](wia-properties-supported-by-an-auto-item.md)) and then acquires the image.

-   The application can perform the scanning operation under direct program control. First, the application configures the properties of the WIA item (flatbed item, feeder item, or film item) that represents the currently selected input source. Next, the application acquires an image by requesting a data transfer from this item.

For more information about WIA items, see [WIA Item Categories](wia-item-categories.md).

When a scan event occurs, the application receives a notification that includes a WIA event identifier (a GUID value) to specify the nature of the event. The WIA minidriver can assign a custom WIA event identifier GUID to an event, or the minidriver can use one of the WIA\_EVENT\_SCAN\_*XXX* GUID constants defined in header file *Wiadef.h*. For more information about these constants, see [WIA Event Identifiers](http://go.microsoft.com/fwlink/p/?linkid=127716).

Although the WIA event identifier for a scan event provides information about the event, it does not identify the input source to use for the scanning operation. For auto-configured scanning, the application does not need this information. However, to perform a scan under direct program control, the application must know which input source to use. The application must have a way to obtain this information from the device if the device has more than one input source and the user can select the input source from the device instead of from the user interface of the application. When selecting an input source from the device, the user can select the source either explicitly (by pressing a button on the device's front panel) or implicitly (for example, by inserting a document into a feeder on the device).

In Windows 7 and later versions of Windows, when a scan event occurs, an application can query the WIA scanner device's WIA\_DPS\_SCAN\_AVAILABLE\_ITEM property to identify the selected input source, if the device supports this property. WIA\_DPS\_SCAN\_AVAILABLE\_ITEM is an optional property of the root item in a device's WIA item tree. For more information about this property, see [**WIA\_DPS\_SCAN\_AVAILABLE\_ITEM**](https://msdn.microsoft.com/library/windows/hardware/ff551426).

In Windows Vista, the Web Services for Devices (WSD) scan class driver supports the WIA\_DPS\_SCAN\_AVAILABLE\_ITEM property. The driver defines this property (under the name WIA\_WSD\_SCAN\_AVAILABLE\_ITEM) as a custom driver extension. The driver supports only networked WIA scanner devices that implement the Windows Device Protocol (WDP) 1.0 for scanners. In Windows 7, the WSD scan class driver implements the WIA\_DPS\_SCAN\_AVAILABLE\_ITEM property as a standard driver feature, as described in the preceding paragraph, instead of as a custom driver extension. For more information about the WSD scan class driver, see [WIA with Web Services for Devices](wia-with-web-services-for-devices.md). For more information about WDP for scanners, see [Web Services for Devices Scan Service Schema](https://msdn.microsoft.com/library/windows/hardware/ff547963).

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bimage\image%5D:%20Identifying%20the%20Input%20Source%20for%20a%20Scan%20Event%20%20RELEASE:%20%288/17/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



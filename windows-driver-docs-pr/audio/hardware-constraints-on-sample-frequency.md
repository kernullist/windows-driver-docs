---
Description: Hardware Constraints on Sample Frequency
MS-HAID: 'audio.hardware\_constraints\_on\_sample\_frequency'
MSHAttr: 'PreferredLib:/library/windows/hardware'
title: Hardware Constraints on Sample Frequency
---

# Hardware Constraints on Sample Frequency


## <span id="hardware_constraints_on_sample_frequency"></span><span id="HARDWARE_CONSTRAINTS_ON_SAMPLE_FREQUENCY"></span>


Some audio devices require that the sample frequency at the adapter filter's sink pin match the frequency of a digital output port or the input stream from a microphone. For example, Sound Blaster 16-compatible hardware typically has a single crystal, which constrains its input and output streams to run at the same clock rate. An adapter that can support more than one clock rate for its various on-board audio streams might still need to restrict the number of different clock rates to some small number.

For these reasons, an adapter driver might need to constrain the sample frequency on one on-board stream to match that of another on-board stream. For example, a Sound Blaster 16-compatible adapter might require that the sample frequency at the adapter's sink pin match the rate at which the latches are clocked at the output DACs.

As explained previously, KMixer is the system mixer in Windows Server 2003, Windows XP, Windows 2000, and Windows Me/98. When KMixer's source pin is connected to an adapter's sink pin, KMixer might need to call the adapter's **SetFormat** method (for example, see [**IMiniportWavePciStream::SetFormat**](audio.iminiportwavepcistream_setformat)) to adjust the sample frequency at the connection to match the highest sample frequency of the audio streams at its inputs. If the adapter is unable to change the frequency--perhaps because it is constrained by the clock rates of other on-board streams--it can fail the **SetFormat** call. In this case, KMixer will respond by making more **SetFormat** calls with successively lower sample frequencies until the call succeeds. Once KMixer has settled on a reduced sample frequency, it will sample-down its higher frequency input streams accordingly.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[audio\audio]:%20Hardware%20Constraints%20on%20Sample%20Frequency%20%20RELEASE:%20%287/14/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/en-us/default.aspx. "Send comments about this topic to Microsoft")



<img src="images/ORION_logo.png" width=250>

USER MANUAL V2.0  
Revision 1

# Introduction

## The ORION-CONVERT™ algorithm

Conversion between HDR  \<-\>  SDR can be performed either on a 'live' signal, using a hardware signal processor, or in post-production using editing or colour correction (grading) software. This conversion may be done mathematically, using an algorithm that takes the numerical values of each image pixel and calculates new values, based either on preset functions or parameterised controls to modify the conversion. Alternatively, a Look-Up Table (LUT) may be used which is a list of the output colours corresponding to particular input colours. LUTs are an efficient way of processing images but are imprecise because it is not practical to include every possible input colour in the list. A subset is used with in-between colours being created by interpolating (blending) between the nearby colours in the list.  

The ORION-CONVERT™ algorithm takes a novel three-step approach to HDR \<-\> SDR conversion, with a simple and intuitive set of user controls. The intent is not that these controls should be adjusted continuously during a broadcast transmission, or per clip in an edit, but rather that settings suitable for a particular situation will be found and used as 'presets'.  

Other HDR conversion systems based on LUTs offer a range of presets for different scenarios, but this means there are a large number of LUTs and understanding which one is appropriate in a given situation is not easy. There is also no option to 'tweak' the conversion to the needs of a particular production. Other adjustable hardware conversion systems exist, but none have the simplicity and elegance of the ORION-CONVERT™ approach. They require the user to go through multiple pages of menus, making different choices, to set up the conversion.   

Because the ORION-CONVERT™ conversion is mathematically defined, it is mathematically invertible. Applying identical parameter values in HDR to SDR (down-conversion) and then in SDR to HDR (up-conversion) will perform the same process in reverse. The compressed highlights of the SDR signal are expanded and converted to HDR. This can either be used to produce a simulated HDR signal from an SDR camera (such as a specialty slow-mo camera or mini-cam) or to recreate an original HDR signal from a prior down-conversion.   
 
If this process is applied in high precision floating point, the round-trip of a down-conversion followed by an up-conversion can be lossless. A hardware signal processor will always have limited output precision (normally a 10-bit SDI or HDMI signal) so in this case feeding the resulting signal to a second device applying an inverse conversion will not produce a signal identical to the original. Nonetheless, testing has shown that the difference is indistinguishable to a viewer.  

The ORION-CONVERT™ user interface makes it simple to set up two conversions as a matched pair of up and down conversions.  
  
<figure>
  <img src="images/ORION_UI_look.png"
    width="400"
    alt="The ORION-CONVERT™ User Interface in Ennio">
  <figcaption>The ORION-CONVERT™ User Interface in Ennio</figcaption>
</figure>

# The Interface (CONFIG)

## Lock Controls
![](images/grad_bar.png)

The controls can be locked to prevent accidental changing of parameters once selected. 

## Input Compression/Expansion
![](images/grad_bar.png)

Either compression or expansion will be available, depending on whether the output format covers more or less of the dynamic range than the input format.

### Knee

This controls the breakpoint where compression or expansion begins on the selected input signal for the conversion.  The units are in IRE of the input signal.

*Example: In an up-conversion (SDR to HDR), the signal above this level will be boosted to expand the SDR into the HDR domain.*

### Amount

This controls the amount of compression in down-conversion and expansion in up-conversion. 

*When compressing, a value of zero applies no compression (at which point the value of the ***Knee*** is irrelevant) and a value of 1 will compress completely, flattening everything above the value of the ***Knee*** to that value.* 


> *Note: For S-Log3 formats, the IRE values are relative to the full 0-1023 range of the signal, whereas for other formats they are relative to the 64-940 narrow range.*

## Conversion
![](images/grad_bar.png)

Here the user can choose the input and output format of the conversion, including choosing between different primaries where appropriate. Matched input and output formats may be selected in order to apply ORION compression, look processing or limiting without conversion.  
    
After selecting settings for a conversion, if the direction of the conversion is reversed, the parameters relating to input and output compression are swapped to generate an absolute mathematical inverse. This way, a visually lossless roundtrip is possible due to the high-precision calculations used by ORION-CONVERT™.

## Mode: Display light and Scene light
![](images/grad_bar.png)

There’s a great deal of confusion about the use of these two different kinds of conversions described in BT.2390.

> Display-Light mapping is used when the goal is to match the colours and relative tones seen on HDR and SDR displays. An example of this is the inclusion of SDR-graded content within an HDR programme.  
        
*This type of conversion might be used for graphics insertion, for example. These elements are mostly designed in an sRGB environment, so to preserve this creative intent, we want to represent these elements exactly as they were seen at the time of their creation.*  
        
> Scene-Light mapping is used where the source is a direct SDR camera output or an HDR camera output, and the goal is to match the colours and tones of that camera to another camera source after conversion.  
    
 *These conversions would be used when matching the appearance of the content, as seen by a camera, is the intended process. For example, when using the SDR output of a camera (as camera splits/isos, etc...) that can simultaneously produce SDR and HDR, we want the HDR programme (after conversion to SDR) to match in the best way possible\*.*  
    
*\* This depends very much on the camera manufacturer as some have creative tools independent of HDR and SDR outputs. Matching configuration settings to the HDR converted signal 'look' for the SDR output must be found and locked.*  

## HDR and SDR Reference points
![](images/grad_bar.png)

This controls the HDR and SDR 'anchor points'. At the default settings of 75 and 100, for example for an HLG to SDR conversion, if no compression or expansion is applied, 75% HLG will be mapped to exactly 100% SDR (direct mapping). If any compression or expansion is applied, and these values are above the ***Knee*** points, the correspondence will be shifted.

## HDR Peak
![](images/grad_bar.png)

This is the peak luminance (referred to as L<sub>w</sub> in ITU-R BT.2100) in nits of the 'virtual HLG display' used in conversion calculations. This is normally left at the default value of 1000.

The principal effect of this setting is to control the gamma of the HLG OOTF as defined in ITU-R BT.2100. In PQ conversions this value also controls how much of the 10,000 nit range which can be represented by PQ will be mapped to the destination format. In PQ conversions, when this value is altered, the PQ ***Knee*** and reference point will also change to reflect this.

> Note: This value is not used in scene-light conversions.

## Log Offset
![](images/grad_bar.png)

When a log format (S-Log3 or LogC3) is selected as the input or output format, an additional ***Log Offset*** control is made available. This controls the exposure relationship in stops between the log signal and other formats. A ***Log Offset*** of zero will map the standard mid grey value for the log format to the equivalent of 38% HLG. Some cameras which offer S-Log3 and HLG output apply an exposure offset between the signals. Our testing indicated that this was about \-0.7 stops for the camera we tested, but you should test your own setup if an exact match is required.

When this value is changed, the ***Ref*** and ***Knee*** value for the log signal will also be modified to reflect this.

## Look
![](images/grad_bar.png)

As well as technical conversion, the ORION-CONVERT™ plugin provides creative look controls. Preset looks can be loaded to emulate those provided in certain cameras, and if enabled by your licence, looks can be saved as ***.olook*** files to be shared with other users.

### Look Inverted

Applies all the look changes in reverse, allowing removal of a previously applied look.

### Pedestal

Raises and lowers the black level of the image.

### Contrast

A curve-based contrast adjustment.

### Exposure

Exposure modification in stops.

### Saturation

Saturation control.

## Output Compression/Expansion
![](images/grad_bar.png)

Either compression or expansion will be available, depending on whether the output format covers more or less dynamic range than the input format.  
    
### Knee

This slider controls the threshold at which compression (for down-conversion) or expansion (for up-conversion) begins. This is in IRE units of the output signal.


*Example: A value of 50 places this at 50% SDR IRE in a down-conversion. The start of the compression will be seen at this value on a waveform monitor because this compression is the last step of the process.*

### Amount

This controls the amount of compression in down-conversion and expansion in up-conversion. When compressing, a value of zero applies no compression (at which point the value of the ***Knee*** is irrelevant) and a value of 1 will compress completely, flattening everything above the value of the ***Knee*** to that value. 

> *Note: For S-Log3 formats, the IRE values are relative to the full 0-1023 range of the signal, whereas for other formats they are relative to the 64-940 narrow range.*

## Gamma Compensation
![](images/grad_bar.png)

This switch will enable conversion using the optional gamma adjustment, described in ITU-R BT.2446-1 section 5.1.2, to create a closer perceptual match to the HDR when viewing converted SDR at 100 nits. Without the use of this compensation, some users choose to set their SDR monitors to 200 nits.  
    
A custom shadow compression curve is included, to compensate for the lifted blacks that the ITU-suggested formula for this method can produce.  g)

## Clamping
![](images/grad_bar.png)

Internally the result of the conversion is unclamped float, meaning that even HDR values that cannot be represented on an SDR display after conversion, will be converted to SDR values outside the displayable range. When outputting over SDI, it is necessary to limit the range of the output. This control applies a clamp to the output R′G′B′ values.

Options:

- Unclamped (clamping will still occur in hardware output)          
- Clip sub-blacks  
- \-7 to 109 IRE (SDI permissible range)  
-  \-5 to 105 IRE (EBU R 103 preferred range)  
- 0 to 100 IRE  

## P3 Limit
![](images/grad_bar.png)

Most consumer HDR TVs on the market at this time are only capable of displaying up to the P3-D65 colour gamut. It is a common practice in the post-production of HDR-graded content, to limit the image to P3-D65 so that what is seen on the master reference monitor matches what the consumer is able to see.

We have included such a limiter for those that want to use ORION-CONVERT™ as an HDR to HDR mastering conversion tool. This limiter will still retain the primaries of the BT.2020 container, but constrain colours to within the P3-D65 gamut.

<figure>
  <img src="images/unclamped2.png" width=250> <img src="images/limited2.png" width=250
    width="400"
    alt="TThe Effect of P3 Output Limiting">
  <figcaption>The Effect of P3 Output Limiting</figcaption>
</figure>

The ORION-CONVERT™ P3 limiter uses an advanced luminance-preserving approach.

P3 limiting is only available for conversions whose output is a display-referred HDR format (PQ or HLG) and will be applied after the conversion. 

## Conversion Setup
![](images/grad_bar.png)

Thanks to the core design of the ORION-CONVERT™ algorithm, it is very easy and fast to create a conversion roundtrip (up-conversion and down-conversion) in just one go.

To do this, choose one of the conversions needed (we recommend starting with an HDR to SDR for example) and feed content into the device.

Choose on the drop-down menus the direction of the conversion and select the method you would like to use and all other parameters relevant to your conversion (HDR Peak, etc…).

Now you are all set to start creating your highlight compression/expansion using the powerful input and output sliders.

Once you’ve found the desired result, simply invert the direction of the conversion on the drop-down menus and you will get an absolute mathematical inverse to ensure a clean roundtrip.

To understand how the various ORION-CONVERT™ controls operate, it can be useful to view the result on a waveform monitor of passing a linear ramp test pattern through the conversion, while adjusting the controls. This will give the user a more intuitive feel for their effect.

Graphics are normally\* converted without any highlight compression or expansion which is often referred to as 'direct mapping.

\* some creative approaches to graphic content creation can be used to enhance or allow for a more 'HDR look' after conversion, such as using slightly lower RGB values for graphic text to leave room for specular highlights in the graphics. These can then be expanded once the graphics are converted.

## HDR Mastering Setup
![](images/grad_bar.png)

When using ORION-CONVERT™ as an HDR mastering tool, a no-conversion (HLG to HLG or PQ to PQ) can be selected in the conversion drop-down menus.  
    
By using the ***input compression*** slider, the user can roll off any highlights beyond the range that a target consumer TV will be able to display; for example, super white areas  (109% IRE) in HLG or different peak brightnesses when in PQ (2000 nit to 1000 nit, etc.) and adjust the signal to the delivery range desired.  
    
In this mode, a P3 limiter may be applied, if required by the delivery spec, for example.  

![](images/grad_bar.png)  

<img src="images/ORION_logo.png" width=250>

Copyright ORION-CONVERT™  
Version 2.0 September 2024  

![](images/grad_bar.png)
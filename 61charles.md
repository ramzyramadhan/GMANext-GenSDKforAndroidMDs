Select platform: [Android](https://developers.google.com/admob/android/next-gen/charles "View this page for the Android platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/charles "View this page for the Android (Legacy) platform docs.")

<br />

On Android N or higher, ad calls are visible in Charles proxy only when the
following steps are performed:

1. Install Charles SSL certificate on your device, and set up proxy.
2. Enable SSL Proxy for your mobile app.

## Install Charles SSL certificate on your device, and set up proxy

To use Charles as a proxy for your mobile app, you will need to download and
[install](https://www.charlesproxy.com/documentation/installation/) Charles
on a computer. Follow Charles' instructions to install an SSL certificate on
the Android emulator or mobile device.

![](https://developers.google.com/static/admob/images/charles/Copy-of3.png)

It is simpler to [use the emulator with a
proxy](https://developer.android.com/studio/run/emulator-networking#proxy) because the
emulator is already connected to the same Wi-Fi network with the computer
running Charles. When using the emulator with a proxy, set the proxy to
localhost (`http://127.0.0.1`) and the port that Charles proxy is running on
(found in Charles menu option **Proxy \> Proxy Settings**).

If you're using a physical mobile device (phone or tablet), you'll need to
connect the mobile device to the same Wi-Fi network with your computer
running Charles using the [advanced network
settings](https://support.google.com/pixelphone/answer/2819519). When setting up the
proxy settings for your physical device, use the Charles menu option **Help \>
Local IP address** to get the IP address of your computer, to enter for the proxy
address on your device (you must be on the same Wi-Fi network for this to work).
Use the port that Charles proxy is running on.

## Enable SSL Proxy for your mobile app

For Charles to intercept your mobile app's SSL traffic, you will need to declare
that your app can trust a user-provided SSL certificate.

First, you will need to add a new XML resource file for [Network Security
Configuration](https://developer.android.com/training/articles/security-config) under

    <network-security-config>
       <debug-overrides>
           <trust-anchors>
               <!-- Trust user added CAs while debuggable only -->
               <certificates src="user" />
           </trust-anchors>
       </debug-overrides>
    </network-security-config>

![](https://developers.google.com/static/admob/images/charles/Copy-of4.png)

Next, update the `AndroidManifest.xml` file to use the network security
configuration.

    <?xml version="1.0" encoding="utf-8"?>
    <manifest ... >
        <application ...
                     android:networkSecurityConfig="@xml/network_security_config"
                     ... >
            ...
        </application>
    </manifest>

![](https://developers.google.com/static/admob/images/charles/Copy-of5.png)

After that, you can launch the mobile app and look for ad requests in the
Charles log.
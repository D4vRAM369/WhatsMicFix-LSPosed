
<p align="center">
  <img src="https://github.com/user-attachments/assets/5d8cfba5-8faa-4176-a689-c47b982c41ee" width="400" height="400" alt="WhatsMicFix icon"/><br><br>
  <b>🎧 WhatsMicFix v1.4</b><br><br>
  <img src="https://img.shields.io/github/stars/D4vRAM369/WhatsMicFix?style=social" alt="Stars"/>
  <img src="https://img.shields.io/github/downloads/D4vRAM369/WhatsMicFix/total?color=blue" alt="Downloads"/>
  <img src="https://img.shields.io/badge/Kotlin-1.9%2B-orange"/>
  <img src="https://img.shields.io/badge/Android-12%2B-brightgreen?logo=android"/>
  <img src="https://img.shields.io/badge/LSPosed-1.9%2B-blue?logo=android"/>
  <img src="https://img.shields.io/badge/Gradle-8.2%2B-02303A?logo=gradle"/>
  <img src="https://img.shields.io/badge/License-GPLv3-green"/>
  <img src="https://img.shields.io/badge/Made_with-ChatGPT_&_PBL-8A2BE2?logo=openai&logoColor=white"/>
  <img src="https://img.shields.io/badge/ClaudeCode_&_PBL-powered-4B0082?style=flat-square&logo=anthropic&logoColor=white"/>
  <img src="https://img.shields.io/badge/Optimized_for-Pixel_Devices-lightblue?logo=google"/>
  <img src="https://img.shields.io/badge/Audio_Processing-DSP-orange"/>
  <img src="https://img.shields.io/badge/Made_with-Love_&_Coffee-ff69b4"/>
  <a href="https://www.buymeacoffee.com/D4vRAM369"><img src="https://img.shields.io/badge/Buy_me_a_coffee-☕-5F7FFF"/></a>
  <a href="https://deepwiki.com/D4vRAM369/WhatsMicFix-LSPosed"><img src="https://deepwiki.com/badge.svg" alt="Ask DeepWiki"></a>

<p align="center">
  🌐 &nbsp;<strong>Language / Idioma:</strong> &nbsp;
  <strong>English 🇬🇧</strong> &nbsp;|&nbsp;
  <a href="README_spanish-version.md">Español 🇪🇸</a>
</p>

## ❓ What is WhatsMicFix?

**WhatsMicFix** is an **LSPosed module** that fixes the **abnormally low microphone volume in WhatsApp voice notes**, a long-standing issue affecting several **Google Pixel devices**, even on recent Android versions.

The module internally intercepts the audio recording pipeline using hooks on `AudioRecord`, allowing it to:

- Adjust the **actual microphone gain** (pre and post-processing).
- Apply **dynamic compression**, AGC, and noise suppression.
- Prevent initialization failures and **race conditions** at the start of recording.
- Maintain stability and compatibility even when WhatsApp reports inconsistent audio formats.

WhatsMicFix **does not modify WhatsApp**, does not patch APKs, and does not alter system files.  
It works **at runtime**, in a reversible and controlled manner.

It is primarily intended for **advanced users with root and LSPosed**, and has been **optimized for Pixel devices**, where this bug has remained unresolved for years without a clear official fix from Google or Meta.

> ⚠️ This module is only useful if your device actually suffers from this issue.  
> On models where the bug has already been fixed, WhatsMicFix is not necessary.



---


## ✨ What's New in v1.4
* **Full stability**: The boost and compressor are now prepared *before* recording starts, completely eliminating the **race condition** that caused audio without the effect applied.
* **Triple protection layer**:
  * Early format detection (`AudioFormat.ENCODING_PCM_16BIT`) before the first `read()`.
  * **Permissive mode**: If WhatsApp returns an unknown format, the module assumes PCM16.
  * Automatic fallback to prevent rejections or initialization errors.
* **Smart compressor reset** on every session → prevents inheriting low gain from previous audio.
* **Black splash screen** on app launch (clean start without flicker).
* **Improved performance**: Lower CPU load, faster startup, and consistent behavior.
* **New support section** in the README for collaboration and starring ⭐.

---
## 🚀 Usage
1. Install the APK as an **LSPosed module** and enable it for:
   * WhatsApp (`com.whatsapp`)
   * **System Framework** (`system`)
   * **Android System** (`android`)
   > **If you don't see “system” or “android”** in the scope list: Go to **LSPosed Settings → (top-right icon) → Hide** and **uncheck “Hide system apps”**.
2. Open **WhatsMicFix** and adjust preferences:
   * Gain in dB (**–6 dB … +12 dB**, up to ×4.0)
   * Optional pre-boost
   * AGC and Noise suppression
   * Force internal microphone
3. Adjust the **gain** slider (recommended: **1.5× – 3.0×**).
4. (Optional) **Force stop** WhatsApp after changing settings:
   * **System Settings → Apps → WhatsApp → Force stop**, then reopen WhatsApp.
5. Open WhatsApp and record a voice note.
   * If you only close from **Recents**, the process may remain active and changes won't apply.
   * Wait about **5 seconds** for WhatsApp to initialize `AudioRecord` and hooks to take effect.
> *Note:* v1.4 supports hot changes, but **force stopping** ensures full activation of new parameters.

---
## ⚙️ Advanced Configuration
* **Respect requested format**: Keeps the audio format requested by the app (recommended).
* **Force MIC source**: Use only if microphone switching fails.
* **AGC / Noise suppression**: Additional input quality improvement.
* **Debug logs**: View with `adb logcat | grep WhatsMicFix`.

---
## 📊 Technical Improvements
* `updateGlobalBoostFactor()` moved to **beforeHookedMethod()** → boost applied before recording.
* **Early PCM16 format detection** with thread-safe caching using `ConcurrentHashMap`.
* **Compressor reset** per session: Prevents residual gain states.
* **Permissive mode in ensurePcm16()** → Handles delayed format from WhatsApp.
* **Full audio flow validation** for maximum compatibility.

---
## 🛠️ Fixes
* Eliminated the bug of **first audio without boost**.
* Fixed invalid format detection (`AudioFormat.ENCODING_INVALID`).
* Prevented inheritance of old compressor values.
* No false negatives or hook rejections.

---
## 🔹 Technical Comparison
| Aspect                        | v1.3          | v1.4 (current) |
|-------------------------------|---------------|----------------|
| **First audio stable**        | ~90 %         | ✅ 100 %       |
| **Consecutive audios OK**     | ~95 %         | ✅ 100 %       |
| **"Invalid format" alerts**   | 1–2 per session | 🚫 0         |
| **Hook timing**               | Variable      | ⚡ Consistent  |

---
## 📚 Technical Notes
WhatsMicFix was developed using the **Project-Based Learning (PBL)** method, combining practical learning with real-world development.
The module intercepts and modifies **`AudioRecord`** to enhance the input signal in WhatsApp, especially on **Pixel devices** where microphone volume is often low.
This v1.4 version marks the transition from an experimental fix to a **professional audio module**, with major improvements in stability, compatibility, and efficiency.

## Final Compatibility Note and Personal Message
**Important**: NOT ALL PIXEL DEVICES HAVE THIS ISSUE.

On Pixel 9 series models (I'm currently using a Pixel 9a), low volume in WhatsApp voice notes is already fixed natively, and the module is not needed (nor does it negatively affect anything, though I haven't tested it personally on this model yet). However, many users of older models continue to suffer from this annoying, distressing, and frustrating bug—even on Android 16.

I hope this module keeps helping those users until Google provides an official fix for all legacy models through a clear statement on this well-known issue, which has been around for at least 2 years without a definitive solution from Google or Facebook (Meta), as they keep passing the ball to each other.

I endured this problem for several months on my Pixel 8, and what started as a personal fix gradually became my **first public project**. I never imagined the acceptance it would get in such a short time (16 stars in less than 4 months) and the spread it received in large Telegram channels like popMODS, MRP-Discussion, and Magisk Root Port (the latter has since been closed). I found out about it through a comrade in a Magisk and root group I'm in, where it was shared and mentioned me.

**Thank you from the bottom of my heart** to everyone for the support and encouragement ❤️ Without knowing it, you gave me more fuel for the metaphorical Ferrari of learning and creation I'm riding **full throttle** ever since, in an even more active and deeper way. Motivation is an incredibly powerful tool.

If anyone is willing to collaborate (code improvements, support for more apps, a non-root version via Shizuku, or any ideas), go for it! Open a **Pull Request** or Issue. I'm open to anything that makes the module more useful and accessible.

Keep pushing! ☕🔥

---
## 💬 Project Support
If this module has been useful to you, consider supporting it:
<p align="center">
  <a href="https://github.com/D4vRAM369/WhatsMicFix/stargazers">
    <img src="https://img.shields.io/badge/Give_a_Star_on_GitHub-⭐-yellow?style=for-the-badge"/>
  </a>
  <a href="https://www.buymeacoffee.com/D4vRAM369">
    <img src="https://img.shields.io/badge/Buy_me_a_coffee-☕-blueviolet?style=for-the-badge"/>
  </a>
</p>

---

💡 *Developed by D4vRAM using PBL learning and collaborative AI.*  
💚 License: **GPLv3 – Free, open-source, and transparent software.**

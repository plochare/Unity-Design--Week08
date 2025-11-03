# Unity Design - Week 08

## Agenda

1. [Unity Vuforia SDK - Area Targets](#Unity-Vuforia)  
2. [Niantic Lightship ARDK - VPS](#Niantic-ARDK)  
3. [Meta Quest SDK](#Meta Quest)

---

# 🧭 Vuforia Area Target Application in Unity

Design an **Augmented Reality (AR)** experience that recognizes and tracks **real-world environments** using **Vuforia Area Targets** in **Unity**.  
This tutorial walks through setup, scanning, importing, and deploying an Area Target experience for mobile or headset-based AR devices.

---

## 📘 Overview

**Vuforia Area Targets** enable your AR application to recognize and augment large physical environments — such as rooms, factories, or museums — by using spatial scans captured with supported 3D scanning devices.

Once a physical space has been scanned, you can:
- Detect and localize users inside the mapped area  
- Anchor 3D content directly to real-world surfaces  
- Create context-aware AR experiences

---

## 🏗️ Step 1: Create a Vuforia Developer Account

1. Go to [Vuforia Developer Portal](https://developer.vuforia.com).
2. Log in or sign up.
3. Create a **License Key** for your Unity project.
4. Download the **Vuforia Area Target Creator App** from the App Store or Google Play.

---

## 🗺️ Step 2: Scan Your Environment

1. Open the **Vuforia Area Target Creator App** on your device.  
2. Walk around the physical space while scanning.  
   - Ensure good lighting and surface coverage.  
   - Aim for **complete 3D coverage** of walls, floor, and key furniture.  
3. Save and **upload** your scan to the **Vuforia Developer Portal**.  
4. In the portal, go to **Target Manager → Area Targets** and **download** your `.unitypackage` file.

---

## 🧩 Step 3: Set Up Your Unity Project

1. Open Unity Hub → **New Project → 3D (URP optional)**  
2. In **Project Settings → XR Plug-in Management**, enable:
   - ✅ ARCore (Android)
   - ✅ ARKit (iOS)
3. In **Player Settings → XR Settings**, enable **Vuforia Engine**.
4. Import your **Vuforia Engine SDK** via the Package Manager.
5. Import your **Area Target .unitypackage**.

---

## 🗃️ Step 4: Add Area Target to Scene

1. In **Hierarchy → Right-click → Vuforia Engine → Area Target**.  
2. Assign your **Area Target Data Set** in the **Inspector**.  
3. Drag your **Area Target Prefab** into the Scene.  
4. Position the **ARCamera** inside or near the scanned environment.  

🎨 *You should now see the 3D mesh representation of your scanned area.*

---

## 🪄 Step 5: Add 3D Augmented Content

1. Create or import 3D assets (e.g., models, text, effects).  
2. Place them as **children** of the Area Target GameObject.  
   - This ensures the objects appear anchored to the real-world surfaces.  
3. Try adding:
   - 🖼️ A floating holographic sign  
   - 🔊 Spatial audio  
   - 🔥 Particle effects  

---

## 📱 Step 6: Build and Deploy to Mobile

1. Set your platform:
   - **File → Build Settings → Android** or **iOS**  
2. Ensure **Vuforia** and **AR Support** are enabled.  
3. Connect your device via USB.  
4. Build and Run to test the experience.

🧩 When your app launches, move your device inside the scanned area —  
the AR content will automatically align with the physical environment.

---

## 🎓 Applications Extension

You can extend this project by:
- Adding **interaction logic** using Unity C# scripts (e.g., tap-to-trigger animations).  
- Creating **narrative tours** inside museums or campus spaces.  
- Building **safety training modules** in industrial environments.  
- Integrating **UI buttons** for toggling layers or switching scenes.

---

## 🧠 References & Resources

- 📘 [Vuforia Developer Portal](https://developer.vuforia.com)  
- 🧭 [Vuforia Engine Area Targets Guide](https://library.vuforia.com/area-targets)  
- 🎓 [Unity AR Foundation Documentation](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@latest)  
- 🎥 [Vuforia Area Target Creator Tutorial Video](https://library.vuforia.com/articles/Training/how-to-create-an-area-target)  

---

# Niantic Lightship ARDK 🌍✨

Niantic’s **Augmented Reality Developer Kit (ARDK)** empowers developers to build **multiplayer, context-aware AR experiences** in Unity. With ARDK, you can blend the real world with digital content, leveraging Niantic’s expertise in AR (Pokémon GO, Peridot, Pikmin Bloom).  

---

## 🎯 What is ARDK?
Niantic ARDK provides advanced AR capabilities in Unity:
- 🧩 **Semantic Segmentation** → Understand real-world surfaces (sky, ground, buildings, people).  
- 🔍 **Meshing** → Scan & generate meshes of your environment in real-time.  
- 🌐 **Multiplayer** → Share AR experiences with multiple users via Lightship Networking.  
- 🕹 **Occlusion** → Make digital objects appear naturally behind real-world objects.  
- 🧭 **Persistent AR** → Save and reload AR content at specific real-world locations.  

---

## 🛠 Requirements
- Unity **2021 LTS or newer** (recommended).  
- AR-capable device (iOS with ARKit, Android with ARCore).  
- [Niantic Lightship Account](https://lightship.dev/) (for API keys).  
- ARDK package installed in Unity.  

---

## 📥 Installation

### 1. Create a Lightship Account
- Sign up at [lightship.dev](https://lightship.dev/).  
- Create an app and retrieve your **API key**.  

### 2. Install ARDK
1. Open Unity → `Window → Package Manager`.  
2. Click the **+** icon → *Add package from Git URL*.  
   ```text
   https://github.com/niantic-lightship/ardk-unity-package.git
````

3. Import samples for example scenes.

### 3. Configure API Key

* Go to: `Edit → Project Settings → Niantic Lightship`.
* Paste your API key in the **API Key field**.

---

## 🚀 Quick Start Tutorial

### 1. Create a New AR Scene

* Delete the default **Main Camera**.
* Add an **AR Session** prefab from ARDK (handles lifecycle).
* Add an **AR Camera** (child of AR Session).

### 2. Add Meshing

* Add **AR Mesh Manager** to your AR Session Origin.
* When running on device, ARDK will generate a mesh of your environment in real-time.

### 3. Place Objects with Tap

Example script to place an object when the user taps on a surface:

```csharp
using Niantic.ARDK.AR;
using Niantic.ARDK.AR.Camera;
using Niantic.ARDK.AR.ARSessionEventArgs;
using Niantic.ARDK.Extensions;
using UnityEngine;

public class TapToPlace : MonoBehaviour
{
    public GameObject prefab;
    private IARSession _session;

    void Start()
    {
        ARSessionFactory.SessionInitialized += OnSessionInitialized;
    }

    private void OnSessionInitialized(AnyARSessionInitializedArgs args)
    {
        _session = args.Session;
    }

    void Update()
    {
        if (_session == null || Input.touchCount == 0) return;

        Touch touch = Input.GetTouch(0);
        if (touch.phase != TouchPhase.Began) return;

        var hitResults = _session.CurrentFrame.HitTest
        (
            Camera.main.pixelWidth,
            Camera.main.pixelHeight,
            touch.position
        );

        if (hitResults.Count > 0)
        {
            var hit = hitResults[0];
            Instantiate(prefab, hit.WorldTransform.ToPosition(), hit.WorldTransform.ToRotation());
        }
    }
}
```

✅ This script spawns a prefab where the user taps, anchored to real-world surfaces detected by ARDK.

---

## 🔗 Multiplayer with Lightship

1. Add a **Multipeer Networking Manager** prefab.
2. Configure it for **Host** or **Client** mode.
3. Use `NetworkingManager.Instance.BroadcastData(...)` to sync game state across devices.

💡 Example: Multiple users can see and interact with the same AR object in real-time.

---

## ⚡ Advanced Features

* **Semantic Segmentation** → Mask out sky, ground, buildings, water, etc.
* **Occlusion** → Hide AR objects behind real-world objects.
* **Persistent AR** → Store anchors to return to the same AR scene later.
* **Shared Multiplayer AR** → Niantic’s peer-to-peer networking system.

---

## 🧪 Best Practices

* Always test on real devices (Editor play mode won’t simulate full AR).
* Optimize assets for mobile AR (low poly, compressed textures).
* Use **anchors** to keep AR objects stable.
* Handle **tracking loss** gracefully.
* Limit network payloads for smoother multiplayer AR.

---

## 📚 Resources

* [Niantic Lightship Docs](https://lightship.dev/docs/)
* [ARDK Unity API Reference](https://lightship.dev/docs/ardk/)
* [Lightship GitHub Samples](https://github.com/niantic-lightship/ardk-unity-samples)
* [Community Forum](https://lightship.dev/forums/)

---

## ✅ Summary

With Niantic’s ARDK you can:

* Build AR experiences that understand the real world (meshes, segmentation).
* Create **multiplayer shared AR sessions**.
* Make AR objects behave naturally with occlusion.
* Deliver persistent, location-based AR apps.

---

# 🚀 Meta Quest SDK Getting Started

* Meta Quest Hello World:
* https://developers.meta.com/horizon/documentation/unity/unity-tutorial-hello-vr/

* Meta Quest All-In-One SDK / Building Blocks:
https://assetstore.unity.com/packages/tools/integration/meta-xr-all-in-one-sdk-269657

---

# 🚀 Unity OpenXR Templates

* XR Interaction Toolkit 
* https://docs.unity3d.com/6000.2/Documentation/Manual/xr-meta-quest-packages.html#templates

* XR Learn XR Escape Game Tutorial
* https://learn.unity.com/course/vr-beginner-the-escape-room?version=2020.3


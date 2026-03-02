# Report: Achieving Web Exports for Godot Projects without C# (.NET)

**To:** Godot Engine Codeowners
**From:** tommythekatzo
**Date:** March 1, 2026 (or current date)
**Subject:** Strategy for Web Exports when Bypassing C# (.NET) Limitations

---

## 1. Executive Summary

This report outlines the recommended strategy for achieving successful web exports (HTML5) for Godot Engine projects when the project's codebase is **not** utilizing C# (.NET). This approach effectively circumvents the known limitations and challenges associated with C# web exports (as discussed in Issue #70796, "Readd support for web platform exports when using the C# (.NET) version of the engine").

By adhering to native Godot scripting languages (GDScript) or integrated C++ (via GDExtension/modules), developers can leverage Godot's robust HTML5 export pipeline. This document provides clear, step-by-step instructions for configuring and executing such exports.

## 2. Background: The C# Web Export Challenge (Issue #70796)

As detailed in GitHub Issue #70796, direct HTML5 export for Godot projects written in C# (.NET) currently faces significant hurdles, primarily due to:
*   Lack of mature upstream .NET support for WebAssembly (WASM) in the context required by Godot.
*   Complex architectural challenges related to JavaScript interoperability, virtual filesystems, and multi-threading on WASM when integrating the .NET runtime.

These issues are not easily resolved through simple code modifications within the Godot engine itself and often require fundamental changes at the .NET platform level or extensive re-engineering of the Godot-mono integration.

## 3. Recommended Strategy: Utilize Native Godot Languages for Web Exports

The most effective and currently supported method for reliable web exports from Godot Engine is to develop project logic using its native languages:

*   **GDScript:** Godot's built-in scripting language, optimized for engine integration.
*   **C++ (via GDExtension or Custom Modules):** For high-performance needs, C++ code compiled into WebAssembly can integrate seamlessly.

This strategy avoids the .NET-specific constraints for web targets.

## 4. Step-by-Step Instructions for HTML5 Export (C++ / GDScript Projects)

For projects developed using GDScript or native C++, the HTML5 export process is straightforward within the Godot editor:

### 4.1. Ensure Project Codebase Compliance

*   **Verification:** Confirm that all game logic, custom nodes, and extensions are implemented exclusively in GDScript or C++. Any lingering C# components will not be executable in a non-Mono HTML5 export.
*   **GDExtension/Custom Modules:** If using C++ via GDExtension or custom modules, ensure these are correctly compiled for the `web` platform. Godot's build system typically handles this during the export process if configured correctly.

### 4.2. Configure HTML5 Export Preset in Godot Editor

1.  **Open Project:** Launch your Godot Engine project.
2.  **Access Export Manager:** Go to `Project` -> `Export...` from the top menu bar.
3.  **Add HTML5 Preset:**
    *   Click the `Add...` button at the top of the Export dialog.
    *   Select `HTML5` from the dropdown list. A new "HTML5" preset will appear.
4.  **Configure Preset Settings:**
    *   **Export Path:** Choose the destination folder where the exported web files will be saved.
    *   **Custom Template:** (Optional) If your project uses a custom HTML5 shell, specify its path here. For most projects, the default template is sufficient.
    *   **Features:** Review and adjust features as needed. Common settings include:
        *   `WebGL2`: Ensure this is enabled for modern graphics.
        *   `Threads`: Enable if your C++ code or engine features utilize multi-threading (WASM multi-threading support varies across browsers).
    *   **Options:** Adjust other settings as required, such as:
        *   `GDScript Compile With Debug`/`Release`: Choose between debug and release versions of GDScript.
        *   `Head Include`: Add custom HTML `<head>` elements if necessary.
    *   **Resources:** Ensure `Export Texture PVRTC` is **disabled** unless you specifically target iOS Safari with PVRTC textures. Typically, this is not needed for generic HTML5.

### 4.3. Execute the Export

1.  **Select Preset:** In the Export dialog, ensure your configured `HTML5` preset is selected.
2.  **Export Project:** Click the `Export Project...` button at the bottom of the dialog.
3.  **Confirm Destination:** Select the target directory (as defined in `Export Path`) and click `Save` (or similar, depending on your OS).

## 5. Outcome

Upon successful export, Godot will generate a set of files in your chosen `Export Path`, typically including:
*   An `index.html` file (your game's entry point).
*   `.js` JavaScript files (engine logic, WASM loader).
*   `.wasm` WebAssembly module (compiled C++/GDScript engine and game code).
*   Asset files (`.pck`, images, sounds, etc.).

These files can then be hosted on any web server to make your Godot project playable in web browsers.

## 6. Conclusion

By adhering to native Godot development paradigms for project logic, developers can reliably leverage Godot's HTML5 export capabilities, effectively sidestepping the current complexities associated with C# web exports. This provides a clear path forward for web deployment for a broad range of Godot projects.

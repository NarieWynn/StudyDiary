## <span style="color: #00bfff;">Essential Include Libraries for QML-C++ Bridge</span>

### <span style="color: #00ffff;">1. #include &lt;QAbstractListModel&gt;</span>
* **The Problem:** QML (Frontend) and C++ (Backend) are two completely different worlds. QML cannot directly access RAM to read C++ data structures.
* **The Solution:** This library acts as a bridge. It forces C++ to encapsulate data into a standard format that QML can read, then ships this data directly to `ListView` or `GridView` on the UI.
* **When to use:** Whenever the frontend needs to display a list or a grid view, this library is $100\%$ mandatory.

### <span style="color: #00ffff;">2. #include &lt;QList&gt;</span>
* **The Concept:** A `struct AppInfo` is just a single box containing information about exactly **one** app (name, exec, icon). However, a computer contains hundreds of apps. 
* **The Solution:** We need to create a large contiguous memory block on the RAM to store hundreds of `AppInfo` boxes in one place.
* **Implementation:** `QList<AppInfo> m_apps;` (Works exactly like an `ArrayList` in Java).
* **The Connection:** `QAbstractListModel` will automatically pull data out from this `QList` and pass it to QML so the frontend can render the UI.
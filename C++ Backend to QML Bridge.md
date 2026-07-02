## <span style="color: #00bfff;">Standard Format of C++ to QML (The General Skeleton)</span>
## **1. '.h' file:**
To define what data and actions we want to send to QML, a C++ `.h` file acts as the official contract. Depending on the complexity of the data, the skeleton will morph, but the core foundation remains unchanged.

#"example.h"
```cpp
#pragma once // Must have to prevent the compiler from reading this file multiple times
#include <---> // Depending on the data structure, include the suitable Qt library

struct DataStructure {
    // Optional: Define a custom type if you need to package multiple fields together
    type field1;
    type field2;
};

class ClassName : public QtBaseClass // Inheriting a Qt Class is required to play with QML
{
    Q_OBJECT // <--- MANDATORY! Must have to activate QML communication

public:
	//MANDATORY CONSTRUCTOR: Bắt buộc phải có để cấp phát ô nhớ dưới RAM! 
	explicit ClassName(QObject *parent = nullptr); 
    // 1. Methods that QML can trigger directly from the UI
    Q_INVOKABLE type methodCanBeCalledByQML(); 

    // 2. Pure Virtual Overrides (Only appears if inheriting a specific Model Class)
    // If you inherit QAbstractListModel, the "Big Three" methods must be declared here.
    void SomeThing(); //custom method
private:
	//optional to store DataStructure
	
};
```

## **2. '.cpp' file:**
Everything we promise in .h file must define clearly in file.
"something.cpp"
```cpp
#include "example.h" //must declare .h file 
#include "---" // declare needed library (optional depend on your purpose)
ClassName::ClassName(QObject *parent) : QtBaseClass(parent) //
{ 
	SomeThing(); //write all the methods that need to be auto load as the time user open
}
type ClassName::methodCanBeCalledByQML() //method was declared in .h
{
	//logic here
}
void ClassName::SomeThing() //method was declared in .h
{
	//logic here
}
```

## 3. 'main.cpp' file:
The Bridge that glues the C++ Backend and QML Frontend together.

```cpp
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QQmlContext> // REQUIRED to use setContextProperty
#include "example.h"   // Include your class contract

int main(int argc, char *argv[]) {
    // 1. MANDATORY: Initialize the system application and engine
    QGuiApplication app(argc, argv); //communicate with kernel prepare hardware resource and RAM to render UI 
    QQmlApplicationEngine engine; //reader read code QML to render UI
---------------------------------------------------------------------
    // 2. OPTIONAL: Instantiate your backend data model under the RAM
    ClassName modelObject; // load logic ClassName and name modelObject this help conncent .h and .cpp to main.cpp 
---------------------------------------------------------------------
    // 3. MANDATORY FOR COMMUNICATION: Inject C++ pointer into QML context
    // QML will use the string "backendName" to access this C++ object
    engine.rootContext()->setContextProperty("backendName", &modelObject); //naming modelObject to backendName for QML to access 

    // 4. MANDATORY: Load the frontend QML entry point and start the event loop
    const QUrl url(u"qrc:/---/main.qml"); // Pure String literal, safe for all contexts
    engine.load(url); //purpose to access main.qml file to render the root window.
    
    return QGuiApplication::exec(); //infinite loop maintain app to active
}
```

## **4. '.qml' file:**
```cpp
import QtQuick //
Item{
	id: root
	Button{
		onClicked: backendName.methodCanBeCalledByQML() //alias that define in .cpp allow qml interact with method define in cpp
	} // only call method Q_INVOKABLE
}
```
## **5. 'main.qml' file**
```cpp
import QtQuick
import QtQuick.Controls
import "components" //other file qml that render inside the main window
Window { 
	id: root 
	width: 500 
	height: 600 
	visible: true 
	title: "Polaris Shell" 
	// 1. HOSTING CUSTOM COMPONENTS: called every .qml file that need to render inside the large window
	SearchBar { 
		id: searchInput 
		width: parent.width 
	} 
	AppLauncher { 
		id: launcherWidget 
		anchors.top: searchInput.bottom //
	} 
}

```
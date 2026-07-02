## Struct : 
- A block to declare variables instead of discrete variables. Struct makes them a unified set of variables (Data Package) where everything is **public** by default.
```cpp
struct AppInfo {
    QString name;
    QString exec;
private: 
	int something; //force a variable to private
};
```
## class:  
- inside class is private by default for each public: and private: will set the below code to public and private
```cpp
class SomeThing {
    int a;
    
public: 
	int b;

private:
	int c;
};
```

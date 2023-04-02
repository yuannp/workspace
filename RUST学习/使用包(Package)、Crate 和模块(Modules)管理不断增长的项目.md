## 1、模块系统(the module system)
-   **包**（_Packages_）： Cargo 的一个功能，它允许你构建、测试和分享 crate。
-   **Crates** ：一个模块的树形结构，它形成了库或二进制项目。
-   **模块**（_Modules_）和 **use**： 允许你控制作用域和路径的私有性。
-   **路径**（_path_）：一个命名例如结构体、函数或模块等项的方式

## 2、包与Crate
*Crate*是 Rust 在编译时最小的代码单位，Crate分为两种：二进制Crate与库Crate。二进制Crate可以编译成可执行程序，`main`函数是二进制Crate定义的函数入口；库Crate不可编译成可执行程序，但提供诸如函数之类的以供使用

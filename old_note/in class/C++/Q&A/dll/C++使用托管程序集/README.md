https://msdn.microsoft.com/zh-cn/library/ms235638(v=vs.100).aspx
创建引用类库的控制台应用程序
在“文件”菜单上指向“新建”，然后单击“项目”。
在“项目类型”窗格中，选择“Visual C++”下的“CLR”。
在“模板”窗格中，选择“CLR 控制台应用程序”。
在“名称”框中键入项目的名称，例如，MyExecRefsAssembly。 在“解决方案”旁边的列表中，选择“添入解决方案”以将新项目添加到包含类库的解决方案中。
单击“确定”创建项目。
在“解决方案资源管理器”中选中 MyExecRefsAssembly 项目，然后在“项目”菜单上，单击“属性”，为该项目禁用预编译头。 依次展开“配置属性”节点和“C/C++”节点，然后选择“预编译头”。 在“创建/使用预编译头”旁边的列表中，选择“不使用预编译头”。 单击“确定”保存这些更改。
在控制台应用程序中使用类库的功能
在您创建 CLR 控制台应用程序后，向导将生成一个仅向控制台写入“Hello World”的程序。 生成的源文件的名称与您在创建项目时为项目指定的名称相同。 在本示例中，名称为“MyExecRefsAssembly.cpp”。
若要使用在类库中创建的算术例程，必须引用类库。 为此，请在“解决方案资源管理器”中选择 MyExecRefsAssembly 项目，然后在“项目”菜单上，单击“属性”。 在“属性页”对话框中展开“通用属性”节点，选择“框架和引用”，然后单击“添加新引用”。 有关更多信息，请参见“<Projectname> 属性页”对话框 ->“通用属性”->“框架和引用”。
“添加引用”对话框列出了所有可以引用的库。 “.NET”选项卡列出了 .NET Framework 附带的库。 “COM”选项卡列出了计算机上的所有 COM 组件。 “项目”选项卡列出了当前解决方案中的所有项目，以及它们包含的所有库。 在“项目”选项卡上，选择“MathFuncsAssembly”，然后单击“确定”。
// carclr0428.cpp: 主项目文件。
#include "stdafx.h"
using namespace System;
int main(array<System::String ^> ^args)
{
    Console::WriteLine(L"Hello World");
	My0422car::CarDll c1;
	c1.CarConnect();
	c1.move(1);
	//printf('')
	
    return 0;
}


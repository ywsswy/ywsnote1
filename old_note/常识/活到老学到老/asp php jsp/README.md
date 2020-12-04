三种动态网页语言有
ASP(Active Server Pages),
JSP(JavaServer Pages),
PHP (Hypertext Preprocessor)。
    ASP全名Active Server Pages，是一个WEB服务器端的开发环境，
利用它可以产生和执行动态的、互动的、高性能的WEB服务应用程序。
ASP采用脚本语言VBScript（Java script）作为自己的开发语言。
PHP是一种跨平台的服务器端的嵌入式脚本语言。
它大量地借用C,Java和Perl语言的语法, 并耦合PHP自己的特性,
使WEB开发者能够快速地写出动态产生页面。它支持目前绝大多数数据库。
还有一点，PHP是完全免费的，不用花钱，你可以从PHP官方站点
(http: //www.php.net)自由下载。而且你可以不受限制地获得源码，
甚至可以从中加进你自己需要的特色。
JSP是Sun公司推出的新一代网站开发语言，Sun公司借助自己在Java上的不凡造诣，
将Java从Java应用程序和Java Applet之外，又有新的硕果，
就是JSP，Java Server Page。JSP可以在Serverlet和JavaBean的支持下，
完成功能强大的站点程序。
三者都提供在 HTML代码中混合某种程序代码、
由语言引擎解释执行程序代码的能力。
但JSP代码被编译成 Servlet并由Java虚拟机解释执行，
这种编译操作仅在对JSP页面的第一次请求时发生。
在ASP 、PHP、JSP环境下，HTML代码主要负责描述信息的显示样式，
而程序代码则用来描述处理逻辑。普通的 HTML页面只依赖于Web服务器，
而ASP 、PHP、JSP页面需要附加的语言引擎分析和执行程序代码。
程序代码的执行结果被重新嵌入到HTML代码中，然后一起发送给浏览器。
ASP 、PHP、JSP三者都是面向Web服务器的技术，
客户端浏览器不需要任何附加的软件支持。
技术特点:
    ASP:
    1. 使用VBScript 、 JScript等简单易懂的脚本语言，结合HTML代码，即可快速地完成网站的应用程序。
    2. 无须compile编译，容易编写，可在服务器端直接执行。
    3. 使用普通的文本编辑器，如Windows的记事本，即可进行编辑设计。
    4. 与浏览器无关(Browser Independence), 客户端只要使用可执行HTML码的浏览器，即可浏览Active Server Pages所设计的网页内容。Active ServerPages 所使用的脚本语言(VBScript 、 Jscript)均在WEB服务器端执行，客户端的浏览器不需要能够执行这些脚本语言。
    5.Active Server Pages能与任何ActiveX scripting语言兼容。除了可使用VB Script或JScript语言来设计外，还通过plug－in的方式，使用由第三方所提供的其它脚本语言，譬如REXX 、Perl 、Tcl等。脚本引擎是处理脚本程序的COM(Component Object Model) 对象。
    6. 可使用服务器端的脚本来产生客户端的脚本。
    7. ActiveX Server Components(ActiveX 服务器组件 )具有无限可扩充性。可以使用Visual Basic 、Java 、Visual C＋＋ 、COBOL等程序设计语言来编写你所需要的ActiveX Server Component 。
    PHP:
    1 数据库连接
    PHP可以编译成具有与许多数据库相连接的函数。PHP与MySQL是现在绝佳的群组合。你还可以自己编写外围的函数去间接存取数据库。通过这样的途径当你更换使用的数据库时，可以轻松地修改编码以适应这样的变化。PHPLIB就是最常用的可以提供一般事务需要的一系列基库。但PHP提供的数据库接口支持彼此不统一，比如对Oracle, MySQL，Sybase的接口，彼此都不一样。这也是PHP的一个弱点。
    JSP:
    1.将内容的产生和显示进行分离
    使用JSP技术，Web页面开发人员可以使用HTML或者XML标识来设计和格式化最终页面。使用JSP标识或者小脚本来产生页面上的动态内容。产生内容的逻辑被封装在标识和JavaBeans群组件中，并且捆绑在小脚本中，所有的脚本在服务器端执行。如果核心逻辑被封装在标识和Beans中，那么其它人，如Web管理人员和页面设计者，能够编辑和使用JSP页面，而不影响内容的产生。在服务器端，JSP引擎解释JSP标识，产生所请求的内容（例如，通过存取JavaBeans群组件，使用JDBC技术存取数据库），并且将结果以HTML（或者XML）页面的形式发送回浏览器。这有助于作者保护自己的代码，而又保证任何基于HTML的Web浏览器的完全可用性。
    2.强调可重用的群组件
    绝大多数JSP页面依赖于可重用且跨平台的组件（如：JavaBeans或者Enterprise JavaBeans）来执行应用程序所要求的更为复杂的处理。开发人员能够共享和交换执行普通操作的组件，或者使得这些组件为更多的使用者或者用户团体所使用。基于组件的方法加速了总体开发过程，并且使得各种群组织在他们现有的技能和优化结果的开发努力中得到平衡。
    3.采用标识简化页面开发
    Web页面开发人员不会都是熟悉脚本语言的程序设计人员。JavaServer Page技术封装了许多功能，这些功能是在易用的、与JSP相关的XML标识中进行动态内容产生所需要的。标准的JSP标识能够存取和实例化JavaBeans组件，设定或者检索群组件属性，下载Applet，以及执行用其它方法更难于编码和耗时的功能。通过开发定制化标识库，JSP技术是可以扩展的。今后，第三方开发人员和其它人员可以为常用功能建立自己的标识库。这使得Web页面开发人员能够使用熟悉的工具和如同标识一样的执行特定功能的构件来工作。 JSP技术很容易整合到多种应用体系结构中，以利用现存的工具和技巧，并且扩展到能够支持企业级的分布式应用。作为采用Java技术家族的一部分，以及Java 2EE的一个成员，JSP技术能够支持高度复杂的基于Web的应用。由于JSP页面的内置脚本语言是基于Java程序设计语言的，而且所有的JSP页面都被编译成为Java Servlet，JSP页面就具有Java技术的所有好处，包括健壮的存储管理和安全性。作为Java平台的一部分，JSP拥有Java程序设计语言“一次编写，各处执行”的特点。随着越来越多的供货商将JSP支持加入到他们的产品中，您可以使用自己所选择的服务器和工具，修改工具或服务器并不影响目前的应用。

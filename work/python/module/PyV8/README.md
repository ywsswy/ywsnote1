import PyV8
ctxt = PyV8.JSContext()
ctxt.enter()
# func = ctxt.eval('''(function(){console.log("hh");})''')
func = ctxt.eval('''(function(){document.write("nihao");})''')
func()
安装教程
https://www.jianshu.com/p/2da6f6ad01f0

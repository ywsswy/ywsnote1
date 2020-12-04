static double SavingsAccount::getTotal(){
	log1.w(YFL,YLOG_TRACE,"static double SavingAccount::getTotal(){\t\ttotal=%d",SavingsAccount::total);
	return SavingsAccount::total;
}
【error
 should not be used on member functions defined at file scope
static在声明中就好了，不应该在定义中使用

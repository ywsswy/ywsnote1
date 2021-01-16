## github.com/jinzhu/copier

copier.Copy(&employee, &user) //从后拷贝到前面



var (
	logger = tars.GetLogger("")
	commu  = tars.NewCommunicator() //global?
)
logger.Debugf("YWS: here")
logger.Infof("qrwRes %s", qrwRes) //2021-01-12 20:06:33.295|<file>:<func>:<line>|INFO|qrwRes


## github.com/smartystreets/goconvey/convey

单测的方法是在同文件夹创建个同package的<原文件名>_test.go的文件
import (
	"testing"
	"github.com/smartystreets/goconvey/convey"
)

func Test<被测函数名>(t *testing.T) {
	convey.Convey("描述", t, func() {
		//调用被测函数，得到结果
		convey.So(结果1, convey.ShouldBeNil)
		convey.So(结果2, convey.ShouldEqual, "")
	})

}


使用go test进行测试
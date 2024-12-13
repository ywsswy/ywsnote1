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


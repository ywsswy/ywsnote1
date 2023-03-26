```
// 相比encoding/json，支持优先解析成整数，但是uint64支持有问题。。
package common

import (
	"reflect"
	"unsafe"

	jsoniter "github.com/json-iterator/go"
	"github.com/modern-go/reflect2"
)

type wrapCodec struct {
	encodeFunc  func(ptr unsafe.Pointer, stream *jsoniter.Stream)
	isEmptyFunc func(ptr unsafe.Pointer) bool
	decodeFunc  func(ptr unsafe.Pointer, iter *jsoniter.Iterator)
}

func init() {
	jsoniter.RegisterExtension(&NumExtension{})
}
func (codec *wrapCodec) Encode(ptr unsafe.Pointer, stream *jsoniter.Stream) {
	codec.encodeFunc(ptr, stream)
}

func (codec *wrapCodec) IsEmpty(ptr unsafe.Pointer) bool {
	if codec.isEmptyFunc == nil {
		return false
	}

	return codec.isEmptyFunc(ptr)
}

func (codec *wrapCodec) Decode(ptr unsafe.Pointer, iter *jsoniter.Iterator) {
	codec.decodeFunc(ptr, iter)
}

// NumExtension jsoniter定制化扩展，继承全部默认实现
type NumExtension struct {
	jsoniter.DummyExtension
}

// CreateDecoder 创建 interface decoder 对于数值类型优先使用 Int64 解析， 如果无法解析，使用默认实现解析
func (e *NumExtension) CreateDecoder(typ reflect2.Type) jsoniter.ValDecoder {
	if typ == reflect2.TypeOfPtr((*interface{})(nil)).Elem() {
		return &wrapCodec{
			decodeFunc: func(ptr unsafe.Pointer, iter *jsoniter.Iterator) {
				exitingValue := *((*interface{})(ptr))
				if exitingValue != nil && reflect.TypeOf(exitingValue).Kind() == reflect.Ptr {
					iter.ReadVal(exitingValue)
					return
				}
				// TODO(wenshanyan) 考虑是uint64, 18446744073709551615 如何解析成功
				if iter.WhatIsNext() == jsoniter.NumberValue {
					v := iter.ReadAny()
					vInt64 := v.ToInt64()
					if v.LastError() == nil {
						*((*interface{})(ptr)) = vInt64
						return
					}
					*((*interface{})(ptr)) = v.ToFloat64()
				} else {
					*((*interface{})(ptr)) = iter.Read()
				}
			},
		}
	}
	return nil
}
```
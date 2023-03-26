数值序列化（-> []byte）
bufData[key] = make([]byte, 4)
binary.LittleEndian.PutUint32(bufData[key], uint32(642314))


序列化转数值（[]byte ->）
binary.LittleEndian.Uint16(bb)
math.Float64frombits(binary.LittleEndian.Uint64(protoData))
启用需要在文件中指定

option cc_enable_arenas = true;

启用前
```
inline ::serving::DebugInfo* SmartBoxResponse::release_debug_info() {
  // @@protoc_insertion_point(field_release:serving.SmartBoxResponse.debug_info)
  
  ::serving::DebugInfo* temp = debug_info_;
  debug_info_ = NULL;
  return temp;
}
```
启用后
```
inline ::serving::DebugInfo* SmartBoxResponse::release_debug_info() {
  // @@protoc_insertion_point(field_release:serving.SmartBoxResponse.debug_info)
  
  ::serving::DebugInfo* temp = debug_info_;
  if (GetArenaNoVirtual() != NULL) {
    temp = ::google::protobuf::internal::DuplicateIfNonNull(temp);
  }
  debug_info_ = NULL;
  return temp;
}
```
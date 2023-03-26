// 这个库不支持前视断言和后视断言

// 只判断是否匹配：
matched, err := regexp.MatchString("pattern_string", "source_string") // nil 并且true表示匹配了

// 获取匹配的部分：
  str := "1: 24: error: invalid"
  pattern := "1: (\\d+):"

  re := regexp.MustCompile(pattern)
  s := re.FindStringSubmatch(str)

  if len(s) == 2 { 
    fmt.Printf("找到的部分：%s\n", s[1])  // s[0]是整个匹配的部分，s[1]是第一个匹配的捕获组
  } else {
    fmt.Printf("未找到匹配的部分\n")
  }
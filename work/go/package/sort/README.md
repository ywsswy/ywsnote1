import "sort"

// 基础类型
ids := []int{3,1,2}
sort.Ints(ids)
sort.Strings(result)  // 是原地操作，不用取返回值

// 自定义类型，需要传递less方法，非stable排序，i在前j在后，return true时，保持i和j的顺序
vs := []XX{
           {Name: "c"},
           {Name: "a"},
           {Name: "b"},
        }
  sort.Slice(vs, func(i, j int) bool {
        return vs[i].Name < vs[j].Name
      })
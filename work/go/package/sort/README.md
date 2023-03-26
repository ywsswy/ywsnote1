import "sort"

// 基础类型
ids := []int{3,1,2}
sort.Ints(ids)

// 自定义类型，需要传递less方法，非stable排序
vs := []XX{
           {Name: "c"},
           {Name: "a"},
           {Name: "b"},
        }
  sort.Slice(vs, func(i, j int) bool {
        return vs[i].Name < vs[j].Name
      })
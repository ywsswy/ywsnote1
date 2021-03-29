有error要一层层上报的话，最好return fmt.Errorf("msg|%w", err)


最上层打印的时候可以用fmt.Printf("%v\n", err)
// list的访问方法

config_setting_t *setting;
setting = config_lookup(&cfg, "inventory.IT_book");
int count = config_setting_length(setting);
for(i = 0; i < count; ++i)
        {
            config_setting_t *IT_book = config_setting_get_elem(setting, i);
            config_setting_name(IT_book); //获取这个object的名字
            config_setting_index(IT_book); //这个objext在list中的index(0s)
            config_setting_length(IT_book);//这个objext有多少个元素
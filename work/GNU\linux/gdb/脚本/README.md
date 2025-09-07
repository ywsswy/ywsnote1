# debug_script.gdb

set pagination off
set confirm off

attach 857

b aa.cpp:55

continue

echo "let me see is_auto"
p is_auto

delete
detach
quit

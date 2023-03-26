# 脚本中的内容写入文件
cat << EOF >file
content
EOF


# 这种是把file1和脚本内容拼接写到file2
cat file1 - <<EOF >file2
content
EOF
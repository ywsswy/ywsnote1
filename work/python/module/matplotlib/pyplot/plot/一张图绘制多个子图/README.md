_, axs = plt.subplots(1, 2, figsize=(12, 6)) # 1行2列的图，宽12寸，高6寸；
axs[0].plot(time, data)  # 如果是2行2列，这里就是axs[x][y]
axs[1].plot(time2, data2)

plt.gcf().autofmt_xdate()
plt.tight_layout()
plt.show()
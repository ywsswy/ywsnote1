function sleep(time){
    return new Promise((resolve) => setTimeout(resolve, time));
}
for (i = 0; i < 2; ++i) {
    console.log(i);
    await sleep(1000);  // 一秒钟
}
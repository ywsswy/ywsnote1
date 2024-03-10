for(int i = 0;i<10;i++){
    for(int j = 0;j<10;j++){
        res.data[i].data[j] += (
                this->data[i].data[0] * y.data[0].data[j]+
                this->data[i].data[1] * y.data[1].data[j]+
                this->data[i].data[2] * y.data[2].data[j]+
                this->data[i].data[3] * y.data[3].data[j]+
                this->data[i].data[4] * y.data[4].data[j]+
                this->data[i].data[5] * y.data[5].data[j]+
                this->data[i].data[6] * y.data[6].data[j]+
                this->data[i].data[7] * y.data[7].data[j]+
                this->data[i].data[8] * y.data[8].data[j]+
                this->data[i].data[9] * y.data[9].data[j]
                );
    }
}
//emmmmmmmmmmmmmmm循环展开就是，原来你的循环是原子性的，改成块的，会减少for循环带来的开销，比如上面，如果写3层for循环就会慢很多。。。


    Xapian::MSet mset = enquire.get_mset(offset, pagesize); //获取 [offset, offset + pagesize]之间的结果
    for (Xapian::MSetIterator m = mset.begin(); m != mset.end(); ++m) {
    Xapian::docid did = *m; 
    //m.get_rank() //排名，0s
    const string & data = m.get_document().get_data();
    std::cout << "YWS all data: " << data << endl;
    //get_field(data, loc) << endl; //loc: data中的数据位置（这是指data是\n分割的情况下）/code/c++/support.cc
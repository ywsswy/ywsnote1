汉字太少时乱码，多了反而正常
	sprintf(rl.m_cInfo, "%s%ld ms\n小东东\n", ctime(&(aTime.time)), aTime.millitm);
乱码
	sprintf(rl.m_cInfo, "%s%ld ms\n小东东四五\n", ctime(&(aTime.time)), aTime.millitm);
正常


try {
			String gbk = new String(decodedData.getBytes("gbk"), "utf-8");
			System.out.println("gbk如下："+gbk);
		} catch (UnsupportedEncodingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} 

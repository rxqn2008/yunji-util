  文档地址：
	https://api.yunjiweidian.com/showdoc/index.php?s=/57&page_id=781
	
	
	//云集POP业务使用方式
	$yunji = new Yunji('云集POP平台提供的app_key' , '云集POP平台提供的secret' , '云集POP平台提供的customerid');	
	$yunji->method = 'order.list.get';  //动态设置方法
	$param = array();
	$param['query_type']=0; //1:修改时间 0:创建时间
	$param['start_modified']= date('Y-m-d H:i:s',strtotime("-1 day"));
	$param['end_modified']= date('Y-m-d H:i:s');
	$param['status']=40;  //待发货
	$param['page_no']=1;
	$param['page_size']=50;	
	$yunji->body=json_encode($param);
	$rs=$yunji->request();

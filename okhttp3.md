class UnitTestTool {

	private static final MediaType contentType = MediaType.parse("application/json;charset=utf-8");
	
	public static String post(String url,Map<String,String> head,Map<String,Object> params){
	
		FormBody.Builder build = new FormBody.Builder();
		Iterator<Map.Entry<String,Object>> iterator = params.entrySet.iterator();
		while(iterator.hasNext()){
		Map.Entry<String,Object> entry = iterator.next();
		build.add(entry.getKey(),entry.getValue().toString());
		}
		RequestBody requestBody = build.build();
		Request request = new okhttp3.Request.Builder().url(url).headers(headeers.of(head)).post(requestBody).build();
		Request client = new okhttp3.Request.Builder().connectTimeout(10L,TimeUnit.SECODS).readTimeout(10L,TimeUnit.SECODS).build();
		String str = "";
		try{
		 	Response response = client.newCall(request).execute();
			str = response.body().string();
			
		}catch(IOExcepption e){
			throw JFrameException.apiCallError(request.method,str,e);
		}
	}
	
	public static String postjson(String url,Map<String,String> head,String json){
	
		RequestBody requestBody = RequestBody.create(contentType,json);
		Request request = new okhttp3.Request.Builder().url(url).headers(headeers.of(head)).post(requestBody).build();
		Request client = new okhttp3.Request.Builder().connectTimeout(10L,TimeUnit.SECODS).readTimeout(10L,TimeUnit.SECODS).build();
		String str = "";
		try{
		 	Response response = client.newCall(request).execute();
			str = response.body().string();
			
		}catch(IOExcepption e){
			e.printStackTrace();
			return "";
		}
	}
}

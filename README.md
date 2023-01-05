# A certain tower Remove the .so file of the mobile phone number bound to the login and build the cloud by yourself!
  
  
Delete the official .so file of Pagoda: libAuth.aarch64.so libAuth.glibc-2.14.x86_64.so libAuth.loongarch64.so libAuth.x86-64.so libAuth.x86.so pluginAuth.cpython-37m-aarch64-linux- gnu.so pluginAuth.cpython-37m-i386-linux-gnu.so pluginAuth.cpython-37m-loongarch64-linux-gnu.so pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.cpython-310-aarch64- linux-gnu.so pluginAuth.so
  
Replace the downloaded four files panelPlugin.py panelSSL.py pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so under the /www/server/panel/class directory
  
pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so is not open source because it has been compiled to prevent flooding. If you are worried about safety, you can close this GitHub project. Those who know the code can drag it to IDA to view the code by themselves!
  
# principle
1. In line 1042 of panelSSL.py, rtmp = public.httpPost('http://www.example.com/api'+'/GetToken',pdata) is replaced with your fake login token interface
2. Line 38 of panelPlugin.py #_check_url=__api_root_url+'/panel/get_soft_list_status' #Comment out the detection of cloud status (the uploaded file has been commented out)
3. Line 1304 of panelPlugin.py #public.run_thread(self.is_verify_unbinding,args=(get,)) #Every time the list is loaded, the account binding will be detected! (The uploaded file has been commented out)
### It’s that simple, nothing, the main thing is the pluginAuth file, the encryption list, and the encryption plug-in (because the plug-in installation is not needed, it is directly uploaded and the decrypted plug-in has been downloaded, so this function is not written)
  
# deployment method
1. First install a pagoda panel and then install the environment, then create a site - example.com / www.example.com (this domain name must be filled in, the domain name in the pluginAuth.so list for hosts redirection is this)
2. The example.com / www.example.com site 301 redirects to its own domain name
3. Create a site with your own domain name - bind your own domain name For example: domian.com / www.domian.com import pseudo-static as follows:
```
if (!-d $request_filename){
set $rule_0 1$rule_0;
}
if (!-f $request_filename){
set $rule_0 2$rule_0;
}
if ($rule_0 = "21"){
   # list
rewrite ^/(panel/get_plugin_list)$ /panel/get_plugin_list.json?s=/$1 last;
	# Log in
rewrite ^/(api/GetToken)$ /api/token.json?s=/$1 last;
rewrite ^/(.*)$ /index.php/$1;
}

```
4. Create a panel and api file in the directory under your own domain name site, put the get_plugin_list.json file in the panel file, put the token.json file in the api file, and then visit http://domian.com/panel/get_plugin_list / http:// domian.com/api/GetToken See if you can access it! (These two files are downloaded in the project data directory)
  
5. Make sure the above operations are correct, and then delete all the official so under the /www/server/panel/class directory in the pagoda panel: libAuth.aarch64.so libAuth.glibc-2.14.x86_64.so libAuth.loongarch64.so libAuth .x86-64.so libAuth.x86.so pluginAuth.cpython-37m-aarch64-linux-gnu.so pluginAuth.cpython-37m-i386-linux-gnu.so pluginAuth.cpython-37m-loongarch64-linux-gnu.so pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.cpython-310-aarch64-linux-gnu.so pluginAuth.so
  
6. Then download the four files panelPlugin.py panelSSL.py pluginAuth.cpython-37m-x86_64-linux-gnu.so pluginAuth.so in the project and put them in line 1042 in panelSSL.py rtmp = public.httpPost('http ://www.example.com/api'+'/GetToken',pdata) is replaced by your pseudo login token interface. For example: rtmp = public.httpPost('http://www.domian.com/api'+' /GetToken',pdata)
7. Modify the etc/hosts guide to:
```
Your server IP example.com
Your server IP www.example.com
```
8. Finally, just restart the panel, and then log in to the panel and enter the mobile phone number and password to bind the account!
  
### If you want to modify the expiration time, search for 1893513599 in the list file and replace it with the new Unix timestamp in batches

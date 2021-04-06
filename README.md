# nginx-dynamic-modules
Nginx dynamic module collections

## How to Compile Modules
- [x] Go to nginx folder and choose version.

If doesn't exist you can download (https://github.com/nginx/nginx/releases) and extract it.
```conf
$ cd /folder/location/nginx-dynamic-modules/nginx/1.19.8
```

- [x] Copy module file from the chosen nginx version on src/http/modules to new folder
```conf
$ cp src/http/modules/ngx_http_image_filter_module.c /folder/location/nginx-dynamic-modules/image-filter/1.19.8/ngx_http_image_filter_module.c
```

- [x] Add `config file`
```conf
$ nano /folder/location/nginx-dynamic-modules/image-filter/1.19.8/config
```

- [x] Change `config file` based on your chosen module name:
```conf
ngx_addon_name=ngx_http_image_filter_module #change this module name

if test -n "$ngx_module_link"; then
	ngx_module_type=HTTP
	ngx_module_name=ngx_http_image_filter_module #change this module name
	ngx_module_srcs="$ngx_addon_dir/ngx_http_image_filter_module.c" #change this module file
	. auto/module
else
	HTTP_MODULES="$HTTP_MODULES ngx_http_image_filter_module" #change this module name
	NGX_ADDON_SRCS="$NGX_ADDON_SRCS $ngx_addon_dir/ngx_http_image_filter_module.c" #change this module file
fi
```

- [x] Now, lets compile it!
```conf
$ ./auto/configure --with-compat --add-dynamic-module=/folder/location/nginx-dynamic-modules/image-filter/1.19.8
$ make modules
```

- [x] Copy compiled module with `.so extension` in objs folder like `ngx_http_image_filter_module.so` to destination folder

- [x] Add the `load_module` directive in the top‑level (main) context of your nginx.conf configuration file (not within the http or stream context)
```conf
load_module /folder/location/ngx_http_image_filter_module.so;
```

- [x] Restart nginx server. Done!

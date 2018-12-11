    function opts(action, args) {
        var options$$1 = assign({}, action), params = {}, body;
        switch (args.length) {
            case 2:
                params = args[0];
                body = args[1];
                break;
            case 1:
                if (/^(POST|PUT|PATCH)$/i.test(options$$1.method)) {
                    body = args[0];
                } else {
                    params = args[0];
                }
                break;
            case 0:
                break;
            default:
                throw 'Expected up to 2 arguments [params, body], got ' + args.length + ' arguments';
        }
    
        options$$1.body = body;
    options$$1.params = assign({}, options$$1.params, params);
    return options$$1;
    }

##### resource请求传参，最多2个，第一个是以query形式，第二个是以body形式，如果只传一个，并且type=“POST|PUT|PATCH”，就会以body方式，除外就是query方式.
##### 接口参数如果是以query形式，并且是post或者put的时候，要带上第二个参数，比如加个空的对象



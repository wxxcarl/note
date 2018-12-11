
# Promise遍历目录下文件
````
const  fs  =  require('fs');
const  path  =  require('path');

function readDirAsync(dir) {
	return new Promise((resolve, reject) => {
		fs.readdir(dir, (err, list) => {
			if (err) {
				reject(err);
			}
			resolve(list);
		});
	});
}

function  lstatAsync(dir) {
	return  new  Promise((resolve, reject) => {
		fs.stat(dir, (err, stats) => {
			if (err) {
				reject(err);
			}
			resolve(stats);
		})
	});
}
function walk(dir) {
    return readDirAsync(dir).then(files => {
        return Promise.all(files.map(f => {
            let file = path.join(dir, f);
            return lstatAsync(file).then(stat => {
                if (stat.isDirectory()) {
                    return walk(file);
                } else {
                    return [file];
                }
            });
        }));
    }).then(files => {
        return files.reduce((pre, cur) => pre.concat(cur));
    }); 
}
````

    walk("~/home").then(x => console.log(x));
# Migrate-Cubird-To-Mysql

Migrate cubird to mysql.

## How to Install

NPM
```bash
npm install migrate-cubrid-to-mysql
```

GIT
```bash
git clone https://github.com/iamdenny/migrate-cubird-to-mysql.git
```

## How to use

Make tables schema on Mysql same as tables schema on Cubrid first.

app.js
```js
var Mcm = require('migrate-cubird-to-mysql');
var htCubrid = {
    sHostname : '127.0.0.1',
    sUser : 'username',
    sPassword : 'password',
    nPort : 1527,
    sDatabase : 'database'
};
var htMysql = {
    sHostname : '127.0.0.1',
    sUser : 'username',
    sPassword : 'password',
    nPort : 3306,
    sDatabase : 'database',
    bDebug : false
};
var oMcm = new Mcm(htCubrid, htMysql);

oMcm.once('connected', function(){
	// first arg : oracle query
	// second arg : mysql table name
	// third arg : truncate(delete data from mysql table)
	// forth arg : callback
    oMcm.migrateByQuery("SELECT * FROM user", 'user', true, function(htResult){
          console.log('First migration is done', htResult);
      });

    oMcm.migrateByQuery("SELECT * FROM group", 'group', true, function(htResult){
          console.log('Second migration is done', htResult);
      });
    
    oMcm.migrateByQuery("SELECT * FROM article", 'article', true, function(htResult){
          console.log('Thrid migration is done', htResult);
      });      
}).once('done', function(htResult){
    console.log('All done', htResult);
    process.exit(0);
});
```

Execute
```bash
node app.js
```
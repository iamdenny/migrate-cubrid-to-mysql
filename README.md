# Migrate-Cubird-To-Mysql

Mirate cubird to mysql.

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
var Mom = require('migrate-cubird-to-mysql');
var htOracle = {
    sHostname : '127.0.0.1',
    sUser : 'username',
    sPassword : 'password',
    nPort : 1527,
    sDatabase : 'database or sid'
};
var htMysql = {
    sHostname : '127.0.0.1',
    sUser : 'username',
    sPassword : 'password',
    nPort : 3306,
    sDatabase : 'database',
    bDebug : false
};
var oMom = new Mom(htOracle, htMysql);

oMom.on('connected', function(){
	// first arg : oracle query
	// second arg : mysql table name
	// third arg : truncate(delete data from mysql table)
	// forth arg : callback
    oMom.migrateByQuery("SELECT username, nickname FROM user",
      'user', true, function(htResult){
          console.log('First migration is done', htResult);
      });

    oMom.migrateByQuery("SELECT group_id, group_name FROM group",
      'group', true, function(htResult){
          console.log('Second migration is done', htResult);
      });
    
    oMom.migrateByQuery("SELECT article_idx, title FROM article",
      'article', true, function(htResult){
          console.log('Thrid migration is done', htResult);
      });      
}).on('done', function(htResult){
    console.log('All done', htResult);
    process.exit(0);
});
```

Execute
```bash
node app.js
```
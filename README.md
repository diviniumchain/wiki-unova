# wiki-dev

**ngnix file for ref**
/ngnix.conf

**heroku setup in nodejs typescript**
- create .procfile in source and add `web: npm run heroku-start`
- in package.json add a new line in scripts for build and run 
`"heroku-start": "npm run build-ts && concurrently -k -p \"[{name}]\" -n \"TypeScript,Node\" -c \"yellow.bold,cyan.bold,green.bold\" \"tsc -w\" \"nodemon dist/server.js\"",`

- confugure "process.env.PORT" is considered by heroku and "port" is local  for `app.listen(process.env.PORT || port);`

**local check heroku commands***
-  `heroku logs --tail --app unova-consumerapp` (check logs)
-  `heroku run bash -a unova-consumerapp` ( bash into heroku)

# Problem
**Problem heroku free dynamo bug when building or running tsc/npm run build**
![herokuIssues](/images/heroku-heap-error-for-typescript-node-build.png)

# Solution 

- only configure in package.json add a new line in scripts for `node dist/server.js`

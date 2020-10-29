# wiki-dev

**ngnix file for ref**
/ngnix.conf

**heroku setup in nodejs typescript**
- create .procfile in source and add `web: npm run heroku-start`
- in package.json add a new line in scripts for build and run 
`"heroku-start": "npm run build-ts && concurrently -k -p \"[{name}]\" -n \"TypeScript,Node\" -c \"yellow.bold,cyan.bold,green.bold\" \"tsc -w\" \"nodemon dist/server.js\"",`

- confugure "process.env.PORT" is considered by heroku and "port" is local  for `app.listen(process.env.PORT || port);`

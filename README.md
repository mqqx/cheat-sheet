# cheat sheet

collection of useful commands and hints

## contents

- [contents](#contents)
- [curl](#curl)
- [git](#git)
- [helpful links](#helpful links)
- [java](#java)
- [keystore](#keystore)
- [kubectl](#kubectl)
- [maven](#maven)
- [sql](#sql)

## curl

### curl url with authorization token

    curl 'https://domain.dev' \
        -H 'Authorization: Bearer 6d3918a1-bd8e-4e0b-9141-d0b347740906'

## git

### change repository specific author

copy the following to `%PROJECT_BASE%` in terminal
    
    cat >> .git/config <<- EOM
    [user]
            name = mqqx
            email = repository@mqqx.dev
    EOM
    cat .git/config

## helpful links

### app development

* building modern web apps / saas [The Twelve-Factor App](https://12factor.net)
* easy and fast deployment with [netlify](https://www.netlify.com/)

## java

### useful libs

* Apache Batik - svg manipulation
* Apache Commons / Google Guava - useful utility methods (`StringUtils`, `Strings`, `Sets`, ...)
* Apache Tika - content type detection (`new Tika().detect(base64ByteArray)`)

## keystore

### list content

    keytool -list -keystore KEYSTORE.jks
    
### remove alias from keystore

    keytool -delete -alias ALIAS_NAME -list -keystore KEYSTORE.jks
    
### add cert with alias to keystore

    keytool -import -trustcacerts -alias ALIAS_NAME -file CERT.cert -keystore KEYSTORE.jks
    
## kubectl

### list all deployments

    kubectl get deployments --all-namespaces
    
### delete deployment
    
    kubectl delete -n NAMESPACE deployment DEPLOYMENT

## maven

### update versions to latest releases 

    mvn versions:use-latest-releases -DexcludeReactor=false -DgenerateBackupPoms=false

only specific package

    mvn versions:use-latest-releases -Dincludes=package.to.include -DexcludeReactor=false -DgenerateBackupPoms=false

additional properties: https://www.mojohaus.org/versions-maven-plugin/use-reactor-mojo.html

### build with profile

    mvn clean install -P profile

### execute enforcer

    mvn validate

### execute google java style guide

    mvn git-code-format:format-code && mvn git-code-format:validate-code-format

## sql

### generate test data

generate test data in postgresql

    INSERT INTO item (id, name)
    SELECT x.id, 'item #' || x.id
      FROM generate_series(1,100000) AS x(id);




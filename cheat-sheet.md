# cheat sheet

## contents

- [contents](#contents)
- [curl](#curl)
- [git](#git)
- [kubectl](#kubectl)
- [maven](#maven)

## curl

### curl url with authorization token

    curl 'https://domain.dev' \
        -H 'Authorization: Bearer 6d3918a1-bd8e-4e0b-9141-d0b347740906'

## git

### change repository specific author

add the following to `%PROJECT_BASE%/.git/config` file

    
    [user]
          name = username
          email = username@domain.com

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

## kubectl

### list all deployments

    kubectl get deployments --all-namespaces
    
### delete deployment
    
    kubectl delete -n NAMESPACE deployment DEPLOYMENT

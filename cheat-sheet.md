# cheat sheet

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

## git

### change repository specific author

add the following to `%PROJECT_BASE%/.git/config` file

    
    [user]
          name = username
          email = username@domain.com

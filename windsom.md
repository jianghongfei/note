#### Run postgres in docker
``` bash
docker run --name windsom.db -p 5432:5432 -e POSTGRES_PASSWORD=masterkey -e POSTGRES_USER=windsom -d postgres:alpine
```

#### Connect to postgres running in docker
```
docker run -it --rm --link windsom.db postgres:alpine psql -h windsom.db -U windsom -d windsom
```
Or
```
docker run -it --rm --link windsom.db:postgres postgres:alpine psql -h postgres -U windsom -d windsom
```

#### Docker debain volume
```
docker run --interactive --tty --rm --hostname devbox --link windsom.db:database --name devbox --volume ~/Projects/c:/c debian
```

#### Common psql command
##### options

`-c command`  
`--command=command`  
Specifies that psql is to execute the given command string, *command*.

`-f filename`  
`--file=filename`  
Read commands from the file *filename*, rather than stand input.

`-l`  
`--list` 
List all availabel databases

`-o filename`  
`--output=filename`  
Put all query output into file *filename*.

##### Meta-Commands
`\c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}`  Connect to new database
xn
`\i FILE`  
execute commands from file

`\set [NAME [VALUE]]`    set internal variable, or list all if no parameters  
`\unset NAME`            unset (delete) internal variable

`\ds[S+] [PATTERN]`      list sequences  
`\dt[S+] [PATTERN]`      list tables  
`\dT[S+] [PATTERN]`      list data types  
`\du[S+] [PATTERN]`      list roles  
`\dv[S+] [PATTERN]`      list views  
`\dE[S+] [PATTERN]`      list foreign tables  
`\dx[+]  [PATTERN]`      list extensions  
`\dy     [PATTERN]`      list event triggers  

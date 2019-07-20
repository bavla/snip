# Snippets in R

## Generating node numbers for Pajek

```
> out <- file("num.txt","w"); n <- 10000
> s <- vector('character',n); for(i in 1:n) s[i] <- paste(format(i,width=6),"")
> cat(paste("*vertices ",n),s,sep="\n",file=out)
> close(out)
```

## Saving R data frame (network) as a Pajek NET file

It is assumed that the rownames are not empty.
```
df2pajek <- function(df,netfile){
  n <- nrow(df); nams <- rownames(df)
  net <- file(netfile,"w"); cat("*vertices ",n,"\n",file=net)
  for(v in 1:n) cat(v,' "',nams[v],'"\n',sep='',file=net)
  cat('*arcs\n',file=net)
  for(u in 1:n) for(v in 1:n) if(df[u,v] != 0) 
    cat(u," ",v," ",df[u,v],'\n',sep='',file=net)
  close(net)
}
```

and the version for two-mode networks:

```
df2Pajek <- function(df,netfile){
  n <- nrow(df); namr <- rownames(df)
  m <- ncol(df); namc <- colnames(df)
  net <- file(netfile,"w")
  cat("*vertices ",n+m," ",n,"\n",file=net) 
  for(u in 1:n) cat(u,' "',namr[u],'"\n',sep='',file=net)
  for(v in 1:m) cat(n+v,' "',namc[v],'"\n',sep='',file=net)
  cat('*arcs\n',file=net)
  for(u in 1:n) for(v in 1:m) if(df[u,v] != 0) 
    cat(u," ",n+v," ",df[u,v],'\n',sep='',file=net)
  close(net)
}
```

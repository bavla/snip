# Snippets in R

* [Rnet - networks in R](https://github.com/bavla/Rnet/tree/master/R)

## Generating node numbers for Pajek

```
> out <- file("num.txt","w"); n <- 10000
> s <- vector('character',n); for(i in 1:n) s[i] <- paste(format(i,width=6),"")
> cat(paste("*vertices ",n),s,sep="\n",file=out)
> close(out)
```
## Saving R partition or vector as a Pajek file

Vector  M  contains a partition and vector  mode  its classes (legend)
```
clu <- file("Mode.clu","w")
cat("%",file=clu); n <- length(M)
for(i in 1:length(mode)) cat(" ",i,mode[i],file=clu)
cat("\n*vertices ",n,"\n",file=clu)
for(v in 1:n) cat(M[v],"\n",file=clu)
close(clu) 
```
Vector P contains a numerical property (attribute)
```
vec <- file("P.vec","w"); n <- length(P)
cat("*vertices ",n,"\n",file=clu) 
for(v in 1:n) cat(P[v],"\n",file=clu)
close(clu) 
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

## Saving igraph network in Pajek NET file

[wiki](http://vladowiki.fmf.uni-lj.si/doku.php?id=ru:hse:rnet18:crnet1#saving_igraph_network_as_pajek_project_file)

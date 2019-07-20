# Snippets in R

## Generating node numbers for Pajek

```
> out <- file("num.txt","w"); n <- 10000
> s <- vector('character',n); for(i in 1:n) s[i] <- paste(format(i,width=6),"")
> cat(paste("*vertices ",n),s,sep="\n",file=out)
> close(out)
```

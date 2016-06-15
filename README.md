# testing - neuroblastoma 

# get and format the data
gse <- getGEO("GSE16476")
eset <- GSE[[1]]
e <- exprs(eset)

exprs(gse[1])
gse[[1]]
pData(gse[[1]])
myTargets <- pData(gse[[1]])
myExprs <- exprs(pData(gse[[1]])
myExprs[1:10,1:10];
myTargets[1:10,]

dim(myTargets)
dim(myExprs)



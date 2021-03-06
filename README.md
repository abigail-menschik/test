# testing - neuroblastoma 

# get and format the data

gse <- getGEO("GSE16476")
eset <- GSE[[1]]
e <- exprs(eset)
myTargets <- pData(gse[[1]])

# work

exprs(gse[1])
gse[[1]]
pData(gse[[1]])
myTargets <- pData(gse[[1]])
myExprs <- exprs(pData(gse[[1]])
myExprs[1:10,1:10];
myTargets[1:10,]

# e and myExprs are the same matrix, show each gene and the expression by sample

dim(e)
dim(myTargets)

# now to make and label the design matrix

myDesign <- model.matrix(~ 0+factor(c(1,1,1,1,1,1,1,1,1,1,1,2,2,2,2,2,2,2,2,2,2,2,3,3,3,3,3,3,3,3,3,3,3,4,4,4,4,4,4,4,4,4,4,4,5,5,5,5,5,5,5,5,5,5,5,6,6,6,6,6,6,6,6,6,6,6,7,7,7,7,7,7,7,7,7,7,7,8,8,8,8,8,8,8,8,8,8,8)))
colnames(myDesign) <- c("group1", "group2", "group3","group4","group5","group6","group7","group8")
View(myDesign)

# brute force way
# each unique number defines a group, and the groups will then be comared to each other
# in this case there are 11 groups of 8, since there are 88 samples
# now to make the linear model

fit <- lmFit(eset, myDesign)

# now to make the contrast matrix and compare the groups

contrast.matrix <- makeContrasts(group2-group1, group3-group1, group3-group2, group4-group1, group4-group2, group4-group3, group5-group1, group5-group2, group5-group3, group5-group4, group6-group1, group6-group2, group6-group3, group6-group4, group6-group5, group7-group1, group7-group2, group7-group3, group7-group4, group7-group5, group7-group6, group8-group1, group8-group2, group8-group3, group8-group4, group8-group5, group8-group6, group8-group7, levels=myDesign)
View(contrast.matrix)
fit2 <- contrasts.fit(fit, contrast.matrix)
fit2 <- eBayes(fit2)

# run differential expression

tt<-topTable(fit2, coef=1, number = Inf)
View(tt)

# find significant figures

tt$adj.P.Val<=0.05

# assign and look at outcomes of each hypothesis test

results <- decideTests(fit2)
vennDiagram(results)

# Quantile-Regression-with-R
Produced an ordinary least square regression for 35th, 60th, 95th  percentile, and ANOVA for 35th and 95th percentile.

## Quantile regression
```{r}
### reading the medical expense dataset. we assume  
medical.expenses<-read.csv("quantile_health.csv", header = T, sep = ",")
attach(medical.expenses)
### By defining the dependent and independent variables.
dependent<-cbind(totexp)
independent<-cbind(suppins, totchr, age, female, white)

### Running ols regression 
regression<-lm(dependent~independent, data = medical.expenses)
summary(regression)

### Quantile regression for 35th percentile
quant35<-rq(dependent~independent, data = medical.expenses, tau = 0.35)
summary(quant35)

### Quantile regression for 60th percentile
quant60<-rq(dependent~independent, data = medical.expenses, tau = 0.60)
summary(quant60)

### Quantile regression for 95th percentile
quant95<-rq(dependent~independent, data = medical.expenses, tau = 0.95)
summary(quant95)


### Running all the regression above simultaneously
quant.simul<- rq(dependent~independent, data = medical.expenses, tau = c(0.35,0.60,0.95))
summary(quant.simul)

### Anova for the 35th and the 95th percentile
anova(quant35, quant95)

### Plotting all the data
all.quant<-rq(dependent~independent, tau = seq(0.05,0.90, by=0.10),data = medical.expenses) 
quant.plot<-summary(all.quant)
plot(quant.plot)

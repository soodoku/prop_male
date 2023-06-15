Male Children
================
Gaurav
June 10, 2021

# Stopping Rule = 5 kids or if I have 1 male baby

``` r
set.seed(31415)

n_samples = 10^4
prop_male = rep(NA, n_samples)
n_kids = rep(0, n_samples)
max_kids = 5 # pref. for max family size
# stopping rule is if i have 1 male kid
n_male = rep(0, n_samples)

for (i in 1:n_samples){

    children = c()

    kids = 1
    while (kids <= max_kids){
        child = rbinom(1, 1, .5)
        children <- c(child, children)
        n_kids[i] = kids
        kids = kids + 1
        if (child == 1) {
            break
       }
    }
    prop_male[i] = mean(children)
    n_male[i] = sum(children)
}

total_male_kids = sum(prop_male*n_kids)
total_kids = sum(n_kids)
total_male_kids/total_kids
```

    ## [1] 0.5027745

``` r
cor(prop_male*n_kids, n_kids)
```

    ## [1] -0.459482

``` r
mean(prop_male)
```

    ## [1] 0.6897983

``` r
### average family size in which there is a girl vs. not
mean(n_kids[n_male == 0])
```

    ## [1] 5

``` r
mean(n_kids[n_male > 0])
```

    ## [1] 1.831666

# Stopping Rule = When I have 60% male kids or 3 kids

``` r
set.seed(31415)

n_samples = 10^6
prop_male = rep(NA, n_samples)
n_kids = rep(0, n_samples)
max_kids = 5
pref = .65

for (i in 1:n_samples){

    p = c()

    kids = 1
    while (kids <= max_kids){
        p <- c(p, rbinom(1, 1, .5))
        n_kids[i] = kids
        prop_male[i] = mean(p)
        if (prop_male[i] >= pref){
            break
        }
        kids = kids + 1
    }
}

total_male_kids = sum(prop_male*n_kids)
total_kids = sum(n_kids)
mean = total_male_kids/total_kids
print(mean)
```

    ## [1] 0.500214

``` r
cor(prop_male, n_kids)
```

    ## [1] -0.9479468

# Stopping Rule = When I have 10–70% male kids or 15 kids

``` r
set.seed(31415)

n_samples = 10^6
prop_male = rep(NA, n_samples)
n_kids = rep(0, n_samples)
max_kids = 15
pref = runif(n_samples, .1, .7)

for (i in 1:n_samples){

    p = c()

    kids = 1
    while (kids <= max_kids){
        p <- c(p, rbinom(1, 1, .5))
        n_kids[i] = kids
        prop_male[i] = mean(p)
        if (prop_male[i] >= pref[i]){
            break
        }
        kids = kids + 1
    }
}

total_male_kids = sum(prop_male*n_kids)
total_kids = sum(n_kids)
mean = total_male_kids/total_kids
print(mean)
```

    ## [1] 0.4999013

``` r
cor(prop_male, pref)
```

    ## [1] 0.07677377

``` r
lm(prop_male ~ pref)
```

    ## 
    ## Call:
    ## lm(formula = prop_male ~ pref)
    ## 
    ## Coefficients:
    ## (Intercept)         pref  
    ##      0.6759       0.1270

# How does cor(pref, prop_male) vary by max_kids?

``` r
# Write a Function

sim_male_kids <- function(n_samples, 
                          max_kids,
                          pref) {

   prop_male = rep(NA, n_samples)
   n_kids = rep(0, n_samples)

   for (i in 1:n_samples){

    p = c()

    kids = 1
        while (kids <= max_kids){
            p <- c(p, rbinom(1, 1, .5))
            n_kids[i] = kids
            prop_male[i] = mean(p)
            if (prop_male[i] >= pref[i]){
                break
            }   
        kids = kids + 1
        }
    }
    
    total_male_kids = sum(prop_male*n_kids)
    total_kids = sum(n_kids)
    mean = total_male_kids/total_kids

    cor_n_kids_male <- cor(n_kids, prop_male)
    cor_pref <- cor(prop_male, pref)
    lm_pref  <- summary(lm(prop_male ~ pref))

    return(c(max_kids, mean, cor_n_kids_male, cor_pref, lm_pref$coef[2,1]))
}


set.seed(31415)
pref = runif(n_samples, .1, .7)

res <- data.frame(max_kids = NA, mean_male = NA, cor_n_kids_prop_male = NA, cor_pref = NA, lm_pref_coef = NA)
k = 1
for(max_kids in 3:10) {
    res[k, ] <- sim_male_kids(n_samples = 10^6, max_kids = max_kids, pref = pref)
    k = k + 1
}

res
```

    ##   max_kids mean_male cor_n_kids_prop_male      cor_pref lm_pref_coef
    ## 1        3 0.4996022           -0.9292635 -0.0006330946 -0.001340316
    ## 2        4 0.5001548           -0.9115618  0.0111349699  0.021878703
    ## 3        5 0.4999639           -0.8767592  0.0211827395  0.039717607
    ## 4        6 0.5006418           -0.8421468  0.0302210382  0.054866310
    ## 5        7 0.4995152           -0.8093580  0.0417951803  0.074243055
    ## 6        8 0.4998686           -0.7799539  0.0482482691  0.084251025
    ## 7        9 0.5001601           -0.7533060  0.0534403994  0.092073273
    ## 8       10 0.5000473           -0.7295298  0.0594090846  0.101308938

# What Happens If We Vary Preference for Large Family With Preference for Male Kids?

``` r
# Write a Function

set.seed(31415)

n_samples = 10^6
prop_male = rep(NA, n_samples)
n_kids = rep(0, n_samples)
pref = runif(n_samples, .1, .7)
max_kids = as.integer(pref*20 + runif(n_samples, .4, 1)*4)

for (i in 1:n_samples){

    p = c()

    kids = 1
    while (kids <= max_kids[i]){
        p <- c(p, rbinom(1, 1, .5))
        n_kids[i] = kids
        prop_male[i] = mean(p)
        if (prop_male[i] >= pref[i]){
            break
        }
        kids = kids + 1
    }
}

total_male_kids = sum(prop_male*n_kids)
total_kids = sum(n_kids)
mean = total_male_kids/total_kids
print(mean)
```

    ## [1] 0.500096

``` r
cor(prop_male, pref)
```

    ## [1] 0.08407288

``` r
lm(prop_male ~ pref)
```

    ## 
    ## Call:
    ## lm(formula = prop_male ~ pref)
    ## 
    ## Coefficients:
    ## (Intercept)         pref  
    ##      0.6654       0.1425

``` r
cor(max_kids, prop_male)
```

    ## [1] 0.08346871

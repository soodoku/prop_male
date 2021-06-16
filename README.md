## How does the stopping rule matter for the sex ratio?

1. Preference for male children, etc., doesn't affect the aggregate sex ratio

    Let there be $n$ families and let the stopping rule be that after the birth of a male child, the family stops procreating. Let $p$ be the probability a male child is born and $q = 1 -p$

    After 1 round:  $\frac{pn}{n} = p$

    After 2 rounds: $\frac{(pn + qpn)}{(n + qn)} = \frac{(p + pq)}{(1 + q)} = \frac{p(1 + q)}{(1 + q)}$

    After 3 rounds: $\frac{(pn + qpn + q^2pn)}{(n + qn + q^2n)}$
                    $= \frac{(p + pq + q^2p)}{(1 + q + q^2)}$
                    

    After $k$ rounds: $\frac{(pn + qpn + q^2pn + ... + q^kpn)}{(n + qn + q^2n + \ldots q^kn)}$

    After infinite rounds:  

    Total male children $= pn + qpn + q^2pn + \ldots$
                        $= pn (1 + q + q^2 + \ldots)$
                        $= \frac{np}{(1 - q)}$

    Total children $= n + qn + q^2n + \ldots$
                   $= n (1 + q + q^2 + \ldots)$
                   $= \frac{n}{(1 - q)}$

    Prop. Male     $= \frac{np}{(1 - q)} * \frac{(1 - q)}{n}$
                   $= p$

    If it still seems like a counterintuitive result, here's one way to think: In each round, we get $pq^k$ successes and the total number of kids increases by $q^k$.  

2. When families prefer male children but have similar preferences for family size, there is a strong negative correlation between the number of children and the proportion of male children.

3. The preference for male children and the proportion of male children is weakly correlated if people have a preference of not having too many children. But if there is a preference for very large families, e.g., 15 children, the correlation can be non-negligible (though still small). 

4. If the preference for large families is correlated with a preference for male children, the correlation between preference for male children and the proportion of male children is yet stronger.


### Script and Output

* [Script (Rmd)](prop_men.Rmd) and [Output (md)](prop_men.md)

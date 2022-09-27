---
layout: post
title: '# A system to quantify risk across projects'
date: '2022-09-15 00:00Z'
categories: post
---
# A system to quantify risk across projects

Risk has a severity axis and a likelihood axis, these two axes combine to tell us a story about the potential for calamity in a time duration, as in “during this time, I am at risk to this degree”, and an of what, or the outcome, as in “during this time I am at risk of …”. So, “In the next five years” is a time duration and, “material impact on product revenue” is the outcome.

Once we have an outcome and at time box, then we can ask about how likely something is, and the impact or consequence it makes towards the outcome. This is a matrix the more likely something is to happen raises the risk, put them together and you get a matrix.

So now we must create a standard for measuring likelihood and impact. For likelihood, where there is enough data, Bayes’ Theorem is the right approach. You should always use Bayes’ Theorem when you have enough data and therefor, if you are using risk as a factor in decision making, you should collect data with Bayes’ Theorem in mind.

If you don’t have enough of the right kind of data, then an adjectival system may be substituted. The reason why this is less desirable is because the impact will almost always be a guess and since we are going to formulate a value by combining these two together, having a sound mathematical basis for at least one of them should help us feel better about the weight we are going to be giving to risk in our decision-making process.

Bayes’ Theorem will return a likelihood (Θ) of something as a percentage between 0% and 100%. When we are forced to use an adjectival measure, we can use the following table (with Bayesian percentiles for a reference)

| Likelihood Adjective | Bayesian percentile range |
|----------------------|---------------------------|
| Extremely Common     | Over 80%                  |
| Very Common          | 60-80%                    |
| Fairly Common        | 40-59%                    |
| Less Common          | 20-39%                    |
| Very Uncommon        | 10-19%                    |
| Extremely Uncommon   | Below 10%                 |

## Impact

As mentioned above, the impact of something is nearly always a guess. We apply structure to those guesses to help us focus on what we know with greater confidence and to promote uniformity. To do this does require some subject matter expertise, but that should not be too narrow or deep that it loses view of, or influence on, the timeframe or outcome we are interrogating with our assessment.

Some Examples of good impact scoring for different contexts can be found in the chart below.

| Impact Level     | Individual                                | Team/group                                   | Business                              | Investment                | Environment                        |
|------------------|-------------------------------------------|----------------------------------------------|---------------------------------------|---------------------------|------------------------------------|
| No Impact        | No injury risk                            | No impact                                    | No impact                             | No loss                   | No impact                          |
| Less Severe      | Injury that does not impact effectiveness | Distraction / diminished attention           | Diminished ability to use equipment   | Loss of not more than 20% | Insignificant impact               |
| Fairly Severe    | Injury causing loss of effectiveness      | Diminished readiness                         | Degraded ability to use equipment     | Loss of not more than 40% | Minimal yet reversible impact      |
| Very Severe      | Injury causing lost workdays              | Diminished capability                        | Degraded equipment                    | Loss of not more than 60% | Moderate yet reversible impact     |
| Extremely Severe | Hospitalization or partial disability     | Loss of capability                           | Damage to equipment or investment     | Loss of not more than 80% | Significant yet reversible impact  |
| Catastrophic     | Death or permanent disability             | Mission failure / not able to accept mission | Total loss of equipment or investment | Loss of more than 80%     | Irreversible or irrevocable impact |

## Risk Factor

Finally, to create a useful metric that we can use to compare risk across contexts, we use this matrix to assign a numerical value to each intersection and then use that number for comparisons. To create a clear differentiation in the degree of risk that is easily understood with the leas cognitive load, we use the Fibonacci sequence as the basis for assigning numerical values to matrix intersections.

|                    | No Impact | Less Severe | Fairly Severe | Very Severe | Extremely Severe | Catastrophic |
|--------------------|-----------|-------------|---------------|-------------|------------------|--------------|
| Extremely Common   | 13        | 26          | 39            | 65          | 104              | 169          |
| Very Common        | 8         | 16          | 24            | 40          | 64               | 104          |
| Fairly Common      | 5         | 10          | 15            | 25          | 40               | 65           |
| Less Common        | 3         | 6           | 9             | 15          | 24               | 39           |
| Very Uncommon      | 2         | 4           | 6             | 10          | 16               | 26           |
| Extremely Uncommon | 1         | 2           | 3             | 5           | 8                | 13           |

This Risk Factor can, and likely will, be different timeboxes for the same outcome, so we should also label the results with the duration, in most cases it will be years, so a five year timebox would be labeled RF5, and a 12 year timebox would be labeled RF12.

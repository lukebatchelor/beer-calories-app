# beer-calories-app

Small webapp to estimate the calories in a beer given either it's ABV or it's Specific Gravity

## Calculations

### Simple estimate from ABV

```
Beer calories = ABV% x factor 2.5 x ounces of beer
```

where

```
1 fl oz = 29.5735ml
```

### From Specific Gravity

We calculate the amount of calories from alcohol and also from carbohydrates

```
TotalCalories = CaloriesFromAlcohol + CaloriesFromCarbs
```

where

```
CaloriesFromAcolhol = 1881.22 * Final_Gravity * (Original_Gravity – Final_Gravity)/(1.775 – Original_Gravity)
```

and

```
CaloriesFromCarbs = 3550 * Final_Gravity * ((0.1808 * Original_Gravity) + (0.8192 * Final_Gravity) – 1.0004)
```

### Estimating ABV from Specific Gravity

We can also estimate the abv given the specific gravity's.

The "basic" formula is

```
ABV = (Original_Gravity - Final_Gravity) * 131.25
```

The more accurate formula (especially for higher ABV beers) is

```
ABV = (76.08 * (Original_Gravity-Final_Gravity) / (1.775-Original_Gravity)) * (Final_Gravity / 0.794)
```

## Credits

Formulas compiled from:

- [brewersfriend.com](https://www.brewersfriend.com/2011/06/16/alcohol-by-volume-calculator-updated/)
- [homebrewacademy.com](https://homebrewacademy.com/beer-calories-calculator/)

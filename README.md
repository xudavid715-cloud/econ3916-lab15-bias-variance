# The Bias-Variance Tradeoff: Choosing How Flexible a Model Should Be

## Objective
I fit polynomials of increasing degree to simulated data with a known true curve to see how prediction error splits into bias and variance, and to choose a degree using only the training data.

## Methodology
- I simulated 60 training points from a known curve plus noise, so every fit could be checked against the truth.
- I drew 200 fresh samples at each of six degrees (1, 2, 3, 5, 9 and 15) and split each degree's error into bias squared and variance.
- I fit degrees 1 to 15 to the training points and scored each fit twice: on the same 60 points and on 2,000 new points from the same world.
- I used 5-fold cross-validation on the 60 points to choose a degree, and recorded how much the error varied from fold to fold.
- I ran nested cross-validation, with an inner loop that chose the degree and an outer loop that scored it, to check whether the smallest CV error is a fair estimate of error on new data.
- I wrote a function, `kfold_mse()`, that returns the k-fold CV error for any degree and number of folds, and used it to repeat the comparison with 10 folds.
- For the take-home Challenge, I fit minimum-norm Legendre polynomials up to degree 991, well past the 60 coefficients needed to pass through every training point.

## Key Findings
- Bias squared dropped to near zero by degree 5. Variance stayed low through degree 5 (0.0180) and then rose to 0.0994 at degree 15.
- Training error fell at every degree, so it could not tell me where to stop. Error on the 2,000 new points was lowest at degree 5.
- 5-fold CV chose degree 4, with a CV MSE of 0.1401. Degrees 4 and 5 also had the smallest spread across folds.
- The nested CV estimate was 0.2070, higher than 0.1401. The smallest of eleven CV errors understates the error on new data, because picking the minimum favors whichever degree got lucky on those folds.
- With 10 folds, the CV MSE was 0.1409 for degree 4 and 0.1457 for degree 5, so the ranking stayed the same.
- Challenge: held-out error rose to about 1.1 × 10²⁴ near the interpolation point (degree 51), then fell again to 0.32 at degree 991, even though training error stayed near zero. This is double descent. Even so, the very large models never did better than degrees 4 and 5.

# ``rotation``

Estimate a stellar rotation period based on its location on an HR diagram. 

**Note:** If computation time is not a problem and you would like more physically-motivated estimates, please see my colleague Zach Claytor's software [kīahōkū](https://github.com/zclaytor/kiauhoku) - which uses stellar model grids x (crossed with) theoretical gyrochronology models.

### How it works
The program uses Gibbs Sampling, a special type of Markov Chain Monte Carlo (MCMC) sampling that is helpful for estimating properties from correlated, multidimensional parameter spaces for which direct sampling is not trivial. Assuming the rotation period strongly depends on the effective temperature and surface gravity of the star, we use the extensive observations available from the *Kepler* ([McQuillan, Mazeh, & Agrain 2014](https://ui.adsabs.harvard.edu/abs/2014ApJS..211...24M/)) and K2 ([Reinhold & Hekker 2019](https://ui.adsabs.harvard.edu/abs/2020A%26A...635A..43R)) missions to statistically infer an approximate distribution. 

Since we have a very large sample, we can essentially reduce this into a 1D problem by "marginalizing" over effective temperature and surface gravity aka restricting the sample to stars with similar properties that have measured rotation periods. Once we have the relevant sample of rotation periods, we construct the distribution and integrate it to get the CDF, which is then inverted to get the quantile function. Therefore, a random variable drawn from the standard uniform distribution (i.e. X~[0,1]) will successfully map back to the original distribution to estimate a rotation period. 

#### Proof

<img src="http://latex.codecogs.com/svg.latex?\inline&space;\large&space;\textrm{Three&space;random&space;variables&space;}X,&space;Y&space;\textrm{and&space;}Z&space;\textrm{&space;are&space;jointly&space;continuous&space;if&space;there&space;exists&space;a&space;nonnegative&space;function&space;}&space;f_{XYZ}:\mathbb{R}^3&space;\to&space;\mathbb{R},&space;\textrm{&space;such&space;that,&space;for&space;any&space;set&space;}A\in&space;\mathbb{R}^3,&space;\textrm{we&space;have}" title="http://latex.codecogs.com/svg.latex?\inline \large \textrm{Three random variables }X, Y \textrm{and }Z \textrm{ are jointly continuous if there exists a nonnegative function } f_{XYZ}:\mathbb{R}^3 \to \mathbb{R}, \textrm{ such that, for any set }A\in \mathbb{R}^3, \textrm{we have}" />

<img src="http://latex.codecogs.com/svg.latex?P((X,Y,Z)\in&space;A)=\iiint_{A}f_{XYZ}(x,y,z)&space;dx&space;dy&space;dz." title="http://latex.codecogs.com/svg.latex?P((X,Y,Z)\in A)=\iiint_{A}f_{XYZ}(x,y,z) dx dy dz." />

The function <img src="http://latex.codecogs.com/svg.latex?f_{XYZ}(x,y,z)" title="http://latex.codecogs.com/svg.latex?f_{XYZ}(x,y,z)" /> is the joint probability density function (PDF) of X, Y and Z.

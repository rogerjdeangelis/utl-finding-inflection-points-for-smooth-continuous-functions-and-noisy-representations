# utl-finding-inflection-points-for-smooth-continuous-functions-and-noisy-representations
Find inflection points for closed form derivatives and noisy data in python and R   
    %let pgm=utl-finding-inflection-points-for-smooth-continuous-functions-and-noisy-representations;

    Find inflection points for closed form derivatives and noisy data in python and R

    github
    https://tinyurl.com/47pzxzd4
    https://github.com/rogerjdeangelis/utl-finding-inflection-points-for-smooth-continuous-functions-and-noisy-representations

    SAS Communities
     https://communities.sas.com/t5/SAS-Programming/Detect-inflection-point-or-changing-point/td-p/872663

    Related
     https://github.com/rogerjdeangelis?tab=repositories&q=regression&type=&language=&sort=

    Most of the solutions below rely on solving the second derivative, ie f``(x)=0.
    Newton-Raphson can be used  without the derivatives.
    For noisy grahical representation I have fit linear or non-linear functions to your data.
    Matlib provides slmengine and smlsolve provide a more general method to fit smooth curves using splines.


        Finding inflection points for smooth continuous functions and noisy representations


          1. Python sympy  Inflection points for standard normal distribution

             %utl_submit_py64_310("
             from sympy import *;
             x = symbols('x');
             df1_x = diff(exp(-x**2/2)/(2*pi)**(1/2) ,x);
             print(df1_x);
             df2_x = diff(df1_x);
             print(df2_x);
             s=solve(df2_x);
             print(s);
             ");

             OUTPUT
             f`(x)  = 0.707106781186547*x*exp(-x**2/2)/pi**0.5
             f``(x) = 0.707106781186547*x**2*exp(-x**2/2)/pi**0.5 - 0.707106781186547*exp(-x**2/2)/pi**0.5
             Inflection points [-1, 1]  in genral [mean - standard deviation, mean + standard deviation]?

          2. Python sympy: Inflection points for  x**4/2 - x**3 - 6*x**2 - 3*x - 1  [domain -3,4]

             %utl_submit_py64_310("
             from sympy import *;
             x = symbols('x');
             df1_x = diff(x**4/2 - x**3 - 6*x**2 - 3*x - 1,x);
             print(df1_x);
             df2_x = diff(df1_x);
             print(df2_x);
             s=solve(df2_x,x);
             print(s);
             ");

             OUTPUT
             f`(x)  = 2*x**3 - 3*x**2 - 12*x - 3
             f``(x) = 6*x**2 - 6*x - 12
             Inflection points [-1, 2]

          3. R lm and Python sympy: R to fit a smooth curve (some what of an art)

             Inflection points for noisy f(x) = x**4/2 - x**3 - 6*x**2 - 3*x - 1 + rand ('normal', 0,1)

             I use high order polinomials to fit the noisy data first. You may wat to look
             at the graph and chose other functions ie splines, fourier seriex etc

             Inflection Points: [-0.999602027461793, 2.00195239993064]

          4. Python lm and Python sympy: Python was able to fit a smooth curve

             I could fit a smooth curve, but could not figure out how to output a panda
             datareame with the regression output. (tried conveting dics, series, list output
             to a panda dataframe but gave up afyer abot an hour.
             R seems well suited for stats?

             SOAPBOX ON
              Pyhton (except python SQL) does not integrate easily with other languages
             SOAPBOX OFF


    /*           _              __
     _ __   ___ (_)___ _   _   / _| ___  _ __ _ __ ___
    | `_ \ / _ \| / __| | | | | |_ / _ \| `__| `_ ` _ \
    | | | | (_) | \__ \ |_| | |  _| (_) | |  | | | | | |
    |_| |_|\___/|_|___/\__, | |_|  \___/|_|  |_| |_| |_|
                       |___/
    */


    options validvarname=upcase;

    data grf(keep=x y) grf_noisy grf15;

     do x=-3 to 4 by .01;
       x2=x**2;
       x3=x**3;
       x4=x**4;
       y = x**4/2 - x**3 - 6*x**2 - 3*x - 1;
       output grf;
       if abs(x-int(x))<.0010 then output grf15;
       y = y + rand ('normal', 0,1);
       output grf_noisy;
     end;
    run;quit;


    %let _y=y111111111111;  /*--- width less than 64 ----*/

    options ls=64 ps=54;
    proc plot data=sd1.grf_noisy(rename=y=&_y);
      plot &_y*x='*' / box href=-1 2 vref=0;
    run;quit;
    options ls=171 ps=65;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SAMPLE MAY NOT EXACTLY MATCH RESULT ABOVE                                                                              */
    /*                                                                                                                        */
    /*                y= x**4/2 - (x+1)**3 -3*x**2                  y= x**4/2 - (x+1)**3 -3*x**2 + rand ('normal', 0,1        */
    /*                                                                                                                        */
    /*        -4        -2   -1    0         2         4                 -4        -2   -1    0         2         4           */
    /*      ---+---------+---------+---------+---------+---            ---+---------+---------+---------+---------+---        */
    /*      |                 |              |            |         10 +        *        |              |            +  10    */
    /*      | Inflection      |              |            |            |         *       |              |            |        */
    /*   40 + Points [-1,2]   |              |            +  40        |                 |              |            |        */
    /*      |                 |              |            |            |                 |              |            |        */
    /*      |                 |              |            |            |          *      |              |            |        */
    /*      |                 |              |            |            |                 |              |            |        */
    /*      |                 |              |            |            |          *      |    *         |            |        */
    /*      |                 |              |            |          0 +-----------------+--**----------+------------+   0    */
    /*   20 +       *         |              |            +  20        |           **    *    *         |            |        */
    /*      |        *        |              |            |            |           **** **** * **       |            |        */
    /*      |        *        |              |            |            |              ***| *     *      |            |        */
    /*      |         *       |              |            |            |                 |      * *     |            |        */
    /*      |         *       |              |            |            |               * |              |            |        */
    /*      |          *      |              |            |            |                 |         *    |            |        */
    /*    0 +-----------*-----+******--------+------------+   0    -10 +                 |        *     |            + -10    */
    /*      |            *****/     ***      |            |            |                 |         *    |            |        */
    /*      |                 |       **     |            |            |                 |          *   |            |        */
    /*      |                 |         *    |            |            |                 |              |            |        */
    /*      |                 |          *   |            |            |                 |              |            |        */
    /*      |                 |           *  |            |            |                 |           *  |            |        */
    /*  -20 +                 |            * |            + -20        |                 |              |            |        */
    /*      |                 |            **|            |        -20 +                 |            * |            + -20    */
    /*      |                 |             *|            |            |                 |              |            |        */
    /*      |                 |              *            |            |                 |              |            |        */
    /*      |                 |              |*           |            |                 |            **|            |        */
    /*      |                 |              |**          |            |                 |             *|            |        */
    /*  -40 +                 |              | *          + -40        |                 |              |            |        */
    /*      |                 |              |  *      *  |            |                 |              *            |        */
    /*      |                 |              |   *     *  |        -30 +                 |              |            + -30    */
    /*      |                 |              |    **  *   |            |                 |              |            |        */
    /*      |                 |              |     ***    |            |                 |              *            |        */
    /*      |                 |              |            |            |                 |              |*           |        */
    /*  -60 +                 |Inflection    |Inflection  + -60        |                 |              |            |        */
    /*      |                 |              |            |            |                 |              | *          |        */
    /*      ---+---------+----+----+---------+---------+---            |                 |              |            |        */
    /*        -4        -2   -1    0         2         4           -40 +                 |              | *          + -40    */
    /*                                                                 |                 |              |            |        */
    /*               y= x**4/2 - (x+1)**3 -3*x**2                      |                 |              |  *         |        */
    /*                                                                 ---+---------+----+----+---------+---------+---        */
    /*                                                                   -4        -2   -1    0         2         4           */
    /*                                                                                                                        */
    /*                                                                                      Noisy Version                     */
    /*                                                                                                                        */
    /*       GRAF,SAS&BDAT                                 GRSF_NOISY.SAS7NDAT                                                */
    /*   =======================       ===================================================================                    */
    /*                                                                                                                        */
    /*   Obs      X         Y          Obs      X        X2         X3          X4         X5          Y                      */
    /*                                                                                                                        */
    /*     1    -3.00    21.5000         1    -3.00    9.0000    -27.0000    81.0000    -243.000    24.1999                   */
    /*     2    -2.99    21.0230         2    -2.99    8.9401    -26.7309    79.9254    -238.977    20.5026                   */
    /*     3    -2.98    20.5519         3    -2.98    8.8804    -26.4636    78.8615    -235.007    21.8164                   */
    /*     4    -2.97    20.0868         4    -2.97    8.8209    -26.1981    77.8083    -231.091    18.3010                   */
    /*     5    -2.96    19.6276         5    -2.96    8.7616    -25.9343    76.7656    -227.226    17.1039                   */
    /*     ...                                                                                                                */
    /*                                                 proc reg data=grf_noisy;                                               */
    /*   Sample vales at x integers                      model y=x x2 x3 x4;                                                  */
    /*                                                 run;quit;                                                              */
    /*   Obs     X    X2     X3     X4      Y                      Parameter                                                  */
    /*                                                 Variable     Estimate                                                  */
    /*    1     -3     9    -27     81     21.5                                                                               */
    /*    2     -2     4     -8     16     -3.0                                                                               */
    /*    3     -1     1     -1      1     -2.5       Intercept     -1.03064                                                  */
    /*    4     -0     0     -0      0     -1.0       X             -3.03072                                                  */
    /*    5      1     1      1      1    -10.5       X2            -5.97818                                                  */
    /*    6      2     4      8     16    -31.0       X3            -0.99813                                                  */
    /*    7      3     9     27     81    -50.5       X4             0.49789                                                  */
    /*    8      4    16     64    256    -45.0                                                                               */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*                            __ _ _
    __      ___ __  ___   _ __   / _(_) |_
    \ \ /\ / / `_ \/ __| | `__| | |_| | __|
     \ V  V /| |_) \__ \ | |    |  _| | |_
      \_/\_/ | .__/|___/ |_|    |_| |_|\__|
             |_|
    */

    proc datasets lib=work nolist;
      delete want coef;
    run;quit;

    %let _pth=%sysfunc(pathname(work));

    %utl_submit_wps64('
    libname wrk "&_pth";
    proc r;
    export data=wrk.grf_noisy r=grf;
    submit;
    library(haven);
    library(data.table);
    library(backports);
    model <- lm(formula = grf$Y ~ grf$X + grf$X2 + grf$X3 + grf$X4);
    print(formula);
    summary(model);
    want<-data.table(model$residuals);
    want$fitted  <-  model$fitted.values;
    want$resid   <-  model$residuals;
    want$Y       <-  grf$Y;
    want$X       <-  grf$X;
    print(want);
    want<-want[,2:5];
    coef    <-  as.data.table(model$coefficients);
    coef    <-  cbind(c("INTERCEPT","X**1","X**2","X**3","X**4"),coef);
    colnames(coef)<-c("PARAMTR","VALUE");
    print(coef);
    endsubmit;
    import data=wrk.want r=want;
    import data=wrk.coef r=coef;
    run;quit;
    ');

    data _null_;
      length equ $128;
      retain equ;
      set coef end=dne;
      if PARAMTR = 'INTERCEPT' then equ=value;
       else equ=cats(equ,'+',value,'*',lowcase(paramtr));
      if dne then call symputx('_equ',equ);
    run;quit;

    %put &_equ;
    /*---- -1.03064269178646+-3.030721058*x**1+-5.978176599*x**2+-0.998127833*x**3+0.4978936809*x**4                      ----*/

    /**************************************************************************************************************************/
    /*                     _               _                                                                                  */
    /*  _ __    ___  _   _| |_ _ __  _   _| |_                                                                                */
    /* | `__|  / _ \| | | | __| `_ \| | | | __|                                                                               */
    /* | |    | (_) | |_| | |_| |_) | |_| | |_                                                                                */
    /* |_|     \___/ \__,_|\__| .__/ \__,_|\__|                                                                               */
    /*                        |_|                                                                                             */
    /*                                                                                                                        */
    /*   PARAMTR      VALUE                                                                                                   */
    /* INTERCEPT -1.0306427                                                                                                   */
    /*      X**1 -3.0307211                                                                                                   */
    /*      X**2 -5.9781766                                                                                                   */
    /*      X**3 -0.9981278                                                                                                   */
    /*      X**4  0.4978937                                                                                                   */
    /*                                                                                                                        */
    /* Fitted polynomial (macro variable _equ)                                                                                */
    /*                                                                                                                        */
    /* f(x) =-1.03064269178646+-3.030721058*x**1+-5.978176599*x**2+-0.998127833*x**3+0.4978936809*x**4                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
    /*          _      __ _ _    __    __
      __ _  ___| |_   / _( | )  / /_  _\ \
     / _` |/ _ \ __| | |_ \|\| | |\ \/ /| |
    | (_| |  __/ |_  |  _|     | | >  < | |
     \__, |\___|\__| |_|       | |/_/\_\| |
     |___/                      \_\    /_/
    */

    /*---- get f``(x) ----*/

    %utl_submit_py64_310("
    from sympy import *;
    x = symbols('x');
    df1_x = diff(&_equ);
    print(df1_x);
    df2_x = diff(df1_x);
    print(df2_x);
    s=solve(df2_x,x);
    print(s);
    ");

    /*                                             _               _
     ___ _   _ _ __ ___  _ __  _   _    ___  _   _| |_ _ __  _   _| |_
    / __| | | | `_ ` _ \| `_ \| | | |  / _ \| | | | __| `_ \| | | | __|
    \__ \ |_| | | | | | | |_) | |_| | | (_) | |_| | |_| |_) | |_| | |_
    |___/\__, |_| |_| |_| .__/ \__, |  \___/ \__,_|\__| .__/ \__,_|\__|
         |___/          |_|    |___/                  |_|
    */

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* f`(x)  = 1.9915747236*x**3 - 2.994383499*x**2 - 11.956353198*x - 3.030721058                                           */
    /* f``(x) = 5.9747241708*x**2 - 5.988766998*x - 11.956353198                                                              */
    /*                                                                                                                        */
    /* And solve (note I have subsututes f``(x) into the sympy code above                                                     */
    /*                                                                                                                        */
    /* Inflection Points: [-0.999602027461793, 2.00195239993064]                                                              */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*            __ _ _
     _ __  _   _ / _(_) |_
    | `_ \| | | | |_| | __|
    | |_) | |_| |  _| | |_
    | .__/ \__, |_| |_|\__|
    |_|    |___/
    */

    /*---- I gave up on python. I was only able to create an output text file           ----*/
    /* too many output file structures that do not lend themselves t0 panda dataframes. ----*/

    %let _pth=%sysfunc(pathname(work));

    %utl_submit_wps64("
    libname wrk '&_pth';
    proc python;
     export data=wrk.grf_noisy python=grf;
    submit;
    import pandas as pd;
    import numpy as np;
    import statsmodels.api as sm;
    lm = sm.OLS.from_formula('Y ~ X + X2 + X3 + X4', grf);
    result = lm.fit();
    print(result.summary());
    predictions = result.predict(clas['X']);
    print(predictions);
    clas['PREDICT']=predictions;
    f = open('d:/txt/aov.txt', 'a');
    print(result.summary(), file=f);
    f.close();
    endsubmit;
    run;quit;
    ");

    /*---- I leave it up to the reader to parse the text file                                                             ----*/

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* I leave it up to the reader to parse the text file                                                                     */
    /*                                                                                                                        */
    /* The WPS System                                                                                                         */
    /*                                                                                                                        */
    /* The PYTHON Procedure                                                                                                   */
    /*                                                                                                                        */
    /*                             OLS Regression Results                                                                     */
    /* ==============================================================================                                         */
    /* Dep. Variable:                      Y   R-squared:                       0.998                                         */
    /* Model:                            OLS   Adj. R-squared:                  0.998                                         */
    /* Method:                 Least Squares   F-statistic:                 8.028e+04                                         */
    /* Date:                Wed, 10 May 2023   Prob (F-statistic):               0.00                                         */
    /* Time:                        08:33:43   Log-Likelihood:                -973.20                                         */
    /* No. Observations:                 701   AIC:                             1956.                                         */
    /* Df Residuals:                     696   BIC:                             1979.                                         */
    /* Df Model:                           4                                                                                  */
    /* Covariance Type:            nonrobust                                                                                  */
    /* ==============================================================================                                         */
    /*                  coef    std err          t      P>|t|      [0.025      0.975]                                         */
    /* ------------------------------------------------------------------------------                                         */
    /* Intercept     -1.0306      0.067    -15.492      0.000      -1.161      -0.900                                         */
    /* X             -3.0307      0.053    -56.702      0.000      -3.136      -2.926                                         */
    /* X2            -5.9782      0.032   -188.489      0.000      -6.040      -5.916                                         */
    /* X3            -0.9981      0.009   -117.003      0.000      -1.015      -0.981                                         */
    /* X4             0.4979      0.003    155.734      0.000       0.492       0.504                                         */
    /* ==============================================================================                                         */
    /* Omnibus:                        0.380   Durbin-Watson:                   2.039                                         */
    /* Prob(Omnibus):                  0.827   Jarque-Bera (JB):                0.480                                         */
    /* Skew:                           0.029   Prob(JB):                        0.787                                         */
    /* Kurtosis:                       2.885   Cond. No.                         137.                                         */
    /* ==============================================================================                                         */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

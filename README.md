# Turtle-Trading-Backtest
This is a beginner's guideline for using Python to backtest Turtle Trading (or any other automated trading) strategy

I.  Turtle Trading System Explained

1.  General

    *  Introduction

        -   Key Takeaways

            -   Turtle is a nickname given to a group of traders who
                were part of an experiment in 1983.

            -   The experiment was run by Richard Dennis and Bill
                Eckhardt, who wanted to test if successful trading
                could be taught to novices.

            -   Dennis and Eckhardt taught the turtles a trading
                system that resulted in very positive results among
                the study participants.

            -   Some traders continue to use their trading system,
                or version of it, to this day.

        -   Related Concepts

            - [Trend Following Strategy](https://www.investopedi*com/terms/t/trendtrading.asp)

            - [Donchian Channel](https://www.investopedi*com/terms/d/donchianchannels.asp)

            - [Average True Range](https://www.investopedi*com/terms/a/atr.asp)

    *  Reference

        -   Briefings

            -   [Turtle Trading: A Market Legend](https://www.investopedi*com/articles/trading/08/turtle-trading.asp)

            -   [How Strangers Made \$175 Million With This](https://www.youtube.com/watch?v=-Yt1hOfKrYY&ab_channel=TRADINGRUSH)

            -   [The Complete Turtle Trader by Michael Covel.(Richard Dennis)](https://www.youtube.com/watch?v=NJkXSZUHl1g&ab_channel=FinancialWisdom)

        -   Detailed Breakdown

            -   [Original Turtle Rules](https://bigpicture.typepad.com/comments/files/turtlerules.pdf)

            -   [Case Study: Complete Guide On Turtle Trading System](https://www.youtube.com/watch?v=kBstdk-_kb8&ab_channel=TheVIXGuy)

2.  Trading System Breakdown

    *  Position Sizing

        -   Sizing based on Volatility -- Constant Percentage Risk

            -   High (Low) Volatility = Small (Large) Position Size

            -   Aim = risk the same amount of dollar

        -   Sizing Formula:
        ```math
        U = floor \left( \frac{f*C}{r*N*P} \right)
        ```

        -   Parameter explanation

            -   U = unit = no. of stocks to buy

            -   P = current price of the stock (so U\*P =
                position size for this trade)

            -   C = Result-Adjusted Amount of Capital (detailed
                below)

            -   N = ATR (detailed below)

            -   f = Capital Fraction -- usually set at 0.02

            -   r = Risk Coefficient -- set to 1 (2) to small
                (large) account size

        -   Example: Buy Apple shares (P = \$150) for a
            portfolio of C = \$100,000

            -   N = 2.5, f = 0.02, r = 1

        ```math 
        U = floor\left( \frac{0.02*100,000}{1*2.5*150} \right) = floor\left( 5.33 \right) = 5\ shares 
        ```

        -   Sizing Limit

            -   1 Market -- Max 5 Units (pyramid included)

            -   Single Direction (Long / Short) -- Max 12 Unit

        -   N = ATR value

            -   [ATR explanation & formula](https://www.investopedi*com/terms/a/atr.asp)

            -   [Sample ATR Python implementation](https://www.learnpythonwithrune.org/calculate-the-average-true-range-atr-easy-with-pandas-dataframes/)

            -   Note: ATR value are available in most trading
                software (e.g.
                [Trading View](https://www.tradingview.com/support/solutions/43000501823-average-true-range-atr/))

        -   C = Result-Adjusted Amount of Capital

            -   Aim = Make trader to be more aggressive
                (conservative) when they are winning (losing)

            -   Account size change will be 2x

                -   Account = 1,000,000 and down 10% (100,000),
                    trader will proceed as if their account is now
                    1,000,000 -- 2x100,000 = 800,000 (actually
                    amount is 900,000)

                -   Account = 1,000,000 and up 10% (100,000), trader
                    will proceed as if their account is now
                    1,000,000 + 2x100,000 = 1200,000 (actually
                    amount is 1,100,000)

            -   Account is adjusted for every 10% up / down

    *  Trade Entries & Exposure Adjustment

        -   Trade Entries

            -   2 systems used, each based on Donchian Channel
                breakout

            -   Breakout = Price exceed high / low of x days (x = 20
                / 55)

            -   System 1 -- Price exceed (drop below) 20-day High
                (Low) = Long (Short) 1 Unit

            -   System 2 -- Price exceed (drop below) 55-day High
                (Low) = Long (Short) 1 Unit

        -   Exposure Adjustment -- Pyramiding

            -   1 more unit is added when price move 0.5N in the
                trend breakout direction

            -   Continue until max position size is reached

    *  Stop Loss & Profit Taking

        -   Stop Loss

            -   Initial Stop Loss = 2N away from entry price

            -   Trailing Stop Loss -- Move up stop loss ) 0.5N every
                time adding 1 more Unit

        -   Profit Taking

            -   System 1 -- Exit Long (Short) position at 10-day low
                (high)

            -   System 2 -- Exit Long (Short) position at 20-day low
                (high)

II. Turtle Trading -- Backtest Implementation

1.  Python Pre-requisites

    *  Python Programing Concepts & Tools

        -   Python 101

            -   Reference (Python-Specific):
                [pythontutorial.net](https://www.pythontutorial.net/),
                [pynative.com](https://pynative.com/),
                [pythonbasics.org](https://pythonbasics.org/)

            -   Reference (Generic):
                [studytonight](https://www.studytonight.com/python/),
                [w3schools](https://www.w3schools.com/python/default.asp),
                [javatpoint](https://www.javatpoint.com/python-tutorial),
                [tutorialspoint](https://www.tutorialspoint.com/python3/index.htm)

        -   Python 102

            -   Theoretical: [Python
                OOP](https://www.tutorialspoint.com/python/pdf/python_classes_objects.pdf),
                [Python Data Structure](https://intellipaat.com/mediaFiles/2019/02/Python-Data-structures-cheat-sheet.pdf?US)

            -   Practical: [Python Database](https://pynative.com/python/databases/)

        -   Python Development Environment & Tools

            -   VS Code:
                [Guideline](https://adamtheautomator.com/visual-studio-code-tutorial/),
                [Cheatsheet](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf),
                [Reference](https://code.visualstudio.com/docs)

            -   Jupyter Notebooks: [Guideline 1](https://realpython.com/jupyter-notebook-introduction/),
                [Guideline 2](https://www.datacamp.com/tutorial/tutorial-jupyter-notebook),
                [Cheatsheet](https://miro.medium.com/v2/resize:fit:1400/1*totJoCc3l7BdeY-mEQ6HHQ.png),
                [Reference](https://www.tutorialspoint.com/jupyter/index.htm)

            -   JupyterLab:
                [Guideline](https://towardsdatascience.com/getting-the-most-out-of-jupyter-lab-9b3198f88f2d?gi=e5cf49abd26f),
                [Cheatsheet](https://blog.ja-ke.tech/assets/jupyterlab-shortcuts/Shortcuts.png),
                [Reference](https://jupyterla*readthedocs.io/en/stable/)

    *  Python Libraries

        -   Data Manipulation

            -   numpy:
                [Manual](https://numpy.org/doc/stable/index.html),
                [Cheatsheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Numpy_Python_Cheat_Sheet.pdf),
                [Reference](https://www.studytonight.com/numpy/)

            -   pandas:
                [Manual](https://pandas.pydat*org/pandas-docs/stable/index.html),
                [Cheatsheet](https://www.javatpoint.com/pandas-cheat-sheet),
                [Reference](https://www.studytonight.com/pandas/)

        -   Data Visualization

            -   matplotlib:
                [Manual](https://matplotli*org/stable/index.html),
                [Cheatsheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Python_Matplotlib_Cheat_Sheet.pdf),
                [Reference](https://www.studytonight.com/matplotlib/)

            -   seaborn:
                [Manual](https://seaborn.pydat*org/index.html),
                [Cheatsheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Python_Seaborn_Cheat_Sheet.pdf),
                [Reference](https://www.tutorialspoint.com/seaborn/index.htm)

        -   Financial Data:

            -   pandas-datareader:
                [Review](https://thecleverprogrammer.com/2021/03/22/pandas-datareader-using-python-tutorial/),
                [Main Page](https://githu*com/pydata/pandas-datareader),
                [Reference](https://buildmedi*readthedocs.org/media/pdf/pandas-datareader/latest/pandas-datareader.pdf)

            -   yfinance:
                [Review](https://algotrading101.com/learn/yfinance-guide/),
                [Main Page](https://aroussi.com/post/python-yahoo-finance),
                [Reference](https://pypi.org/project/yfinance/)

2.  Backtest Settings & Results

    *  Backtest Settings

        -   High Level Overview

            -   Use both System 1 and System 2 to get breakout (if
                any)

            -   Ticker followed: SPY, QQQ, TLT, GLD, KWEB, XLE, XLI,
                XLB, XLF, XLRE

            -   System is backtested from 01/01/2000 to 31/03/2023

            -   Dividends were not included.

        -   Notes

            -   For System 1, ignore the breakout if the last
                breakout led to a winning trade

            -   Capital is split 50:50 between System 1 and System 2

    *  Backtest Results

> ![](media/image1.png){width="5.409159011373578in"
> height="3.5976640419947508in"}
>
> ![](media/image2.png){width="5.705521653543307in"
> height="0.6171073928258968in"}

-   Key Observations

    -   Turtle Trading outperform SPY from 2000-2010, but underperform
        from 2010-2023

    -   Turtle Trading have much lower volatility & maximum drawdown

    -   "Fundamentalist" may argue that the FED's QE have a great effect
        in boosting the return of SPY from 2010 onwards

-   Further Analysis / Fine Tune

    -   Tickers to track can include other individual stocks / ETF

    -   Fundamental Analysis can be incorporated to select better
        candidates

    -   System parameters can be adjusted and observe performance

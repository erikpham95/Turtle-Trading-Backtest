Backtest Turtle Trading Strategy with Python

This is a beginner’s guideline for using Python to backtest Turtle Trading (or any other automated trading) strategy

1) Turtle Trading System Explained
   1) General
      1. Introduction
         1. Key Takeaways
            1. Turtle is a nickname given to a group of traders who were part of an experiment in 1983.
            1. The experiment was run by Richard Dennis and Bill Eckhardt, who wanted to test if successful trading could be taught to novices.
            1. Dennis and Eckhardt taught the turtles a trading system that resulted in very positive results among the study participants.
            1. Some traders continue to use their trading system, or version of it, to this day.
         1. Related Concepts
            1. [Trend Following Strategy](https://www.investopedia.com/terms/t/trendtrading.asp)
            1. [Donchian Channel](https://www.investopedia.com/terms/d/donchianchannels.asp)
            1. [Average True Range](https://www.investopedia.com/terms/a/atr.asp)
      1. Reference
         1. Briefings
            1. [Turtle Trading: A Market Legend](https://www.investopedia.com/articles/trading/08/turtle-trading.asp)
            1. [How Strangers Made $175 Million With This](https://www.youtube.com/watch?v=-Yt1hOfKrYY&ab_channel=TRADINGRUSH)
            1. [The Complete Turtle Trader by Michael Covel. (Richard Dennis)](https://www.youtube.com/watch?v=NJkXSZUHl1g&ab_channel=FinancialWisdom)
         1. Detailed Breakdown
            1. [Original Turtle Rules](https://bigpicture.typepad.com/comments/files/turtlerules.pdf)
            1. [Case Study: Complete Guide On Turtle Trading System](https://www.youtube.com/watch?v=kBstdk-_kb8&ab_channel=TheVIXGuy)
   1) Trading System Breakdown
      1. Position Sizing
         1. Sizing based on Volatility – Constant Percentage Risk
            1. High (Low) Volatility = Small (Large) Position Size
            1. Aim = risk the same amount of dollar
         1. Sizing Formula: U=floorf\*Cr\*N\*P
            1. Parameter explanation
               1. U = unit = no. of stocks to buy
               1. P = current price of the stock (so U\*P = position size for this trade)
               1. C = Result-Adjusted Amount of Capital (detailed below)
               1. N = ATR (detailed below)
               1. f = Capital Fraction – usually set at 0.02
               1. r = Risk Coefficient – set to 1 (2) to small (large) account size 
            1. Example: Buy Apple shares (P = $150) for a portfolio of C = $100,000
               1. N = 2.5, f = 0.02, r = 1
               1. U=floor0.02\*100,0001\*2.5\*150=floor5.33=5 shares
            1. Sizing Limit
               1. 1 Market – Max 5 Units (pyramid included)
               1. Single Direction (Long / Short) – Max 12 Unit
         1. N = ATR value
            1. [ATR explanation & formula](https://www.investopedia.com/terms/a/atr.asp)
            1. [Sample ATR Python implementation](https://www.learnpythonwithrune.org/calculate-the-average-true-range-atr-easy-with-pandas-dataframes/) 
            1. Note: ATR value are available in most trading software (e.g. [TradingView](https://www.tradingview.com/support/solutions/43000501823-average-true-range-atr/))
         1. C = Result-Adjusted Amount of Capital
            1. Aim = Make trader to be more aggressive (conservative) when they are winning (losing) 
            1. Account size change will be 2x
               1. Account = 1,000,000 and down 10% (100,000), trader will proceed as if their account is now 1,000,000 – 2x100,000 = 800,000 (actually amount is 900,000)
               1. Account = 1,000,000 and up 10% (100,000), trader will proceed as if their account is now 1,000,000 + 2x100,000 = 1200,000 (actually amount is 1,100,000)
            1. Account is adjusted for every 10% up / down
      1. Trade Entries & Exposure Adjustment
         1. Trade Entries
            1. 2 systems used, each based on Donchian Channel breakout
            1. Breakout = Price exceed high / low of x days (x = 20 / 55)
            1. System 1 – Price exceed (drop below) 20-day High (Low) = Long (Short) 1 Unit
            1. System 2 – Price exceed (drop below) 55-day High (Low) = Long (Short) 1 Unit
         1. Exposure Adjustment – Pyramiding
            1. 1 more unit is added when price move 0.5N in the trend breakout direction
            1. Continue until max position size is reached
      1. Stop Loss & Profit Taking
         1. Stop Loss
            1. Initial Stop Loss = 2N away from entry price 
            1. Trailing Stop Loss – Move up stop loss ) 0.5N every time adding 1 more Unit
         1. Profit Taking
            1. System 1 – Exit Long (Short) position at 10-day low (high)
            1. System 2 – Exit Long (Short) position at 20-day low (high)

1) Turtle Trading – Backtest Implementation
   1) Python Pre-requisites
      1. Python Programing Concepts & Tools
         1. Python 101
            1. Reference (Python-Specific): [pythontutorial.net](https://www.pythontutorial.net/), [pynative.com](https://pynative.com/), [pythonbasics.org](https://pythonbasics.org/)   
            1. Reference (Generic): [studytonight](https://www.studytonight.com/python/), [w3schools](https://www.w3schools.com/python/default.asp), [javatpoint](https://www.javatpoint.com/python-tutorial), [tutorialspoint](https://www.tutorialspoint.com/python3/index.htm) 
         1. Python 102
            1. Theoretical: [Python OOP](https://www.tutorialspoint.com/python/pdf/python_classes_objects.pdf), [Python Data Structure](https://intellipaat.com/mediaFiles/2019/02/Python-Data-structures-cheat-sheet.pdf?US)
            1. Practical: [Python Database](https://pynative.com/python/databases/)
         1. Python Development Environment & Tools
            1. VS Code: [Guideline](https://adamtheautomator.com/visual-studio-code-tutorial/), [Cheatsheet](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf), [Reference](https://code.visualstudio.com/docs)
            1. Jupyter Notebooks: [Guideline 1](https://realpython.com/jupyter-notebook-introduction/), [Guideline 2](https://www.datacamp.com/tutorial/tutorial-jupyter-notebook), [Cheatsheet](https://miro.medium.com/v2/resize:fit:1400/1*totJoCc3l7BdeY-mEQ6HHQ.png), [Reference](https://www.tutorialspoint.com/jupyter/index.htm)
            1. JupyterLab: [Guideline](https://towardsdatascience.com/getting-the-most-out-of-jupyter-lab-9b3198f88f2d?gi=e5cf49abd26f), [Cheatsheet](https://blog.ja-ke.tech/assets/jupyterlab-shortcuts/Shortcuts.png), [Reference](https://jupyterlab.readthedocs.io/en/stable/)
      1. Python Libraries
         1. Data Manipulation
            1. numpy: [Manual](https://numpy.org/doc/stable/index.html), [Cheatsheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Numpy_Python_Cheat_Sheet.pdf), [Reference](https://www.studytonight.com/numpy/)
            1. pandas: [Manual](https://pandas.pydata.org/pandas-docs/stable/index.html), [Cheatsheet](https://www.javatpoint.com/pandas-cheat-sheet), [Reference](https://www.studytonight.com/pandas/)
         1. Data Visualization
            1. matplotlib: [Manual](https://matplotlib.org/stable/index.html), [Cheatsheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Python_Matplotlib_Cheat_Sheet.pdf), [Reference](https://www.studytonight.com/matplotlib/)
            1. seaborn: [Manual](https://seaborn.pydata.org/index.html), [Cheatsheet](https://s3.amazonaws.com/assets.datacamp.com/blog_assets/Python_Seaborn_Cheat_Sheet.pdf), [Reference](https://www.tutorialspoint.com/seaborn/index.htm)
         1. Financial Data:
            1. pandas-datareader: [Review](https://thecleverprogrammer.com/2021/03/22/pandas-datareader-using-python-tutorial/), [Main Page](https://github.com/pydata/pandas-datareader), [Reference](https://buildmedia.readthedocs.org/media/pdf/pandas-datareader/latest/pandas-datareader.pdf)
            1. yfinance: [Review](https://algotrading101.com/learn/yfinance-guide/), [Main Page](https://aroussi.com/post/python-yahoo-finance), [Reference](https://pypi.org/project/yfinance/)
   1) Backtest Settings & Results
      1. Backtest Settings
         1. High Level Overview
            1. Use both System 1 and System 2 to get breakout (if any)
            1. Ticker followed: SPY, QQQ, TLT, GLD, KWEB, XLE, XLI, XLB, XLF, XLRE
            1. System is backtested from 01/01/2000 to 31/03/2023
            1. Dividends were not included.
         1. Notes
            1. For System 1, ignore the breakout if the last breakout led to a winning trade
            1. Capital is split 50:50 between System 1 and System 2
      1. Backtest Results

![](Aspose.Words.cf57a774-d04f-47f1-9380-7f527ce875c0.001.png)

![](Aspose.Words.cf57a774-d04f-47f1-9380-7f527ce875c0.002.png)

1. Key Observations
   1. Turtle Trading outperform SPY from 2000-2010, but underperform from 2010-2023
   1. Turtle Trading have much lower volatility & maximum drawdown
   1. “Fundamentalist” may argue that the FED’s QE have a great effect in boosting the return of SPY from 2010 onwards 
1. Further Analysis / Fine Tune
   1. Tickers to track can include other individual stocks / ETF
   1. Fundamental Analysis can be incorporated to select better candidates
   1. System parameters can be adjusted and observe performance

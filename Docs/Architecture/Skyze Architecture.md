
# Skyze Architecutre
Draft v0.1

## Contents
<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Skyze Architecutre](#skyze-architecutre)
	- [Contents](#contents)
	- [Python](#python)
		- [Language](#language)
		- [Modules](#modules)
	- [Persistence](#persistence)
		- [Database](#database)
			- [MongoDB](#mongodb)
		- [File System](#file-system)
			- [csv](#csv)
			- [Excel](#excel)
	- [Code Resository](#code-resository)
		- [GitHub](#github)
	- [Code Quality](#code-quality)
		- [Standards](#standards)
			- [PEP8](#pep8)
		- [Linter](#linter)
			- [PyLint](#pylint)
		- [General code quality?](#general-code-quality)
			- [Code Climate](#code-climate)
	- [Testing](#testing)
		- [Test running](#test-running)
			- [nose](#nose)
		- [Unit Testing](#unit-testing)
			- [unittest2](#unittest2)
		- [Continuous Integration](#continuous-integration)
	- [Documentation](#documentation)
		- [Requirements](#requirements)
		- [Solutions](#solutions)
			- [Markdown](#markdown)
			- [Sphynx](#sphynx)
			- [GitWiki](#gitwiki)
	- [Project Management](#project-management)
		- [Requirement Management](#requirement-management)
			- [Trello](#trello)
		- [Defect Management](#defect-management)
			- [GIT Issues](#git-issues)
		- [Work Management](#work-management)
			- [Trello](#trello)
		- [Team Communications](#team-communications)
			- [Slack](#slack)

<!-- /TOC -->

## Service architecture
### Services - Business / Functional
1. __Market data Updater__ - connects to external sources (exchanges, brokers and other data providers), extracts, transforms (to Skyze format) and loads data to Market data stores e.g Artic, CSV, etc - runs hourly
2. __Market Data Store__ - Serves data to other internal services on an adhoc basis - will exract data from various stores e.g. artic, sql, csv etc...
3. Portfolios
4. **Statistics** - Creates _Market_ and _Portfolio_ statistics on back test results
5. **BackTester** - runs _Strategies_ over a _Portfolio_ and collects results
	1. Optimizer
6. Trading
7. **Screener** - runs a screener signal on a portfolio at regular intervals and triggers notifications
8. **Notifications** - Sends SMS, emails etc
4. **Front End** - web and mobile, simple data entry and graphs

### Services - Non-Functional
1. **Services Registrar** - etcd (Opensource used by Kubernetes)
2. **Messaging Bus** between services
3. **User Data Store** - user data
4. **Load Balancer** -
5. **Logging** -
6. **Load Balancer** - HA Proxy


	1. https://finance.google.com/finance#stockscreener
	2. https://finviz.com/futures.ashx
	3. https://www.roburir.com/stock-screener.html
	4. https://www.zacks.com/screening/stock-screener
	5. https://markets.ft.com/data/equities?expandedScreener=true


## Class architecture

2. Indicator
3. Strategy
4. Portfolio

## Data Types

### numpy floating point
Problem with equality with very small differences. Have ben finding this problem
when comparing dataframes of timeseries of the output of indicators. It reports
that 13.84 != 13.84. When the difference is taken it is x*10^-15 ... so very
small diferences. Documented in github:
https://stackoverflow.com/questions/33549193/pandas-dataframe-comparison-and-floating-point-precision
https://stackoverflow.com/questions/5595425/what-is-the-best-way-to-compare-floats-for-almost-equality-in-python

PEP485: https://docs.python.org/3/whatsnew/3.5.html#pep-485-a-function-for-testing-approximate-equality
				https://www.python.org/dev/peps/pep-0485/

Documentation: Python: https://docs.python.org/3/library/math.html
							 Numpy:  https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.isclose.html

#### How python works with FP numbers
Python built on C. FP numbers go from python floating point to C as a fractoin and then back to Python as floating point. These conversions are not always accurate.

Numbers like 1.1 and 2.2 do not have exact representations in binary floating point. End users typically would not expect 1.1 + 2.2 to display as 3.3000000000000003 as it does with binary floating point.

#### Solution - Decimal
https://www.youtube.com/watch?v=JOGPAduCC7c

a = decimal.decimal(7.6)+decimal.decimal(7.9)
a.quantize("0.00")
a.quantize(decimal.Decimal("0.00"), rounding=decimal.ROUND_UP)


## Python

### Language
version 3.6

### Modules
Modules are documented in pipenv file at: https://github.com/SkyzeTrading/Skyze/blob/develop/Pipfile

Key packages are:

__Database__
1. pymongo   http://api.mongodb.com/python/current/index.html
2. AHL Artic https://github.com/manahl/arctic

__Numerical Datatypes and scientific calculations__
1. pandas
2. numpy
3. scipy

__Code quality__
1. pylint

__Visualisation__
1. matplotlib
2. seaborn: statistical data visualization http://seaborn.pydata.org/

### Typing
Dropbox, Facebook have mandated static typing for their Python projets

__Dropbox Pycon Talk__  https://www.youtube.com/watch?v=7ZbwZgrXnwY&feature=youtu.be

__MyPy library__ http://mypy-lang.org/

__Static typing in Python__  https://medium.com/@ageitgey/learn-how-to-use-static-type-checking-in-python-3-6-in-10-minutes-12c86d72677b

### Performance
#### Python function call overhead
Function call overhead in Python is relatively high, especially compared with the execution speed of builtin functions. The overhead in Python is mainly due to the dynamic type checking of function arguments that must be performed before and after the function call. This strongly suggests that, where appropriate, functions should handle data aggregation rather than being called on a per element basis.

Testing in a Jupyter notebook:
http://localhost:8888/notebooks/Jupyter/Function%20Call%20Overhead.ipynb#Function-Call-Overhead

shows 10e8 function calls took 2.4 times longer than a single call over 10e8 iteration

## Virtual Environment
### VE Manager
#### pipenv
https://pypi.python.org/pypi/pipenv

### Development Environment
Set up like: ....

### Production environment
when we have one

## Persistence
### Database
#### MongoDB

### File System
#### csv
#### Excel

## Code Resository
### GitHub
https://github.com/SkyzeTrading/Skyze/

## Code Quality

### Standards
#### PEP8

### Linter
#### PyLint

### General code quality?
#### Code Climate
Used by over 100,000 projects, and analyzing over 2 billion lines of code daily, Code Climate incorporates fully-configurable test coverage and maintainability data throughout the development workflow, making quality improvement explicit, continous, and ubiquitous.

#### PyCodeStyle

#### PyDocStyle

#### coverage.py

## Testing

### Test running
#### nose
test runner and a great one at that. It can run tests created using unittest, py.test or doctest.

##### Doc
 http://nose.readthedocs.io/en/latest/index.html

##### Install
pipenv install nose --dev

#### Test coverage
Nose supports the coverage module:
##### Doc
https://coverage.readthedocs.io/en/coverage-4.4.1/

##### Install
pipenv install coverage --dev


### Unit Testing
#### unittest2


### Continuous Integration
http://docs.python-guide.org/en/latest/scenarios/ci/

## Documentation
### Requirements
1. Would love to get away from boring black and white!
2. General documentation - manuals, how to's, architecture etc
3. Code documentation - inline documentation mapped into code documents
4. Tags - to do 's etc
5. UML and other design tools

### Solutions
#### UML and other modelling
1. Looking for round trip class diagrams
##### StarUML
* generates Python
##### PieNSource
* Reverse engineers an image of a UML diagram

#### Markdown

#### Sphynx
Seems to be a favourite in the pythoniverse
http://www.sphinx-doc.org/en/stable/

#### GitWiki
Do we do this? How does this fit in?

## Project Management

### Requirement Management
#### Trello
https://trello.com/b/VdJ7MOCk/skyze-the-limit
Wishlist / Backlog

### Defect Management

#### Production error reporting
rollbar.com

#### GIT Issues
https://github.com/SkyzeTrading/Skyze/issues

### Work Management
#### Trello
https://trello.com/skyze2

### Team Communications
#### Slack
https://skyze.slack.com

## Deployment Management
### Deployment Process

### Deployment Tools
layout: page
title: “Palmys API Client” 
permalink: /client

# PalmyClient
The client class "PalmyClient" is used for handling API requests and response. For data manipulation read about the <a href="https://mikauser001.github.io/palmy-python/handler#PalmyHandler">PalmyHandler</a>, but also the methods down below.

## Create an instance
#### Before working with the libary, please make sure to have a valid token. 

This is how you're token should look like (64 places)
example_token = 'a58fb2e039ab9a0e2b8b55126d20d8fb9c6926ce05c107c38d8072fe51a69663'

```python
from palmy_python import PalmyClient

# Always keep your api token secure via environment variables
import environ
env = environ.Env()
environ.Env.read_env()
SET_YOUR_TOKEN = env("my_api_token")

# Decide which path you'd like to use
REPLACE_WITH_YOUR_PATH = 'scores-private' 

client = PalmyClient(
  path=REPLACE_WITH_YOUR_PATH,
  token=SET_YOUR_TOKEN,
)

````
You should almost always keep your tokens secure via environment variables. 
For using this snippet make sure to overwrite SET_YOUR_TOKEN or set my_api_token in a .env file.

## Request Methods
#### GET
```python
response = client.get()

# Receive the meta information via meta=True
response_with_meta = client.get(meta=True)

# Set additional requests parameters. 
my_rules_kwargs = {"limit": 5}
reponse_with_kwargs = client.get(**my_rules_kwargs)
```
#### POST
```python

```

#### PUT
```python

```

#### Response
The response is in json format. If you want to change the way the response is handled overwrite the PalmyClient.format() method

## Create your own Client
You often like to initiate your own Client. Here is an example:

```python

class MyResponseBaseClient(PalmyClient):
    """A test client for handling requests and responses"""

    path = "quotes"
    token = REPLACE_WITH_YOUR_TOKEN

    def __init__(self):
        """
        Call PalmyClient via super()
        """
        super().__init__(self.path, self.token)
```
### Overwriting methods
You will often expand the default class functionality. Lets create a function that calculates the averages eps and pe ratio values through .format(). You have to add them to the MyResponseBaseClient, or whatever your Client's name is.

```python

def get_format(self, *args):
  """Handle your different format methods"""
  if self.path == "quotes":
      return self.format(*args)
  elif self.path == "stocks":
      return self.format_stock_response(*args)
      
@staticmethod
def format_stock_response(response, *args, **kwargs):
  """
  New method 
  """
  return response.status_code
  
@staticmethod
def format(response, meta=False):

  # Simple example of calculating stock 
  # averages for eps and pe_ratios

  response = PalmyClient.format(response, meta)

  eps_values = []
  pe_ratio_values = []

  # Iterate the data and append each value.
  # Ignore None entries
  
  for items in response:
      eps = items.get("eps")
      pe_ratio = items.get("pe_ratio")
      if eps:
          eps_values.append(float(eps))
      if pe_ratio:
          pe_ratio_values.append(float(pe_ratio))

  # Work with the iterated data
  len_eps = len(eps_values)
  len_pe = len(pe_ratio_values)
  quote_format = {
      "averagePeRatio": sum(pe_ratio_values) / len_pe,
      "averageEps": sum(eps_values) / len_eps,
      "n_eps": len_eps,
      "n_pg_ratio": len_pe,
  }
  return quote_format

```
### GET request with your new client
If we use .get() with the path "quotes" we receive a dictionary with our averages and their amount of data

```
x = MyResponseBaseClient()
x.path = "quotes"
print(x.get())
>> {'averagePeRatio': 31.074074074074073, averageEps': 5.98196, 'n_eps': 100,'n_pg_ratio': 54}
x.path = "stocks"
print(x.get())
>> 200 OK
```

### Controllbility
If we set path = stocks we still can access get() the same way, but get_format method redirects to format_stock_response.
So as you can see the get_format method can be a nice way to control the formats based on the path you've specified

# Score
Each Score object represents a Palmy Score. The method you'd like to use doesnt matter. Therefore you can use your Score object to either update a known Score or create a new one. <b>The Score class should always help you to avoid plain error messages when sending an api put/post request.</b> Thats why a subclass of a Score comes with some risk. Before creating subclasses please go through <a href="">this section</a>.

## Creating an instance
```python

MyScore = Score(
    components=["IncomeCik", "RatiosDebtRatio", 
    "IncomeEps", "Income", "IncomeSymbol"],
    math_operators=("+", "-", "/", "*"),
    stocks=["MSFT", "AAPL"]
)

```

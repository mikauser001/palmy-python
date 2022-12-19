layout: page
title: "Endpoints"
permalink: /endpoint

# Endpoints
### Setting an endpoint via 'path'
Each path represents the base url. So one PalmyClient instance has one path <br>
  Anyways that does not influence individualisation, since you have full access on the base url.
  See <a href="mikauser001.github.io/palmy-python/base#methods">request methods</a><br>

```
from palmy_python import PalmyClient

client = PalmyClient(
  path='private-scores',
  token=REPLACE_WITH_YOUR_API_TOKEN,
)
````
### Available paths
<table>
  <tr>
    <th>endpoint</th>
    <th>url path</th>

  </tr>

  <tr>
    <td>private-scores</td>
    <td>http://127.0.0.1:8000/api/private-scores/</td>
  </tr>
    <tr>
    <td>tickers</td>
    <td>http://127.0.0.1:8000/api/tickers/</td>
  </tr>
    <tr>
    <td>technicals-analysis</td>
    <td>http://127.0.0.1:8000/api/technicals/analysis/</td>
  </tr>

  <tr>
    <td>public-scores</td>
    <td>http://127.0.0.1:8000/api/public-scores/</td>
  </tr>
    <tr>
    <td>long-term-analysis</td>
    <td>http://127.0.0.1:8000/api/long-term/analysis</td>
  </tr>
    <tr>
    <td>short-term-analysis</td>
    <td>http://127.0.0.1:8000/api/short-term/analysis</td>
  </tr>

</table>




# Python Requestsライブラリの完全ガイド

[![Bright Data Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.jp/)

このチュートリアルでは、WebスクレイピングのためにPythonのRequestsライブラリを扱う方法を紹介します。

- [Requestsライブラリの紹介](#introduction-to-the-requests-library)
- [HTTPメソッド](#http-methods)
- [Requestsのレスポンスオブジェクトの内訳](#breaking-down-a-response-object-from-requests)
- [Python Requestsライブラリによるリクエストのカスタマイズ](#request-customization-with-the-python-requests-library)
- [その他の設定](#other-configurations)

## Requestsライブラリの紹介

[`Requests`](https://github.com/psf/requests)は、Python向けのシンプルでユーザーフレンドリーなHTTPツールキットです。詳細には、HTTPリクエストの送信とレスポンスの処理を、簡単で分かりやすい方法で行うための直感的なAPIを提供します。[GitHubで50k以上のスター](https://github.com/psf/requests)と1日あたり数百万のダウンロードを誇り、RequestsはPythonで最も人気のあるHTTPクライアントを代表しています。

このライブラリが提供する主な機能には、すべてのHTTPメソッドをカバーする包括的なAPI、サーバーからの返信の処理、リクエストのカスタマイズ、ログインメカニズム、セキュア証明書の取り扱いなどがあります。さらに、Python Requestsモジュールはデフォルトで[HTTP/1.1](https://datatracker.ietf.org/doc/html/rfc2616)をサポートします。

### Setting up

Requestsをインストールする最も簡単で推奨される方法は`pip`を使用することです。特に、Requestsライブラリに関連付けられている`pip`パッケージは`requests`です。そのため、次のコマンドでHTTPクライアントをインストールできます。

```sh
pip install requests
```

Pythonスクリプトで`requests`を使用するには、以下の行でインポートします。

```python
import requests
```

素晴らしいです！Requestsパッケージがインストールされ、使用できる状態になりました。

### Use Cases

Pythonの`requests`ライブラリの主なユースケースは以下のとおりです。

- **WebサーバーへのHTTPリクエストの送信**: GETリクエストを送信してWebサーバーからデータを取得します。
- **APIの利用**: APIエンドポイントにリクエストを送信してレスポンスを処理し、さまざまなWebサービスと連携してデータにアクセスします。
- **Webスクレイピング**: Webページに関連付けられたHTMLドキュメントを取得し、その後、特定の情報を抽出するために[`BeautifulSoup`のようなライブラリでパース](https://brightdata.jp/blog/how-tos/beautiful-soup-web-scraping)できます。
- **Webアプリケーションのテスト**: HTTPリクエストをシミュレートしてレスポンスを検証し、テストプロセスを自動化してWebサービスが正しく機能することを保証します。
- **ファイルのダウンロード**: 画像、ドキュメント、その他のメディアファイルなどを、対象URLにHTTP `GET`リクエストを送信してWebサーバーから取得します。

**Methods**

次の表で、`requests`ライブラリが公開しているパブリックメソッドをご覧ください。

|     |     |
| --- | --- |
| **Method** | **Description** |
| [`requests.request()`](https://requests.readthedocs.io/en/latest/api/#requests.request) | 指定したメソッドで、指定URLにカスタムHTTPリクエストを送信します |
| [`requests.get()`](https://requests.readthedocs.io/en/latest/api/#requests.get) | 指定URLに`GET`リクエストを送信します |
| [`requests.post()`](https://requests.readthedocs.io/en/latest/api/#requests.post) | 指定URLに`POST`リクエストを送信します |
| [`requests.put()`](https://requests.readthedocs.io/en/latest/api/#requests.put) | 指定URLに`PUT`リクエストを送信します |
| [`requests.patch()`](https://requests.readthedocs.io/en/latest/api/#requests.patch) | 指定URLに`PATCH`リクエストを送信します |
| [`requests.delete()`](https://requests.readthedocs.io/en/latest/api/#requests.delete) | 指定URLに`DELETE`リクエストを送信します |
| [`requests.head()`](https://requests.readthedocs.io/en/latest/api/#requests.head) | 指定URLに`HEAD`リクエストを送信します |

これらは、最も有用なHTTPリクエストメソッドをカバーしています。使い方の詳細は、[公式APIドキュメント](https://requests.readthedocs.io/en/latest/api/)をご確認ください。

## HTTP Methods

HTTPの`GET`、`POST`、`PUT`、`DELETE`、`HEAD`メソッドを扱う際の、Python `requests`ライブラリの動作を確認します。

### GET

HTTPでは、[`GET`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)メソッドはサーバーから特定のリソースを要求するために使用されます。`requests.get()`でHTTP `GET`リクエストを行う方法は以下のとおりです。

```python
import requests

# send a GET request to the specified URL

response = requests.get('https://api.example.com/data')
```

以下のように`requests.request()`でも同じ結果を得られます。

```python
import requests

response = requests.request('GET', 'https://api.example.com/data')
```

この場合、追加の文字列変数で使用するHTTPメソッドを手動で指定する必要があります。

### POST

HTTP `POST`メソッドは、追加処理のためにデータをサーバーへ送信するために使用されます。`requests.post()`で`POST`リクエストを行う方法は以下のとおりです。

```python
import requests

# data to be sent in the POST request

product = {

'name': 'Limitor 500',

'description': 'The Limitor 500 is a high-performance electronic device designed to regulate power consumption in industrial settings. It offers advanced features such as real-time monitoring, adjustable settings, and remote access for efficient energy management.',

'price': 199.99,

'manufacturer': 'TechCorp Inc.',

'category': 'Electronics',

'availability': 'In Stock'

}

# send a POST request to the specified URL

response = requests.post('https://api.example.com/product', data=product)
```

`GET`リクエストと比べると、今回は`data`オプションでサーバーに送信するデータも指定する必要があります。`requests`はこのデータをHTTPリクエストのボディに追加します。

JSONボディの場合は、`data`ではなく`json`オプションにデータオブジェクトを渡します。

```python
response = requests.post('https://api.example.com/product', json=product)
```

同様に、`request.request()`で同じリクエストを行うには次のとおりです。

```python
import requests

product = {

'name': 'Limitor 500',

'description': 'The Limitor 500 is a high-performance electronic device designed to regulate power consumption in industrial settings. It offers advanced features such as real-time monitoring, adjustable settings, and remote access for efficient energy management.',

'price': 199.99,

'manufacturer': 'TechCorp Inc.',

'category': 'Electronics',

'availability': 'In Stock'

}

response = requests.request('POST', 'https://api.example.com/product', data=product)
```

### PUT

[`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT)メソッドは、サーバー上のリソースを更新または置換するために使用されます。Pythonの`requests`モジュールで`PUT`リクエストを送るのは簡単で、POSTリクエストと似たパターンに従います。変更点は、使用するメソッドが`requests.put()`になることです。また、`requests.request()`内のHTTPメソッド文字列は`'PUT'`になります。

### PATCH

[`PATCH`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH)メソッドは、オンラインリソースに部分的な変更を適用するために使用されます。`PUT`リクエストと同様に、Pythonの`requests`ライブラリで`PATCH`リクエストを送る方法はPOSTリクエストに似ています。変更点は、使用するメソッドが`requests.patch()`であり、`requests.request()`内のHTTPメソッド文字列が`'PATCH'`になることです。

### DELETE

[`DELETE`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE)メソッドは、指定されたURIで識別されるリソースを削除するために使用されます。`delete()`メソッドを使用して`requests`でHTTP `DELETE`リクエストを行う方法は以下のとおりです。

```python
import requests

# send a DELETE request for the product with id = 75

response = requests.delete('https://api.example.com/products/75')
```

同様に、`requests.request()`でもDELETEリクエストを実行できます。

```python
import requests

response = requests.request('DELETE', 'https://api.example.com/products/75')
```

### HEAD

[`HEAD`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD)メソッドは`GET`に似ていますが、実際のボディ内容は要求せず、レスポンスのヘッダーのみを要求します。そのため、`HEAD`リクエストに対してサーバーが返すレスポンスは、ボディデータがないことを除き`GET`リクエストのものと同等です。

PythonでHTTP `HEAD`リクエストを行うには`requests.head()`を使用します。

```python
import requests

# send a HEAD request to the specified URL

response = requests.head('https://api.example.com/resource')
```

同様に、`requests.request()`でも`HEAD`リクエストを実行できます。

```python
import requests

response = requests.request('HEAD', 'https://api.example.com/resource')
```

## Requestsのレスポンスオブジェクトの内訳

レスポンスオブジェクトの扱い方を見ていきます。

### Response Object

HTTPリクエストを行うと、`requests`はサーバーからレスポンスを受け取り、それを特別な[Response](https://requests.readthedocs.io/en/latest/api/#requests.Response)オブジェクトにマッピングします。

以下のPython `requests`の例をご覧ください。

```python
import requests

response = requests.get('http://lumtest.com/myip.json')

print(response)
```

これは次を返します。

```python
<Response [200]>
```

`response`は、いくつかの便利なメソッドとプロパティを公開している`Response`オブジェクトです。次のセクションで重要なものを確認していきましょう。

> **Warning**:
> `requests`は常にレスポンスを返すとは限りません。エラー（例: 無効なURL）の場合、`RequestException`を送出します。以下のロジックでこの例外に備えてください。

```python
try:

response = requests.get('http://lumtest.com/myip.json')

# handle the response

except requests.exceptions.RequestException as e:

print('An error occurred during the request:', e)
```

### Status Codes

[レスポンスステータスコード](https://brightdata.jp/blog/proxy-101/proxy-error-codes)は、リクエストの成功、失敗、またはその他の状態を示すためにサーバーが返す標準化された値です。

これらはエラーハンドリングで特に役立ち、クライアントがさまざまな種類のエラーを適切に識別して処理できるようにします。たとえば、`4xx`ステータスコードはクライアント側のエラー（例: 無効なリクエスト）を示し、`5xx`ステータスコードはサーバー側のエラーを示します。

一般に、Pythonで`requests`ライブラリを使用してレスポンスを処理する際、ステータスコードの確認は最初のステップです。リクエスト後は常にレスポンスのステータスコードを確認し、リクエストが成功したかどうかを判断するべきです。ステータスコードはレスポンスオブジェクトの`status_code`属性から取得します。

```python
response.status_code # 200
```

受け取ったステータスコードに応じて、条件分岐で異なるシナリオを適切に処理する必要があります。

```python
import requests

response = requests.get('http://lumtest.com/myip.json')

# check if the request was successful (status code 200)

if response.status_code == 200:

print('Successful request!')

# handle the response...

elif response.status_code == 404:

print('Resource not found!')

else:

print(f'Request failed with status code: {response.status_code}')
```

多くのシナリオでは、成功したリクエストとエラーレスポンスを区別するだけで十分です。`requests`はカスタムの[`__bool()__`](https://www.pythontutorial.net/python-oop/python-__bool__/)オーバーロードにより、そのプロセスを簡素化します。具体的には、`Response`オブジェクトを条件式で直接使用できます。これは、ステータスコードが`200`〜`399`の間であれば`True`、それ以外は`False`と評価されます。

言い換えると、次のロジックでリクエストの成功をチェックできます。

```
if response:

print('Successful request!')

# handle the response...

else:

print(f'Request failed with status code: {response.status_code}')
```

### Response Headers

サーバーレスポンスのヘッダーは`headers`属性からアクセスします。

```python
import requests

response = requests.get('http://lumtest.com/myip.json')

response_headers = response.headers

print(response_headers)
```

これは次を出力します。

```
{'Server': 'nginx', 'Date': 'Thu, 09 May 2024 12:51:08 GMT', 'Content-Type': 'application/json; charset=utf-8', 'Content-Length': '279', 'Connection': 'keep-alive', 'Cache-Control': 'no-store', 'Access-Control-Allow-Origin': '*'}
```

ご覧のとおり、`response.headers`は辞書のようなオブジェクトを返します。つまり、キーでヘッダー値にアクセスできます。たとえば、レスポンスの`Content-Type`ヘッダーにアクセスしたいとします。以下のように行えます。

```python
response_headers['Content-Type'] # 'application/json; charset=utf-8'
```

HTTP仕様ではヘッダーは大文字・小文字を区別しないため、`requests`では大文字小文字を気にせずアクセスできます。

```python
response_headers['content-type'] # 'application/json; charset=utf-8'
```

### Response Content

`requests`は、レスポンスのペイロードにアクセスするための複数の属性とメソッドを提供します。

- [`response.content`](https://requests.readthedocs.io/en/latest/api/#requests.Response.content): レスポンスの内容をbytesで返します。
- [`response.text`](https://requests.readthedocs.io/en/latest/api/#requests.Response.text): レスポンスの内容をUnicodeの文字列として返します。
- [`response.json()`](https://requests.readthedocs.io/en/latest/api/#requests.Response.json): レスポンスのJSONエンコードされた内容をdictionaryで返します。

以下の例で動作を確認してください。

```python
import requests

response = requests.get('http://lumtest.com/myip.json')

# access the response as bytes

response_bytes = response.content

print(type(response_bytes))

print(response_bytes)

print()

# retrieve the response as text

response_text = response.text

print(type(response_text))

print(response_text)

print()

# retrieve the response as a JSON-encoded dictionary

response_json = response.json()

print(type(response_json))

print(response_json)

print()
```

`http://lumtest.com/myip.json`は、呼び出し元のIPに関する情報を返す特別なエンドポイントです。上記スニペットの結果は次のようになります。

```json
<class 'bytes'>

b'{"ip":"45.85.135.110","country":"US","asn":{"asnum":62240,"org_name":"Clouvider Limited"},"geo":{"city":"Ashburn","region":"VA","region_name":"Virginia","postal_code":"20149","latitude":39.0469,"longitude":-77.4903,"tz":"America/New_York","lum_city":"ashburn","lum_region":"va"}}'

<class 'str'>

{"ip":"45.85.135.110","country":"US","asn":{"asnum":62240,"org_name":"Clouvider Limited"},"geo":{"city":"Ashburn","region":"VA","region_name":"Virginia","postal_code":"20149","latitude":39.0469,"longitude":-77.4903,"tz":"America/New_York","lum_city":"ashburn","lum_region":"va"}}

<class 'dict'>

{'ip': '45.85.135.110', 'country': 'US', 'asn': {'asnum': 62240, 'org_name': 'Clouvider Limited'}, 'geo': {'city': 'Ashburn', 'region': 'VA', 'region_name': 'Virginia', 'postal_code': '20149', 'latitude': 39.0469, 'longitude': -77.4903, 'tz': 'America/New_York', 'lum_city': 'ashburn', 'lum_region': 'va'}}
```

3つの異なるレスポンス形式に注目してください。dictionaryとして取得できる`response.json()`は、データアクセスを簡素化するため特に便利です。

```python
response_json['country'] # 'US'
```

詳細については、[PythonでJSONをパースする方法](/blog/how-tos/parse-json-data-with-python)のガイドをご覧ください。

### Response Cookies

[HTTP Cookie](/blog/web-data/http-cookies)はヘッダーで定義されますが、`Response`オブジェクトはCookieを扱うための特別な`cookies`属性を提供します。これは、サーバーが返したCookieを含む[http.cookiejar](https://docs.python.org/3/library/http.cookiejar.html)オブジェクトを返します。

Python `requests`ライブラリでレスポンスオブジェクトからCookieにアクセスする方法を示す以下の例をご覧ください。

```python
import requests

# define the login credentials

credentials = {

'username': 'example_user',

'password': 'example_password'

}

# send a POST request to the login endpoint

response = requests.post('https://www.example.com/login', data=credentials)

# access the cookies set by the server

cookies = response.cookies

# print the cookies received from the server

for cookie in cookies:

print(cookie.name, ':', cookie.value)
```

上記のサンプルスニペットは、次のような出力になる場合があります。

```
session_id : be400765483cf840dfbbd39

user_id : 7164

expires : Sat, 01 Jan 2025 14:30:00 GMT
```

## Python Requestsライブラリによるリクエストのカスタマイズ

HTTPリクエストには、特別なフィルタリングパラメータやカスタムヘッダーが含まれることがよくあります。`requests`でそれらを指定する方法を見ていきます。

### Query String Parameters

[クエリパラメータ](https://www.semrush.com/blog/url-parameters/)（URLパラメータとも呼ばれます）は、HTTPリクエストにおいてURLの末尾に追加される追加パラメータです。これらは、通常データのフィルタリング方法やレスポンスのカスタマイズ方法など、リクエストに関する追加情報をサーバーに提供します。

次のURLを考えてください。

`https://api.example.com/data?key1=value1&key2=value2`

この例では、`?key1=value1&key2=value2`がクエリ文字列で、`key1`と`key2`がクエリパラメータです。

クエリ文字列は`?`で始まり、イコール（`=`）で区切られたキーと値のペアで構成され、`&`で連結されます。このクエリ文字列をPythonコードでプログラム的に指定するのは、特にオプションパラメータを扱う場合、必ずしも簡単ではありません。そのため`requests`は`params`オプションを提供しています。

```python
import requests

# define query parameters as a dictionary

params = {

'page': 1,

'limit': 10,

'category': 'electronics'

}

# send a GET request to the following URL:

# 'https://api.example.com/products?page=1&limit=10&category=electronics'

response = requests.get('https://api.example.com/products', params=params)
```

同様に、パラメータをタプルのリストとして`requests`に渡すこともできます。

```python
import requests

# define query parameters as a list of tuples

params = [

('page', '1'),

('limit', '10'),

('category', 'electronics')

]

response = requests.get('https://api.example.com/products', params=params)
```

または`bytes`文字列として渡すこともできます。

```python
import requests

# define query parameters as a bytes string

params = b'page=1&limit=10&category=electronics'

response = requests.get('https://api.example.com/products', params=params)
```

### Request Headers

`requests`でHTTPリクエストのヘッダーをカスタマイズするには、`headers`オプションにdictionaryとして渡します。たとえば、`requests`でカスタムの`User-Agent`文字列を設定するには次のとおりです。

```python
import requests

# define custom headers

custom_headers = {

'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36',

# other headers...

}

# send a GET request with custom headers

response = requests.get('https://api.example.com/data', headers=custom_headers)
```

### Request Cookies

HTTP Cookieはヘッダー経由でサーバーに送信されますが、`requests`はそれらをカスタマイズするための専用の`cookies`オプションを提供します。以下の例のように使用します。

```python
# define custom cookies

custom_cookies = {

'session_id': 'be400765483cf840dfbbd39',

'user_id': '7164'

}

# send a GET request with custom cookies

response = requests.get('https://www.example.com', cookies=custom_cookies)
```

`cookies`はdictionaryまたは`http.cookiejar`オブジェクトを受け付ける点に注意してください。

## その他の設定

`request`は豊富なAPIを提供しており、多くの高度なテクニックが利用可能です。ここでは特に関連性の高いものをいくつか紹介します。

### Proxy Setup

`requests`におけるプロキシ統合により、HTTPリクエストをプロキシサーバー経由でルーティングできます。これは、IPアドレスを隠したり、レート制限を回避したり、ジオ制限されたコンテンツへアクセスしたりするための強力な仕組みです。

Python `requests`ライブラリでプロキシサーバーを統合するには、`proxies`オプションを使用します。

```python
import requests

# define the proxy settings

proxy = {

'http': 'http://username:[email protected]:8080',

'https': 'https://username:[email protected]:8080'

}

# Make a request using the proxy

response = requests.get('https://www.example.com', proxies=proxy)
```

完全なチュートリアルについては、[Python Requestsでプロキシを使用する](/blog/proxy-101/proxy-with-python-requests)ガイドをご覧ください。

### Basic Authentication

[HTTPログインメカニズム](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)（より一般に「basic login mechanisms」として知られています）は、HTTPプロトコルに組み込まれたシンプルなログインメカニズムのスキームです。これは、[Base64](https://developer.mozilla.org/en-US/docs/Glossary/Base64)形式でエンコードしたユーザー名とパスワードを`Authorization`ヘッダーで送信するものです。

`Authorization`ヘッダーを手動で設定して実装することもできますが、`requests`はそのための専用の`auth`オプションを公開しています。これはユーザー名とパスワードを含むタプルを受け取ります。Python `requests`ライブラリでbasic login mechanismsを扱うには次のようにします。

```python
import requests

# define the username and password for basic login mechanisms

username = 'sample_username'

password = 'sample_password'

# send a GET request with basic login mechanisms

response = requests.get('https://api.example.com/private/users', auth=(username, password))
```

### SSL Certificate Verification

SSL証明書の検証は、インターネット上でクライアントとサーバー間の安全な通信を確保するために重要です。一方で、宛先サーバーを信頼しており、検証を強制する必要がない状況もあります。

特に、プロキシサーバー経由でHTTPトラフィックをルーティングしている場合、SSL証明書に関連するエラーが発生することがあります。この場合、SSL証明書の検証を無効化する必要があるかもしれません。`requests`では、[`verify`](https://requests.readthedocs.io/en/latest/user/advanced/#ssl-cert-verification)オプションでそれが可能です。

```python
import requests

# send a GET request to a website with SSL certificate verification disabled

response = requests.get('https://api.example.com/data', verify=False)
```

### Timeouts

デフォルトでは、requestsはサーバーの応答を無期限に待機します。サーバーが過負荷状態であったり、ネットワークが低速化していたりすると、この挙動が問題になることがあります。

到着しないかもしれないレスポンスを待つことでアプリケーションが遅くなるのを避けるために、`requests`には[`timeout`](https://requests.readthedocs.io/en/latest/user/advanced/#timeouts)オプションがあります。これはレスポンスを待機する秒数を表す整数または浮動小数を受け付けます。

```python
import requests

# timeout after 2 second

response1 = requests.get("https://api.example.com/data", timeout=2)
```

または、`timeout`は2要素のタプル（接続タイムアウト、読み取りタイムアウト）も受け付けます。以下の例のように指定します。

```python
import requests

# timeout after 2.5 seconds for connections and 4 seconds for reading response

response = requests.get("https://api.example.com/data", timeout=(2.5, 4))
```

指定した接続タイムアウト内に接続が確立し、読み取りタイムアウト内にデータを受信できた場合、レスポンスは通常どおり返されます。そうでない場合、リクエストがタイムアウトすると`Timeout`例外が送出されます。

```python
import requests

from requests.exceptions import Timeout

try:

response = requests.get("https://api.example.com/data", timeout=(2.5, 4))

except Timeout:

print("The request timed out")
```

## Conclusion

Pythonの`requests`モジュールは、複数のユースケースをカバーする便利で人気のあるHTTPライブラリです。しかし、HTTPリクエストを行うと公開IPが露出します。これにより、あなたが誰でどこに住んでいるかといった情報が提供されることになり、プライバシーの観点で好ましくありません。

IPアドレスを隠す方法はいくつかありますが、より高いセキュリティとプライバシーを実現する最も効果的な方法はプロキシサーバーを使用することです。Bright Dataは世界最高の[proxy servers](https://brightdata.jp/proxy-types)を提供しており、Fortune 500企業や20,000社を超える顧客に利用されています。

今すぐ登録して無料トライアルを開始してください！
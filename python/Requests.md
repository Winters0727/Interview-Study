# Requests

출처 : https://docs.python-requests.org/en/master/user/quickstart/

### 요청을 만들자(Make a request)

```python
import requests

r = requests.get('http://api.github.com/events')
r = requests.post('https://httpbin.org/post', data = {'key':'value'})
r = requests.put('https://httpbin.org/put', data = {'key':'value'})
r = requests.delete('https://httpbin.org/delete')
r = requests.head('https://httpbin.org/get')
r = requests.options('https://httpbin.org/get')
```



### URL에 파라미터 건네기(Passing Parameters in URLs)

requests의 params 인자에 dictionary를 넣으면 query string으로 URL에 추가된다.

```python
import requests

payload = {'key1' : 'value1', 'key2' : 'value2'}
r = requests.get('https://httpbin.org/get', params = payload)
print(r.url) # https://get?key2=value2&key1=value1
```

value가 `None`인 dictionary는 무시된다. 다음과 같이 리스트로 파라미터를 넘길 수도 있다.

```python
payload = {'key1' : 'value1', 'key2' : ['value2', 'value3']}
r = requests.get('https://httpbin.org/get', params = payload)
print(r.url) # https://httpbin.org/get?key1=value1&key2=value2&key2=value3
```



### 응답 컨텐츠(Response Content)

서버의 응답 컨텐츠를 읽을 수 있다.

```python
import requests

r = requests.get('https://api.github.com/events')
print(r.text) # [{"repository" : {"open_issue" : 0, url : "https://github.com/..."}}]
```

Requests는 서버로부터 온 응답을 자동으로 디코딩한다. 대부분의 유니코드 문자열은 원활하게 디코딩된다.

요청을 보낼 때, Requests는 http 헤더에 기반하여 어떻게 응답 컨텐츠를 인코딩할지 정한다. 텍스트 인코딩 방식은 `r.text`로 컨텐츠 텍스트에 접근할 때 사용된다. `r.encoding` 프로퍼티로 어떤 인코딩 방식이 사용되었는지 확인할 수 있다.

인코딩 방식을 바꾸면 `r.text`를 호출할 때마다 해당 인코딩 방식이 적용된다. 컨텐츠에 따라 적절한 인코딩 방식을 설정하고 싶다면 `r.content`에 접근하여 현재 인코딩 방식을 확인하고, `r.encoding`으로 인코딩 방식을 지정한 뒤, `r.text`로 결괏값에 접근하면 된다.

또한, Requests는 필요에 따라 커스텀 인코딩 방식을 추가할 수 있다.



### 이진 응답 컨텐츠(Binary Response Content)

비텍스트 요청을 위한 bytes 응답에도 접근 가능하다.

```python
from PIL import Image
from io import BytesIO

i = Image.open(BytesIO(r.content))
```



### JSON 응답 컨텐츠(JSON Response Content)

JSON 디코더를 사용하여 JSON 데이터를 다룰 수 있다.

```python
import requests

r = requests.get('https://api.github.com/events')
r.json() # [{"repository" : {"open_issue" : 0, url : "https://github.com/..."}}]
```

JSON 디코딩에 실패할 경우, `r.json()`은 예외를 발생시킨다.

여기서 알아둬야할 것은 `r.json()` 호출의 성공은 응답의 성공을 가리키는 것이 아니라는 점이다. 몇몇 서버는 상태 코드가 아닌 JSON 객체를 반환하여 실패 응답을 보내기 때문이다. 이런 경우에는 JSON 객체가 디코딩되어 반환될 것이다. 요청의 성공 여부를 확인하고 싶다면 `r.raise_for_status()`나 `r.status_code`를 확인해야한다.



### 원 응답 컨텐츠(Raw Response Content)

서버로부터 보내진 스트림 형태의 응답을 받고 싶을 때는 `r.raw`에 접근하면 된다. 또한, 요청 과정에서 `stream=True` 파라미터를 추가해야한다.

```python
r = requests.get('https://api.github.com/events', stream = True)

print(r.raw) # <urllib3.response.HTTPResponse object at 0x101194810>
print(r.raw.read(10)) # '\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x03'
```

일반적으로 다음과 같은 패턴을 통해 파일 스트림을 받아 파일을 저장한다.

```python
with open(filename, 'wb') as fd:
    for chunk in r.iter_content(chunk_size=128):
        fd.write(chunk)
```

`Response.iter_content`을 사용하면 스트림 데이터를 원하는대로 다룰 수 있으며, 굳이 `Response.raw`에 직접 접근하지 않아도 된다. 스트리밍 다운로드를 할 때, 상기와 같은 방법이 권장하는 방법이다. `chunk_size`는 케이스에 따라 조정하면 된다.
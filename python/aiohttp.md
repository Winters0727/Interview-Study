# aiohttp - Client

### 요청 생성하기

```python
import aiohttp
import asyncio

async def main(): # 각각의 요청에 대해 세션을 만들지 말 것!
    async with aiohttp.ClientSession() as session: # 클라이언트 세션 설정
        async with session.get('http://httpbin.org/get') as response: # 클라이언트 응답
            print(response.status)
            print(await response.text())

loop = asyncio.get_event_loop()
loop.run_until_complete(main())

'''
session.post('http://httpbin.org/post', data=b'data')
session.put('http://httpbin.org/put', data=b'data')
session.delete('http://httpbin.org/delete')
session.head('http://httpbin.org/get')
session.options('http://httpbin.org/get')
session.patch('http://httpbin.org/patch', data=b'data')
'''
```

필수는 아니지만 `await session.close()` 메서드를 호출해서 세션을 종료한다.

```python
session = aiohttp.ClientSession()
async with session.get('...'):
    # ...
    await session.close()
```



### URL로 파라미터 넘기기

```python
async with aiohttp.ClientSession() as session:
    params = {'key1' : 'value1', 'key2' : 'value2'}
    async with session.get('http://httpbin.org/get',
                          params=params) as response:
        expect = 'http://httpbin.org/get?key2=value2&key1=value1'
        assert str(response.url) == expect
        
params = [('key1', 'value1'), ('key2', 'value2')]
async with session.get('http://httpbin.org/get',
                       params=params) as response:
    expect = 'http://httpbin.org/get?key2=value2&key1=value1'
    assert str(response.url) == expect
```


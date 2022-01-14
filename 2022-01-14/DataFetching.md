# react-query vs swr

https://github.com/tannerlinsley/react-query
https://react-query.tanstack.com/installation

reactQuery

https://github.com/vercel/swr

https://swr.vercel.app/ko/docs/getting-started

swr

```
전역상태 를 건드리지 않고 react 어플리케이션의 data 를 패치(fetch) 캐시(cache)를 하고 update 를한다. 즉, react 의 서버상태를 가져오고, 캐싱하고 , 동기화하고 업데이트 하는 작업을 쉽게 만들어준다.
비동기 로직을 쉽게 다루게 해주는 라이브러리
reactQuery  요약

작은 보일러플레이트
saga 에서처럼 비동기로 관련된 성공, 실패 액션을 하나하나 모두 선언해서
장황스럽게 정리할 필요가 없다. useQuery 를 통해서 만들어진 query는
고유한 key 로 구분이 되어있어서 여러개의 쿼리를 컴포넌트 곳곳에다가
흩 뿌려놓아도 key 만 같으면 동일한 쿼리와 데이터에 접근할 수 있다.



```

초기세팅

[swr] 1.설치

```
yarn add swr
```

```
import useSWR from 'swr'

function Profile () {
  const { data, error } = useSWR('/api/user/123', fetcher)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>

  // 데이터 렌더링
  return <div>hello {data.name}!</div>
}
```

[react-query]

1.설치

```
yarn add react-query
```

2.App.js 에서 Context Provider로
이하 컴포넌트를 감싸고 queryClient 를 내려보내준다
==> context 는 웹에서 비동기 요청을 알아서
처리하는 background 계층이 된다.

```
import { QueryClient, QueryClientProvider, useQuery } from 'react-query'

const queryClient = new QueryClient()

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
    </QueryClientProvider>
  )
}
```

SWR

```
data 의 패칭(fetching)을 위한 리액트 훅
```

이 두개의 라이브러리들의 공통점에 대해서 얘기를 해보겠다.

[공통점]
1.query

2.caching

3.polling

4.parallel Queries

5.initial Data

6. window focus re-fetching

7.network status re-fetching

[차이점]

```
import {ReactQueryDevtools} from 'react-query/devtools'


function ReactQuery() {
   return (
     <QueryClientProvider client={queryClient}>
       <ReactQueryDevtools initialIsOpen={false} />
     </QueryClientProvider>
   )
 }

```

reactQuery 에서는 Devtool 을 제공하는데 Devtool을 root 안에 전달을 해주기만 하면된다. 제공을 하지만 아쉽게도 SWR 에서는 이런 dev tool 을 가지고 있지 않다.

global Error Handler

SWR 에서는 전역 에러 핸들링을 SWRConfig의 도움으로 설정 할 수 있다.

전역 에러 핸들링을 세팅하기 위해서는 root 을 감싸면 된다.
[SWR]

```
<SWRConfig value={{
    onError : (error, key) => {
       console.log('비상! 비상! 에러 발생!')
    }
}}>
</SWRConfig>
```

특별한 훅을 직접 만들어서 이런 기능을 추가할수도 있다.

react-query 에서도 전역 에러 핸들링 기능을 지원해준다.
전역 설정에서 query cache의 onError 을 사용해야한다.

[React-query]

```
const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error, query) => {
     console.log(`${error.message}`)
    },
  }),
})
```

Mutation Hooks

Apollo 어플리케이션에서 돌연변이를 실행하기 위한 기본 api 이다.
useQuery 와 다르게 useMutation 은 렌더링시 자동으로 작업을 실행하지 않는다.
[SWR]

swr 에서는 mutation 훅을 지원하지 않는 대신에 데이터를 수동으로 조작할 수 있는 옵션이 있다. but mutation 훅 보다는 편리하지 않다.

```
import useSWR , {useSWRConfig} from 'swr'

function Main() {
    const {mutate} = useSWRConfig()

    const onClickHandler = () => {
        logout()
        mutate('/api/main')
    }

    return <button>logout</button>
}

```

[React-query]
react-query 에서는 mutation 훅은 기본으로 지원한다.

```
const mutation = useMutation(newMix => axios.post('/mix' , newTodo))
```

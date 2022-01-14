# react-query vs swr

reactQuery

```
전역상태 를 건드리지 않고 react 어플리케이션의 data 를 패치(fetch) 캐시(cache)를 하고 update 를한다.
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

```
<SWRConfig value={{
    onError : (error, key) => {
        if(error.status !== 403 && error.status !==404) {

        }
    }
}}>
</SWRConfig>
```

특별한 훅을 직접 만들어서 이런 기능을 추가할수도 있다.

react-query 에서도 전역 에러 핸들링 기능을 지원해준다.
전역 설정에서 query cache의 onError 을 사용해야한다.

```
const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error, query) => {
     console.log(`${error.message}`)
    },
  }),
})
```

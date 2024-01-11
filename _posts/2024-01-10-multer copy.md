---
pin: true
layout: post
title: "20240110TIL"
categories:
  - node.js
tags:
  - 패스파인더
author: insidepixce
show_excerpts: true
date: 2024-01-10 12:00:00
disqus: true
paginate: true
disqus-count-scr: true
comments: true
toc: true
---
독감은 독해서 독감이구나

티아이엘.
# 오늘의 피드백

### 컴포넌트의 재사용성

컴포넌트의 재사용성 진짜 중요하다. 최적화와 너무 깊게 연관되어 있기 때문이다. 이번 프로젝트 진행 중 상품 목록 페이지를 개발하면서, 각 상품 카드를 표시하는 부분을 여러 곳에서 사용하여야 했다. 이러한 상황에서 재사용 가능한 컴포넌트를 어떻게 설계하고 최적화하는지에 대한 고민을 많이 하게 되었다.

재사용성을 높이기 위해 컴포넌트들을 작은 단위로 분류하고 props를 통해 데이터를 동적으로 전달하는 방식으로 개발했다. 또한, React.memo 나 PureComponent 와 같은 최적화 기술을 활용하여 불필요한 리렌더링을 방지하고 성능을 향상시키는 방법을 생각해봐야 할 것 같다

### 상태 관리의 복잡성과 미들웨어

위에서 언급한 Redux를 사용하여 상태 관리를 하는데 복잡하더라. 특히 비동기 작업과 관련된 부분이 어려웠다. . Redux Thunk, Redux Saga, Redux Observable 등 미들웨어가 너무 많아서 어떤걸 선택해야 할지 혼란스러웠다. 비동기 작업을 효과적으로 처리하고 상태 관리의 복잡성을 줄이기 위해 Redux Thunk 를 활용하여 API 호출 및 데이터 처리를 구현해보았다. 고르기 진짜 힘들었따

### 상태 최적화와 선택적 리렌더링

→ 대규모 앱에서는 성능 최적화가 중요한 이슈이다. Reselect와 같은 라이브러리를 활용해보았지만 사실 나는 별다른걸 못 느꼈다.

# 오늘의 이슈

1.  **상품 목록을 불러오는 동안 네트워크 연결이 불안정한 상황에서 API 호출이 실패하는 문제가 발생했다.**

→ Redux의 ‘fetchUser.rejected’ 케이스가 트리거되어 오류 메세지를 Redux 상태에 저장했다.’

앱이 서버로 api요청을 보내는 도중에 네트워크 연결이 불안정한 상황이 발생했다. API 호출이 실패하고 대신 오류 응답을 받게 되었다.

→ Redux의 ‘fetchUser.rejected’케이스 트리거: Redux를 사용하여 API 호출을 관리하고 있을 때, Redux Toolkit과 Redux Thunk 등을 활용하여 비동기 액션을 처리하는데, 호출이 실패하면 자동으로 ‘Rejected’케이스가 트리거된다. 이 경우, Redux에서 사용자가 정의한 ‘fetchUser’액션에 대한 ‘rejected’케이스가 실행된다

→’fetchUser.rejected’케이스에서는 API 호출 실패와 관련된 오류 메시지를 포함한 액션 객체가 생성된다. 이 오류 메시지는 Redux 상태에 저장되고, 일반적으로 Redux 상태에는 error 또는 errorMessage와 같은 필드가 있어 오류 메시지를 저장할 수 있다

👰🏻 사실 불안정한 네트워크 연결에 대한 해결책은 아니다. 길이 없는데 어떻게 가겠는가. 그냥, 오류 메시지를 어떻게 담을 것인지 확인했다.

1.  **사용자 인증 토큰이 만료되어 API 요청이 실패했다**

→ 토큰 만료를 감지하고 리프레시 토큰을 사용하여 새로운 액세스 토큰을 얻어 API 요청을 재시도한다

→ 인증 토큰 만료에 대한 오류 핸들링을 통해 사용자에게 다시 로그인을 유도한다

이것도 Redux의 도움을 얻어보았다. fetchUser액션에서 토큰 만료 오류가 발생했을 때 리프레시 토큰을 사용하여 새로운 액세스 토큰을 얻는 과정이다

```
const fetchUser = createAsyncThunk('user/fetchUser', async (_, { dispatch, rejectWithValue }) => {
  try {
    const response = await fetch('/api/user', {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    });
    const data = await response.json();
    return data;
  } catch (error) {
    // 토큰 만료 오류인 경우 리프레시 토큰을 사용하여 재인증 시도
    if (error.message === 'TokenExpiredError') {
      try {
        const newAccessToken = await dispatch(refreshToken());
        // 새로 받은 액세스 토큰을 사용하여 다시 API 요청을 시도
        const response = await fetch('/api/user', {
          headers: {
            Authorization: `Bearer ${newAccessToken}`,
          },
        });
        const data = await response.json();
        return data;
      } catch (refreshError) {
        // 리프레시 토큰으로도 재인증에 실패한 경우
        return rejectWithValue('로그인이 필요합니다.'); // 오류 메시지 반환
      }
    }
    return rejectWithValue(error.message);
  }
});
```

1.  서**버에서 받은 데이터의 유효성을 검사하는 과정에서 실패했다**

외부 데이터 소스에서 데이터를 가져와서 사용하는 중이였는데, 데이터 형식이 일치하지 않아 오류가 발생했다.

데이터 유효성 검사 오류 = 애플리케이션 서버로부터 받은 데이터가 예상과 다른 형식이거나 유효하지 않을 때 발생할 수 있는 어려움이다. → 데이터 형식 명확히 정의하기 : 서버와 클라이언트간에 통신할때 데이터의 형식을 명확히 정의해야 한다.

→ Joi /Yup 과 같은 유효성 검사 라이브러리를 사용한다 : 클라이언트 측에서는 Joi 나 Yup 같은 유효성 검사 라이브러리를 사용하여 데이터의 유효성을 간편하게 검사할 수 있다. 이러한 라이브러리를 활용하면 데이터 스키마를 정의하고 데이터를 검증하는 과정을 간소화할 수 있다

# 화면 전환시에 화면전환 애니를 사용자 정의하고 싶다면?

작업 중에 조금 밋밋했던 부분이 있었다. 리엑트 네이티브에서 화면 전환 애니메이션을 사용자 정의하는 것이 가능하고, ‘react-navigation’이라는 라이브러리를 활용하여 구현할 수 있다’



화면 전환 애니메이션을 구현하려면 ‘createStackNavigator’함수를 사용하여 네비게이션 스택을 만들고, screenOptions 속성을 활용하여 사용자 정의 애니메이션을 설정해줄 수 있다.

{% raw %}
```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator
        initialRouteName="Home"
        screenOptions={{
          cardStyleInterpolator: ({ current, layouts }) => {
            return {
              cardStyle: {
                transform: [
                  {
                    translateX: current.progress.interpolate({
                      inputRange: [0, 1],
                      outputRange: [layouts.screen.width, 0],
                    }),
                  },
                ],
              },
            };
          },
        }}
      >
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
{% endraw %}

화면 전환 시 애니메이션이 오른쪽으로 슬라이드되는 효과를 정의해보았다. ‘cardStyleInterpolator’ 함수 내에서 애니메이션을 정의했다. ‘inputRange’와 ‘outputrange’를 조절하여 다양한 애니메이션 효과를 적용했다

저건 연습 코드였고, 실제 프로젝트에서는 좀 더 복잡한 애니메이션을 구현해줬는데, react-navigation 라이브러리의 다양한 옵션들을 정리해보려 한다.

### 내가 주로 쓸 것 같은 옵션들 모음 (react-navigation )

-   **createStackNavigator - ‘screenOptions’**

스택 네비게이터를 생성할 때 화면 전환에 관련된 옵션 설정가능

‘cardStyleInterpolator : 화면 전환 애니메이션을 사용자 정의할 수 있게 하는 함수를 지정한다. 화면 슬라이드. 페이드, 회전 등 다양한 애니메이션 효과 정의 가능! !

‘headerStyle’: 화면 헤더의 스타일을 설정한다. 배경색, 그림자, 높이 등을 조절할 수 있다

‘headerTintColor’: 헤더 아이콘 및 텍스트의 샞상을 설정한다

‘headerTitleStyle” : 헤더 제목의 스타일 설정
```
-   **‘createBottomTabNavigator’ 및 ‘createMaterialBottomTabNavigator’**
```
탭 네비게이터 생성 시 다양한 옵션을 사용하여 탭의 모양과 동작을 조절할 수 있다. 예를 들어 , ‘ tabBarIcon’을 사용하여 각 탭에 아이콘을 지정하거나, ‘tabBarOptions’를 사용하여 탭바의 스타일을 설정할 수 있다.
```
-   **‘createDrawerNavigator’:**
```
드로어 네비게이터를 생성할 때 ‘drawerContent’나 ‘drawerStyle’등을 사용하여 드로어의 내용과 스타일을 조절할 수 있다

# 데이터베이스 충돌

데이터베이스에 사용자 정보를 저장했다. 근데, 동시에 두 사용자가 동일한 사용자 정보를 수정하려고 시도하면 어떻게 될까 궁금해서 해봤다.

-   사용자 A는 웹 앱에서 자신의 사용자 정보를 수정하려고 한다.

```
const updateUserA = async () => {
	const user = await getUserFromDB(1);\\
	user.age = 31;
	await updateUserInDB(user);
};
```

-   동일한 시간에 사용자 B도 자신의 정보를 수정하려고 한다

```
const updateUserB = async () => {
	const user = await getUIserFromDB(1);
	user.age = 32;
	await updateUserInDB(user);
```

→ 충돌 발생

-   사용자 a와 사용자 b는 동시에 데이터베이스에서 동일한 사용자 정보를 가져와 수정하고 저장하려고 한다. 데이터베이스에는 사용자 a와 b가 동시에 업데이트를 시도하므로 충돌이 발생한다
-   충돌이 발생한 후 , 데이터베이스에서 최정적으로 저장된 사용자 정보를 마지막으로 업데이트해서 반영한다

## 충돌이 해결되는 과정

1.  데이터베이스 업데이트

-   사용자 A와 사용자 B가 동시에 데이터베이스의 사용자 정보를 가져오고 수정합니다.
-   각각의 업데이트는 데이터베이스 트랜젝션 내에서 처리된다

1.  데이터베이스 충돌 감지

-   데이터베이스는 사용자 A와 사용자 B의 업데이트가 동시에 실행되어 충돌이 발생했음을 감지한다

1.  충돌 해결

-   충돌 해결에는 두가지 방법이 있다.
    -   비관적 잠금 (Pessimistic Locking) : 사용자 A가 데이터를 가져올 때 데이터베이스에서 해당 데이터를 잠그고, 사용자 B는 데이터를 가져오는 동안 기다려야 한다. 이후 사용자 A와 B중 먼저 업데이트를 시도한 사용자의 변경 내용이 반영된다
    -   낙관적 잠금(Optimistic Locking) : 사용자 A와 B가 데이터를 가져온 후 업데이트를 시도한다. 데이터베이스에서는 A와 B애가 가져온 데이터의 버전을 비교한다. 만약 버전이 동일하다면 업데이트를 허용하고 , 버전이 다르다면 충돌이 감지되었다고 알린다. 이후 A와 B에게 충돌을 해결하라고 선택권을 주거나, 나중에 다시 시도하도록 유도한다

1.  최종 업데이트

-   충돌이 해결되고 데이터베이스에서 사용자 A와 B의 업데이트를 모두 반영한 후 , 최종 사용자 정보가 데이터베이스에 저장된다
-   이때, 마지막으로 업데이트한 사용자의 변경 내용이 반영되므로, 최종 나이가 32인 경우에는 사용자 B의 업데이트가 마지막으로 반영된 경우이다.

1.  응답 및 결과

-   데이터베이스가 최종 업데이트를 수행한 후 , 사용자 A와 B에게 각각의 요청에 대한 응답을 반환합니다
-   응답은 수정 후의 최종 결과를 나타내며, 데이터베이스에서 저장된 최신 상태를 반영한다

이렇게 충돌이 발생하고 해결되면 최종적으로 데이터베이스에 반영된 결과를 사용자 A와 B에게 반환하여 제공된다. 충돌이 발생할 경우 충돌 해결 전까지 어느 한 사용자의 업데이트가 최종 반영되지 않은 것이다

## 오늘 하루 리뷰 

오늘 몸이 너무 안따라준다. 헤롱헤롱한다. 너무 아프다. 진짜 두통이 너무 심하다. 글자를 읽을 때마다 울렁거린다. 약간 속도 메스껍고 토할 것 같다. 잠이 너무 온다. 죽을 것 같다 진짜. 속도 안좋다. 잠도 많이 온다. 어제부터 죽을 것 같아서 몬스터 흰색 캔을 2개를 땄다. 어쩌겠니... 돈벌어야 하는데...

몸이 너무 아프다. 잠이 너무 많아진다. 이러다가 진짜 죽는건 아닐지 하고 아픈데 열은 또 안 난다. 춥고 덥다. 근데 또 갑자기 미친듯이 먹고싶을때가 있다. 또 밥먹기 싫을때는 미친듯이 먹기 싫다.

이게 무슨 일이람... 내과 가서 물어봐야 하나,,,

글자 보면 울렁거려서 못참겠다니깐.

그래도 해야지 어떡해...그래도 해야지 어떡해.


독감이 괜히 독감이 아닌가봐...독해서 독감이야 
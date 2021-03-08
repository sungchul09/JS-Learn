### 🎇JS 중고급 START!!

---

다음 내용은 인프런 '김영보'의 [자바스크립트 중고급: 근본 핵심 이해](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%A4%91%EA%B3%A0%EA%B8%89/dashboard ) 강의 정리 내용 입니다.

### [SECTION1   중고급 강좌 소개, 범위]

#### L1 - ES3/ES5 스펙의 아키텍처 / 메커니즘 관련 키워드

---

ES3 스팩

1. Execution Contexts

   >  실행 콘텍스트. 함수 호출 시 함수를 실행하는 환경. 함수가 실행 되었을때 결과를 저장하는 영역. 즉, 함수의 모든 처리는 이 안에서 이루어진다.

2. Definition

   * Function Objects

     : 엔진이 Function 키워드를 만났을때 만드는 Function Object를 뜻한다.

   * Types of Executable Code 

      : 실행 가능 코드. (Global, Eval, Function 코드가 있다.)

   * Global, Function Code

      : 둘은 위치만 다르고 코드는 같다.  단, 실행하는 영역이 다름.

   * Variable Instantiation 

      : 변수를 어떻게 인스턴스화 시켜서 해결할 것이냐.

   * Scope Chain and Identifier Resolution:

     * Identifier Resolution
       : 함수를 호출할때 어떻게 함수 이름을 찾을것이냐. 
       변수의 값을 설정할때 어떻게 변수의 이름을 찾을것이냐. (여기서 Id는 함수, 변수 이름을 뜻한다.)

       1. Global Object

          : 전역 객체이다. (Function Object와 구분하는 이유는 영역이 다르기때문이다.)

       2. Activation Object

          : 함수 호출 시 함수를 실행할 수 있는 환경.  
          그리고 함수가 실행되었을 때 실행되는 결과를 저장하는 오브젝트 
          (Object니까 Property이다. 즉,  이 안에는 또 다른 Object가 존재할 수 있다.)

   * This

     : 인스턴스에서 중요한 위치이다.

   * Arguments Object

     함수의 파라미터를 처리하는 오브젝트

3. Entering An Execution Context

   * Global Code
   * Eval Code
   * Function Code

ES5 스펙

1. Executable Code and Execution Contexts

2. Types of Executable Code

   * Strict Mode Code

     : use strict 로 작성했을때의 실행 모드

3. Lexical Environments

   > Lexical(정적), Dynamic(동적)이다.  
   > 자바스크립트는 Lexical 환경을 취한다. 그래서 실행 콘텍스트가 하나의 실행하는 묶음이라면 그 안에서 환경적인 측면을 처리하는것이다.

   * Environment Records

     : 함수가 호출되어 실행될 때, 실행되기 전 상황을 기록

   * Lexical Environment Operations

     : 어떤 변수에 값을 할당했을때 그것을 정적인 환경에 설정한다는 것.

   * The Global Environment

     : Global Object를 처리하는 Environment 

4. Execution Contexts

   > Identifier Resolution: 실행 Context가 실행하는 묶음이면. 식별자 해결은 바탕이 되는 개념. 함수를 호출했을때 어디서 찾을꺼냐? 이러한 개념이 여기 들어가있다.

5. Establishing an Execution Context

   >  Entering이란? 함수 안으로 엔진 컨트롤이 이동했을때 함수 안에 작성된 코드를 어떻게 처리할 것인가. 이러한 개념적인 이야기.

   * Entering Global Code 
   * Entering Eval Code
   * Entering Function Code

6. Declaration Binding Instantiation

   > 바인딩은 변수, this와 관련이다. 
   > 변수를 어떻게 Execution Contexts의 Lexical Environments에 바인딩시켜서 Recording 할것인가.
   > 또한 This를 어떻게 바인딩할 것인가. (This는 함수 안에서만 되는데 ..)
   > 그리고 이것들은 Record 될것이고 Environment에 정리가 될것이다.

7. Arguments Object

   > 파라미터 처리 오브젝트.

   

**함수가 실행되기전에 어떻게 할 것인가. 그리고 함수가 호출되었을때 어떻게 처리할 것인가에 대한 내용이 위에 서술한 모든 내용에 담겨있는것이다.**



#### L2 - 엔진 관점의 핵심 키워드

---

1. 엔진 처리 : 크게 해석, 실행으로 나뉜다.

   * 해석: 컴파일, 실행을 할 환경을 설정
   * 실행: 해석 단계에서 설정된 환경을 바탕으로 코드를 실행하는 것.
     * 함수가 호출 된 다음 단계에 하는것.
     * 키워드는 Context(함수라는 단위를 어떠한 묶음 ,덩어리로 가져갈 것인지 정하는 것)

2. 엔진 관점에서의 부담

   > 단일 Context 개념으로 하나의 box 안에서 모든 함수, 변수 관련된 일을 처리하는 것이 심플하고 빠르다. 여기저기 선언된 변수, 함수를 찾아다니는것은 복잡하고 시간도 오래거린다.
   > 이미 Context 실행중인데 Scope Chain 개념으로 어디가에서 다시 끌고오는 ... 이것은 부담스럽다.

   -> 이것은 궁극적으로 Identifier Resolution(식별자 해결)을 위한것이다. 즉, 함수, 변수이름을 어떻게 빨리 찾고 실행할 것인가.



#### L3 - Execution Context 형태

---

```javascript
function book(){
    var point = 123;   
    function show(){   
        var title = "js책";
        // getPoint();
        // this.bookAmount;
    };
    function getPoint(){
        return point;
    };
    show();
}
book();
```

1. book() 함수 호출 

   1. show function 오브젝트 생성

   2. show의 [[Scope]]에 스코프 설정 (point, getPoint() (변수이름,함수이름)을 설정)
      [[Scope]]: 대괄호 두개([[)는 엔진이 설정하는 프로퍼티를 뜻한다.

   3. getPoint function 오브젝트 생성

   4. show() 호출

      1. 엔진 컨트롤이 show() 함수 안으로 이동

         1. 이동하기전 show 실행 콘텍스트를 만든다. (함수 실행을 위한 Context 환경 구축)

            즉, 하나의 덩어리 안에서 함수가 실행될 수 있도록 만드는 것.

            * 실행 컨텍스트 구성요소

              ``` javascript
              show 실행 콘텍스트(EC): {
              	렉시컬 환경 컴포넌트(LEC): {  //1
                      환경 레코드(ER): {  //3
                          선언적 환경 레코드(DER): {
              				title: "JS책"
                          },
                          오브젝트 환경 레코드(OER): {}
                      },
                      외부 렉시컬 환경 참조(OLER):{
                          point: 123,
                          getPoint: function(){}
                      }
                  },
                  변수 환경 컴포넌트(VEC): {},  //1
                  this 바인딩 컴포넌트(TBC): {  //2
                      글로벌 오브젝트(window)
                  }
              }
              ```

              1. LEC와 같은 레벨에 VEC이 있다. 그리고 LEC, VEC의 초기값은 같다.
              2. TBC를 만든다. (this로 참조할 오브젝트를 바인딩 시키는 것)
              3. 함수코드에서 var title을 만나게되면 ER의 DER의 title:"JS책"을 프로퍼티 형태로 설정
              4. 또한 OER도있지만 나중에 다루자.
              5. show 밖의 변수를 값으로 사용할 수 있다. 이 원리는 show Function Object를 만들때 [[Scope]]에 설정된 것을 OLER에 설정해놨기 때문이다. (point, getPoint)
              6. 함수 밖에있는것이지만 하나의 show 실행 컨텍스트(하나의 덩어리)이기 때문에 내 것처럼 쓸수있는것이다.
              7. this로 참조할 수 있는것은 TBC에 바인딩 시킨다.
              8. show() 오브젝트는 Global Object이다. 하지만 Global Object는 실체가없기때문에 Host Object 개념으로 window 오브젝트를 참조하게된다.

              >  즉, 실행 콘텍스트안에 함수에서 구할 수 있는 값의 덩어리를 만들어 놓는것이다. 
              > 따라서 함수가 메모리위에 올라갔을때, 실행될때 함수에서 다른 값을 구하려고 메모리에서
              > 빠져나오거나 메모리 올리거나 할 필요가없다는 것. 하나의 Context개념으로 묶어놨기 때문이다.

              **:sparkles: :sparkles:이것이 자바스크립트의 Lexical Context(정적 컨텍스트) 개념이다. 이러한 환경을 정적으로 만든다는 것. 이에 맞추어 코드를 작성하면 js엔진이 심플하고 빠르게 처리할 수 있을것이다. 그래서 실행 컨텍스트를 알아야한다.**

            





#### L4 - 식별자 해결

---

1. Identifier Resolution

   : 사용할 변수/함수를 결정하는 것. Resolution의 사전적의미는 해결, 결정이다.

   ```javascript
   var point = 100;
   function getPoint(){
       var point = 200;
       return point;
   };
   
   var result = getPoint();
   console.log(result);
   ```

   1. getPoint() 를 호출하면 point 변수에 값이 200으로 초기화되고 return한다.
   2. 근데 왜 100이아닌 200이 return 될까? Scope 때문이다.
   3. Scope에 설정된 범위안에서 point 변수가있으면 그 값을 할당하고 없으면 그 상위 함수에서 찾아 할당하는 식
   4. 스코프에 이름을 설정한다. 스코프안에서 찾을수있도록. 스코프에 설정된 이름은 변경되지않는다. 단 값으 변경가능하다. 즉, 식별자 해결대상은 이름이다.

2. Scope 용도

   :식별자 해결을 위한 수단과 방법이다. (스코프가 목적이 아님)



#### L5 - Scope Chain / 스펙의 Scope Chain 사용

---

1. .ES3 - Scope Chain

   * 식별자를 검색하기 위한 Object({name:value}) 리스트이다. 
     실행 콘텍스트(Execution Context)와 관련이 있고 식별자 해결(Identifier Resolution)을 위해 사용한다
     (다만 ES5는 Scope는 사용하지만 Scope Chain은 사용하지 않는다.)

   * 1. 함수가 호출되면 Scope를 생성 
     2. 함수의 변수, 함수를 {name: value} 형태로 설정 
     3. Scope Chain에 연결
     4. Scope Chain에서 식별자를 해결 

   * 동적(Dynamic)  처리이다. (ES5는 Lexical이며 Lexical한 화면이 더 빠르다.)

   * ES3 실행 콘텍스트 환경 

     * Scope Chain

     * Activation Object : 함수가 실행될 때 실행한 결과를, 그리고 실행하기 위한 환경을 제공.

       (ES5에는 Activation Object가없고 이에 대응하는 Lexical Environment가 있다.)

2. ES5 - Lexical Enviroment의 Declarative Environment Record에 함수의 변수와 함수 이름을 바인드

   (Scope Chain을 사용하지 않으며 DER에서 변수와 함수 이름을 검색)

   [참고 내용]

   ``` javascript
   show 실행 콘텍스트(EC): {
   	렉시컬 환경 컴포넌트(LEC): {  //1
           환경 레코드(ER): {  //3
               선언적 환경 레코드(DER): {
   				title: "JS책"
               },
               오브젝트 환경 레코드(OER): {}
           },
           외부 렉시컬 환경 참조(OLER):{
               point: 123,
               getPoint: function(){}
           }
       },
       변수 환경 컴포넌트(VEC): {},  //1
       this 바인딩 컴포넌트(TBC): {  //2
           글로벌 오브젝트(window)
       }
   }
   ```

3. Scope Chain은 Scope외에 또다른 Scope를 하나의 Context 개념으로 가져가야하는데 이것을 완전한 하나의 Context 개념으로 보기엔 무리가있다.
4. 하지만, ES5는 완전하게 하나의 콘텍스트 개념으로 가져간다. 이것이 중요한 이유는 함수가 호출되어 메모리에 올라갈때 Context 하나만 올라가면 된다는 이야기이다.
5. 물론 여기에 맞춰 우리가 프로그램 코드를 작성해야한다는 전제가있지만 아키텍쳐가 받쳐준다는건 이야기가 다르다. ES3는 아키텍쳐가 받쳐주지 못했고, ES5는 완전하게 정적환경에서 아키텍쳐가 받쳐준다.



#### L6 - Lexical Environment / var 키워드 문제와 해결 / 동적 환경

1. ES5의 정적환경 (Lexical Environment)

   * 함수가 호출될때 Scope가 설정되는것이 아니라, 
     function 키워드를 만나면 function Object를 생성하고, 스코프가 FO(Function Object)의 [[Scope]]에 결정되는것 (=함수 안이 아니라 함수 밖의 Scope가 결정되는 것)

     ``` javascript
     var point = 123;
     function book(){  // function 키워드를 만나면 Scope가 결정된다.
         function getPoint(){};
     };
     book(); // 호출할때 Scope가 설정되는것이 아니라
     ```

     **그래서 동적이아니라 정적인 것이다.**

   * 함수가 호출되면 ??
     : FO에 설정된 [[Scope]]를 실행 콘텍스트(Execution Context)의 
     렉시컬 환경 컴포넌트(LEC)의 외부 렉시컬 환경 참조(OLER)에 설정한다.

2. ES5의 var 키워드 문제

   * 함수에서 var 키워드를 사용하지않고 변수를 선언하면 Global Object에 설정된다.
     Lexical 환경 구조에 맞지않다.
   * 왜냐하면 지금 Lexical 환경은 함수 안에있는것과 함수 밖에있는 것. 두 개 단계의 계층만 갖고있는데 이것을 몇단계 올라간다. 그리고 그것을 끌고다녀야한다면. Lexical 환경 구조에 맞지 않는 것이다.
   * 그래서 ES5에선 "use strict" 사용으로 그것을 해결한다. use strict 미사용시 var 없이 변수선언을하면 에러 처리된다.
   * ES5에선 use strict 사용이 필수아닌 필수이다. 
   * ES6에선 let, const를 사용해 좀 더 근본적으로 var 변수를 사용하지 않아서 생기는 정적 환경 구조의 문제를 해결. (변수 자체에 스코프 제약을 두어 lexical 환경을 유지하겠다는 것)

3.  동적환경

   * 실행 시점에 스코프 결정. 

     * with 문

       strict 모드에서 에러 발생

     * eval() 함수

       보안에 문제가 있음



#### L7 - Node.js 코드 형태

---

1. Node.js 코드 형태

   * 서버 프로그램 고려 사항 : 동시 접속자 10K (10,000명)

   * JS는 싱글 스레드이다. (하나의 함수가 호출돼 끝나야 다른 함수를 부를 수 있다.)

   * 그런데 Node.js에서 JS는 비동기 처리이다.

     > 하나의 함수가 호출되어 끝나기전에 다른 함수가 시작되는것을 말한다.
     >
     > 순차적으로, 동기적으로 처리하지않게된다.
     >
     > (C++의 semapore, Mutex때문에 비동기처리를 한다.)

   * 이러한 환경에서 Context 형태가 효율성이 높다. 

     - ES3는 Scope Chain과 Activation Object가 메모리에 올라가게된다.
       이것을 실행하는도중에 다른 함수가 호출되면 그 함수도 Scope Chain과 Activation Object를 갖고 올라오게된다. 그러면 그 두개가 왔다갔다하게된다.
     - 만약 이것을 동접 10k에서 실행하게된다면 ?  엄청난 부담.
     - 반면, ES5의 실행 콘텍스트(Execution Context)는 Scope Chain이 없이 딱 하나이다.
       그래서 엔진처리가 더 빠르다

 ✨**실행 콘텍스트에 최적화된 형태로 코드를 작성해야하며 이를 위해 엔진 처리를 이해할 필요가있다.**



#### L8 - 강좌 범위

---

1. 자바스크립트(ES5) 엔진 처리 중심이고 ES5 스펙의 모든 항목을 하나도 빠짐없이 다룬다.
2. 각 항목의 바탕이되는 개념과 이로 인해 파생되는 활용을 다룬다.
3. 기능보다 논리에 중점을 두고 접근한다.
   * Function Object 구조와 생성
   * ✨**Execution Context**
   * ✨**Identifier Resolution**, ✨**Scope**
   * ✨**this**, prototype, OOP
   * Hoisting, Overloading, Argument
   * Recursive, Immediately Invoked Function Expression, Closure



### [SECTION2 function 오브젝트]

---





#### L9 - function 형태 / function 오브젝트 생성 / 오브젝트 저장 / 생각의 전환

---

1. function 형태

   * 빌트인 Function 오브젝트 
     - Function.prototype.call()  : 빌트인 Function 오브젝트 prototype의 메소드와 연결된 형태
   * function 오브젝트
     * function book() {...} : function 키워드를 사용해 함수를 선언
     * var book = function(){...}: function 키워드를 사용해 function 오브젝트를 만들어 변수에 할당하는 형태
     * 인스턴스지만, new 연산자로 생성한 인스턴스와 구분하기위해 강좌에서 function 오브젝트로 표기
   * function 인스턴스
     * new Book(): new 연산자를 사용하여 Book.prototype에 연결된 메소드로 생성

2. function 오브젝트 생성

   ```javascript
   var book = function(){...};
   ```

   * 엔진이 function 키워드를 만나면,
     빌트인 Function 오브젝트의 prototype에 연결된 메소드로 function 오브젝트를 생성한다.
   * 생성한 오브젝트를 book 변수에 할당.
   * book() 형태로 호출. (코드는 단지 Text이다. function 키워드를 만나 엔진이 function 오브젝트를 만들었기때문에 호출할 수 있는것이다. )

3. 오브젝트 저장

   * 함수를 호출하려면 생성한 function 오브젝트를 저장해놔야한다.
   * function 오브젝트 저장형태는 {name:value} 형태이다.
     ex) {book: 생성한 function 오브젝트} 형태이다.
   * 함수를 호출하면
     * 저장된 오브젝트에서 함수 이름(book)으로 검색
     * value값을 구하고
     * value가 function 오브젝트이면 호출한다. (number이면 연산을하고 String이면 문자열처리를 할수있고 등등 ...)

4. 생각의 전환

   * 함수가 호출되면,
     * 엔진은 함수의 변수, 함수를 {name: value} 형태로 실행 환경을 설정한다. 
     * 그리고 함수코드를 실행한다.
   * function(){} 코드를 보면 함수의 변수와 함수가 {name:value} 형태로 연상되어야한다. (엔진 관점)

✨프로퍼티({name:value}) 형태로 생각을 전환해야 JS의 아키텍처와 메커니즘을 쉽게 이해할 수 있다. 
내가 만약 엔진이 된것처럼 key, value({name:value})관점에서 보는것이다.



#### L10 - function 오브젝트 생성 과정 / function 오브젝트 구조

---

1. function 오브젝트 생성 과정

   * function sports(){...} 형태에서 function 키워드를 만나면
   * 오브젝트를 생성하고 저장한다. ({sports:  {...}}의 형태 )
   * sports는 function 오브젝트 이름,   오브젝트{...}에 프로퍼티가 없는 상태이다.

   ```javascript
   >sports: f ()
   	arguments: (...)
       caller: (...)
   	name: "sports"
      >prototype: {constructor: f}
   	 >__proto__:          // 빌트인 Object 관련 메소드
          >constructor: f Object()
          >hasOwnProperty: f hasOwnProperty()
          >propertyIsEnumerable: f hasOwnProperty()
          >toLocaleString: f hasOwnProperty()
          >toString: f hasOwnProperty()
          >valueOf: f hasOwnProperty()
   		...
      >__proto__: f ()       // 빌트인 Function 관련 메소드
       >apply: f apply()
        arguments: (...)
       >bind: f bind()
       >call: f call()
       ...
       [[FunctionLocation]]: ...
      >[[Scopes]]: Scopes[1]
     >this: Window
   >Global
                  
   ```

   * 위와 같은 구조의 function 오브젝트 생성과정에대해 알아보자

     ```javascript
     sports = {
         prototype: {
             constructor: sports
             __proto__: {}
         }
     }
     ```

     1. sports 오브젝트에 prototype 오브젝트 첨부

     2. prototype에 constructor 프로퍼티를 첨부한다.
        * prototype.constructor가 sports 오브젝트 참조
     3. prototype에  __ proto __ 오브젝트 첨부
        * ES5 스펙에 __ proto __가 기술되어 있지 않으며 ES6 스펙에 기술되어있다.
          그리고 엔진이 사용한다는 늬앙스로 정의되어있다.
        * ES5 기준으로 보면 표준이 아니지만 2000년대 초부터 파이어폭스에서 사용했다.

     4. 빌트인 Object.prototype의 메소드로 Object 인스턴스를 생성하여
        prototype.__ proto __에 첨부한다.
     5. sports 오브젝트에 __ proto __ 오브젝트를 첨부한다. (sport.__ proto __)
     6. 빌트인 Function.prototype의 메소드로 function 인스턴스를 생성하여
        sports.__ proto __에 첨부한다.
     7. sports 오브젝트 프로퍼티에 초기값 설정
        * arguments, caller, length, name 프로퍼티]

2. function 오브젝트 구조

   * function 오브젝트에 prototype이 있으며 constructor가 연결된다.

     * __ proto __ 가 연결되어 있으며 Object 인스턴스가 연결된다.

   * function 오브젝트에 __ proto __가 있으며 Function  인스턴스가 연결된다.

     * Array이면 Array 인스턴스가 연결되고 String이면 String 인스턴스가 연결된다.

     ``` javascript
     sports = {
         arguments: {},
         caller: {},
         length: 0,
         name: "sports",
         prototype: {
             constructor: sports,
             __proto__: Object.prototype
         },
         __proto__:Function.prototype //String.prototye,   Array.prototype ...
     }
     ```

✨**이름(키):value 형태로 바라보자!**






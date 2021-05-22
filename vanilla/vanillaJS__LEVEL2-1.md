

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



#### L11 - 함수 실행 환경 인식 / 함수 실행 환경 저장 / 내부 프로퍼티

1. 함수 실행 환경 인식
   * 함수 실행 환경 인식이 필요한 이유는 ? 
     * 함수가 호출되었을 때 실행될 환경을 알아야 실행 환경에 맞춰 실행할 수 있기 때문
   * function 키워드를 만나 function 오브젝트를 생성할 떄 실행 환경 설정을 한다.
   * 설정하는 것은 실행 영역(함수가 속한 Scope (Lexical Scope)), 파라미터, 함수 코드
2. 함수 실행 환경 저장
   * function 오브젝트를 생성하고 바로 실행하지 않는다.
   * 함수가 호출되었을 때 사용할 수 있도록 환경을 저장한다.
   * 저장하는 위치는 생성한 function 오브젝트이다.
   * 인식한 환경을 function 오브젝트의 내부 프로퍼티에 {name: value} 형태로 저장한다.
3. 내부 프로퍼티
   * 엔진이 내부 처리에 사용하는 프로퍼티. 스펙 표기로 외부에서 사용 불가
   * [[ ]] 형태.   ( ex. [[Scope]])



#### L12 - 내부 프로퍼티 분류: 공통 내부 프로퍼티 /  선택적 내부 프로퍼티

---

1. 내부 프로퍼티 분류
   * 공통 프로퍼티 
     * 모든 오브젝트에 공통으로 설정되는 프로퍼티
   * 선택적 프로퍼티
     * 오브젝트에 따라 선택적으로 설정되는 프로퍼티
     * 해당되는 오브젝트에만 설정
2. 공통 내부 프로퍼티
   * [[Prototype]] : 오브젝트의 prototype   (Object 또는 Null)
   * [[Class]] : 오브젝트 유형 구분 (String)
   * [[Extensible]] : 오브젝트에 프로퍼티 추가 가능 여부 (Boolean)
   * [[Get]] : 이름의 프로퍼티 값 (any)
   * [[GetOwnProerty]] : 오브젝트 소유의 프로퍼티 디스크립터 속성 (프로퍼티 디스크립터)
   * [[GetProperty]] : 오브젝트의 프로퍼티 디스크립터 (프로퍼티 디스크립터)
   * [[Put]] : 프로퍼티 이름으로 프로퍼티 값 설정
   * [[CanPut]] : 값 설정 가능 여부 (Boolean)
   * [[HasProperty]] : 프로퍼티의 존재 여부 (Boolean)
   * [[Delete]] : 오브젝트에서 프로퍼티 삭제 가능 여부 (Boolean)
   * [[DefaultValue]] : 오브젝트의 디폴트 값 (any)
   * [[DefinedOwnProperty]] : 프로퍼티 추가, 프로퍼티 값 변경 가능 여부 (Boolean)
3. 선택적 내부 프로퍼티
   * [[PrimitiveValue]]: Boolean, Date, Number, String 오브젝트에서만 제공 (프리미티브 값)
   * [[Construct]]: new 연산자로 호출되며 인스턴스를 생성한다. (Object)
   * [[Call]]: 함수 호출 (any)
   * [[HasInstance]]: 지정한 오브젝트로 생성한 인스턴스 여부 (instance of 연산자로 생각하면 쉬움) (Boolean)
   * [[Scope]]: Function 오브젝트가 실행되는 렉시컬(정적) 환경 (렉시컬 환경)
   * [[FormalParameters]]: 호출된 함수의 파라미터 이름 리스트 (문자열 리스트)
   * [[Code]]: 함수에 작성한 JS 코드 설정, 함수가 호출되었을 때 실행 (JS 코드)
   * [[TargetFunction]]: Function 오브젝트의 bind()에 생성한 타깃 함수 오브젝트 설정 (Object)
   * [[BoundThis]]: bind()에 바인딩된 this 오브젝트 (any)
   * [[BoundArguments]]: bind()에 바인딩된 아규먼트 리스트 (리스트)
   * [[Match]]: 정규표현식의 매치 결과 (매치 결과)
   * [[ParameterMap]]: 아규먼트 오브젝트와 함수의 파라미터 매핑 (Object)



#### L13 - 함수 정의 형태: 함수 정의 / 함수 선언문 / 함수 표현식

---

1. 함수 정의 (Function Definition)
   * 함수 코드가 실행될 수 있도록 JS 문법에 맞게 함수를 작성하는 것
   * 함수 정의 형태
     * 함수 선언문(Function Declaration)
     * 함수 표현식(Function Expression)
     * new Function(param, body) 문자열로 작성

2. 함수 선언문

   | 구분      | 타입     | 데이터(값)               |
   | --------- | -------- | ------------------------ |
   | function  | Function | function 키워드          |
   | 식별자    | String   | 함수 이름                |
   | 파라미터  | Any      | 파라미터 리스트 opt      |
   | 함수 블록 | Object   | {실행 가능 코드 opt}     |
   | 반환      | Function | 생성한 Function 오브젝트 |

   ```javascript
   function book(one, two){
   	return one + ", " + two;
   };
   log(book("JS", "DOM"));
   ```

   * function getBook(title){함수 코드}
     * function, 함수이름, 블록{} 작성은 필수
     * 파라미터, 함수 코드는 선택
   * 엔진이 function 키워드를 만나면 function  오브젝트를 생성하고
     함수 이름을 function 오브젝트 이름으로 사용

3. 함수 표현식

   ```javascript
   var getBook = function(title){
       return title;
   };
   getBook("JS책");
   ```

   * var getBook = function(title){함수 코드}
     * function 오브젝트를 생성하여 변수에 할당
     * 변수 이름이 function 오브젝트 이름

   ```javascript
   var getBook = function inside(value){
       if(value === 102){
           return value;
       };
       console.log(value);
       return inside(value+1);
   };
   getBook(100);
   ```

   * 식별자 위치의 함수 이름은 생략 가능
   * var name = function abc(){} 에서 abc가 식별자 위치의 함수 이름
     (최근에는 이러한 형태로 안쓴다)



#### L14 - 엔진 해석 방법: 엔진 해석 순서 / 함수 코드 작성 형태 / 엔진 처리 상태

---

1. 엔진 해석 순서

   * 자바스크립트는 스크립팅 언어다. 
     스크립팅 언어는 작성된 코드를 위에서부터 한줄씩 해석한다. 하지만!

   * 자바스크립트는 중간에 있는 코드가 먼저 해석될수도있다.

   * 첫번째, 함수 선언문을 순서대로 해석한다.

     function sports(){};

   * 두번째, 표현식을 순서대로 해석한다. (표현식 = 변수 형태로 할당한 것)

     var value = 123;    (정수 표현식?)

     var book = function(){};     (함수 표현식)

2. 함수 코드 작성 형태

   ``` javascript
   function book(){
   	debugger;
       var title = "JS책";
       function getBook(){
           return title;
       };
       var readBook = function(){};
       getBook();
   };
   
   book();
   ```

   * 마지막 줄에서 book 함수 호출 (  book(); )
   * title 변수 선언 (var title = "JS책"; )
   * 함수 선언문 작성 (function getBook(){return title;})
   * 함수 표현식 작성 (var readBook = function(){};)

3. 엔진 처리 상태

   ``` javascript
   function book(){
   	console.log(title);
       console.log(readBook);
       console.log(getBook);
       debugger;
       var title = "JS책";
       function getBook(){
           return title;
       }
       var readBook = function(){};
       getBook();
   }
   book();
   
   //실행결과
   // undefined
   // undefined
   // function getBook(){ return title; }
   ```

   * undefined도 해석을했다는 뜻이다. 
     해석을 했다라는 뜻은 Scope에 식별자해결을위해 등록했다는 뜻이다.
   * 함수 선언문은 값이 전체가 등록되어있고,
     title과 readBook은 이름은 정상적으로 등록되어있는데 값은 아니다라는 것.
   * 자바스크립트는 프로퍼티를 등록할때 이름만 있으면 값을 자동적으로 undefined로 설정해버린다. 그래야 name, value의 구조가 맞기 때문이다.
   * 프로퍼티로 등록하는데 값이없고 이름만등록할수는 없다. 그럴때 undefined를 사용한다. 시멘틱 그대로 defined 하지 않았다는 뜻.
   * 이것이 바로 자바스크립트 엔진이 프로퍼티를 등록하는 기준이다.



#### L15 - 함수 코드 해석 순서

---

1. 함수 코드 해석 순서

   ``` javascript
   function book(){
       debugger;
       var title = "JS책";
       function getBook(){
           return title;
       };
       var readBook = function(){};
       getBook();
   };
   book();
   ```

   * 함수 선언문 해석
     * function getBook(){};

   * 변수 초기화
     * var title = undefined;
     * var readBook = undefined;
   * 코드 실행
     * var title = "JS책";
     * var readBook = function(){};
     * getBook();



#### L16 - Hoisting / 함수 앞에서 호출 

---

1. 호이스팅

   ``` javascript
   var result = book();
   console.log(result);
   
   function book(){
   	return "호이스팅";
   };
   book = function(){
       return "함수 표현식";
   }
   
   [실행 결과]
   >> 호이스팅
   ```

   * 함수 선언문은 초기화 단계에서 function 오브젝트를 생성하므로 어디에서도 함수를 호출할 수 있다.
   * 함수 앞에서 호출 가능하다. (호이스팅)
   * 초기화 단계에서 값이 있으면 초기화하지 않는다.
     (book이 이미 있으므로 undefined 하지않는다.)

   2. 코딩 연습

   ``` javascript
   // 1. 함수 선언문, 함수 호출(), 함수 선언문
   function book(){
       function getBook(){
           return "책1";
       };
       console.log(getBook());
       function getBook(){
           return "책2";
       };
   };
   book();
   
   // 2. 함수 표현식, 함수 호출(), 함수 표현식
   function book(){
       var getBook = function(){
       	return "책1";
       };
       console.log(getBook());
       var getBook = function(){
           return "책2";
       }    
   }
   book();
   
   // 3. 함수 선언문, 함수 호출(), 함수 표현식
   function book(){
   	function getBook(){
           return "책1";
       }
       console.log(getBook());
       var getBook = function(){
   		return "책2";
       }
   }
   book();
   
   // 4. 함수 표현식, 함수 호출(), 함수 선언문
   function book(){
       var getBook = function(){
       	return "책1";
       };
       console.log(getBook());
       function getBook(){
           return "책2";
       }    
   }
   book();
   
   ```

   

#### L17 - 오버로딩

---

1. 오버로딩

	``` javascript
   function book(one){};
   function book(one, two){};
   function book(one, two, three){};
   
   book(one, two);
   ```
   
   * 함수 이름이 같더라도 파라미터 수 또는 값 타입이 다르면 각각 존재
   * 함수를 호출하면 파라미터 수와 값 타입이 같은 함수가 호출된다.
   * JS는 오버로딩을 지원하지 않는다.
   * JS는 파라미터 수와 값 타입을 구분하지 않고 {name: value} 형태로 저장하기 때문이다.
   
2. 오버로딩 미지원: 함수 선언문 초기화

   ``` javascript
   function book(){
   	function getBook(){
   		return "책1";
   	};
   	getBook();
   	function getBook(){
   		return "책2";
   	};
   };
   book();
   ```

   * 마지막 줄에서 book() 함수 호출시
   * function getBook(){return "책1";}을 만나 getBook 오브젝트를 생성
   * getBook()호출 하지 않고 아래로 이동
   * function getBook(){return "책2";}를 만나 getBook 오브젝트를 생성
   * 위의 오브젝트와 이름이 같으므로 마지막에 만났던 getBook 오브젝트로 대체된다.
   * {name: value} 형태에서 name이  같으므로 value가 변경된다.

3. 오버로딩 미지원: 변수 초기화

   * getBook() 함수를 호출하면, return "책2"의 getBook 함수가 실행된다.
   * 함수 이름이 같으므로 위의 함수가 아래 함수로 대체되었기 때문.
   * "책2"가 실행결과에 출력된다.

✨**자바스크립트는 오버로딩을 지원하지않는다 {key:value}, {name:value} 의 프로퍼티인것이다. 이것이 자바스크립트의 가장 기본이되는 구조이다.**



### [SECTION3 argument]

#### L18 - Argument 처리 메커니즘 / Argument 처리 구조 / 엔진의 파라미터 처리

---

1. Argument 처리 구조

   ```javascript
   function get(){
   	return arguments;
   };
   console.log(get('A','B'));
   
   //실행 결과
   // {0:A, 1:B}
   ```

   * 파라미터를 {key: value} 형태로 저장한다.
   * 근데 key가 없기때문에 파라미터 수만큼 0부터 인덱스를 부여한다.
   * 이러한 형태를 Array-like라고 한다. 
     * key값이 0부터 1씩 증가하며 (0,1,2,3,4,...)
     * length 프로퍼티가 있어야한다. (for문을 돌릴 수 있다는 뜻)

2. 엔진의 파라미터 처리

   ``` javascript
   var get = function(one){
   	return one;
   };
   get(77,100);
   ```

   * get() 함수를 호출하면서 77과 100을 파라미터 값으로 넘겨준다.
   * 넘겨받은 값을 함수의 파라미터 이름에 설정한다.
     * 정적 환경의 선언적 환경 레코드에 설정한다.
     * one:77
   * Argument 오브젝트를 생성한다
   * 넘겨받은 파라미터 수를 Argument 오브젝트의 length 프로퍼티에 설정
   * 파라미터 수만큼 반복하면서 0부터 key로 하여 순서대로 파라미터 값을 설정
     * {0: 77}, {1: 100} 형태가 된다.
   * 함수의 초기화 단계에서 실행한다.

### [SECTION4 스코프]

#### L19 - 스코프

---



1. 스코프 목적

   * 범위를 제한하여 식별자를 해결하려는 것
     * 식별자 해결(Identifier Resolution) 
       * 변수 이름, 함수 이름을 찾는 것
       * 이때 스코프를 사용한다
       * 이름을 찾게 되면 값을 구할 수 있다.
       * 크게는 이름을 설정하는 것도 식별자 해결이다.
   * 식별자 해결을 위해 스코프가 있는 것 (스코프를 위해 식별자해결이 있는게 아님)

2. 스코프 설정

   ``` javascript
   function book(){
   	var point = 123;
     function get(){
   		console.log(point);
     };
     get();
   };
   book();
   ```

   * 엔진이 function book(){} 을 만나면 function 오브젝트를 생서한다.
   * 그리고 스코프를 설정한다
     * 생성한 function 오브젝트의 [[Scope]]에 스코프를 설정한다.
     * 즉, 이때 스코프가 결정된다.
   * 이러한 것을 정적 스코프라고한다.
     (function 오브젝트를 만드는 시점에 스코프를 결정하는 것)
   * 한편 함수를 함수를 호출할 때 스코프를 결정하는 것은 동적스코프
     (만약 for문으로 1만번을 돌면 1만번 결정해줘야한다. 즉 비효율적)

 ✨**여기서 [[Scope]]는 영역, 범위의 개념으로 get() 함수가 속한 영역이다. 그러므로 Scope범위내의 함수(var point)에도 get함수는 접근 할 수 있다.**



#### L20 - Global 오브젝트 / Global 오브젝트 특징

---

``` javascript
var value = 100;
function book(){
	var point = 200;
  return value;
};
book();
```

1. 글로벌 오브젝트
   * 100을 value변수에 할당한 것은 value로 검색하여 겂을 사용하기 위한 것
   * 함수 안에 변수를 선언하면 변수가 함수에 속하게되지만, value 변수는 ? 어느 함수에도 포함되어있지않다.
   * value 변수가 속하는 오브젝트가 없다는 뜻이다. 이 때, global 오브젝트에 설정된다.
   * 이런 메커니즘을 구현할 수 있는 것은 global 오브젝트가 하나만 있기 때문이다.
2. 글로벌 오브젝트 특징
   * Global 오브젝트는 JS 소스파일 전체에서 하나만있다.
   * 그래서 new 연산자로 인스턴스 생성 불가하다.
     (만약 new 연산자로 생성 가능하면 하나만 있다는 전제가 깨지는 것이다.)
   * JS 소스 파일 전체 = <script>에 작성된 모든 코드



#### L21 - Global 스코프

---

1. 글로벌 스코프

   * 오브젝트는 함수와 변수를 작성 (개발자 관점) 
   * 스코프는 식별자 해결을 위한 것 (엔진 관점) 

   * 글로벌 오브젝트가 글로벌 스코프이다 (Window를 사용하기도함)
   * 글로벌 스코프는 최상위 스코프이며 함수에서보면 최종 스코프이다.

2. 글로벌 스코프 (코드)

   ``` javascript
   var value = 100;
   function book(){
   	return value;
   };
   book();
   ```

   * function book(){코드} : book 함수가 속한 오브젝트가 없으므로 book 함수가 글로벌 오브젝트에 설정된다. (글로벌 함수)
   * var value =100; : value 변수가 글로벌 오브젝트에 설정된다. (글로벌 변수)
     * 글로벌 오브젝트에 설정된다 (오브젝트 관점)
     * Scope가 글로벌 스코프 (식별자 해결 관점)
   * book();  : book 함수를 호출하려면 오브젝트.book() 형태로 작성해야하는데 오브젝트를 작성하지 않고 함수만 작성한다.
   * 오브젝트를 작성하지않으면 글로벌 오브젝트를 오브젝트로 간주한다.
     즉, 글로벌 오브젝트의 book() 함수를 호출하게된다.
     (여기서의 식별자해결 : 글로벌오브젝트의 book() 함수를 찾아서 호출하는 것.)

✨**글로벌 오브젝트는 실체가 없다. 하지만 HOST 오브젝트 개념으로 window 오브젝트를 사용한다. 따라서 window 오브젝트의 book함수를 호출한다고도 할 수 있다.**



#### L22 - 스코프 바인딩 / 정적,동적 바인딩 / 바인딩 시점의 중요성

---

1. 바인딩
   * 구조적으로 결속된 상태로 만드는 것
   * 바인딩의 목적은 스코프 설정, 식별자 해결
   * 바인딩 시점에따라 정적바인딩(lexical, static binding)과
     동적바인딩(dynamic binding)으로 구분한다.

2. 정적, 동적 바인딩

   * 정적(렉시컬) 바인딩
     * 초기화 단계에서 바인딩한다.
     * 함수 선언문 이름을 바인딩한다.
     * 표현식(변수, 함수) 이름을 바인딩한다.
   * 동적(다이나믹) 바인딩
     * 실행할 때 바인딩
     * Eval() 함수, with 문

3. 바인딩 시점의 중요성

   * 바인딩 할 때 스코프가 결정되기 때문

   * function 오브젝트 생성 시점에 스코프를 결정하면 스코프를 function 오브젝트의 [[Scope]]에 설정한다.

   * 한 번 결정된 스코프는 변경되지 않는다. (그래서 정적 스코프인 것)

   * 함수 안의 모든 함수의 스코프가 같다.

     ```javascript
     function book(){
     	var point = 100;
       function add() {point += 200;};
       function get() {return point;};
     }
     ```

   * book 스코프에있는 모든 변수, 함수가 공유할 수 있다. (하나의 스코프안에 있으니까)

   * 이 환경을 내부 프로퍼티인 [[Scope]]에 설정한다. 그리고 이것은 변경되지않기는다.

4. 스코프 바인딩 예시

   ```javascript
   function book(){
     var point = 100;
     function add(param){
   		point += param;
     };
     var get = function(){
       return point;
     };
     add(200);
     console.log(get());
   };
   book();
   ```

   1. 마지막 줄 book() 함수 호출.
      * 초기화 단계에서 함수, 변수 이름을 선언적 환경 레코드에 바인딩한다. 
   2. function add(param){...}
      * function 오브젝트 생성
      * add 함수가 속한 스코프(영역)을 add 오브젝트의 [[Scope]]에 설정
      * add 이름을 선언적 환경 레코드에 바인딩한다.
   3. var point = 100;
      * point 이름을 레코드에 바인딩한다.
   4. var get = function(){...}
      * get 이름을 레코드에 바인딩한다.
   5. 바인딩으로 함수와 변수의 식별자가 해결된다.

   ------ 코드 실행 ------

   6. var point = 100;
      * point 변수에 100 할당
   7. var get = function(){...}
      * Function 오브젝트 생성, get에 할당
      * get 함수가 속한 스코프(영역)를 get 오브젝트의 [[Scope]]에 설정

   ----- add() 함수 호출 -----

   8. add(200) 함수를 호출한다.
   9. point += param;
      * 먼저 선언적 환경 레코드에서 point 이름을 찾는다.
      * point가 없다면 다시 검색한다.
      * 검색 시 add 오브젝트의 [[Scope]]를 스코프로 사용한다.
      * book 오브젝트가 스코프이며 point가 있으므로 값을 더한다.
        (내가 만든 변수가 아니라 함수 밖에있는것도 변경. 함수가 속한 스코프를 내부 프로퍼티인 스코프에 설정해놓고 마치 내 것 처럼 쓰기 때문이다.)

   ----- get() 함수 호출 -----

   8. get() 함수를 호출한다.
   9. return point;
      * 선언적 환경 레코드에 point가 없으므로 다시 검색한다.
      * get 오브젝트의 [[Scope]]를 스코프로 사용한다.
      * book 오브젝트가 스코프이며 point가 있으므로 값을 반환

5. 동적 바인딩
   * 코드를 실행할 때 마다 바인딩
     * with문, eval() 함수
   * with문 
     * 'use strict' 환경에서 에러가 발생한다. (사용하지 말라는 뜻)
   * eval() 함수
     * 보안에 문제가 있다.
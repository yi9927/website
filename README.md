'''
코드
'''
# enum과 if-else문 비교 (BMI 계산기)
#### _우선 공부한 내용을 바탕으로 제가 생각한 결론부터 언급하겠습니다._
## enum vs if-else 문 사용 시기(결론)
### enum 사용이 좋은 경우:
- 관련된 상수 값들을 하나의 타입으로 관리해야 할 때
- 상수 값들의 의미를 명확히 나타내고 싶을 때
- 상수 값들의 타입 안전성이 중요할 때
### if-else 문 사용이 좋은 경우:
- 상수 값들이 자주 변경되거나 추가될 때
- 상수 값들의 조건 검사가 복잡할 때
- 예외 처리가 중요할 때

------------------------------------------------------------
## 공부한 내용 - enum과 if-else문의 장단점

### enum을 사용한 경우
#### 장점:
- 가독성이 좋습니다.
- enum 상수 이름만으로 비만도 상태를 쉽게 파악할 수 있습니다.
- 타입 안전성이 높습니다.
- enum 상수 외의 값이 입력되는 것을 방지할 수 있습니다.
- 코드가 간결해집니다.
- Switch 문을 사용하여 간단하게 구현할 수 있습니다.
#### 단점:
- 새로운 비만도 상태가 추가되면 enum 정의와 Switch 문을 모두 수정해야 합니다.
- 예외 처리가 복잡할 수 있습니다.
- 모든 enum 상수를 처리해야 하기 때문입니다.
### if-else 문을 사용한 경우
#### 장점:
- 새로운 비만도 상태가 추가되면 if-else 문만 수정하면 됩니다.
- enum 정의를 변경할 필요가 없습니다.
- 예외 처리가 상대적으로 간단합니다.
- 필요한 조건만 처리하면 됩니다.
#### 단점:
- 가독성이 떨어질 수 있습니다.
- if-else 문이 길어지면 코드 이해가 어려워질 수 있습니다.
- 타입 안정성이 낮습니다.
- 잘못된 값이 입력되어도 컴파일 시점에 오류를 발견하기 어렵습니다.
- 코드가 길어질 수 있습니다.
- 각 조건을 모두 명시해야 하기 때문입니다.
------------------------------------------------------------
### 용어 의미
#### 타입안정성?
- 프로그램이 실행 중 타입 관련 오류를 발생시키지 않는 것을 의미
#### 점프테이블?
- 점프 테이블은 enum 상수들의 인덱스를 저장하는 배열.
- 각 enum 상수는 배열의 인덱스로 매핑.
- 이를 통해 enum 상수에 빠르게 접근.

----------------------------------------------------------
## 앞으로 공부해야 할 것들
### 1. 예외 처리
- 예외 처리가 무엇인지?
- 어떻게 코드를 작성하는지?
- 언제 예외 처리가 중요한지?
### 2. enum과 if-else문의 실행속도
- enum은 switch 문과 유사한 방식으로 구현된다? -> 그래서 빠르다?
- 내부적으로 점프테이블을 사용한다?
- 조건문이 몇 개 이하면 if-else문이 빠를까?
### 3. 인덱스가 무엇인지
------------------------------------------------
## enum을 활용한 코드 작성
_BMI 지수 범주를 상수라 판단하여 enum으로 간다히 BMI 계산기를 작성했습니다._


### 자바 enum 클래스 정의
이를 통해 BMI 지수에 따른 비만도 판정을 체계적으로 관리할 수 있습니다.

```java
package bodymessindex;

public enum Category {
UNDERWEIGHT("저체중", 0, 18.5),
NORMAL("정상", 18.5, 23.0),
OVERWEIGHT("과체중", 23.0, 25.0),
OBESE("비만", 25.0, Double.MAX_VALUE);


private final String name;
private final double minBmi;
private final double maxBmi;

Category(String name,double minBmi, double maxBmi) {
this.name = name;//영어로 카테고리 지정 함으로 한글을 받기위해 추가함
this.minBmi = minBmi;
this.maxBmi = maxBmi;
}

public String getName() {
return name;
}

public double getMaxBmi() {
return maxBmi;
}
public double getMinBmi() {
return minBmi;
}

}
```

-----------------
### BMI 계산 클래스
BMI 계산 로직을 별도의 클래스로 분리했습니다.
```java
package bodymessindex;

public class Calculator {
// BMI 계산 로직
public static double calulateBMI(double height, double weight) {
double heightPercentage = height / 100.0; //키를 미터 단위로 만듬
double bmi = weight / (heightPercentage * heightPercentage);
return bmi;
}

//계산된 BMI 지수를 기반으로 해당하는 Category enum 값을 반환
public static Category getCategory(double bmi) {
for (Category category : Category.values()) {
if (bmi >= category.getMinBmi() && bmi < category.getMaxBmi()) {
return category;
}
}
return Category.OBESE; //25.0이상은 모두 비만으로 지정
}
}

```
--------------------------------------------
### 메인 클래스
마지막으로 메인 클래스에서 사용자의 키와 몸무게를 입력받고, BMI 계산 및 비만도 판정 결과를 출력보았습니다.
```java
package bodymessindex;

import java.util.Scanner;

public class BmiMain {
public static void main(String[] args) {
Scanner in = new Scanner(System.in);

//신장 입력
System.out.print("신장을 입력하세요: ");
double height = in.nextDouble();

//체중 입력
System.out.print("체중 입력하세요: ");
double weight = in.nextDouble();

//BMI계산
double result = Calculator.calulateBMI(height, weight);

//계산된 Bmi 반환
Category category = Calculator.getCategory(result);
System.out.println("당신의 BMI 지수는 " + category.getName() + "입니다.");
}

}
```
---------------------------
### 결과물
![](https://i.imgur.com/JYJC0cR.png)
기대한 결과가 출력되었습니다.

-----------
#### 참고
- (BMI 계산법) 행복드림옥천보건소, 2022.12.19, https://www.oc.go.kr/health/contents.do?key=1487&
- 우종정, 쉽게 배우는 자바 프로그래밍, 한빛아카데미, 2020, p.780

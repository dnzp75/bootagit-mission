# 해피 아일랜드의 수학자

### 사고 과정
**1. 문제 이해**

주어진 문제는 특정 수 `n`이 "행복한 수"인지 판별하는 문제. 행복한 수란, 해당 수의 각 자릿수를 제곱하여 합한 결과를 반복적으로 계산했을 때, 최종적으로 1이 되는 수를 뜻한다. 이에 따라 고려해야 할 점으로 반복적인 계산과 이 계산에서 필요한 **제약 조건을 분석**하여 핵심을 찾아내는 것을 중심으로 문제를 이해했다.

**2. 제약 조건 분석**

- **n의 범위 :** 1 <= n <= 10^6
    - n의 범위가 최대 10^6인 점과 실행 시간이 1초(약 10^8)인 조건으로 보아 코드 작성 시에 시간 복잡도를 고려하여 작성
- **사이클 발생**: 각 자릿수를 제곱한 합이 반복되면 사이클이 발생한다.
    - 이를 확인하기 위해 중간 계산 결과를 저장해야 할 집합이 필요하다.

**3. 분석을 통한 고려 사항**

- **자료구조 선택**: 중간 결과를 저장하기 위해 반복 여부를 확인할 수 있도록 HashSet을 사용하여 사이클을 확인한다.
- **반복 구조**: 각 자릿수를 분리하고 제곱한 합을 반복적으로 계산하는 구조를 설계해야 한다.
- **효율성 고려**: 검색 시간 복잡도가 O(1)인 Set을 사용하여 반복적인 계산을 최소화하고, 필요한 중간 결과만 저장하여 실행 시간 1초 넘지 않도록 한다.

**4. 로직 설계**

위의 고려 사항을 토대로 로직 설계를 작성한다.

- **자릿수 분리 및 제곱 합 계산**: 주어진 수의 각 자릿수를 분리하고, 각 자릿수의 제곱을 합산한다.
- **반복 및 종료 조건**: 위의 과정을 합산 결과가 1이 되거나, 이전의 합 결과가 HashSet에 포함되어 있을 때까지 반복한다.
    - 만약, 결과가 1이 되면 "행복한 수"로 판별하여 1을 리턴한다.
    - 만약, 계산된 중간 결과가 이전에 나온 적이 있으면 사이클이 발생한 것으로 보고 "행복한 수"가 아니라고 판별하여 0을 리턴한다.

<p align="center">
 <img src="https://github.com/dnzp75/bootagit-mission/assets/105201451/4b5bec1c-6615-455b-ac57-bf1ded9bd4db.png" width="400" height="400"/>
</p>

### 디버깅 과정

**초기 접근 방식**

처음에는 각 자릿수를 문자열로 변환하여 제곱합을 계산하는 방법을 사용했습니다. 이 방식도 문제 해결은 가능했지만, 더 쉽고 가독성 좋은 코드가 있는지 인터넷을 통해 찾아보았고, 굳이 변환과정 없이 수학적 연산을 통해 직접 자릿수를 분리하는 방식으로도 충분히 풀 수 있는 문제임을 깨닫고 디버깅 과정을 진행했습니다. 

```java
// 초기 접근 방식: 문자열로 변환하여 자릿수 분리
private static int sumOfSquares(String n) {
    int sum = 0;
    for (char c : n.toCharArray()) {
        int digit = Integer.parseInt(String.valueOf(c));
        sum += digit * digit;
    }
    return sum;
}
```

**수학적 연산 방식으로 작성**

자릿수를 문자열로 변환하지 않고, 수학적 연산을 통해 직접 자릿수를 분리하고 제곱합을 계산하는 방식으로도 코드를 작성해 보았습니다.

```java
// 최적화된 방식: 수학적 연산으로 자릿수 분리
private static int sumOfSquares(int n) {
    int sum = 0;
    while (n > 0) {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    return sum;
}
```

### 최종 코드

```java
import java.util.HashSet;
import java.util.Set;

public class HappyIsland {

    // 각 자릿수의 제곱의 합을 구하는 함수
    private static int sumOfSquares(int n) {
        int sum = 0;
        while (n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }

    // 주어진 수가 행복한 수인지 판별하는 함수
    public static int isHappyNumber(int n) {
        Set<Integer> seen = new HashSet<>();
        while (true) {
            n = sumOfSquares(n);
            if (n == 1) {
                return 1; // 행복한 수
            }
            if (seen.contains(n)) {
                return 0; // 사이클 발견, 행복하지 않은 수
            }
            seen.add(n);
        }
    }

    // 테스트 함수
    public static void testIsHappyNumber(int n) {
        System.out.println("n = " + n + ", 결과: " + isHappyNumber(n));
    }

    // 메인 함수
    public static void main(String[] args) {
        int n1 = 19;
        testIsHappyNumber(n1);

        // n의 최대값 1,000,000을 사용하여 테스트
        int n2 = 1000000;
        testIsHappyNumber(n2);

        // 사이클 발생으로 인해 0을 리턴하는 경우 테스트
        int n3 = 4;
        testIsHappyNumber(n3);

        // 최종 결과가 0으로 나오는 경우 테스트
        // 행복하지 않은 수 테스트
        int n4 = 2; 
        testIsHappyNumber(n4);
    }
}

```

---
# 로마 해독 

### 사고 과정

**1. 문제 이해**

로마 숫자의 규칙과 이를 정수로 변환하는 방법을 이해해야 한다.

- 로마 숫자에서는 큰 기호가 작은 기호의 왼쪽에 위치하면 더하기로 계산하고, 작은 기호가 큰 기호의 왼쪽에 위치하면 빼기로 계산한다.
    - ex) II는 2, XII는 12, IV는 4, IX는 9로 표현
- 추가 조건에서는  "IIX"나 "VX"와 같은 정형화된 규칙을 위반하는 표기는 잘못된 것으로 간주한다고 한다.
    - 정형화된 규칙을 따르는 로마 숫자 표기법을 따로 찾아보았다.
> **감산 규칙**:
> 
> - 감산 규칙은 특정 조합에만 적용됩니다.
> - I는 V(5)와 X(10) 앞에 올 수 있습니다. 예: IV(4), IX(9)
> - X는 L(50)과 C(100) 앞에 올 수 있습니다. 예: XL(40), XC(90)
> - C는 D(500)와 M(1000) 앞에 올 수 있습니다. 예: CD(400), CM(900)
> - 이 외의 경우에는 작은 숫자가 큰 숫자 앞에 올 수 없습니다. 예: IL, IC, ID, IM, VX, LC 등은 잘못된 표기입니다.

> **반복 규칙**:
> 
> - I, X, C, M은 각각 최대 3번까지 반복될 수 있습니다. 예: III(3), XXX(30), CCC(300), MMM(3000)
> - V, L, D는 반복될 수 없습니다. 예: VV, LL, DD는 잘못된 표기입니다.

### 로직 설계

1. **문자열 검증**: 입력 문자열이 위의 규칙에 따라 로마 숫자 표기법을 따르는지 확인한다.
2. **해독 알고리즘**: 각 문자를 순차적으로 탐색하여 로마 숫자를 정수로 변환한다.
3. **범위 확인**: 변환된 정수 값이 3999를 초과하는지 확인한다.

### 세부 설계

1. **문자열 검증**:
    - 정규 표현식을 사용하여 로마 숫자 표기법을 따르는지 확인한다.
    - 잘못된 표기인 경우 오류 메시지를 출력
    - HashMap 사용 <Character, Integer>
2. **로마 숫자 해독**:
    - 문자열을 순차적으로 탐색하여 현재 문자의 값이 다음 문자의 값보다 작으면 빼고, 그렇지 않으면 더한다.
    - HashMap의 키, 값 확인을 통한 구현
3. 3999 값 초과 여부 검증:
    - 계산한 결과 값을 확인한다.
    - 3999를 초과할 경우 오류 메세지를 출력

### 로직 예시

1) 올바른 로마 규칙 + 3999 값  이하 : 올바른 정수 값 출력
2) 올바른 로마 규칙 + 3999 값 초과 : “표현할 수 없는 범위의 수입니다” 출력
3) 잘못된 로마 규칙 + 3999 값 이하 : “잘못된 로마 숫자 표기입니다” 출력
4) 잘못된 로마 규칙 + 3999 값 초과 : “잘못된 로마 숫자 표기입니다” 출력

### 디버깅

**첫 번째**

초기 접근 방식

정형화된 로마 숫자 표기법에 대한 규칙을 확인하는 코드를 작성해야 하는데, 문제의 조건만 보고는 어떠한 규칙을 통해 로마 숫자 표기법에 위반되는지 알 수 없었다. 이에 따라 인터넷에서 올바른 로마 숫자 표기법을 검증하는 방법을 찾아 나의 코드에 적용시켰다.

```java

import java.util.HashMap;
import java.util.Map;

public class RomanToInteger {
    private static final Map<Character, Integer> romanMap = new HashMap<>();

    static {
        romanMap.put('I', 1);
        romanMap.put('V', 5);
        romanMap.put('X', 10);
        romanMap.put('L', 50);
        romanMap.put('C', 100);
        romanMap.put('D', 500);
        romanMap.put('M', 1000);
    }

    // 로마 숫자 표기법을 검증하는 메서드
    private static boolean isValidRoman(String s) {
        return s.matches("^(M{0,3})(CM|CD|D?C{0,3})(XC|XL|L?X{0,3})(IX|IV|V?I{0,3})$");
    }
}
```

하지만 위와 같이 작성하고 나니 문제를 제대로 이해할 수 없는 문제가 발생하여,  실제로 정형화된 로마 숫자 표기법에 대한 규칙들에 대해 찾아보고 그 규칙들을 코드로 직접 구현해보면서 이해가 안되는 문제를 해결할 수 있었다.

작성해야 하는 요구사항
- I, X, C, M은 각각 최대 3번까지 반복될 수 있다.
- I는 V(5)와 X(10) 앞에 올 수 있다. 예: IV(4), IX(9)
    - I는 두 번 반복 후 V나 X 앞에 올 수 없음
- X는 L(50)과 C(100) 앞에 올 수 있다. 예: XL(40), XC(90)
    - X는 두 번 반복 후 L이나 C 앞에 올 수 없음
- C는 D(500)와 M(1000) 앞에 올 수 있다. 예: CD(400), CM(900)
    - C는 두 번 반복 후 D나 M 앞에 올 수 없음
- 그 외에는 잘못된 표기이다.

```java
   // 잘못된 로마 숫자 표기를 검증하는 메서드
    private static boolean isValidRoman(String s) {
        // 반복 횟수를 확인하기 위한 맵
        Map<Character, Integer> repetitionMap = new HashMap<>();
        repetitionMap.put('I', 0);
        repetitionMap.put('X', 0);
        repetitionMap.put('C', 0);
        repetitionMap.put('M', 0);

        for (int i = 0; i < s.length(); i++) {
            char current = s.charAt(i);
            if (!romanMap.containsKey(current)) {
                return false; // 유효하지 않은 문자
            }

            // 반복 횟수 체크
            if (current == 'I' || current == 'X' || current == 'C' || current == 'M') {
                repetitionMap.put(current, repetitionMap.get(current) + 1);
                if (repetitionMap.get(current) > 3) {
                    return false; // I, X, C, M은 3번 이상 반복 불가
                }
            } else {
                repetitionMap.put('I', 0);
                repetitionMap.put('X', 0);
                repetitionMap.put('C', 0);
                repetitionMap.put('M', 0);
            }

            if (i > 0) {
                char previous = s.charAt(i - 1);
                if (romanMap.get(previous) < romanMap.get(current)) {
                    // 예외 상황 처리
                    if (previous == 'I' && (current == 'V' || current == 'X')) {
                        if (i > 1 && s.charAt(i - 2) == 'I') {
                            return false; // I는 두 번 반복 후 V나 X 앞에 올 수 없음
                        }
                        continue;
                    } else if (previous == 'X' && (current == 'L' || current == 'C')) {
                        if (i > 1 && s.charAt(i - 2) == 'X') {
                            return false; // X는 두 번 반복 후 L이나 C 앞에 올 수 없음
                        }
                        continue;
                    } else if (previous == 'C' && (current == 'D' || current == 'M')) {
                        if (i > 1 && s.charAt(i - 2) == 'C') {
                            return false; // C는 두 번 반복 후 D나 M 앞에 올 수 없음
                        }
                        continue;
                    } else {
                        return false; // 잘못된 표기
                    }
                }
            }
        }
        return true;
    }
```

**두 번째**

코드를 작성하고 테스트 케이스를 돌리는 부분에서 문제가 발생했다. 

문제의 조건대로라면 “MMMM”에 대한 케이스는 4000의 값을 내어주고, 그 값이 3999보다 높은 값이기 때문에 “표현할 수 없는 범위의 수입니다.” 의 출력을 기대했지만, 실제 출력 값은 "잘못된 로마 숫자 표기입니다.”가 출력되는 문제가 발생했다.

```java

    // 테스트 함수
    public static void testRomanToInt(String s) {
        romanToInt(s);
    }

    public static void main(String[] args) {
        testRomanToInt("II"); // 정상적인 로마 숫자
        testRomanToInt("XII"); // 정상적인 로마 숫자
        testRomanToInt("IV"); // 정상적인 로마 숫자
        testRomanToInt("IX"); // 정상적인 로마 숫자
        testRomanToInt("MMMM"); // 3999 범위를 초과하는 경우
        testRomanToInt("IIX"); // 잘못된 로마 숫자 표기
    }
}

**출력 값**
로마 숫자: II -> 정수: 2
로마 숫자: XII -> 정수: 12
로마 숫자: IV -> 정수: 4
로마 숫자: IX -> 정수: 9
**잘못된 로마 숫자 표기입니다.**
잘못된 로마 숫자 표기입니다.
```

위의 문제에 대해서 한참 찾아본 결과, 정형화된 로마 숫자 표기법에 대해서는 “ I, X, C, M은 각각 최대 3번까지 반복될 수 있다” 라는 규칙이 있었고, 이에 따라 M이 4번 사용되었기 때문에 애초에 “잘못된 로마 숫자 표기”가 출력되고 있었던 것. 

관점에 따라 다르겠지만 애초에 문자열 입력 시에 잘못된 로마 표기법을 입력했다면, 입력 문자열에 대해서 정수 변환할 필요도 없다고 생각해서 위와 같이 문자열 입력 후 isValidRoman메서드를 통해 잘못된 로마 표기가 확인되면 “ 잘못된 로마 숫자 표기”가 출력되도록 하는 것이 맞다고 생각하여 위의 코드를 그대로 사용하여 해결하였다.

### 최종 코드

```java
package org.example;

import java.util.HashMap;
import java.util.Map;

public class RomanDecoder {

    // 로마 숫자 문자와 그 값을 매핑할 수 있는 HashMap 생성
    private static final Map<Character, Integer> romanMap = new HashMap<>();

    static {
        romanMap.put('I', 1);
        romanMap.put('V', 5);
        romanMap.put('X', 10);
        romanMap.put('L', 50);
        romanMap.put('C', 100);
        romanMap.put('D', 500);
        romanMap.put('M', 1000);
    }

    // 잘못된 로마 숫자 표기를 검증하는 메서드
    private static boolean isValidRoman(String s) {
        // 반복 횟수를 확인하기 위한 맵
        Map<Character, Integer> repetitionMap = new HashMap<>();
        repetitionMap.put('I', 0);
        repetitionMap.put('X', 0);
        repetitionMap.put('C', 0);
        repetitionMap.put('M', 0);

        for (int i = 0; i < s.length(); i++) {
            char current = s.charAt(i);
            if (!romanMap.containsKey(current)) {
                return false; // 유효하지 않은 문자
            }

            // 반복 횟수 체크
            if (current == 'I' || current == 'X' || current == 'C' || current == 'M') {
                repetitionMap.put(current, repetitionMap.get(current) + 1);
                if (repetitionMap.get(current) > 3) {
                    return false; // I, X, C, M은 3번 이상 반복 불가
                }
            } else {
                repetitionMap.put('I', 0);
                repetitionMap.put('X', 0);
                repetitionMap.put('C', 0);
                repetitionMap.put('M', 0);
            }

            if (i > 0) {
                char previous = s.charAt(i - 1);
                if (romanMap.get(previous) < romanMap.get(current)) {
                    // 예외 상황 처리
                    if (previous == 'I' && (current == 'V' || current == 'X')) {
                        if (i > 1 && s.charAt(i - 2) == 'I') {
                            return false; // I는 두 번 반복 후 V나 X 앞에 올 수 없음
                        }
                        continue;
                    } else if (previous == 'X' && (current == 'L' || current == 'C')) {
                        if (i > 1 && s.charAt(i - 2) == 'X') {
                            return false; // X는 두 번 반복 후 L이나 C 앞에 올 수 없음
                        }
                        continue;
                    } else if (previous == 'C' && (current == 'D' || current == 'M')) {
                        if (i > 1 && s.charAt(i - 2) == 'C') {
                            return false; // C는 두 번 반복 후 D나 M 앞에 올 수 없음
                        }
                        continue;
                    } else {
                        return false; // 잘못된 표기
                    }
                }
            }
        }
        return true;
    }

    /*
    - 문자열을 순회하면서 각 문자의 값을 읽어 들인다.
    - 현재 문자의 값이 다음 문자보다 크거나 같으면 값을 더하고, 작으면 값을 뺀다.
    - 변환된 값이 3999를 초과하면 에러 메시지를 출력한다.
    - 입력된 로마 숫자 문자열이 잘못된 형식을 가지면 에러 메시지를 출력한다.
     */

    // 로마 숫자를 정수로 변환하는 메서드
    public static void romanToInt(String s) {

        if (!isValidRoman(s)) {
            System.out.println("잘못된 로마 숫자 표기입니다.");
            return;
        }

        int sum = 0;
        int length = s.length();

        for (int i = 0; i < length; i++) {
            int currentVal = romanMap.get(s.charAt(i));

            if (i + 1 < length) {
                int nextVal = romanMap.get(s.charAt(i + 1));
                if (currentVal < nextVal) {
                    sum -= currentVal;
                } else {
                    sum += currentVal;
                }
            } else {
                sum += currentVal;
            }
        }

        if (sum > 3999) {
            System.out.println("표현할 수 없는 범위의 수입니다.");
            return;
        }

        System.out.println("로마 숫자: " + s + " -> 정수: " + sum);
    }

    // 테스트 함수
    public static void testRomanToInt(String s) {
        romanToInt(s);
    }

    public static void main(String[] args) {
        // 올바른 로마 숫자 표기 + 3999 이하 -> 기대 값 : 정수 값 
        testRomanToInt("II"); 
        testRomanToInt("XII"); 
        testRomanToInt("IV"); 
        testRomanToInt("IX"); 
        testRomanToInt("MMMDCCCLXXXVIII"); // 정상적인 로마 숫자 (3888)

        // 올바른 로마 숫자 표기 + 3999 초과
        // 정형화된 로마 숫자 표기법에는 이러한 케이스가 존재하지 않음

        // 잘못된 로마 숫자 표기 + 3999 이하 -> 기대 값 : "잘못된 로마 숫자 표기"
        testRomanToInt("IIX"); 
        testRomanToInt("VX"); 

        // 잘못된 로마 숫자 표기 + 3999 초과 -> 기대 값 : "잘못된 로마 숫자 표기"
        testRomanToInt("MMMM"); 
        testRomanToInt("MMMMCMXC"); // 4990, 범위를 초과하는 경우    
        }
    }
}

```

---
# 외계어 사전 만들기

### 문제 요구사항 분석

1. 문자열에서 중복된 문자를 제거해야 한다.
2. 중복 제거 후 남은 문자들 중 사전순으로 가장 앞서는 문자를 찾아야 한다.
3. 그 문자가 원래 문자열에서 처음 등장하는 위치를 출력해야 한다.
4. 중복 제거 후 남은 문자가 없다면 "중복 제거 후 빈 문자열이 되었습니다."를 출력해야 한다.

## 설계

### 알고리즘 및 자료구조

1. 입력 문자열을 왼쪽에서 오른쪽으로 차례로 읽으면서 각 문자가 처음 등장하는지 확인한다.
2. 처음 등장하는 문자는 남기고, 이미 등장한 문자는 제거한다.
    1.  이를 위해 `LinkedHashSet`을 사용하여 입력 문자열에서 중복된 문자를 제거하고 순서를 유지한다.
3. `LinkedHashSet`을 리스트로 변환한 후, 정렬하여 사전순으로 가장 앞서는 문자를 찾는다.
4. 해당 문자가 원래 문자열에서 처음 등장하는 위치를 찾아서 결과를 출력한다.

### 코드 흐름도

입력 문자열 -> 중복 제거 -> 리스트 변환 및 정렬 -> 사전순 첫 문자 찾기 -> 위치 확인 -> 결과 출력

### 최종 코드

```java

import java.util.LinkedHashSet;
import java.util.Set;
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

public class AlienDictionary {
    public static void processInput(String input) {
        System.out.println("입력: " + input);

        if (input.isEmpty()) {
            System.out.println("중복 제거 후 빈 문자열이 되었습니다.");
            return;
        }

        // LinkedHashSet을 사용하여 입력 문자열에서 중복된 문자를 제거하고 순서를 유지
        Set<Character> uniqueCharsSet = new LinkedHashSet<>();
        for (char c : input.toCharArray()) {
            uniqueCharsSet.add(c);
        }

        // Set을 List로 변환하여 정렬
        List<Character> uniqueCharsList = new ArrayList<>(uniqueCharsSet);
        Collections.sort(uniqueCharsList);

        if (uniqueCharsList.isEmpty()) {
            System.out.println("중복 제거 후 빈 문자열이 되었습니다.");
        } else {
            // 사전순으로 가장 앞서는 문자 찾기
            char firstChar = uniqueCharsList.get(0);

            // 해당 문자가 원래 문자열에서 처음 등장하는 위치 찾기
            int firstIndex = input.indexOf(firstChar);

            System.out.println("결과: " + firstChar + " (인덱스: " + firstIndex + ")");
        }
        System.out.println(); // 결과 간 공백 추가
    }

    public static void main(String[] args) {
        // 테스트할 입력 값 배열
        String[] testInputs = {
            "cbacabcde",
            "abcabc",
            "zxyab",
            "",
            "a",
            "bb",
            "abcdefghijklmnopqrstuvwxyz",
            "thequickbrownfoxjumpsoverthelazydog"
        };

        // 각 입력 값에 대해 결과 출력
        for (String input : testInputs) {
            processInput(input);
        }
    }
}

실행 결과 
입력: cbacabcde
결과: a (인덱스: 2)

입력: abcabc
결과: a (인덱스: 0)

입력: zxyab
결과: a (인덱스: 3)

입력: 
중복 제거 후 빈 문자열이 되었습니다.

입력: a
결과: a (인덱스: 0)

입력: bb
결과: b (인덱스: 0)

입력: abcdefghijklmnopqrstuvwxyz
결과: a (인덱스: 0)

입력: thequickbrownfoxjumpsoverthelazydog
결과: a (인덱스: 29)
```

# 해피 아일랜드의 수학자

### 사고 과정
**1. 문제 이해**

주어진 문제는 특정 수 `n`이 "행복한 수"인지 판별하는 문제입니다. 행복한 수란, 해당 수의 각 자릿수를 제곱하여 합한 결과를 반복적으로 계산했을 때, 최종적으로 1이 되는 수를 뜻합니다. 이에 따라 고려해야 할 점으로 반복적인 계산과 이 계산에서 필요한 **제약 조건을 분석**하여 핵심을 찾아내는 것을 중심으로 문제를 이해했습니다.

**2. 제약 조건 분석**

n의 범위가 최대 10^6인 점과 실행 시간이 1초(약 10^8)인 조건으로 보아 코드 작성 시에 시간 복잡도를 고려하여 작성해야 했고, ‘ 각 자릿수를 제곱한 합의 결과가 반복되면 사이클이 발생한다.’ 의 조건을 보았을 때 이전의 결과를 확인하기 위해 중간 계산 결과를 저장해야 하는 **집합**이 필요하다고 생각하여 로직을 설계했습니다.

**3. 분석을 통한 고려 사항**

- **자료구조 선택**: 중간 결과를 저장하기 위해 반복 여부를 확인할 수 있도록 HashSet을 사용하여 사이클을 확인한다.
- **반복 구조**: 각 자릿수를 분리하고 제곱한 합을 반복적으로 계산하는 구조를 설계해야 한다.
- **효율성 고려**: 검색 시간 복잡도가 O(1)인 Set을 사용하여 반복적인 계산을 최소화하고, 필요한 중간 결과만 저장하여 실행 시간 1초 넘지 않도록 한다.

**4. 로직 설계**

위의 고려 사항을 토대로 로직 설계를 작성했습니다.

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

자릿수를 문자열로 변환하지 않고, 수학적 연산을 통해 직접 자릿수를 분리하고 제곱합을 계산하는 방식으로 최적화했습니다.

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

---
layout: post
title: "Programmers-신규 아이디 추천"
tags: [Programmers]
comments: true

---

위 문제는 Programmers 신규 아이디 추천 문제에 관한 설명입니다.

---

## 문제 설명

카카오에 입사한 신입 개발자 네오는 "카카오계정개발팀"에 배치되어, 

카카오 서비스에 가입하는 유저들의 아이디를 생성하는 업무를 담당하게 되었습니다. 

"네오"에게 주어진 첫 업무는 새로 가입하는 유저들이 카카오 아이디 규칙에 맞지 않는 아이디를 입력했을 때, 

입력된 아이디와 유사하면서 규칙에 맞는 아이디를 추천해주는 프로그램을 개발하는 것입니다.

다음은 카카오 아이디의 규칙입니다.

* 아이디의 길이는 3자 이상 15자 이하여야 합니다.
* 아이디는 알파벳 소문자, 숫자, 빼기(`-`), 밑줄(`_`), 마침표(`.`) 문자만 사용할 수 있습니다.
* 단, 마침표(.)는 처음과 끝에 사용할 수 없으며 또한 연속으로 사용할 수 없습니다.

"네오"는 다음과 같이 7단계의 순차적인 처리 과정을 통해 신규 유저가 입력한 아이디가 

카카오 아이디 규칙에 맞는 지 검사하고 규칙에 맞지 않은 경우 규칙에 맞는 새로운 아이디를 추천해 주려고 합니다.

신규 유저가 입력한 아이디가 `new_id` 라고 한다면,

```
1단계 new_id의 모든 대문자를 대응되는 소문자로 치환합니다.
2단계 new_id에서 알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 모든 문자를 제거합니다.
3단계 new_id에서 마침표(.)가 2번 이상 연속된 부분을 하나의 마침표(.)로 치환합니다.
4단계 new_id에서 마침표(.)가 처음이나 끝에 위치한다면 제거합니다.
5단계 new_id가 빈 문자열이라면, new_id에 "a"를 대입합니다.
6단계 new_id의 길이가 16자 이상이면, new_id의 첫 15개의 문자를 제외한 나머지 문자들을 모두 제거합니다.
     * 만약 제거 후 마침표(.)가 new_id의 끝에 위치한다면 끝에 위치한 마침표(.) 문자를 제거합니다.
7단계 new_id의 길이가 2자 이하라면, new_id의 마지막 문자를 new_id의 길이가 3이 될 때까지 반복해서 끝에 붙입니다.
```

예를 들어, new_id 값이 "...!@BaT#*..y.abcdefghijklm" 라면, 

위 7단계를 거치고 나면 new_id는 아래와 같이 변경됩니다.

1단계 대문자 'B'와 'T'가 소문자 'b'와 't'로 바뀌었습니다.
> "...!@BaT#*..y.abcdefghijklm" → "...!@bat#*..y.abcdefghijklm"

2단계 '!', '@', '#', '*' 문자가 제거되었습니다.
> "...!@bat#*..y.abcdefghijklm" → "...bat..y.abcdefghijklm"

3단계 '...'와 '..' 가 '.'로 바뀌었습니다.
> "...bat..y.abcdefghijklm" → ".bat.y.abcdefghijklm"

4단계 아이디의 처음에 위치한 '.'가 제거되었습니다.
> ".bat.y.abcdefghijklm" → "bat.y.abcdefghijklm"

5단계 아이디가 빈 문자열이 아니므로 변화가 없습니다.
> "bat.y.abcdefghijklm" → "bat.y.abcdefghijklm"

6단계 아이디의 길이가 16자 이상이므로, 처음 15자를 제외한 나머지 문자들이 제거되었습니다.
> "bat.y.abcdefghijklm" → "bat.y.abcdefghi"

7단계 아이디의 길이가 2자 이하가 아니므로 변화가 없습니다.
> "bat.y.abcdefghi" → "bat.y.abcdefghi"

따라서 신규 유저가 입력한 new_id가 "...!@BaT#*..y.abcdefghijklm"일 때, 

네오의 프로그램이 추천하는 새로운 아이디는 "bat.y.abcdefghi" 입니다.

### [문제]

신규 유저가 입력한 아이디를 나타내는 new_id가 매개변수로 주어질 때, 

"네오"가 설계한 7단계의 처리 과정을 거친 후의 추천 아이디를 return 하도록 solution 함수를 완성해 주세요.

### 제한 사항

* new_id는 길이 1 이상 1,000 이하인 문자열입니다.
* new_id는 알파벳 대문자, 알파벳 소문자, 숫자, 특수문자로 구성되어 있습니다.
* new_id에 나타날 수 있는 특수문자는 -_.~!@#$%^&*()=+[{]}:?,<>/ 로 한정됩니다.

```java
class Solution {
    public String solution(String new_id) {
        String answer;
        String step1 = new_id.toLowerCase();

        StringBuilder step2 = new StringBuilder();
        for (char c : step1.toCharArray()) {
            if (('a' <= c && c <= 'z') || ('0' <= c && c <= '9') || c == '-' || c == '_' || c == '.') {
                step2.append(c);
            }
        }
        String step3 = step2.toString();
        while (step3.contains("..")) {
            step3 = step3.replace("..", ".");
        }
        String step4 = step3;
        if (step4.length() > 0) {
            if (step4.charAt(0) == '.') {
                step4 = step4.substring(1, step4.length());
            }
        }
        if (step4.length() > 0) {
            if (step4.charAt(step4.length() - 1) == '.') {
                step4 = step4.substring(0, step4.length() - 1);
            }
        }
        String step5 = step4;
        if (step5.equals("")) {
            step5 = "a";
        }
        String step6 = step5;
        if (step6.length() >= 16) {
            step6 = step6.substring(0, 15);
            if (step6.charAt(step6.length() - 1) == '.') {
                step6 = step6.substring(0, step6.length() - 1);
            }
        }
        StringBuilder step7 = new StringBuilder(step6);
        if (step7.length() <= 2) {
            char last = step7.charAt(step7.length() - 1);

            while (step7.length() < 3) {
                step7.append(last);
            }
        }
        answer = String.valueOf(step7);
        return answer;
    }
}
```

# 이번 문제는 시뮬레이션 문제로 생각됩니다.

단계별로 살펴봅니다.

### 1단계
```java
        String step1 = new_id.toLowerCase();
```

toLowerCase()를 사용하면 대문자가 소문자로 변환됩니다.

### 2단계
```java
        StringBuilder step2 = new StringBuilder();
        for (char c : step1.toCharArray()) {
            if (('a' <= c && c <= 'z') || ('0' <= c && c <= '9') || c == '-' || c == '_' || c == '.') {
                step2.append(c);
            }
        }
```

1단계에서 나온 결과(step1)을 하나씩 꺼내서

알파벳 소문자, 숫자, 빼기(-), 밑줄(_), 마침표(.)를 제외한 

모든 문자는 append시키지 않는 방식으로 진행했습니다.

### 3단계
```java
        String step3 = step2.toString();
        while (step3.contains("..")) {
            step3 = step3.replace("..", ".");
        }
```

2단계의 결과는 스트링빌더에 있으므로, toString시켜서

step3에 넣어줬습니다.

`...`와 `..`을 변환 시켜야하는데 

`...`은 `..`가 1번 `.`으로 변환되고나면 `..`이 되기 때문에

다시 한번 변환을 적용시켜주는 방식으로 while에 넣어주었습니다.

만약 `..`이 없다면 while문은 종료됩니다.

### 4단계

```java
        String step4 = step3;
        if (step4.length() > 0) {
            if (step4.charAt(0) == '.') {
                step4 = step4.substring(1, step4.length());
            }
        }
        if (step4.length() > 0) {
            if (step4.charAt(step4.length() - 1) == '.') {
                step4 = step4.substring(0, step4.length() - 1);
            }
        }
```
4단계입니다.

처음과 끝의 `.` 이 있는가 확인한 후 있다면 substring을 통해 잘라주었습니다.

### 5단계

```java
String step5 = step4;
        if (step5.equals("")) {
            step5 = "a";
        }
```

공백이 있다면 `a`로 바꿔줍니다.

### 6단계

```java
  String step6 = step5;
        if (step6.length() >= 16) {
            step6 = step6.substring(0, 15);
            if (step6.charAt(step6.length() - 1) == '.') {
                step6 = step6.substring(0, step6.length() - 1);
            }
        }
```

길이가 16이라면 15자로 바꾸고,

또 맨끝이 `.`이라면 substring으로 끝단을 잘라줬습니다.

### 7단계

```java
        StringBuilder step7 = new StringBuilder(step6);
        if (step7.length() <= 2) {
            char last = step7.charAt(step7.length() - 1);

            while (step7.length() < 3) {
                step7.append(last);
            }
        }
```

step7의 길이가 2보다 작다면 

길이가 3이 될 때 까지 반복합니다.

<a href= "https://programmers.co.kr/learn/courses/30/lessons/72410">Programmers-신규 아이디 추천</a>

---

# 2021년 06월 16일 화요일 TIL(Set, Greedy)

## 오늘 한 일과 느낀점
- 요즘 내가 아직 많이 부족하다는 느낌을 많이 받는다. 그래서 좀 다르게 공부해보려고 하는데 길을 좀 잃은 기분이다. 저번주 토요일에 배송 온 `프레임워크 없는 프론트엔드 개발` 책을 읽는데 사실 선언형과 함수형 프로그래밍 방식으로 예제가 나오는데 나는 그 두가지의 코드를 아직 수월하게 독해하지 못한다... 스터디를 진행하며 js로 상태관리를 하고 렌더링을 하는 나의 로직이 아주아주 마음에 들지 않아 이 책을 읽기 시작했는데 진전이 너무 없다... 이번 스터디는 코드 추가도 아예 하지 않아 리뷰에 참석하지 못할것 같아 이도 저도 아니게 된것 같아 기분이 매우 별로다...

- 너무 답답해서 요즘 풀지 않던 알고리즘을 프로그래머스를 통해 풀었다. 일단... 아주 간단한건 당연히 언어의 이해도는 어느정도 있으니 금방 푼다. 그래서 지금까지 3번인가... 도전하고 실패했던 체육복 알고리즘을 꼭 풀어버리기로 마음먹었다. 지금 나의 알고리즘 수준은 아주 1차원적이라고 생각한다. 접근방식이 너무 단순하다. 쉽게 컴퓨터가 사고하는 방식이 아니라 완전히 사람이 사고하는 방식으로 접근을 한다는것이다. 그래서 아무것도 모르고 메달려 있는것보단 이 문제가 무엇인지 알아보고 어떤식으로 접근 할지 알아봤다. 일단 Greedy 알고리즘은 한국어로 욕심, 탐욕법 알고리즘 이라고 한다.  
나무위키에는 `"매 선택에서 지금 이 순간 당장 최적인 답을 선택하여 적합한 결과를 도출하자"` 라는 모토를 가지는 알고리즘 설계 기법이라고 설명하고 있다. 물론 그래서 그 순간 이후의 선택에 관해 신경쓰지 않기 때문에 무조건 최적의 알고리즘이 될 순 없고 그래서 부분부분의 선택에서의 최적의 답이 전체 선택의 최적이 될때만 사용하는 알고리즘이라고 한다.  

  - 체육복 문제의 요구사항은 다음과 같다.  

    - 문제 설명
    > 점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.  
    전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

    - 제한사항
      - 전체 학생의 수는 2명 이상 30명 이하입니다.
      - 체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
      - 여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
      - 여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
      - 여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

    - 입출력 예
    | n | lost | reserve | return |
    |:---:|:---:|:---:|:---:|
    | 5 | [2, 4] | [1, 3, 5] | 5 |
    | 5 | [2, 4] | [3] | 4 |
    | 3 | [3] | [1] | 2 |

    - 입출력 예 설명

      - 예제 #1
      1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

      - 예제 #2
      3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.

  - 첫번째로 Set을 이용하여 중복을 제거하며 여분의 체육복을 가진 학생의 명단에서 체육복을 잃어버린 학생이 있다면 그것을 제거해주고 정말 수학적 계산으로 풀려고 했으나 계산실패로 일단 보류해두고 다른 방법을 선택했다.

  - 두번째로 총 학생의 수인 n을 이용하여 length가 n인 배열을 만들고 지문을 그대로 적용시켜보았다. 일단 학생들에게 체육복을 모두 1씩 나누어줌을 나타내기위해 배열의 모든 요소를 1로 채워주었다. 그리고 체육복을 잃어버린 학생이 해당하는 index(만약 lost배열에 2번이라고 한다면 1번 index)를 -1 처리해주고 여분의 체육복을 가지고 온 학생에 해당하는 index는 + 1씩 늘려주었다. 그럼 이제 0, 1, 2중 하나의 수가 배열의 요소에 담겨있게 된다. 여기서 만약 요소가 0인 index는 그 index 의 -1, +1에 해당하는 index의 값이 2인 요소를 찾고 만약 있다면 요소가 0인 index를 +1 처리해주고 빼앗아 온 index에서는 -1을 해준다. 당연히 -1, +1 에 해당하는 index의 값이 2가 아니라면 그 요소는 0으로 남아있을것이다. 그럼 마지막에 0이 아닌 요소들만이 체육복을 가지고 수업을 들을 수 있게된다. `student.filter(v => v > 0).length` 대충 이런 코드가 나오게 될 것이다.

  이렇게해서 해결은 했는데 사실 위에서 알아본 Greedy 알고리즘에 부합되게 푼지는 아직 잘 모르겠다. 프로그래머스에선 1단계에 해당되는 문제였고 푼 사람들도 상당히 많았는데 나는 다른것보다 유난히 이 알고리즘에 애를 많이 먹었던것 같아서 풀고 뿌듯한것도 있지만 또 내가 부족하다는 느낌을 더더욱 받게 됐다... 화이팅ㅠ

- 위 알고리즘을 풀면서 Set에 대해 poiemaWeb을 통해 조금 알게된것 같다. 앞으로 중복을 제거하거나 수학적인 계산을 해야할때면 Set을 좀 더 적극적으로 활용할 수 있었으면 한다. 아 그리고 시간을 한번 내서 체육복 알고리즘을 Set으로 풀다가 말았는데 완성도 시켜봐야겠다.

## 내일 할 일
- 내일 블랙커피 스터디 온라인세션 시간인데 코드를 하나도 작성하지 못했다. 일단... 될 수 있는데까지 코드를 작성해보고 아마 PR은 못보내고 참여는 하여 다른분들이 어떻게 처리했나보고 다음 미션에 대한 설명을 들어야겠다.
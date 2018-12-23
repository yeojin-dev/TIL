# System Sort

실제 시스템에 구현된 정렬 알고리즘은 무엇일까?

## JAVA API에서 퀵 정렬 대신 합병 정렬을 사용하는 이유

1. 퀵 정렬은 안정 정렬이 아니나 합병 정렬은 안정 정렬
2. 항상 O(NlnN)의 시간복잡도를 보장함
3. 추가 메모리에 대한 부담이 과거보다 약해짐

## JAVA API에서 퀵 정렬을 기본 자료형에서 사용하는 이유

1. 추가 메모리를 사용하지 않음
2. 가장 빠르기 때문

## 여러 정렬 정리

정렬|추가 메모리 없음|안정|최악|평균|최선
---|------------|---|---|---|---
selection|O|X|N^2|N^2|N^2
insertion|O|O|N^2|N^2|N
shell|O|X|?|?|N
merge|X|O|NlnN|NlnN|NlnN
quick|O|X|N^2|NlnN|NlnN
3-way quick|O|X|N^2|NlnN|N

* 완벽한 정렬 알고리즘은 없다.
# YAML

## 기본문법

- 기본적으로 2칸 또는 4칸을 지원 (2칸을 추천)
- 데이터는 key: value 형식으로 정의
- 배열은 - 로 표시
- 주석은 #으로 표시
- true/false외에 yes, no를 지원
- 정수 또는 실수를 따옴표(") 없이 사용하면 숫자로 인식

```yaml
apiVersion: v1
kind: pod
metadata:
  name: test
spec:
  containers:
  - name: test
...
```

- newline 여러줄 표현

  - "|" 지시어는 마지막 줄바꿈이 포함
  - "|-" 지시어는 마지막 줄바꿈을 제외
  - ">" 지시어는 중간에 들어간 빈줄을 제외

  ```yaml
  newlines_sample: |
  	number on line
      second line
      lasteline
      
  newlines_sample: |-
  	number on line
      second line
      lasteline
      
  newlines_sample: >
  	number on line
  	second line
  	lasteline
  ```

## 주의사항

- 띄어쓰기

  - key와 value사이에는 반드시 빈칸이 필요

- 문자열 따옴표

  - 대부분의 문자열은 따옴표 없이 사용할 수 있지만 : 가 들어간 경우는 반드시 따옴표가 필요

  ```yaml
  # error
  windows_drive: c:
  ---
  windows_drive: "c"
  ```

## 참고

- json2yaml
  - json → yaml 변환
  - https://www.json2yaml.com/
- yamlint
  - YAML 문법을 체크하고 해석
  - http://www.yamllint.com/
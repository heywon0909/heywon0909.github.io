# 6장, Axios 타입 분석하기

- Axios는 자체적으로 타입이 선언되어 있다..
- 진입점 파일 = node_modules/axios/package.json 의 types 속성
  - type 속성은 axios 패키지가 어떤 모듈 시스템을 사용하는지 나타냄

### axios 의 타입을 어떻게 찾았는지 이해하기

- 타입스크립트가 **진입점 파일**을 찾는 경로
  1. 현재 파일이 있는 폴더에 node_modules가 있는지 확인, 없으면 부모 폴더로 올라가 찾기
  2. node_modules/module.ts
  3. node_modules/module.tsx
  4. node_modules/module.d.ts
  5. node_modules/module/package.json 속성 찾기
  6. node_modules/module/index.ts
  7. node_modules/module/index.d.ts
  8. node_modules/@types/module/package.json 속성 찾기
  9. node_modules/@types/module.d.ts
  10. node_modules/@types/module/index.d.ts
  11. 2-11번 안에 못 찾으면 부모 폴더로 올라가서 다시 반복
  12. 최상위 폴더까지 갔는데도 못 찾으면 에러
  - **자체적으로 타입을 선언해 놓은 경우/ 커뮤니티에서 타입을 지원하는 경우인지 확인해야 함‼ 🧐**

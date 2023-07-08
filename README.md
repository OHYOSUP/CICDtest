# Eslint, Prettier, Husky

## Liter, Formatter
- 하나의 프로젝트에 작업자마다 각자 다른 코딩 스타일을 가지고 있기 때문에 팀 프로젝트를 진행할 때 코드 컨벤션을 통일시켜줄 필요가 있다.
- 이를 도와주는 것이 linter(EsLint), Formatter(Prettier)
- Eslint는 문법 교정과 코드 스타일링을, Pretier는 코드 스타일을 맞춰주는 기능을 한다

### Eslint, Prettier 설치
1. ```npm install eslint --save-dev```
  - CRA의 경우 내장되어 있기 때문에 따로 설치하지 않는게 좋음
2. ```npm install prettier --save-dev```
3. ```npm install eslint-config-prettier --save-dev```
  - eslint는 linting기능과 formatting기능을 동시에 가지고 있다. 때문에 prettier와 같이 사용할 때 서로 다른 설정을 가지고 있다면
    충돌할 가능성이 있기 때문에 eslint는 linting기는만, prettier는 formatting기능만 담당하는 것이 이상적이다.
    
#### prettier 설정
- prettier는 root디렉토리에 `.prettierrc.확장자`파일을 통해서 설정할 수 있다.
```
// .prettierrc.js
module.exports = {
  printWidth: 100, // printWidth default 80 => 100 으로 변경
  singleQuote: true, // "" => ''
  arrowParens: "avoid", // arrow function parameter가 하나일 경우 괄호 생략
};
```
#### prettier 실행
- `npx prettier .`
- `npx prettier --write .`
    - 저장하고 파일에 적용
 
### Eslint 설정
- eslint에서 제공하지 않는 특정 환경을 위한 룰들은 plug in을 통해 적용할 수 있다.
```
// .eslintrc
{
  "extends": ["react-app", "eslint:recommended", "prettier"],
  "rules": {
    "no-var": "error", // var 금지
    "no-multiple-empty-lines": "error", // 여러 줄 공백 금지
    "no-console": ["error", { "allow": ["warn", "error", "info"] }], // console.log() 금지
    "eqeqeq": "error", // 일치 연산자 사용 필수
    "dot-notation": "error", // 가능하다면 dot notation 사용
    "no-unused-vars": "error" // 사용하지 않는 변수 금지
  }
}
```
```
// .eslintrc
{
  "extends": ["react-app", "eslint:recommended", "prettier"],
  "rules": {
    "no-var": "error", // var 금지
    "no-multiple-empty-lines": "error", // 여러 줄 공백 금지
    "no-console": ["error", { "allow": ["warn", "error", "info"] }], // console.log() 금지
    "eqeqeq": "error", // 일치 연산자 사용 필수
    "dot-notation": "error", // 가능하다면 dot notation 사용
    "no-unused-vars": "error" // 사용하지 않는 변수 금지
  }
}
```
### Husky
- linter와 formatter의 설정을 완료 했으면 보다 편리하게 사용하기 위해 자동화를 해주는게 좋다.
- git hook -> git에서 특정 이벤트가 발생하기 전 후로 특정 hook동작을 발생할 수 있도록 하는 것
- git hook 설정을 도와주는 도구가 바로 husky
    - 번거로운 git hook 설정이 편함 + npm install 과정에서 사전에 세팅해둔 git hook을 다 적용시킬 수 있어서 모든 팀원이 git hook 실행이 되도록 하기가 편함
#### 설치
1. `npm install husky --save-dev`
2. (처음 husky 세팅하는 사람만 실행 필요) npx husky install
3. add post install script in package.json
```
// package.json
{
  "scripts": {
    "postinstall": "husky install"
  },
}
```
4. script설정
```
// package.json

{
  "scripts": {
    "postinstall": "husky install",
		"format": "prettier --cache --write .", // --cache : 변경 사항이 있는 부분만 포맷팅
		"lint": "eslint --cache .",
  },
}
```
5. add pre-commit, pre-push hook
    1. `npx husky add .husky/pre-commit "npm run format"`
    2. `npx husky add .husky/pre-push "npm run lint"`
        1. push 전에 lint를 하는 이유
            - 코드에 lint 에러가 있어서 전체적인 코드 컨벤션을 헤치는 것을 막기 위해
          
## git hook에서 linter에러가 발생하면 실행중인 script가 종료되기 전에 해당 rule에 대해 warn으로 할 것인지 error로 할 것인지 잘 고려해서 정해야 한다.

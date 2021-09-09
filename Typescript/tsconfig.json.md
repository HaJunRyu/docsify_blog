# tsconfig.json

## tsconfig.json은 무엇인가

tsconfig.json 파일은 TypeScript 프로젝트를 컴파일하는데 필요한 파일들의 경로와 컴파일러 옵션 등을 지정하는데 사용된다. TypeScript가 설치된 환경에서 CLI로 `tsc` 명령어를 실행하면 현재 디렉토리에서 시작하여 상위 디렉토리 체인으로 tsconfig.json파일을 검색한다.

```bash
# 현재 디렉토리에서 부터 tsconfig.json 파일을 찾을때까지 상위 디렉토리 체인을 통해 검색 후 컴파일
$ tsc
# 만약 상위 디렉토리 체인에서 tsconfig.json파일을 찾지 못할 경우 에러가 발생한다.
```

테스트를 해보니 실제로 프로젝트 root경로에서 tsconfig.json을 삭제하고 상위 디렉토리에 위치시켯더니 작동을 했다. 하지만 include 설정을 해주지 않으면 해당 tsconfig.json가 있는 디렉토리의 모든 하위목록의 `ts`, `d.ts`, `tsc`확장자 파일들이 컴파일 되는 현상이 있었다. tsconfig.json파일은 꼭 프로젝트의 root경로에 위치시키자

무작정 상위 디렉토리 체인을 검색하는게 아닌 tsconfig.json이 포함된 특정 디렉토리를 지정하려면 `—project` 또는 축약 표현인 `-p`를 사용할 수 있다.

```bash
# root 경로의 config 디렉토리 내부의 tsconfig.json 파일로 컴파일 실행
$ tsc --project config
# 또는
$ tsc -p config
```

`tsc`명령어 뒤에 직접 컴파일 될 파일을 지정해주게 되면 tsconfig.json 파일은 무시된다.

```bash
# tsconfig.json 파일과 관계없이 컴파일 된다. 그렇기 때문에 컴파일 옵션을 직접 CLI로 작성해주어야 한다.
$ tsc src/index.ts
```

그리고 만약 CLI명령어로 컴파일 옵션을 직접 지정해준다면 tsconfig.josn의 명령보다 우선시 된다.

```bash
# tsconfig.json에 module옵션이 esnext로 되어있어도 CLI에 commonjs를 입력하면 commonjs로 적용
$ tsc -m "commonjs"
```

tsconfig.json 파일을 직접 만들어 설정해줄수도 있지만 타입스크립트에서는 tsconfig.json 정해진 템플릿으로 생성해주는 명령이 있다.

```bash
# 프로젝트 root경로에서 CLI 명령
$ tsc --init
```

위 명령을 입력해보고 root 디렉토리에 생성된 tsconfig.json파일을 확인해보자. 원래는 주석의 경우도 영어로 표시되지만 해당글은 한글로 번역된것이다.

```json
{
  "compilerOptions": {

    /* 기본 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    // "incremental": true,                   /* 증분 컴파일 활성화 */
    "target": "es5",                          /* ECMAScript 목표 버전 설정: 'ES3'(기본), 'ES5', 'ES2015', 'ES2016', 'ES2017','ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */
    "module": "esnext",                       /* 생성될 모듈 코드 설정: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */
    "lib": ["dom", "dom.iterable", "esnext"], /* 컴파일 과정에 사용될 라이브러리 파일 설정 */
    "allowJs": true,                          /* JavaScript 파일 컴파일 허용 */
    // "checkJs": true,                       /* .js 파일 오류 리포트 설정 */
    "jsx": "react",                           /* 생성될 JSX 코드 설정: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* '.d.ts' 파일 생성 설정 */
    // "declarationMap": true,                /* 해당하는 각 '.d.ts'파일에 대한 소스 맵 생성 */
    // "sourceMap": true,                     /* 소스맵 '.map' 파일 생성 설정 */
    // "outFile": "./",                       /* 복수 파일을 묶어 하나의 파일로 출력 설정 */
    // "outDir": "./dist",                    /* 출력될 디렉토리 설정 */
    // "rootDir": "./",                       /* 입력 파일들의 루트 디렉토리 설정. --outDir 옵션을 사용해 출력 디렉토리 설정이 가능 */
    // "composite": true,                     /* 프로젝트 컴파일 활성화 */
    // "tsBuildInfoFile": "./",               /* 증분 컴파일 정보를 저장할 파일 지정 */
    // "removeComments": true,                /* 출력 시, 주석 제거 설정 */
    "noEmit": true,                           /* 출력 방출(emit) 유무 설정 */
    // "importHelpers": true,                 /* 'tslib'로부터 헬퍼를 호출할 지 설정 */
    // "downlevelIteration": true,            /* 'ES5' 혹은 'ES3' 타겟 설정 시 Iterables 'for-of', 'spread', 'destructuring' 완벽 지원 설정 */
    "isolatedModules": true,                  /* 각 파일을 별도 모듈로 변환 ('ts.transpileModule'과 유사) */

    /* 엄격한 유형 검사 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "strict": true,                           /* 모든 엄격한 유형 검사 옵션 활성화 */
    // "noImplicitAny": true,                 /* 명시적이지 않은 'any' 유형으로 표현식 및 선언 사용 시 오류 발생 */
    // "strictNullChecks": true,              /* 엄격한 null 검사 사용 */
    // "strictFunctionTypes": true,           /* 엄격한 함수 유형 검사 사용 */
    // "strictBindCallApply": true,           /* 엄격한 'bind', 'call', 'apply' 함수 메서드 사용 */
    // "strictPropertyInitialization": true,  /* 클래스에서 속성 초기화 엄격 검사 사용 */
    // "noImplicitThis": true,                /* 명시적이지 않은 'any'유형으로 'this' 표현식 사용 시 오류 발생 */
    // "alwaysStrict": true,                  /* 엄격모드에서 구문 분석 후, 각 소스 파일에 "use strict" 코드를 출력 */

    /* 추가 검사 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    // "noUnusedLocals": true,                /* 사용되지 않은 로컬이 있을 경우, 오류로 보고 */
    // "noUnusedParameters": true,            /* 사용되지 않은 매개변수가 있을 경우, 오류로 보고 */
    // "noImplicitReturns": true,             /* 함수가 값을 반환하지 않을 경우, 오류로 보고 */
    "noFallthroughCasesInSwitch": true,       /* switch 문 오류 유형에 대한 오류 보고 */
    // "noUncheckedIndexedAccess": true,      /* 인덱스 시그니처 결과에 'undefined' 포함 */

    /* 모듈 분석 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "moduleResolution": "node",               /* 모듈 분석 방법 설정: 'node' (Node.js) 또는 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* 절대 경로 모듈이 아닌, 모듈이 기본적으로 위치한 디렉토리 설정 (예: './modules-name') */
    // "paths": {},                           /* 'baseUrl'을 기준으로 상대 위치로 가져오기를 다시 매핑하는 항목 설정 */
    // "rootDirs": [],                        /* 런타임 시 프로젝트 구조를 나타내는 로트 디렉토리 목록 */
    // "typeRoots": [],                       /* 유형 정의를 포함할 디렉토리 목록 */
    // "types": [],                           /* 컴파일 시 포함될 유형 선언 파일 입력 */
    "allowSyntheticDefaultImports": true,     /* 기본 출력(default export)이 없는 모듈로부터 기본 호출을 허용 (이 코드는 단지 유형 검사만 수행) */
    "esModuleInterop": true                   /* 모든 가져오기에 대한 네임스페이스 객체 생성을 통해 CommonJS와 ES 모듈 간의 상호 운용성을 제공. 'allowSyntheticDefaultImports' 암시 */
    // "preserveSymlinks": true,              /* symlinks 실제 경로로 결정하지 않음 */
    // "allowUmdGlobalAccess": true,          /* 모듈에서 UMD 글로벌에 접근 허용 */

    /* 소스맵 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    // "sourceRoot": "./",                    /* 디버거(debugger)가 소스 위치 대신 TypeScript 파일을 찾을 위치 설정 */
    // "mapRoot": "./",                       /* 디버거가 생성된 위치 대신 맵 파일을 찾을 위치 설정 */
    // "inlineSourceMap": true,               /* 하나의 인라인 소스맵을 내보내도록 설정 */
    // "inlineSources": true,                 /* 하나의 파일 안에 소스와 소스 코드를 함께 내보내도록 설정. '--inlineSourceMap' 또는 '--sourceMap' 설정이 필요 */

    /* 실험적인 기능 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    // "experimentalDecorators": true,        /* ES7 데코레이터(decorators) 실험 기능 지원 설정 */
    // "emitDecoratorMetadata": true,         /* 데코레이터를 위한 유형 메타데이터 방출 실험 기능 지원 설정 */

    /* 고급 옵션
     * ------------------------------------------------------------------------------------------------------------------------------------------------ */
    "skipLibCheck": true,                     /* 선언 파일 유형 검사 스킵 */
    "forceConsistentCasingInFileNames": true  /* 동일한 파일에 대한 일관되지 않은 케이스 참조를 허용하지 않음 */

  }
}
```

파일을 살펴보면 템플릿이기 때문에 아주 많은 내용이 들어있지만 사실은 대부분 주석처리 되어있어 실제로 적용되어 있는 옵션은 몇개 되지 않고 각각의 옵션들에 대해 주석 해제만 하여 사용할 수 있게끔 제공하고 있다.

## tsconfig.json의 속성들

tsconfig.json의 모든 옵션은 생략될 수 있으며 생략하면 컴파일러의 기본값이 사용된다. 즉, tsconfig.json파일은 완전히 비어있어도 정상적으로 작동한다.

### `"compilerOptions"`

`ts`, `d.ts`, `tsx`파일들을 `js`파일로 컴파일할때 적용 할 옵션들을 지정해준다. compilerOptions의 모든 옵션을 살펴보려면 [타입스크립트 공식문서](https://www.staging-typescript.org/tsconfig#compiler-options) 또는 [타입스크립트 핸드북(KR)](https://typescript-kr.github.io/pages/compiler-options.html)을 참고하자.

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  }
  // ...
}
```

### `"include"` 와 `"exclude"`

컴파일에 포함할 디렉토리, 파일 경로를 설정하거나, 제외시킬 수 있다. 포함, 제외될 각 항목에는 [glob 패턴](<https://ko.wikipedia.org/wiki/%EA%B8%80%EB%A1%9C%EB%B8%8C_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)>)을 사용하여 표기할 수 있다.

- `*` 0개 이상의 모든 문자와 일치 (디렉토리 분리 기호 제외)
- `?` 1개 문자와 일치 (디렉토리 분리 기호 제외)
- `**/` 모든 하위 디렉토리까지 포함

```json
{
  // ...
  // 컴파일 포함
  "include": ["src/**/*.tsx?"],
  // 컴파일 제외
  "exclude": [
    "node_modules",
    "build",
    "**/*.(spec|test).ts"
    // 명시하지 않아도 기본적으로 outDir옵션으로 지정한 경로의 하위목록도 제외된다.
  ]
}
```

만약 위와 같이 명시해주지 않을 경우 include와 exclude의 기본값은 다음과 같다.

```json
{
  // ...
  "include": ["**/*"],
  "exclude": ["node_modules", "bower_components", "jspm_packages"]
  // 그리고 기본적으로 outDir옵션으로 지정된 경로의 하위목록 또한 제외시킨다.
  // ...
}
```

하지만 exclude 옵션을 사용할때 예외가 있는데 exclude된 모듈(파일)을 내보내기(export) 하여 다른 파일에서 참조하고 있을때 참조하고 있는 파일이 컴파일 대상이라면 exclude된것과 상관없이 해당 모듈도 같이 컴파일이 된다.

```json
{
  "include": ["src/index.ts"],
  "exclude": ["src/sub.ts"]
}
```

간단하게 위와 같은 옵션이 설정되었을때 sub.ts를 export하여 index.ts에서 import하여 참조하고 있다면 sub.ts는 exclude옵션의 대상이 된것과 상관없이 컴파일 된다.

### `"files"`

include 옵션과 별개로 컴파일 할 파일들을 직접 지정한다. 그리고 files옵션으로 지정해준다면 exclude로 지정된 경로와 상관없이 항상 컴파일 대상이 된다. 하지만 일반적으로 include와 exclude만을 사용하여도 원하는 작업이 가능하기 때문에 많이 사용하지는 않는 옵션이다.

```json
{
  // ...
  "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "emitter.ts",
    "program.ts",
    "commandLineParser.ts",
    "tsc.ts",
    "diagnosticInformationMap.generated.ts"
  ]
  // ...
}
```

### `"extends"`

tsconfig.json 파일은 extends 속성을 사용해 다른 파일의 설정을 상속하여 사용할 수 있다.

`tsconfig.base.json`

```json
{
  "compilerOptions": {
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}
```

`tsconfig.json`

```json
{
  "extends": "./tsconfig",
  "compilerOptions": {
    "strictNullChecks": false
  }
}
```

이렇게 기본적인 설정 파일을 extends 키워드로 상속받고 원하는 옵션들을 재정의하여 사용할 수 있다. 패키지로 만들어서 사용하는 패턴도 가능할것 같고 webpack을 사용하는것처럼 dev환경과 prod환경을 나눌 일이 있을지 모르겠지만 그렇게도 사용이 가능할 것 같다.

### `"compileOnSave"`

최상위 속성 compileOnSave를 IDE에 설정하면 저장 시 지정된 tsconfig.json를 기반으로 컴파일을 시작한다. 마치 프리티어의 formatOnSave옵션을 생각하면 이해하기 편할것 같다.

```json
{
  "compileOnSave": true
  // ...
}
```

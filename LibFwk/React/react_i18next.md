# i18next

> - 개요
> - react-i18next

- [React Multi-lingual with react-i18next](https://medium.com/@ricklee_10931/react-multi-lingual-with-react-i18next-57879f986168)

## 개요

- 여러 플랫폼에서 다양한 언어를 지원하기 위해 만들어진 라이브러리
- 다국어 지원 외에도 언어적 측면에서 활용 가능

## react-i18next

- React에서 i18next 사용하기

```Bash
yarn add i18next i18next-xhr-backend react-i18next  # yarn
# npm i i18next i18next-xhr-backend react-i18next --save  # npm
```

### i18n.ts

- i18next 설정파일
- keySeperator: '.' -> JSON 객체를 .으로 구분

```TypeScript
import i18n from 'i18next';
import Backend from 'i18next-xhr-backend';
import { initReactI18next } from 'react-i18next';

i18n
  .use(Backend)
  .use(initReactI18next)
  .init({
    lng: 'ko',
    backend: {
      /* translation file path */
      loadPath: '/assets/i18n/{{ns}}/{{lng}}.json'
    },
    fallbackLng: 'ko',
    debug: true,
    /* can have multiple namespace, in case you want to divide a huge translation into smaller pieces and load them on demand */
    ns: ['translations'],
    defaultNS: 'translations',
    keySeparator: '.',
    interpolation: {
      escapeValue: false,
      formatSeparator: ','
    },
    react: {
      wait: true
    }
  });

export default i18n;
```

### useTranslation()

- useTranslation(): { t, i18n }
- t: TFunction 함수
- i18n: i18n 객체
- i18n.changeLanguage(lng: string) -> lng의 언어로 바꾸기

Navigation.tsx
```TSX
import { useTranslation } from "react-i18next";

const Navigation = () => {
  const { i18n } = useTranslation();
  const changeLanguage = (e: React.ChangeEvent<HTMLSelectElement>) => {
    i18n.changeLanguage(e.target.value);
  };

  return (
    <div className="language-select">
      <p>Language: </p>
      <select
        className="custom-select"
        value={i18n.language}
        onChange={changeLanguage}
      >
        <option value="ko">한국어</option>
        <option value="en">English</option>
        <option value="ja">日本語</option>
      </select>
    </div>
  );
};

export default Navigation;
```

- t(key: string | TemplateStringsArray | ...)
- key에 해당하는 값을 찾아 반환
- options의 returnObjects: true로 설정 -> 반환값이 string이 아니라 object

- Resume.tsx
```TSX
import { useTranslation } from "react-i18next";
import Navigation from "./Navigation";

// contents.about.title/content: string
// contents.interests.content: Array<{title: string, content: string}>
const Resume = () => {
  const { t } = useTranslation();

  return (
    <div className="app-content resume">
      <Navigation/>
      <resumeContents.About
        title={t('contents.about.title')}
        content={t('contents.about.content')}
      />
      <resumeContents.Interests
        title={t('contents.interests.title')}
        content={t('contents.interests.content', { returnObjects: true })}
      />
    </div>
  );
};

export default Resume;
```

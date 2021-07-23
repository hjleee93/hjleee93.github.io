---
title: "국제화(localization) 자동화"
excerpt: "국제화 자동화 처리하기(구글 스프레트시트 이용)"
categories:
  - frontend
  - javascript
tags:
  - i18n
  - google-spreadSheet
  - 국제화
  - HyeonLog
---

<style>
@font-face { font-family: 'IBMPlexSansKR-Regular';
   src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_20-07@1.0/IBMPlexSansKR-Regular.woff') format('woff'); font-weight: normal; font-style: normal; }
body, a, h3, h4,h1{
font-family: 'IBMPlexSansKR-Regular';
}
td{
	border: 1px solid;
}
</style>

## 국제화(Localization) 자동화 처리하기

[국제화(i18n) 자동화 가이드](https://ui.toast.com/weekly-pick/ko_20210303) - 유동식님의 글을 전적으로 참고하여 작성하였습니다. 다만, 아직 부족한 부분이 많아 좀 더 디테일한 사항들을 기록해두고 싶어 쓴 포스팅입니다.

**사용 언어**

\- javascript



### 구글 스트레드시트 연결

1. 스프레드시트 생성 구글 시트 api 연결

   1. cloud platform 이동[https://console.cloud.google.com/apis/dashboard]
   2. 사용자 인증정보 탭에서 api 생성 후
   3. 사용자 인증 정보(서비스 계정) 만들기
   4. 해당 계정 서비스 계정 수정(키)
   5. 키 추가 - 새 키 만들기
   6. JSON 형식 만들기
   7. 해당 파일 저장 (프로젝트에서 필요!!)
   8. 해당 계정 이메일 구글 스프레드 시트에 공유 설정

2. translationTool/.credentials 폴더 생성후 구글 스프레드시트에서 다운 받은 JSON 파일 저장

   - 해당 JSON파일 git ignore에 추가하기

3. root에 i18next-scanner.config.js 파일 생성 후 설정 파일 수정

   ```javascript
   const fs = require('fs');
   const chalk = require('chalk');
   
   module.exports = {
       input: [
           'src/**/*.{js,ts,vue}',
           // Use ! to filter out files or directories
           '!src/locales/**',
           '!**/node_modules/**',
       ],
   
       output: './',
       options: {
           debug: true,
           func: {
               list: ['i18next.t', 'i18n.t', '\\$t', 'this.\\$t'],
               extensions: ['.js', '.ts', '.vue']
           },
           trans: {
               component: 'Trans',
               i18nKey: 'i18nKey',
               defaultsKey: 'defaults',
               extensions: ['.js', '.ts', '.vue'],
               fallbackKey: function (ns, value) {
                   return value;
               },
               acorn: {
                   ecmaVersion: 10, // defaults to 10
                   sourceType: 'module', // defaults to 'module'
                   // Check out https://github.com/acornjs/acorn/tree/master/acorn#interface for additional options
               }
           },
           lngs: ['en', 'ko'],
           defaultLng: 'en',
           defaultValue: '__STRING_NOT_TRANSLATED__',
           resource: {
               loadPath: 'src/locales/{{lng}}/{{ns}}.json',
               savePath: 'src/locales/{{lng}}/{{ns}}.json',
               jsonIndent: 2,
               lineEnding: '\n'
           },
           nsSeparator: false, // namespace separator
           keySeparator: false, // key separator
           interpolation: {
               prefix: '{{',
               suffix: '}}'
           }
       }
   
   };
   ```

   

4. translationTool/index.js 파일 생성

   ```javascript
   const {GoogleSpreadsheet} = require('google-spreadsheet');
   //구글 sheet json 파일
   const creds = require('./.credentials/json파일.json');
   const i18nextConfig = require('../i18next-scanner.config');
   
   const spreadsheetDocId = '구글스프레드시트 id';
   const ns = 'translation';
   const lngs = i18nextConfig.options.lngs;
   //구글 스프레드시트의 gid  
   const sheetId = 1234;
   const loadPath = i18nextConfig.options.resource.loadPath;
   const localesPath = loadPath.replace('/{{lng}}/{{ns}}.json', '');
   const rePluralPostfix = new RegExp(/_plural|_[\d]/g);
   //번역이 필요없는 부분
   const NOT_AVAILABLE_CELL = 'N/A';
   //스프레드시트에 들어갈 header 설정
   const columnKeyToHeader = {
     key: 'key',
     'ko': 'ko',
     'en': 'en',
     
   };
   
   async function loadSpreadsheet() {
     // eslint-disable-next-line no-console
     console.info(
       '\u001B[32m',
       '=====================================================================================================================\n',
       '# i18next auto-sync using Spreadsheet\n\n',
       '  * Download translation resources from Spreadsheet and make /src/locales/{{lng}}/{{ns}}.json\n',
       '  * Upload translation resources to Spreadsheet.\n\n',
       `The Spreadsheet for translation is here (\u001B[34mhttps://docs.google.com/spreadsheets/d/${spreadsheetDocId}/#gid=${sheetId}\u001B[0m)\n`,
       '=====================================================================================================================',
       '\u001B[0m'
     );
   
     // spreadsheet key is the long id in the sheets URL
     const doc = new GoogleSpreadsheet(spreadsheetDocId);
   
     // load directly from json file if not in secure environment
     await doc.useServiceAccountAuth(creds);
   
     await doc.loadInfo(); // loads document properties and worksheets
   
     return doc;
   }
   
   function getPureKey(key = '') {
     return key.replace(rePluralPostfix, '');
   }
   
   module.exports = {
     localesPath,
     loadSpreadsheet,
     getPureKey,
     ns,
     lngs,
     sheetId,
     columnKeyToHeader,
     NOT_AVAILABLE_CELL
   };
   ```

   

5. translationTool/upload.js, translationTool/download.js 파일 생성

   ```javascript
   //download.js
   const fs = require('fs');
   const mkdirp = require('mkdirp');
   const {loadSpreadsheet, localesPath, ns, lngs, sheetId, columnKeyToHeader, NOT_AVAILABLE_CELL} = require('./index');
   
   //스프레드시트 -> json
   async function fetchTranslationsFromSheetToJson(doc) {
     const sheet = doc.sheetsById[sheetId];
     if (!sheet) {
       return {};
     }
   
     const lngsMap = {};
     const rows = await sheet.getRows();
   
     rows.forEach((row) => {
       const key = row[columnKeyToHeader.key];
       lngs.forEach((lng) => {
         const translation = row[columnKeyToHeader[lng]];
         // NOT_AVAILABLE_CELL("_N/A") means no related language
         if (translation === NOT_AVAILABLE_CELL) {
           return;
         }
   
         if (!lngsMap[lng]) {
           lngsMap[lng] = {};
         }
   
         lngsMap[lng][key] = translation || ''; // prevent to remove undefined value like ({"key": undefined})
       });
     });
   
     return lngsMap;
   }
   
   //디렉토리 설정
   function checkAndMakeLocaleDir(dirPath, subDirs) {
     return new Promise((resolve) => {
       subDirs.forEach((subDir, index) => {
         mkdirp(`${dirPath}/${subDir}`, (err) => {
           if (err) {
             throw err;
           }
   
           if (index === subDirs.length - 1) {
             resolve();
           }
         });
       });
     });
   }
   
   //json 파일 업데이트
   async function updateJsonFromSheet() {
     await checkAndMakeLocaleDir(localesPath, lngs);
   
     const doc = await loadSpreadsheet();
     const lngsMap = await fetchTranslationsFromSheetToJson(doc);
   
     fs.readdir(localesPath, (error, lngs) => {
       if (error) {
         throw error;
       }
   
       lngs.forEach((lng) => {
         const localeJsonFilePath = `${localesPath}/${lng}/${ns}.json`;
   
         const jsonString = JSON.stringify(lngsMap[lng], null, 2);
   
         fs.writeFile(localeJsonFilePath, jsonString, 'utf8', (err) => {
           if (err) {
             throw err;
           }
         });
       });
     });
   }
   
   updateJsonFromSheet();
   ```

   ```javascript
   //upload.js
   const fs = require('fs');
   
   const {
     loadSpreadsheet,
     localesPath,
     getPureKey,
     ns,
     lngs,
     sheetId,
     columnKeyToHeader,
     NOT_AVAILABLE_CELL,
   } = require('./index');
   
   const headerValues = ['key', 'ko', 'en'];
   
   async function addNewSheet(doc, title, sheetId) {
     const sheet = await doc.addSheet({
       sheetId,
       title,
       headerValues,
     });
   
     return sheet;
   }
   
   async function updateTranslationsFromKeyMapToSheet(doc, keyMap) {
       //시트 타이틀 
     const title = 'localization';
     let sheet = doc.sheetsById[sheetId];
     if (!sheet) {
       sheet = await addNewSheet(doc, title, sheetId);
     }
   
     const rows = await sheet.getRows();
   
     // find exsit keys
     const exsitKeys = {};
     const addedRows = [];
   
     rows.forEach((row) => {
       const key = row[columnKeyToHeader.key];
       if (keyMap[key]) {
         exsitKeys[key] = true;
       }
     });
   
     //스프레트시트에 row 넣는 부분
     for (const [key, translations] of Object.entries(keyMap)) {
       if (!exsitKeys[key]) {
         const row = {
           [columnKeyToHeader.key]: key,
           ...Object.keys(translations).reduce((result, lng) => {
             const header = columnKeyToHeader[lng];
             result[header] = translations[lng];
   
             return result;
           }, {}),
         };
   
         addedRows.push(row);
       }
     }
   
     // upload new keys
     await sheet.addRows(addedRows);
   }
   
   // key값에 따른 언어 value 
   function toJson(keyMap) {
     const json = {};
   
     Object.entries(keyMap).forEach(([__, keysByPlural]) => {
       for (const [keyWithPostfix, translations] of Object.entries(keysByPlural)) {
         json[keyWithPostfix] = {
           ...translations,
         };
       }
     });
   
     return json;
   }
   
   //언어 key : value 값 저장
   function gatherKeyMap(keyMap, lng, json) {
     for (const [keyWithPostfix, translated] of Object.entries(json)) {
       const key = getPureKey(keyWithPostfix);
   
       if (!keyMap[key]) {
         keyMap[key] = {};
       }
   
       const keyMapWithLng = keyMap[key];
       if (!keyMapWithLng[keyWithPostfix]) {
         keyMapWithLng[keyWithPostfix] = lngs.reduce((initObj, lng) => {
           initObj[lng] = NOT_AVAILABLE_CELL;
   
           return initObj;
         }, {});
       }
   
       keyMapWithLng[keyWithPostfix][lng] = translated;
     }
   }
   
   async function updateSheetFromJson() {
     const doc = await loadSpreadsheet();
   
     fs.readdir(localesPath, (error, lngs) => {
       if (error) {
         throw error;
       }
   
       const keyMap = {};
   
       lngs.forEach((lng) => {
         const localeJsonFilePath = `${localesPath}/${lng}/${ns}.json`;
   
         //.json file read
         // eslint-disable-next-line no-sync
         const json = fs.readFileSync(localeJsonFilePath, 'utf8');
   
         gatherKeyMap(keyMap, lng, JSON.parse(json));
       });
       //스프레드 시트에 업데이트
       updateTranslationsFromKeyMapToSheet(doc, toJson(keyMap));
     });
   }
   
   updateSheetFromJson();
   ```

   

6. 컴포넌트 별로 읽기 때문에 스트레드 시트의 순서는 컴포넌트 별로 구성되어있음





**참고 문서**

- [국제화(i18n) 자동화 가이드](https://ui.toast.com/weekly-pick/ko_20210303) - 유동식님

- [i18next-scanner](https://i18next.github.io/i18next-scanner/)

- [google-spreadsheet](https://theoephraim.github.io/node-google-spreadsheet/#/getting-started/authentication?id=authentication)